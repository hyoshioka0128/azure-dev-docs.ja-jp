---
title: Azure SDK for Python を使用してリソース グループをプロビジョニングする
description: Azure SDK for Python のリソース管理ライブラリを使用して、Python コードからリソース グループを作成します。
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 99b2f721cf2c215d2ba9ea97cf78250fc65f165a
ms.sourcegitcommit: fbbc341a0b9e17da305bd877027b779f5b0694cc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83631573"
---
# <a name="example-use-the-azure-sdk-to-provision-a-resource-group"></a>例:Azure SDK を使用してリソース グループをプロビジョニングする

この例では、Python スクリプトで Azure SDK の管理ライブラリを使用してリソース グループをプロビジョニングする方法について説明します。

特に記載のない限り、この記事で使用されているコマンドはいずれも、Linux と Mac OS の bash および Windows のコマンド シェルで同じように動作します。

## <a name="1-set-up-your-local-development-environment"></a>1:ローカルの開発環境を設定する

「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」の手順をすべて実行します (まだ済んでいない場合)。

必ず、ローカル開発用のサービス プリンシパルを作成し、このプロジェクト用の仮想環境を作成してアクティブ化してください。

## <a name="2-install-the-resource-management-library"></a>2:リソース管理ライブラリをインストールする

*requirements.txt* という名前のファイルを作成し、内容を次のようにします。

```text
azure-mgmt-resource
azure-cli-core
```

仮想環境をアクティブ化し、ターミナルまたはコマンド プロンプトで要件をインストールします。

```bash
pip install -r requirements.txt
```

## <a name="3-write-code-to-provision-a-resource-group"></a>3:リソース グループをプロビジョニングするコードを作成する

次のコードを使用して、*provision_rg.py* という名前の Python ファイルを作成します。 詳細はコメントで説明しています。

```python
# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(
    "PythonSDKExample-ResourceGroup-rg",
    {
        "location": "centralus"
    }
)

# Within the ResourceManagementClient is an object named resource_groups,
# which is of class ResourceGroupsOperations, which contains methods like
# create_or_update.
#
# The second parameter to create_or_update here is technically a ResourceGroup
# object. You can create the object directly using ResourceGroup(location=LOCATION)
# or you can express the object as inline JSON as shown here. For details,
# see Inline JSON pattern for object arguments at
# https://docs.microsoft.com/azure/developer/python/azure-sdk-overview#inline-json-pattern-for-object-arguments.

print(f"Provisioned resource group {rg_result.name} in the {rg_result.location} region")

# The return value is another ResourceGroup object with all the details of the
# new group. In this case the call is synchronous: the resource group has been
# provisioned by the time the call returns.

# Optional line to delete the resource group
#resource_client.resource_groups.delete("PythonSDKExample-ResourceGroup-rg")
```

このコードは、Azure CLI で直接認証を行う場合の操作をデモンストレーションしているため、CLI ベースの認証方法 (`get_client_from_cli_profile`) を使用しています。 どちらの場合も、同じ ID を認証に使用することになります。

そのようなコードを本番スクリプトで使用する場合は、代わりに `DefaultAzureCredential` (推奨) またはサービス プリンシパル ベースの方法を使用してください (「[Azure サービスを使用して Python アプリを認証する方法](azure-sdk-authenticate.md)」を参照)。

## <a name="4-run-the-script"></a>4:スクリプトを実行する

```bash
python provision_rg.py
```

## <a name="5-verify-the-resource-group"></a>5:リソース グループを確認する

グループが存在することは、Azure portal または Azure CLI から確認できます。

- Azure portal: [Azure portal](https://portal.azure.com) を開いて **[リソース グループ]** を選択し、対象のグループが一覧に表示されていることを確認します。 既にポータルが開いている場合は、 **[最新の情報に更新]** コマンドを使用してリストを最新の情報に更新します。

- Azure CLI: 次のコマンドを実行します。

    ```azurecli
    az group show -n PythonSDKExample-ResourceGroup-rg
    ```

## <a name="6-clean-up-resources"></a>6: リソースをクリーンアップする

```azurecli
az group delete -n PythonSDKExample-ResourceGroup-rg
```

この例でプロビジョニングしたリソース グループを残す必要がなければ、このコマンドを実行してください。 リソース グループの料金がサブスクリプションに課金され続けることはありませんが、実際に使用していないグループは普段からクリーンアップするようにしておきましょう。

コードから [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) メソッドを使用してリソース グループを削除することもできます。

### <a name="for-reference-equivalent-azure-cli-commands"></a>参考: 同等の Azure CLI コマンド

次の Azure CLI コマンドは、Python スクリプトと同じプロビジョニング手順を実行するものです。

```azurecli
az group create -n PythonSDKExample-ResourceGroup-rg -l centralus
```

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [例:Azure Storage の使用 >>>](azure-sdk-example-storage.md)