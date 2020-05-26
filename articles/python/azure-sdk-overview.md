---
title: Azure SDK for Python を使用する
description: Azure リソースのプロビジョニング、使用、管理の際の開発者の生産性を向上させる、Azure SDK for Python の機能の概要。
ms.date: 05/13/2020
ms.topic: conceptual
ms.openlocfilehash: 856487b06e7a84b0659126d8959ce45b5309a103
ms.sourcegitcommit: fbbc341a0b9e17da305bd877027b779f5b0694cc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2020
ms.locfileid: "83631544"
---
# <a name="use-the-azure-sdk-for-python"></a>Azure SDK for Python を使用する

オープンソースである Azure SDK for Python は、Python アプリケーション コードからの Azure リソースのプロビジョニング、管理、使用を容易にします。

## <a name="the-details-you-really-want-to-know"></a>重要情報

- この SDK は Python 2.7 と Python 3.5.3 以降をサポートし、PyPy 5.4 以降でもテストされています。

- この SDK は、特定の Azure サービスに関連した 180 を超える個別の Python ライブラリから成ります。

- 必要なライブラリは、`pip install <library_name>` でインストールします ([リリース リスト](https://azure.github.io/azure-sdk/releases/latest/all/python.html)にあるライブラリ名を使用)。 詳細については、[Azure SDK ライブラリのインストール](azure-sdk-install.md)に関するページを参照してください。

- "管理" と "クライアント" という別個のライブラリが存在します ("管理プレーン" ライブラリおよび "データ プレーン" ライブラリと呼ばれることもあります)。 各セットは異なる目的を果たし、異なる種類のコードで使用されます。 詳細については、この記事の次のセクションを参照してください。
  - [管理ライブラリを使用して Azure リソースをプロビジョニングし、管理する](#provision-and-manage-azure-resources-with-management-libraries)
  - [クライアント ライブラリを使用して Azure リソースに接続し、そのリソースを使用する](#connect-to-and-use-azure-resources-with-client-libraries)

- SDK のドキュメントは、[Azure SDK for Python のリファレンス](/python/api/overview/azure/?view=azure-python) (Azure サービスごとに編成) または [Python API ブラウザー](/python/api/?view=azure-python) (パッケージ名ごとに編成) でご覧いただけます。 現時点では、多くの場合、目的のクラスやメソッドにたどりつくために多数のレイヤーをクリックしなければなりません。 ご不便をおかけして申し訳ありません。 改善に向けて取り組んでまいります。

- ご自身でライブラリを試してみたい場合は、「[例: リソース グループを作成する](azure-sdk-example-resource-group.md)」と「[例: Azure Storage の使用](azure-sdk-example-storage.md)」を順にお読みください。

### <a name="non-essential-but-still-interesting-details"></a>必須ではないが興味深い詳細情報

- Azure CLI は Python で SDK 管理ライブラリを使用して書かれているため、Azure CLI のコマンドでできることなら、Python スクリプトから実行することもできます。 とはいえ、CLI コマンドには、複数のタスクを同時に実行する、非同期操作を自動的に処理する、接続文字列のように出力の書式を設定するといった、便利な機能が数多く用意されています。 結論として、目的のプロセスに対するより厳格な制御が要求される場合を除けば、プロビジョニング スクリプトや管理スクリプトの自動化には、CLI (または同等の Azure PowerShell) を使用した方が、同等の Python コードを記述するよりもはるかに簡単です。

- Azure SDK for Python は、基になる Azure REST API を基盤とする Python レイヤーであるため、使い慣れた Python のパラダイムから、それらの API を使用することができます。 ただし、REST API は、必要に応じていつでも Python コードから直接使用できます。

- SDK のソース コードは [https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) でご覧いただけます。 オープンソース プロジェクトとして、皆さんの参加を歓迎します。

- SDK は、IronPython や Jython などのインタープリターと共に使用できますが、それらに対するテストは行っていません。個別の問題や非互換性が発生する場合があります。

- SDK ドキュメントのソース リポジトリは [https://github.com/MicrosoftDocs/azure-docs-sdk-python/](https://github.com/MicrosoftDocs/azure-docs-sdk-python/) でご覧いただけます。

- 現在、認証プロトコル、ログ記録、トレース、トランスポート プロトコル、バッファーされた応答、再試行などの一般的なクラウド パターンを共有するために、Azure SDK for Python のライブラリを更新しています。

  - この共有機能は、[azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) ライブラリに含まれています。

  - Core ライブラリで現在動作しているライブラリは、[Azure SDK for Python の最新リリース](https://azure.github.io/azure-sdk/releases/latest/#python) ページに記載されています。 これらのライブラリ (主にクライアント ライブラリ) は、"track 2" と呼ばれることがあります。

  - 管理ライブラリはまだ更新されておらず、"track 1" と呼ばれることがあります。

- SDK に適用されているガイドラインについて詳しくは、「[Python ガイドライン: 概要](https://azure.github.io/azure-sdk/python_introduction.html)」を参照してください。

## <a name="provision-and-manage-azure-resources-with-management-libraries"></a>管理ライブラリを使用して Azure リソースをプロビジョニングし、管理する

SDK の "*管理*" ("管理プレーン") ライブラリは、いずれも名前が `azure-mgmt-` で始まります。これは、Azure リソースの作成とプロビジョニング、その他管理を Python スクリプトから行うのに役立ちます。 すべての Azure サービスには、対応する管理ライブラリがあります。

管理ライブラリを使用すると、[Azure portal](https://portal.azure.com) または [Azure CLI](/cli/azure/install-azure-cli) で行うのと同じタスクを実行する構成スクリプトやデプロイ スクリプトを作成することができます。 (既に述べたように、Azure CLI は Python で記述されており、その各種コマンドは管理ライブラリを使用して実装されています。)

各管理ライブラリでの操作の詳細については、SDK の [GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルを参照してください。 また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api?view=azure-python)および [Azure のサンプル](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)で確認できます。

## <a name="connect-to-and-use-azure-resources-with-client-libraries"></a>クライアント ライブラリを使用して Azure リソースに接続し、そのリソースを使用する

SDK の "*クライアント*" ("データ プレーン") ライブラリを使用すると、既にプロビジョニングされたサービスとやり取りする Python アプリケーション コードを記述するのに役立ちます。 この SDK には、クライアント API をサポートするサービスのみを対象としたクライアント ライブラリが用意されています。

各クライアント ライブラリでの操作の詳細については、SDK の [GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルを参照してください。 また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api?view=azure-python)および [Azure のサンプル](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)で確認できます。

## <a name="inline-json-pattern-for-object-arguments"></a>オブジェクト引数に対するインライン JSON パターン

Azure SDK における多くの操作は、オブジェクト引数を個別のオブジェクト (またはインライン JSON) として表すことができる共通のパターンをサポートしています。

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

JSON を使用した場合、インライン JSON が SDK によって自動的に目的の引数の適切なオブジェクト型へと変換されます。

オブジェクトの中で、オブジェクト引数を入れ子にすることもできます。その場合、JSON を入れ子にして使用することもできます。

たとえば、[`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient?view=azure-python) オブジェクトのインスタンスがあり、その [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations?view=azure-python#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) メソッドを呼び出すとします。 このケースでは、第 3 引数の型が [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters?view=azure-python) であり、それ自体に [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties?view=azure-python) 型の引数が含まれます。 さらに、`VaultProperties` には、[`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku?view=azure-python) 型と [`list[AccessPolicyEntry`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry?view=azure-python) 型のオブジェクト引数が含まれます。 `Sku` には [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname?view=azure-python) オブジェクトが含まれ、個々の `AccessPolicyEntry` には [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions?view=azure-python) オブジェクトが含まれています。

埋め込みオブジェクトを使用して `create_or_update` を呼び出すには、次のようなコードを使用します (`tenant_id` と `object_id` は既に定義されているものとします)。

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

どちらの形式も完全に等価であるため、どちらでも好きな方を使用してください。両者が混在していてもかまいません。

JSON の書式が正しくないと、通常、"DeserializationError: Unable to deserialize to object: type, AttributeError: 'str' object has no attribute 'get' (DeserializationError: オブジェクトに逆シリアル化できません: 型、AttributeError: 'str' オブジェクトに属性 'get' がありません)" というエラーが発生します。 プロパティに対し、SDK では入れ子の JSON オブジェクトが想定されているところで、単一の文字列を指定していることが、このエラーの一般的な原因です。 たとえば、前の例で `"sku": "standard"` を使用すると、このエラーが発生します。`sku` パラメーターには、`Sku` オブジェクトとして、インライン オブジェクトの JSON (このケースでは `{ "name": "standard"}`) が想定されていて、それが目的の `SkuName` 型にマッピングされるためです。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [SDK ライブラリのインストール >>>](azure-sdk-install.md)

## <a name="get-help-and-connect-with-the-sdk-team"></a>SDK チームによるサポートと連絡先

- [Azure SDK for Python のドキュメント](https://aka.ms/python-docs)をご覧ください。
- ご質問は、[Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) のコミュニティに投稿してください。
- [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) でこの SDK に対して問題を開いてください。
- Twitter で [@AzureSDK](https://twitter.com/AzureSdk/) について言及する
