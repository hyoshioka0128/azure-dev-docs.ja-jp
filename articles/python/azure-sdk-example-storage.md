---
title: Azure SDK for Python で Azure Storage をプロビジョニングして使用する
description: Azure SDK for Python ライブラリを使用して、Azure Storage アカウントに BLOB コンテナーをプロビジョニングし、そのコンテナーにファイルをアップロードします。
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 9b26dd4c5708231c807ea57979bed6bdcd9de25f
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2020
ms.locfileid: "83405105"
---
# <a name="example-use-the-azure-sdk-with-azure-storage"></a>例:Azure Storage で Azure SDK を使用する

この記事では、Python スクリプトで Azure SDK の管理ライブラリを使用し、Azure Storage アカウントと Blob Storage コンテナーを含んだリソース グループをプロビジョニングする方法について説明します。 さらに、Python アプリケーション コードで Azure SDK のクライアント ライブラリを使用して、その Blob Storage コンテナーにファイルをアップロードする方法についても説明します。

特に記載のない限り、この記事で使用されているコマンドはいずれも、Linux と Mac OS の bash および Windows のコマンド シェルで同じように動作します。

## <a name="1-set-up-your-local-development-environment"></a>1:ローカルの開発環境を設定する

「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」の手順をすべて実行します (まだ済んでいない場合)。

必ず、ローカル開発用のサービス プリンシパルを作成し、このプロジェクト用の仮想環境を作成してアクティブ化してください。

## <a name="2-install-the-needed-management-libraries"></a>2:必要な管理ライブラリをインストールする

1. この例で使用する管理ライブラリを列挙した *requirements.txt* ファイルを作成します。

    ```txt
    azure-mgmt-resource
    azure-mgmt-storage
    azure-cli-core
    ```

1. 仮想環境がアクティブになっているターミナルで、必要なものをインストールします。

    ```bash
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-storage-resources"></a>3:ストレージ リソースをプロビジョニングするコードを作成する

次のコードを使用して、*provision_blob.py* という名前の Python ファイルを作成します。 詳細はコメントで説明しています。

```python
import os, random

# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonSDKExample-Storage-rg"
LOCATION = "centralus"

# Step 1: Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group

# Step 2: Provision the storage account, starting with a management object.

storage_client = get_client_from_cli_profile(StorageManagementClient)

# This example uses the CLI profile credentials because we assume the script
# is being used to provision the resource in the same way the Azure CLI would be used.

STORAGE_ACCOUNT_NAME = f"pythonsdkstorage{random.randint(1,100000):05}"

# You can replace the storage account here with any unique name. A random number is used
# by default, but note that the name changes every time you run this script.
# The name must be 3-24 lower case letters and numbers only.


# Check if the account name is available. Storage account names must be unique across
# Azure because they're used in URLs.
availability_result = storage_client.storage_accounts.check_name_availability(STORAGE_ACCOUNT_NAME)

if not availability_result.name_available:
    print(f"Storage name {STORAGE_ACCOUNT_NAME} is already in use. Try another name.")
    exit()

# The name is available, so provision the account
poller = storage_client.storage_accounts.create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME,
    {
        "location" : LOCATION,
        "kind": "StorageV2",
        "sku": {"name": "Standard_LRS"}
    }
)

# Long-running operations return a poller object; calling poller.result()
# waits for completion.
account_result = poller.result()
print(f"Provisioned storage account {account_result.name}")

# Step 3: Retrieve the account's primary access key and generate a connection string.
keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME)

print(f"Primary key for storage account: {keys.keys[0].value}")

conn_string = f"DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName={STORAGE_ACCOUNT_NAME};AccountKey={keys.keys[0].value}"

print(f"Connection string: {conn_string}")

# Step 4: Provision the blob container in the account (this call is synchronous)
CONTAINER_NAME = "blob-container-01"
container = storage_client.blob_containers.create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME, CONTAINER_NAME, {})

# The fourth argument is a required BlobContainer object, but because we don't need any
# special values there, so we just pass empty JSON.

print(f"Provisioned blob container {container.name}")

```

このコードは、Azure CLI で直接認証を行う場合の操作をデモンストレーションしているため、CLI ベースの認証方法 (`get_client_from_cli_profile`) を使用しています。 どちらの場合も、同じ ID を認証に使用することになります。

そのようなコードを本番スクリプトで使用する場合は、代わりに `DefaultAzureCredential` (推奨) またはサービス プリンシパル ベースの方法を使用してください (「[Azure サービスを使用して Python アプリを認証する方法](azure-sdk-authenticate.md)」を参照)。

## <a name="4-run-the-script"></a>4.次のスクリプトを実行します。

```bash
python provision_blob.py
```

スクリプトが完了するまでに 1 分から 2 分かかります。

## <a name="5-verify-the-resources"></a>5:リソースを確認する

1. [Azure portal](https://portal.azure.com) を開いて、意図したとおりにリソース グループとストレージ アカウントがプロビジョニングされていることを確認します。 Python スクリプトからプロビジョニングしたストレージ アカウントを表示するには、リソース グループで **[非表示の型の表示]** を選択しなければならない場合があります。

    ![新しいリソース グループの Azure portal ページに表示されるストレージ アカウント](media/azure-sdk-example-storage/portal-show-hidden-types.png)

1. ストレージ アカウントを選択して、左側のメニューから **[Blob service]**  >  **[Containers]** を選択し、"bloc-container-01" が表示されることを確認します。

    ![ストレージ アカウントの Azure portal ページに表示される BLOB コンテナー](media/azure-sdk-example-storage/portal-show-blob-containers.png)

Azure Storage の管理ライブラリを使用するその他の例については、[Python でのストレージ管理のサンプル](https://docs.microsoft.com/samples/azure-samples/storage-python-manage/storage-python-manage/)を参照してください。

### <a name="for-reference-equivalent-azure-cli-commands"></a>参考: 同等の Azure CLI コマンド

次の Azure CLI コマンドは、Python スクリプトと同じプロビジョニング手順を実行するものです。

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonSDKExample-Storage-rg -l centralus \
    -n pythonsdkstorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonSDKExample-Storage-rg \
    -n pythonsdkstorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonsdkstorage12345 -n blob-container-01
```

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonSDKExample-Storage-rg -l centralus ^
    -n pythonsdkstorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonSDKExample-Storage-rg ^
    -n pythonsdkstorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

set AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonsdkstorage12345 -n blob-container-01
```

---

## <a name="6-use-resources-through-the-sdk-client-libraries"></a>6: SDK クライアント ライブラリを介してリソースを使用する

以降のセクションでは、前のセクションでプロビジョニングした BLOB コンテナーにアクセスする 2 とおりの方法を紹介します。 具体的には、適切な SDK クライアント ライブラリを使用して、そのコンテナーにファイル BLOB をアップロードする例を取り上げています。

[共通の手順](#common-steps-for-both-methods)に従って、ご自身でコードを試してみましょう。

1 つ目の方法では、[Python アプリを認証する方法](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential)に関するページで説明されている `DefaultAzureCredential` を使用してアプリを認証します。 この方法では最初に、アプリの ID に適切なアクセス許可を割り当てる必要があります。これが、推奨されるプラクティスです。

2 つ目の方法では、接続文字列を使用してストレージ アカウントに直接アクセスします。 こちらの方法の方が一見シンプルに見えますが、大きな欠点が 2 つあります。

- 接続文字列は本質的に、接続元のエージェントをストレージ "*アカウント*" に対して認証するものであり、そのアカウント内の個々のリソースに対して認証を行うものではありません。 そのため、接続文字列では、許可する承認の範囲が必要以上に広くなってしまう可能性があります。

- 接続文字列はアクセス キーをプレーンテキストで保持するため、適切に構築されていなかったり、セキュリティが適切に確保されていなかったりした場合に脆弱性のリスクが生じます。 そのような接続文字列が公開されれば、ストレージ アカウント内の広範なリソースへのアクセスに使用される可能性があります。

以上の理由から、運用環境のコードでは認証方法を使用するようにしてください。 ただし実験目的であれば、接続文字列を使用しても問題ありません。

### <a name="common-steps-for-both-methods"></a>両方の方法に共通の手順

1. *requirements.txt* ファイルに、必要なクライアント ライブラリの行を追加し、ファイルを保存します。

    ```text
    azure-storage-blob
    azure-identity
    ```

1. ターミナルまたはコマンド プロンプトで、要件を再インストールします。

    ```bash
    pip install -r requirements.txt
    ```

1. 次のような内容を含んだソース ファイルを *sample-source.txt* という名前 (コードで想定) で作成します。

    ```text
    Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.
    ```

### <a name="use-blob-storage-with-authentication"></a>認証によって Blob Storage を使用する

1. `AZURE_STORAGE_BLOB_URL` という名前の環境変数を作成します。

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    AZURE_STORAGE_BLOB_URL=https://pythonsdkstorage12345.blob.core.windows.net
    ```

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set AZURE_STORAGE_BLOB_URL=https://pythonsdkstorage12345.blob.core.windows.net
    ```

    ---

    "pythonsdkstorage12345" は、実際のストレージ アカウントの名前に置き換えてください。

1. 次のコードを含んだ *use_blob_auth.py* という名前のファイルを作成します。 ステップは、コメントで説明しています。

    ```python
    import os
    from azure.identity import DefaultAzureCredential

    # Import the client object from the SDK library
    from azure.storage.blob import BlobClient

    credential = DefaultAzureCredential()

    # Retrieve the storage blob service URL, which is of the form
    # https://pythonsdkstorage12345.blob.core.windows.net/
    storage_url = os.environ["AZURE_STORAGE_BLOB_URL"]

    # Create the client object using the storage URL and the credential
    blob_client = BlobClient(storage_url, container_name="blob-container-01", blob_name="sample-blob.txt", credential=credential)

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

1. コードの実行を試みます。

    ```bash
    python use_blob_auth.py
    ```

    使用しているローカル サービス プリンシパルには、BLOB コンテナーへのアクセス許可がないため、"This request is not authorized to perform this operation using this permission. (この要求は承認されません。このアクセス許可を使用してこの操作を実行することはできません)" というエラーが表示されます。

1. サービス プリンシパルにコンテナーのアクセス許可を付与するために、Azure CLI コマンド [az role assignment create](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create) を使用します。コマンドが長いので注意してください。

    # <a name="bash"></a>[bash](#tab/bash)

    ```azurecli
    az role assignment create --assignee $AZURE_CLIENT_ID \
        --role "Storage Blob Data Contributor" \
        --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonSDKExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonsdkstorage12345/blobServices/default/containers/blob-container-01"
    ```

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```azurecli
    az role assignment create --assignee %AZURE_CLIENT_ID% ^
        --role "Storage Blob Data Contributor" ^
        --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonSDKExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonsdkstorage12345/blobServices/default/containers/blob-container-01"
    ```

    ---

    `--scope` 引数は、対象となるロールの割り当て先を指定するものです。 この例では、"ストレージ BLOB データ共同作成者" ロールを、"blob-container-01" という名前の "*特定*" のコンテナーに付与しています。

    `pythonsdkstorage12345` は、ストレージ アカウントの正確な名前に置き換えてください。 必要であれば、リソース グループと BLOB コンテナーの名前を調整することもできます。 間違った名前を使用すると、"ネストされたリソースに対して要求された操作を実行できません。親リソース 'pythonsdkstorage12345' が見つかりません" というエラーが表示されます。 Parent resource 'pythonsdkstorage12345' not found."

    このコマンドの `--scope` 引数には、AZURE_CLIENT_ID と AZURE_SUBSCRIPTION_ID という環境変数が使用されています。これらは、「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」に従い、ローカル環境でサービス プリンシパルに設定したものです。

1. もう一度コードを実行して、今度は正しく機能することを確認します。 アクセス許可エラーが再び表示された場合は、アクセス許可が伝播されるまで数分待ってから、コードの実行を再試行してください。

スコープとロールの割り当てについて詳しくは、[ロールのアクセス許可を割り当てる方法](how-to-assign-role-permissions.md)に関するページを参照してください。

### <a name="use-blob-storage-with-a-connection-string"></a>接続文字列を利用して Blob Storage を使用する

1. 次のコードを使用して、*use_blob_conn_string.py* という名前の Python ファイルを作成します。 ステップは、コメントで説明しています。

    ```python
    import os

    # Import the client object from the SDK library
    from azure.storage.blob import BlobClient

    # Retrieve the connection string from an environment variable.
    conn_string = os.environ["AZURE_STORAGE_CONNECTION_STRING"]

    # Create the client object for the resource identified by the connection string,
    # indicating also the blob container and the name of the specific blob we want.
    blob_client = BlobClient.from_connection_string(conn_string, container_name="blob-container-01", blob_name="sample-blob.txt")

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

1. コードを実行します。

    ```bash
    python use_blob_conn_string.py
    ```

既に述べたように、この方法はシンプルではありますが、接続文字列によって、ストレージ アカウント内のすべての操作が承認されます。 前のセクションで説明したように、運用環境のコードでは、具体的なアクセス許可を使用することをお勧めします。

### <a name="verify-blob-creation"></a>BLOB の作成を確認する

どちらかの方法のコードを実行した後、[Azure portal](https://portal.azure.com) にアクセスし、BLOB コンテナーに移動して、*sample-source.txt* ファイルと同じ内容の新しい BLOB が *sample-blob.txt* という名前で存在することを確認します。

![BLOB コンテナーの Azure portal ページに表示されるアップロード済みのファイル](media/azure-sdk-example-storage/portal-blob-container-file.png)

## <a name="7-clean-up-resources"></a>7:リソースをクリーンアップする

```azurecli
az group delete -n PythonSDKExample-Storage-rg
```

この例でプロビジョニングしたリソースを残す必要がなければ、今後サブスクリプションに課金されないようにするために、このコマンドを実行してください。

コードから [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) メソッドを使用してリソース グループを削除することもできます。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [例:Web アプリをプロビジョニングしてコードをデプロイする >>>](azure-sdk-example-web-app.md)
