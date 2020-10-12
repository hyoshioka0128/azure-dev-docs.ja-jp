---
title: 'チュートリアル、パート 4: Azure サービスで Python アプリを認証する'
description: すべてのコードを含む、メイン アプリの実装の概要。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e2a43f7e204ba3f077beea7cc878076111f71313
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764736"
---
# <a name="part-4-example-main-application-implementation"></a>パート 4:メイン アプリケーションの実装例

[前のパート:サードパーティ API の実装例](walkthrough-tutorial-authentication-03.md)

このシナリオのメイン アプリは、Azure App Service にデプロイされた簡単な Flask アプリです。 このアプリには、 */api/v1/getcode* という名前のパブリック API エンドポイントが用意されています。これにより、アプリで他の目的のコードが生成されます (たとえば、人間のユーザー向けの 2 要素認証など)。

動作中のエンドポイントを確認するには、ブラウザーで [https://msdocs-main-app-example.azurewebsites.net/api/v1/getcode](https://msdocs-main-app-example.azurewebsites.net/api/v1/getcode) にアクセスするか、curl を使用して要求を行います。

メイン アプリには、API エンドポイントへのリンクを表示する単純なホーム ページも用意されています。 アプリのこの部分は [https://msdocs-main-app-example.azurewebsites.net](https://msdocs-main-app-example.azurewebsites.net) で確認できます。

サンプルのプロビジョニング スクリプトでは次の手順が実行されます。

1. App Service ホストを作成し、Azure CLI コマンド [`az webapp up`](/cli/azure/webapp#az-webapp-up) を使用してコードをデプロイします。

1. メイン アプリの Azure Storage アカウントをプロビジョニングします ([`az storage account create`](/cli/azure/storage/account#az-storage-account-create) を使用)。

1. ストレージ アカウントに "code-requests" という名前のキューを作成します ([`az storage queue create`](/cli/azure/storage/queue#az-storage-queue-create) を使用)。

1. アプリがキューへの書き込みを許可されるようにするには、[`az role assignment create`](/cli/azure/role/assignment#az-role-assignment-create) を使用して、アプリに "ストレージ キュー データ共同作成者" ロールを割り当てます。 ロールの詳細については、[Azure CLI を使用してロールのアクセス許可を割り当てる方法](/azure/role-based-access-control/role-assignments-cli)に関するページを参照してください。

メイン アプリのコードは次のとおりです。重要な詳細については、このシリーズの次のパートで説明します。

```python
from flask import Flask, request, jsonify
import requests, random, string, os
from datetime import datetime
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential
from azure.storage.queue import QueueClient

app = Flask(__name__)

# Retrieve the URL of the third-party API endpoint we invoke.
number_url = os.environ["THIRD_PARTY_API_ENDPOINT"]

# Authenticate with Azure. First, obtain the credential object. To run locally,
# you must have the necessary environment variables set up to provide the
# local service principal information. When deployed to the cloud, the app must
# have managed identity enabled in Azure App Service.
credential = DefaultAzureCredential()

# Next, get a client object for the Key Vault. The client object is strictly
# a client-side construct provided by the Azure libraries as a layer on top
# of the Azure REST API.
key_vault_url = os.environ["KEY_VAULT_URL"]
keyvault_client = SecretClient(vault_url=key_vault_url, credential=credential)

# Obtain the secret: for this step to work you must add the app's identity to
# the key vault's access policies for secret management. When deployed to the cloud
# the identity is the name of the app in App Service; when running locally, the
# identity is the local service principal.
api_secret_name = os.environ["THIRD_PARTY_API_SECRET_NAME"]
vault_secret = keyvault_client.get_secret(api_secret_name)

# The "secret" from Key Vault is an object with multiple properties. The access key
# we want for the third-party API is in the secret's value property.
access_key = vault_secret.value

#Set up the Storage queue client to which we write messages
queue_url = os.environ["STORAGE_QUEUE_URL"]
queue_client = QueueClient.from_queue_url(queue_url=queue_url, credential=credential)


@app.route('/', methods=['GET'])
def home():
    return f'Home page of the main app. Make a request to <a href="./api/v1/getcode">/api/v1/getcode</a>.'


def random_char(num):
       return ''.join(random.choice(string.ascii_letters) for x in range(num))


@app.route('/api/v1/getcode', methods=['GET'])
def get_code():
    headers = {
        'Content-Type': 'application/json',
        'x-functions-key': access_key
        }

    r = requests.get(url = number_url, headers = headers)

    if (r.status_code != 200):
        return "Could not get you a code.", r.status_code

    data = r.json()
    chars1 = random_char(3)
    chars2 = random_char(3)
    code_value = f"{chars1}-{data['value']}-{chars2}"
    code = { "code": code_value, "timestamp" : str(datetime.utcnow()) }

    # Log a queue message with the code for, say, a process that invalidates
    # the code after a certain period of time.
    queue_client.send_message(code)

    return jsonify(code)


if __name__ == '__main__':
    app.run()
```

> [!div class="nextstepaction"]
> [パート 5 - メイン アプリの依存関係、import、および環境変数 >>>](walkthrough-tutorial-authentication-05.md)
