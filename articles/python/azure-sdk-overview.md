---
title: Azure SDK for Python
description: Azure リソースのプロビジョニング、使用、管理の際の開発者の生産性を向上させる、Azure SDK for Python の機能の概要。
ms.date: 03/17/2020
ms.topic: conceptual
ms.openlocfilehash: 3d24a512420610f37285a03fe6a39d81e97510ee
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82026115"
---
# <a name="azure-sdk-for-python"></a>Azure SDK for Python

オープンソース である Azure SDK for Python は、Python アプリケーション コードからの Azure リソースのプロビジョニング、使用、管理を容易にします。 この SDK では Python 2.7 および Python 3.5.3 以降がサポートされています。

SDK は個々のコンポーネント ライブラリで構成されており、それぞれ `pip install <library>` を使用してインストールします。 詳細な手順については、[SDK のインストール](azure-sdk-install.md)に関する記事を参照してください。 [Azure SDK for Python のドキュメント](https://azure.github.io/azure-sdk-for-python/)には、ライブラリの名前が示されています。

また、チュートリアル「[Azure SDK for Python の使用を開始する](azure-sdk-get-started.yml)」に従って、ライブラリを体験することもできます。

> [!TIP]
> SDK の変更点の詳細については、[SDK のリリースノート](https://azure.github.io/azure-sdk/)を参照してください。

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="management-and-client-libraries"></a>管理およびクライアント ライブラリ

Azure SDK for Python には、次の 2 種類のライブラリが含まれています。

- "*管理*" ライブラリは、Azure リソースのプロビジョニングと管理に役立ちます。これは、[Azure portal](https://portal.azure.com)、または [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) や [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) などのコマンドライン ツールを使用して行うことができます。 管理ライブラリは、通常、構成とデプロイのスクリプトで使用します。 (実際には、Azure CLI 自体が Python 管理ライブラリで作成されています)。

- "*クライアント*" ライブラリは、アプリケーション コードが、自然な Python 表現を使用して、既にプロビジョニングされたサービスと対話する手段を提供します。 内部的には、クライアント ライブラリでは Azure の REST API が使用されていますが、SDK を使用することで REST の詳細について心配する必要はなくなります。

以下のセクションでは、各カテゴリの詳細と例について説明します。

## <a name="manage-azure-resources-with-management-libraries"></a>管理ライブラリを使用して Azure リソースを管理する

Azure SDK for Python の管理ライブラリは、それぞれ `azure-mgmt-<service name>` という名前が付けられており、Azure リソースの作成、プロビジョニング、および管理を支援します。

たとえば、SQL Server インスタンスを作成するとします。 まず、適切な管理ライブラリをインストールします。

```bash
pip install azure-mgmt-sql
```

Python コードでライブラリをインポートします。

```python
from azure.mgmt.sql import SqlManagementClient
```

次に、資格情報 ([SDK を使用した認証](azure-sdk-authenticate.md)に関する記事を参照) と Azure サブスクリプション ID を使用して、管理クライアント オブジェクトを作成します。

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

最後に、その管理クライアント オブジェクトを使用して、適切なリソース グループ名、サーバー名、場所、および管理者の資格情報を使ってリソースを作成します。

```python
server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

各管理ライブラリでの操作の詳細については、SDK の [GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルを参照してください。 また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api?view=azure-python)および [Azure のサンプル](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)で確認できます。

## <a name="connect-and-use-azure-services-with-client-libraries"></a>クライアント ライブラリを使用した Azure サービスへの接続と使用

Azure SDK の*クライアント ライブラリ*を使用すると、既存の Azure リソースに接続し、それらをアプリで使用できます。たとえば、ファイルのアップロード、テーブル データへのアクセス、さまざまな Azure Cognitive Services の操作などを行うことができます。 SDK では、サービスの汎用 REST API を使用するのではなく、使い慣れた Python プログラミング パラダイムを使用してこれらのリソースを操作します。 (REST API を持たないサービスは、クライアント ライブラリによって表されません)。

たとえば、Web アプリを Azure アプリ サービスにデプロイしており、Azure Storage アカウントにファイルをアップロードする機能を追加するとします。 まず、適切なライブラリをインストールします。

```bash
pip install azure-storage-blob
```

次に、コードでライブラリをインポートします。

```python
from azure.storage.blob import BlobClient
```

最後に、ライブラリの API を使用して、データに接続し、アップロードします。 この例では、接続文字列とコンテナー名はストレージ アカウントで既にプロビジョニングされています。 BLOB 名は、アップロードされたデータに割り当てる名前です。

```python
blob = BlobClient.from_connection_string("my_connection_string", container_name="mycontainer", blob_name="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

各管理ライブラリでの操作の詳細については、SDK の [GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルを参照してください。 また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api?view=azure-python)および [Azure のサンプル](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)で確認できます。

### <a name="the-azure-core-library"></a>Azure コア ライブラリ

現在、認証プロトコル、ログ記録、トレース、トランスポート プロトコル、バッファーされた応答、再試行などの一般的なクラウド パターンを共有するために、Azure SDK for Python のクライアント ライブラリを更新しています。 この共有機能は、[azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) ライブラリに含まれています。 Core ライブラリで現在動作しているライブラリは、[Azure SDK の最新リリース](https://azure.github.io/azure-sdk/releases/latest/#python-packages) ページに記載されています。

Azure Core ライブラリとそのガイドラインの詳細については、「[Python ガイドライン:概要](https://azure.github.io/azure-sdk/python_introduction.html)」を参照してください。

## <a name="get-help-and-give-feedback"></a>質問とフィードバック

- [Azure SDK for Python のドキュメント](https://aka.ms/python-docs)をご覧ください。
- ご質問は、[Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) のコミュニティに投稿してください。
- [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) でこの SDK に対して問題を開いてください。
- Twitter で [@AzureSDK](https://twitter.com/AzureSdk/) について言及する
