---
title: 'チュートリアル、パート 6: Azure サービスで Python アプリを認証する'
description: API エンドポイントに必要な DefaultAzureCredential オブジェクトとクライアント オブジェクトを設定する、メイン アプリのスタートアップ コードについて説明します。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 5fc344fcf00655bbac7b2a20a2401ffb52482447
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379529"
---
# <a name="part-6-main-app-startup-code"></a>パート 6: メイン アプリのスタートアップ コード

[前のパート:依存関係、import ステートメント、および環境変数](walkthrough-tutorial-authentication-05.md)

`import` ステートメントに続くアプリのスタートアップ コードは、HTTP 要求を処理する関数で使用されるさまざまな変数を初期化します。

まず、Flask アプリ オブジェクトを作成し、環境変数からサードパーティ API のエンドポイント URL を取得します。

```python
app = Flask(__name__)

number_url = os.environ["THIRD_PARTY_API_ENDPOINT"]
```

次に、[`DefaultAzureCredential`](/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) オブジェクトを取得します。これは、Azure サービスでの認証時に使用することが推奨されている資格情報です。 [Python アプリの認証方法](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential)に関する記事を参照してください。

```python
credential = DefaultAzureCredential()
```

ローカルで実行すると、`DefaultAzureCredential` は、ローカル サービス プリンシパルに関する情報を含む `AZURE_TENANT_ID`、`AZURE_CLIENT_ID`、および `AZURE_CLIENT_SECRET` 環境変数を検索します。 クラウドで実行すると、`DefaultAzureCredential` は、アプリ用に登録されているサービス プリンシパルを自動的に使用します。これは通常、マネージド ID に含まれています。

次に、コードは、Azure Key Vault からサードパーティ API のアクセス キーを取得します。 プロビジョニング スクリプトでは、[`az keyvault create`](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) を使用して Key Vault を作成し、[`az keyvault secret set`](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) を使用してシークレットを格納します。

Key Vault リソース自体には、`KEY_VAULT_URL` 環境変数から読み込まれた URL を使用してアクセスします。

```python
key_vault_url = os.environ["KEY_VAULT_URL"]
```

Key Vault に接続するには、適切なクライアント オブジェクトを作成する必要があります。 シークレットを取得する必要があるため、[`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python) を使用します。これには、Key Vault の URL と、アプリの実行に使用される ID を表す資格情報オブジェクトが必要です。

```python
keyvault_client = SecretClient(vault_url=key_vault_url, credential=credential)
```

`SecretClient` オブジェクトを作成しても、資格情報は認証されません。 `SecretClient` は、リソース URL と資格情報を内部で管理するクライアント側の構成体にすぎません。 認証と認可は、[`get_secret`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-) など、クライアントを介して操作が呼び出された場合にのみ行われます。これにより、Azure リソースへの REST API 呼び出しが生成されます。

```python
api_secret_name = os.environ["THIRD_PARTY_API_SECRET_NAME"]
vault_secret = keyvault_client.get_secret(api_secret_name)

# The "secret" from Key Vault is an object with multiple properties. The key we
# want for the third-party API is in the value property. 
access_key = vault_secret.value
```

アプリ ID は、Key Vault へのアクセスを認可されている場合でも、シークレットへのアクセスを明示的に認可されている必要があります。  そうでない場合、`get_secret` 呼び出しは失敗します。 このため、プロビジョニング スクリプトは、Azure CLI コマンド [`az keyvault set-policy`](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) を使用して、アプリの "シークレットの取得" アクセス ポリシーを設定します。 詳細については、[Key Vault の認証](/azure/key-vault/general/authentication)および[アプリへの Key Vault のアクセス許可の付与](/azure/key-vault/general/managed-identity#grant-your-app-access-to-key-vault)に関する記事を参照してください。 後者の記事では、Azure portal を使用してアクセス ポリシーを設定する方法について説明しています。 (この記事はマネージド ID 用にも記述されていますが、開発で使用されるローカル サービス プリンシパルにも同様に適用されます。)

最後に、アプリ コードは、Azure Storage キューにメッセージを書き込むために使用できるクライアント オブジェクトを設定します。 キューの URL は、環境変数 `STORAGE_QUEUE_URL` にあります。

```python
queue_url = os.environ["STORAGE_QUEUE_URL"]
queue_client = QueueClient.from_queue_url(queue_url=queue_url, credential=credential)
```

Key Vault と同様に、Azure ライブラリからの特定のクライアント オブジェクト [`QueueClient`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python) と、その [`from_queue_url`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python#from-queue-url-queue-url--credential-none----kwargs-) メソッドを使用して、対象の URL にあるリソースに接続します。 この場合も、このクライアント オブジェクトを作成しようとすると、資格情報によって表されるアプリ ID がキューへのアクセスを認可されているかどうかが検証されます。 前述のように、この認可は、メイン アプリに "ストレージ キュー データ共同作成者" ロールを割り当てることによって付与されています。

このスタートアップ コードがすべて成功すると仮定すると、アプリには、 */api/v1/getcode* API エンドポイントをサポートするためのすべての内部変数が設定されています。

> [!div class="nextstepaction"]
> [パート 7 - メイン アプリの API エンドポイント >>>](walkthrough-tutorial-authentication-07.md)
