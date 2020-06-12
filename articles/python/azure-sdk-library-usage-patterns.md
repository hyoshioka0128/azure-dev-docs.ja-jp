---
title: Python 用 Azure ライブラリでの使用パターン
description: Python 用 Azure SDK ライブラリでの一般的な使用パターンの概要
ms.date: 05/26/2020
ms.topic: conceptual
ms.openlocfilehash: f712dc41233b8301e370c9eb63786d8e2d7f8c70
ms.sourcegitcommit: efab6be74671ea4300162e0b30aa8ac134d3b0a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/01/2020
ms.locfileid: "84256277"
---
# <a name="azure-libraries-for-python-usage-patterns"></a>Python 用 Azure ライブラリの使用パターン

Azure SDK for Python は、[Azure SDK for Python のインデックス ページ](https://azure.github.io/azure-sdk/releases/latest/all/python.html)に記載されている多数の独立したライブラリのみで構成されています。

すべてのライブラリは、インストールや、オブジェクト引数に対するインライン JSON の使用など、特定の共通の特性と使用パターンを共有します。

## <a name="library-installation"></a>ライブラリのインストール

特定のライブラリ パッケージをインストールするには、単に `pip install` を使用します。

```cmd
# Install the management library for Azure Storage
pip install azure-mgmt-storage
```

```cmd
# Install the client library for Azure Storage
pip install azure-storage-blob
```

`pip install` は、現在の Python 環境にライブラリの最新バージョンを取得します。

また、`pip` を使用してライブラリをアンインストールしたり、プレビュー バージョンを含む特定のバージョンをインストールしたりすることもできます。 詳細については、「[Python 用 Azure ライブラリ パッケージをインストールする方法](azure-sdk-install.md)」を参照してください。

## <a name="inline-json-pattern-for-object-arguments"></a>オブジェクト引数に対するインライン JSON パターン

Azure ライブラリ内の多くの操作では、オブジェクト引数を個別のオブジェクトまたはインライン JSON として表すことができます。

たとえば、[`ResourceManagementClient`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.resourcemanagementclient?view=azure-python) オブジェクトがあり、その [`create_or_update`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#create-or-update-resource-group-name--parameters--custom-headers-none--raw-false----operation-config-) メソッドを使用してリソース グループを作成するとします。 このメソッドの第 2 引数の型は、[`ResourceGroup`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.models.resourcegroup?view=azure-python) です。

`create_or_update` を呼び出すために、`ResourceGroup` の独立したインスタンスを、その必須の引数 (このケースでは `location`) を使って直接作成することができます。

```python
rg_result = resource_client.resource_groups.create_or_update(
    "PythonSDKExample-rg",
    ResourceGroup(location="centralus")
)
```

または、同じパラメーターをインライン JSON として渡すこともできます。

```python
rg_result = resource_client.resource_groups.create_or_update(
    "PythonSDKExample-rg",
    {
      "location": "centralus"
    }
)
```

JSON を使用した場合、Azure ライブラリにより、インライン JSON は、目的の引数の適切なオブジェクト型に自動的に変換されます。

オブジェクトの中で、オブジェクト引数を入れ子にすることもできます。その場合、JSON を入れ子にして使用することもできます。

たとえば、[`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient?view=azure-python) オブジェクトのインスタンスがあり、その [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations?view=azure-python#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) メソッドを呼び出すとします。 このケースでは、第 3 引数の型が [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters?view=azure-python) であり、それ自体に [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties?view=azure-python) 型の引数が含まれます。 さらに、`VaultProperties` には、[`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku?view=azure-python) 型と [`list[AccessPolicyEntry`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry?view=azure-python) 型のオブジェクト引数が含まれます。 `Sku` には [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname?view=azure-python) オブジェクトが含まれ、個々の `AccessPolicyEntry` には [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions?view=azure-python) オブジェクトが含まれています。

埋め込みオブジェクトを使用して `create_or_update` を呼び出すには、次のようなコードを使用します (`tenant_id` と `object_id` は既に定義されているものとします)。 また、関数呼び出しの前に、必要なオブジェクトを作成することもできます。

```python
operation = keyvault_client.vaults.create_or_update(
    "PythonSDKExample-rg",
    "keyvault01",
    VaultCreateOrUpdateParameters(
        location="centralus",
        properties=VaultProperties(
            tenant_id=tenant_id,
            sku=Sku(name="standard"),
            access_policies=[
                AccessPolicyEntry(
                    tenant_id=tenant_id,
                    object_id=object_id,
                    permissions=Permissions(keys=['all'], secrets=['all'])
                )
            ]
        )
    )
)
```

インライン JSON を使用して同じ呼び出しを記述した場合は次のようになります。

```python
operation = keyvault_client.vaults.create_or_update(
    "PythonSDKExample-rg",
    "keyvault01",
    {
        'location': 'centralus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': tenant_id,
            'access_policies': [{
                'object_id': object_id,
                'tenant_id': tenant_id,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```

どちらの形式も同等のため、どちらでも好きな方を使用できます。両者が混在していてもかまいません。

JSON の書式が正しくないと、通常、"DeserializationError:Unable to deserialize to object: type, AttributeError: 'str' object has no attribute 'get' (DeserializationError: オブジェクトに逆シリアル化できません: 型、AttributeError: 'str' オブジェクトに属性 'get' がありません)" というエラーが発生します。 このエラーの一般的な原因は、ライブラリでは入れ子になった JSON オブジェクトを予期しているのに、プロパティに単一の文字列を指定していることです。 たとえば、前の例で `"sku": "standard"` を使用すると、このエラーが発生します。`sku` パラメーターには、`Sku` オブジェクトとして、インライン オブジェクトの JSON (このケースでは `{ "name": "standard"}`) が想定されていて、それが目的の `SkuName` 型にマッピングされるためです。

## <a name="next-steps"></a>次のステップ

Python 用 Azure ライブラリを使用するための一般的なパターンを理解したところで、次のスタンドアロンの例を参照して、特定の管理とクライアント ライブラリのシナリオを検討します。

- [例:リソース グループを作成する](azure-sdk-example-resource-group.md)
- [例:Azure Storage を使用する](azure-sdk-example-storage.md)
- [例:Web アプリをプロビジョニングしてコードをデプロイする](azure-sdk-example-web-app.md)
- [例:仮想マシンをプロビジョニングする](azure-sdk-example-virtual-machines.md)

これらの例は連続的でも相互依存的でもないため、任意の順序で試すことができます。
