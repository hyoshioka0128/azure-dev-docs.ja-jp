---
title: 'チュートリアル、パート 3: Azure サービスで Python アプリを認証する'
description: Azure Functions を使用したサードパーティ API の実装例と、アクセス キーを使用してエンドポイントがどのように保護されるのかについて説明します。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 84078a455843cb28f80a633bb5344bc5ab645ac7
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379534"
---
# <a name="part-3-example-third-party-api-implementation"></a>パート 3: サードパーティ API の実装例

[前のパート:認証の課題](walkthrough-tutorial-authentication-02.md)

このサンプル シナリオでは、メイン アプリのパブリック エンドポイントは、アクセス キーによって保護されたサードパーティ API を使用しています。 このセクションでは、Azure Functions を使用したサードパーティ API の実装について説明しますが、API は他の方法で実装し、別のクラウド サーバーや Web ホストにデプロイすることもできます。 唯一の重要な点は、エンドポイントが、すべてのクライアント要求に含まれている必要がある特定のアクセス キーによって保護されていることです。 この API を呼び出すすべてのアプリは、そのキーを安全に管理する必要があります。

デモの目的で、この API はエンドポイント `https://msdocs-api-example.azurewebsites.net/api/RandomNumber` にデプロイされています。 ただし、この API を呼び出すには、`?code=` URL パラメーター、または HTTP ヘッダーの `'x-functions-key'` プロパティのいずれかにアクセス キー `d0c5atM1cr0s0ft` を指定する必要があります。 たとえば、ブラウザーまたは curl で次の URL を試してみてください: [https://msdocs-api-example.azurewebsites.net/api/RandomNumber?code=d0c5atM1cr0s0ft](https://msdocs-api-example.azurewebsites.net/api/RandomNumber?code=d0c5atM1cr0s0ft)。

アクセス キーが有効な場合、エンドポイントは、単一のプロパティ "value" を含む JSON 応答を返します。この値は、1 から 999 の範囲の数値です (`{"value": 959}` など)。

エンドポイントは Python で実装され、Azure Functions にデプロイされます。 コードは次のとおりです。

```python
import logging
import random
import json

import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('RandomNumber invoked via HTTP trigger.')

    random_value = random.randint(1, 1000)
    dict = { "value" : random_value }
    return func.HttpResponse(json.dumps(dict))
```

サンプル リポジトリ内で、このコードは *third_party_api/RandomNumber/\_\_init\_\_.py* にあります。 *RandomNumber* フォルダーは関数の名前を提供し、 *\_\_init\_\_.py* にはコードが含まれます。 そのフォルダー内の別のファイルである *function.json* には、関数がトリガーされるタイミングが記述されています。 *third_party_api* 親フォルダー内の他のファイルは、関数自体をホストする Azure Functions "アプリ" の詳細を提供します。

コードをデプロイするために、サンプルのプロビジョニング スクリプトは次の手順を実行します。

1. Azure CLI コマンド [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) を使用して、Azure Functions 用のバッキング ストレージ アカウントを作成します。

1. Azure CLI コマンド [`az function app create`](/cli/azure/functionapp?view=azure-cli-latest#az-functionapp-create) を使用して、Azure Functions "アプリ" を作成します。

1. ホストが完全にプロビジョニングされるまで 60 秒間待機した後、[Azure Functions Core Tools](/azure/azure-functions/functions-run-local?tabs=linux%2Ccsharp%2Cbash) コマンド [`func azure functionapp publish`](/azure/azure-functions/functions-run-local?tabs=linux%2Ccsharp%2Cbash#project-file-deployment) を使用してコードをデプロイします。

1. アクセス キー `d0c5atM1cr0s0ft` を関数に割り当てます。 (関数キーの背景については、「[Azure Functions のセキュリティ保護](/azure/azure-functions/security-concepts)」を参照してください。)

    プロビジョニング スクリプトでは、この手順は [Functions Key Management API](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) への REST API 呼び出しを介して行われます。これは、Azure CLI では、現在この特定の機能がサポートされていないためです。 その REST API を呼び出すには、プロビジョニング スクリプトは、まず別の REST API 呼び出しを使用して関数アプリのマスター キーを取得する必要があります。

    [Azure portal](https://portal.azure.com) を使用してアクセス キーを割り当てることもできます。 Functions アプリのページで、 **[関数]** を選択してから、セキュリティで保護する特定の関数 (この例では `RandomNumber` という名前) を選択します。 関数のページで **[関数キー]** を選択して、これらのキーを作成して管理するページを開きます。

> [!div class="nextstepaction"]
> [パート 4 - メイン アプリの実装例 >>>](walkthrough-tutorial-authentication-04.md)
