---
title: Python 用 Azure ライブラリを使用して Azure Storage をプロビジョニングする
description: Azure SDK for Python ライブラリを使用して、Azure Storage アカウントに BLOB コンテナーをプロビジョニングし、そのコンテナーにファイルをアップロードします。
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 82d7f83a426e56e4e235d1d4bfcfb8c73042e053
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764551"
---
# <a name="example-provision-azure-storage-using-the-azure-libraries-for-python"></a>例:Python 用 Azure ライブラリを使用して Azure Storage をプロビジョニングする

この記事では、Python スクリプトで Azure 管理ライブラリを使用して、Azure Storage アカウントと Blob Storage コンテナーを含むリソース グループをプロビジョニングする方法について説明します。 ([同等の Azure CLI コマンド](#for-reference-equivalent-azure-cli-commands)については、この記事の後半で説明します。 Azure portal を使用する場合は、「[Azure Storage アカウントの作成](/azure/storage/common/storage-account-create?tabs=azure-portal)」と [BLOB コンテナーの作成](/azure/storage/blobs/storage-quickstart-blobs-portal)に関する記事を参照してください。)

リソースのプロビジョニング後、[Azure Storage の使用例](azure-sdk-example-storage-use.md)に関するページを参照し、Python アプリケーション コードで Azure クライアント ライブラリを使用し、Blob Storage コンテナーにファイルをアップロードします。

特に記載のない限り、この記事で使用されているコマンドはいずれも、Linux と macOS の bash および Windows のコマンド シェルで同じように動作します。

## <a name="1-set-up-your-local-development-environment"></a>1:ローカルの開発環境を設定する

「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」の手順をすべて実行します (まだ済んでいない場合)。

必ず、ローカル開発用のサービス プリンシパルを作成し、このプロジェクト用の仮想環境を作成してアクティブ化してください。

## <a name="2-install-the-needed-azure-library-packages"></a>2:必要な Azure ライブラリ パッケージをインストールする

1. この例で使用する管理ライブラリを列挙した *requirements.txt* ファイルを作成します。

    ```txt
    azure-mgmt-resource
    azure-mgmt-storage
    azure-identity
    ```

1. 仮想環境がアクティブになっているターミナルで、必要なものをインストールします。

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-storage-resources"></a>3:ストレージ リソースをプロビジョニングするコードを作成する

このセクションでは、Python コードからストレージ リソースをプロビジョニングする方法について説明します。 必要に応じて、Azure portal や[同等の Azure CLI コマンド](#for-reference-equivalent-azure-cli-commands)を使用してリソースをプロビジョニングすることもできます。

次のコードを使用して、*provision_blob.py* という名前の Python ファイルを作成します。 詳細はコメントで説明しています。

```python
import os, random

# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonAzureExample-Storage-rg"
LOCATION = "centralus"

# Step 1: Provision the resource group.

rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


# Step 2: Provision the storage account, starting with a management object.

storage_client = StorageManagementClient(credential, subscription_id)

# This example uses the CLI profile credentials because we assume the script
# is being used to provision the resource in the same way the Azure CLI would be used.

STORAGE_ACCOUNT_NAME = f"pythonazurestorage{random.randint(1,100000):05}"

# You can replace the storage account here with any unique name. A random number is used
# by default, but note that the name changes every time you run this script.
# The name must be 3-24 lower case letters and numbers only.


# Check if the account name is available. Storage account names must be unique across
# Azure because they're used in URLs.
availability_result = storage_client.storage_accounts.check_name_availability(
    { "name": STORAGE_ACCOUNT_NAME }
)

if not availability_result.name_available:
    print(f"Storage name {STORAGE_ACCOUNT_NAME} is already in use. Try another name.")
    exit()

# The name is available, so provision the account
poller = storage_client.storage_accounts.begin_create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME,
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

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>コードで使用されているクラスの参照リンク

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)
- [StorageManagementClient (azure.mgmt.storage)](/python/api/azure-mgmt-storage/azure.mgmt.storage.storagemanagementclient)

## <a name="4-run-the-script"></a>4.スクリプトを実行する

```cmd
python provision_blob.py
```

スクリプトが完了するまでに 1 分から 2 分かかります。

## <a name="5-verify-the-resources"></a>5:リソースを確認する

1. [Azure portal](https://portal.azure.com) を開いて、意図したとおりにリソース グループとストレージ アカウントがプロビジョニングされていることを確認します。 Python スクリプトからプロビジョニングしたストレージ アカウントを表示するには、リソース グループで **[非表示の型の表示]** を選択しなければならない場合があります。

    ![新しいリソース グループの Azure portal ページに表示されるストレージ アカウント](media/azure-sdk-example-storage/portal-show-hidden-types.png)

1. ストレージ アカウントを選択して、左側のメニューから **[Blob service]**  >  **[Containers]** を選択し、"bloc-container-01" が表示されることを確認します。

    ![ストレージ アカウントの Azure portal ページに表示される BLOB コンテナー](media/azure-sdk-example-storage/portal-show-blob-containers.png)

1. これらのプロビジョニングされたリソースをアプリケーション コードから使用する場合は、引き続き、[Azure Storage の使用例](azure-sdk-example-storage-use.md)に関するページを参照してください。

Azure Storage の管理ライブラリを使用するその他の例については、[Python でのストレージ管理のサンプル](/samples/azure-samples/storage-python-manage/storage-python-manage/)を参照してください。

### <a name="for-reference-equivalent-azure-cli-commands"></a>参考: 同等の Azure CLI コマンド

次の Azure CLI コマンドは、Python スクリプトと同じプロビジョニング手順を実行するものです。

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
rem Provision the resource group

az group create -n PythonAzureExample-Storage-rg -l centralus

rem Provision the storage account

az storage account create -g PythonAzureExample-Storage-rg -l centralus ^
    -n pythonazurestorage12345 --kind StorageV2 --sku Standard_LRS

rem Retrieve the connection string

az storage account show-connection-string -g PythonAzureExample-Storage-rg ^
    -n pythonazurestorage12345

rem Provision the blob container; NOTE: this command assumes you have an environment variable
rem named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

set AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonazurestorage12345 -n blob-container-01
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonAzureExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonAzureExample-Storage-rg -l centralus \
    -n pythonazurestorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonAzureExample-Storage-rg \
    -n pythonazurestorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonazurestorage12345 -n blob-container-01
```

---

## <a name="6-clean-up-resources"></a>6: リソースをクリーンアップする

[Azure Storage の使用例](azure-sdk-example-storage-use.md)に関する記事を参照してアプリ コードでこれらのリソースを使用する場合は、リソースは残しておきます。

それ以外の場合は、次のコマンドを実行して、サブスクリプションでの継続的な課金を回避します。

```azurecli
az group delete -n PythonAzureExample-Storage-rg  --no-wait
```

[!INCLUDE [resource_group_begin_delete](includes/resource-group-begin-delete.md)]

## <a name="see-also"></a>関連項目

- [例:Azure Storage を使用する](azure-sdk-example-storage-use.md)
- [例:リソース グループをプロビジョニングする](azure-sdk-example-resource-group.md)
- [例:Web アプリをプロビジョニングしてコードをデプロイする](azure-sdk-example-web-app.md)
- [例:データベースをプロビジョニングしてクエリを実行する](azure-sdk-example-database.md)
- [例:仮想マシンをプロビジョニングする](azure-sdk-example-virtual-machines.md)
