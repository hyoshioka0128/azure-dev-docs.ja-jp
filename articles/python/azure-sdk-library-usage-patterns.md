---
title: Python 用 Azure ライブラリでの使用パターン
description: Python 用 Azure SDK ライブラリでの一般的な使用パターンの概要
ms.date: 11/12/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 28c2013245765d6829e46f564f7a7a3c74cb996d
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759533"
---
# <a name="azure-libraries-for-python-usage-patterns"></a>Python 用 Azure ライブラリの使用パターン

Azure SDK for Python は、[Python SDK パッケージ インデックス](azure-sdk-library-package-index.md)に一覧表示されている、多数の独立したライブラリのみで構成されています。

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

## <a name="asynchronous-operations"></a>非同期操作

クライアントおよび管理クライアント オブジェクトを介して呼び出す多くの操作 ([`ComputeManagementClient.virtual_machines.begin_create_or_update`](/python/api/azure-mgmt-compute/azure.mgmt.compute.v2020_06_01.operations.virtualmachinesoperations#begin-create-or-update-resource-group-name--vm-name--parameters----kwargs-)、[`WebSiteManagementClient.web_apps.create_or_update`](/python/api/azure-mgmt-web/azure.mgmt.web.v2019_08_01.operations.webappsoperations#create-or-update-resource-group-name--name--site-envelope--custom-headers-none--raw-false--polling-true----operation-config-) など) では、`AzureOperationPoller[<type>]` 型のオブジェクトが返されます。ここで、`<type>` は該当する操作に固有のものです。

これらのメソッドはどちらも非同期です。 メソッド名が異なるのは、バージョンが異なるためです。 azure.core に基づいていない以前のライブラリでは、通常、`create_or_update` などの名前が使用されます。 azure.core に基づくライブラリでは、非同期であることを示すために、メソッド名の前に `begin_` が付加されます。 新しい azure.core ベースのライブラリへの以前のコードの移行は、ほとんどのメソッド シグネチャが同じままであるため、通常、メソッド名の前に `begin_` を付加することを意味します。

どちらの場合も、戻り値の型 [`AzureOperationPoller`](/python/api/msrestazure/msrestazure.azure_operation.azureoperationpoller) は、操作が非同期であることを明確に意味します。 したがって、そのポーラーの `result` メソッドを呼び出して、操作が完了するまで待機し、その結果を取得する必要があります。

次のコードは、[例: Web アプリのプロビジョニングとデプロイ](azure-sdk-example-web-app.md)に関するページから抜粋したものであり、ポーラーを使用して結果を待機する例を示しています。

```python
poller = app_service_client.web_apps.create_or_update(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": LOCATION,
        "server_farm_id": plan_result.id,
        "site_config": {
            "linux_fx_version": "python|3.8"
        }
    }
)

web_app_result = poller.result()
```

この場合、`create_or_update` の戻り値の型は `AzureOperationPoller[Site]` であり、`poller.result()` の戻り値が [Site](/python/api/azure-mgmt-web/azure.mgmt.web.v2019_08_01.models.site) オブジェクトであることを意味します。

## <a name="exceptions"></a>例外

一般に、Azure ライブラリでは、失敗した Azure REST API への HTTP 要求など、意図したとおりに操作を実行できなかった場合に例外が発生します。 そのため、アプリ コードでは、`try...except` ブロックを使用してライブラリ操作を囲むことができます。

発生する可能性のある例外の種類の詳細については、該当する操作のドキュメントを参照してください。

## <a name="logging"></a>ログ記録

最新の Azure ライブラリでは、Python の標準的な `logging` ライブラリを使用してログ出力が生成されます。 個々のライブラリ、ライブラリのグループ、またはすべてのライブラリのログ記録レベルを設定できます。 ログ記録ストリーム ハンドラーを登録したら、特定のクライアント オブジェクトまたは特定の操作のログ記録を有効にすることができます。 詳細については、[Azure ライブラリでのログ記録](azure-sdk-logging.md)に関するページを参照してください。

## <a name="proxy-configuration"></a>[プロキシ構成]

プロキシを指定するために、環境変数または省略可能な引数を使用できます。 詳細については、[プロキシの構成方法](azure-sdk-configure-proxy.md)に関するページを参照してください。

## <a name="optional-arguments-for-client-objects-and-methods"></a>クライアント オブジェクトおよびメソッドの省略可能な引数

ライブラリ リファレンス ドキュメントでは、多くの場合、クライアント オブジェクト コンストラクターまたは特定の操作メソッドのシグネチャに、`**kwargs` または `**operation_config**` 引数が表示されます。 これらのプレースホルダーは、該当するオブジェクトまたはメソッドで追加の名前付き引数がサポートされる可能性があることを示します。 通常、リファレンス ドキュメントには、使用できる特定の引数が示されます。 次のセクションで説明されているように、多くの場合、いくつかの一般的な引数もサポートされます。

### <a name="arguments-for-libraries-based-on-azurecore"></a>azure.core に基づくライブラリの引数

これらの引数は、「[Python」の「新しいライブラリ](https://azure.github.io/azure-sdk/releases/latest/#python)」に一覧表示されているライブラリに適用されます。

| 名前                       | 種類 | Default     | 説明 |
| ---                        | ---  | ---         | ---         |
| logging_enable             | [bool] | False       | ログ記録を有効にします。 詳細については、[Azure ライブラリでのログ記録](azure-sdk-logging.md)に関するページを参照してください。 |
| proxies                    | dict | {}          | プロキシ サーバーの URL。 詳細については、[プロキシの構成方法](azure-sdk-configure-proxy.md)に関するページを参照してください。 |
| use_env_settings           | [bool] | True        | True の場合、`HTTP_PROXY` および `HTTPS_PROXY` 環境変数をプロキシで使用できるようにします。 False の場合、環境変数は無視されます。 詳細については、[プロキシの構成方法](azure-sdk-configure-proxy.md)に関するページを参照してください。 |
| connection_timeout         | INT  | 300         | Azure REST API エンドポイントへの接続を確立するときのタイムアウト (秒)。 |
| read_timeout               | INT  | 300         | Azure REST API 操作を完了するとき (つまり、応答を待機するとき) のタイムアウト (秒)。 |
| retry_total                | INT  | 10          | REST API 呼び出しに対して許容される再試行の回数。 再試行を無効にするには `retry_total=0` を使用します。 |
| retry_mode                 | enum | exponential | 再試行のタイミングを直線的または指数関数的に適用します。 'single' の場合、再試行は一定の間隔で行われます。 'exponential' の場合、各再試行の待機時間は、前の再試行の 2 倍となります。 |

個々のライブラリには、これらの引数のいずれかをサポートする義務はありません。したがって、詳細については、各ライブラリのリファレンス ドキュメントを常に参照してください。

### <a name="arguments-for-non-core-libraries"></a>非コア ライブラリの引数

| 名前               | 種類 | Default | 説明 |
| ---                | ---  | ---     | ---         |
| 確認             | [bool] | True    | SSL 証明書を確認します。 |
| cert               | str  | なし    | クライアント側で確認するためのローカル証明書へのパス。 |
| timeout            | INT  | 30      | サーバー接続を確立するときのタイムアウト時間 (秒単位)。 |
| allow_redirects    | [bool] | False   | リダイレクトを有効にします。 |
| max_redirects      | INT  | 30      | 許可されるリダイレクトの最大回数。 |
| proxies            | dict | {}      | プロキシ サーバーの URL。 詳細については、[プロキシの構成方法](azure-sdk-configure-proxy.md)に関するページを参照してください。 |
| use_env_proxies    | [bool] | False   | ローカル環境変数からのプロキシ設定の読み取りを有効にします。 |
| retries            | INT  | 10      | 許容される再試行回数の合計。 |
| enable_http_logger | [bool] | False   | デバッグ モードで HTTP のログを有効にします。 |

## <a name="inline-json-pattern-for-object-arguments"></a>オブジェクト引数に対するインライン JSON パターン

Azure ライブラリ内の多くの操作では、オブジェクト引数を個別のオブジェクトまたはインライン JSON として表すことができます。

たとえば、[`ResourceManagementClient`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.resourcemanagementclient) オブジェクトがあり、その [`create_or_update`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations#create-or-update-resource-group-name--parameters--custom-headers-none--raw-false----operation-config-) メソッドを使用してリソース グループを作成するとします。 このメソッドの第 2 引数の型は、[`ResourceGroup`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.models.resourcegroup) です。

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

たとえば、[`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient) オブジェクトのインスタンスがあり、その [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) メソッドを呼び出すとします。 このケースでは、第 3 引数の型が [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters) であり、それ自体に [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties) 型の引数が含まれます。 さらに、`VaultProperties` には、[`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku) 型と [`list[AccessPolicyEntry]`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry) 型のオブジェクト引数が含まれます。 `Sku` には [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname) オブジェクトが含まれ、個々の `AccessPolicyEntry` には [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions) オブジェクトが含まれています。

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

両方の形式は同等であるため、いずれか好きな方を選択できます。また、これらを混在させることもできます。

JSON の形式が正しくない場合は、通常、"DeserializationError: Unable to deserialize to object: type, AttributeError: 'str' object has no attribute 'get' (DeserializationError: オブジェクトに逆シリアル化できません: 型、AttributeError: 'str' オブジェクトに属性 'get' がありません)" というエラーが発生します。 このエラーの一般的な原因は、ライブラリでは入れ子になった JSON オブジェクトを予期しているのに、プロパティに単一の文字列を指定していることです。 たとえば、前の例で `"sku": "standard"` を使用すると、このエラーが発生します。`sku` パラメーターには、`Sku` オブジェクトとして、インライン オブジェクトの JSON (このケースでは `{ "name": "standard"}`) が想定されていて、それが目的の `SkuName` 型にマッピングされるためです。

## <a name="next-steps"></a>次のステップ

Python 用 Azure ライブラリを使用するための一般的なパターンを理解できたので、以下に挙げるそれぞれ独立した例を参照して、管理とクライアント ライブラリの具体的なシナリオを検討します。 これらの例は連続的でも相互依存的でもないため、任意の順序で試すことができます。

- [例:リソース グループを作成する](azure-sdk-example-resource-group.md)
- [例:Azure Storage を使用する](azure-sdk-example-storage.md)
- [例:Web アプリをプロビジョニングしてコードをデプロイする](azure-sdk-example-web-app.md)
- [例:データベースをプロビジョニングしてクエリを実行する](azure-sdk-example-database.md)
- [例:仮想マシンをプロビジョニングする](azure-sdk-example-virtual-machines.md)
- [仮想マシンで Azure Managed Disks を使用する](azure-sdk-samples-managed-disks.md)

- [Azure SDK for Python に関する簡単なアンケートを完了する](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
