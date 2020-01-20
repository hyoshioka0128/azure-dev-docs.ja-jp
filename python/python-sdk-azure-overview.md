---
title: Azure SDK for Python
description: Azure サービス使用時の開発者の生産性を向上させる、Azure SDK for Python の機能の概要。
ms.date: 10/30/2019
ms.topic: conceptual
ms.openlocfilehash: 83a9541330f270e51ffa5ed8952a93dee8ff132f
ms.sourcegitcommit: ac68fb174d606c7af2bfa79fe32b8ca7b73c86a1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946696"
---
# <a name="azure-sdk-for-python"></a>Azure SDK for Python

Azure SDK for Python は、Python アプリケーション コードからの Azure リソースの使用と管理を容易にします。 この SDK では Python 2.7 および Python 3.5.3 以降がサポートされています。

`pip install <library>` を使用して任意の個別のコンポーネント ライブラリをインストールすることによって、この SDK をインストールします。 さまざまなサービスのライブラリの名前については、[Azure SDK for Python のドキュメント](https://azure.github.io/azure-sdk-for-python/)を参照してください。

ライブラリをインストールしてプロジェクトにインポートする手順の詳細については、「[SDK のインストール](python-sdk-azure-install.md)」を参照してください。 その後、[SDK での作業開始](python-sdk-azure-get-started.yml)に関するページを確認して、認証を設定し、独自の Azure サブスクリプションに対してサンプル コードを実行します。

> [!TIP]
> SDK の変更点の詳細については、[SDK のリリースノート](https://azure.github.io/azure-sdk/)を参照してください。

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="connect-and-use-azure-services"></a>Azure サービスへの接続と使用

SDK の多数の*クライアント ライブラリ*を使用すると、既存の Azure リソースに接続し、それらをアプリで使用できます。たとえば、ファイルのアップロード、テーブル データへのアクセス、さまざまな Azure Cognitive Services の操作などを行うことができます。 SDK では、サービスの汎用 REST API を使用するのではなく、使い慣れた Python プログラミング パラダイムを使用してこれらのリソースを操作します。

たとえば、以前にプロビジョニングした Azure Storage アカウントに BLOB をアップロードするとします。 まず、適切なライブラリをインストールします。

```bash
pip install azure-storage-blob
```

次に、コードでライブラリをインポートします。

```python
from azure.storage.blob import BlobClient
```

最後に、ライブラリの API を使用して、データに接続し、アップロードします。 この例では、接続文字列とコンテナー名はストレージ アカウントで既にプロビジョニングされています。 BLOB 名は、アップロードされたデータに割り当てる名前です。

```python
blob = BlobClient.from_connection_string("my_connection_string", container="mycontainer", blob="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

各ライブラリでの操作の詳細については、[GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルを参照してください。 使用可能な [Azure サンプル](https://docs.microsoft.com/samples/browse/?languages=python)も参照してください。

また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api?view=azure-python)で確認できます。

### <a name="the-azure-core-library"></a>Azure コア ライブラリ

現在、Python クライアント ライブラリは、コア機能 (再試行、ログ記録、トランスポート プロトコル、認証プロトコルなど) を共有するために更新中です。この共有機能は、[azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) ライブラリに含まれています。 ライブラリとそのガイドラインの詳細については、「[Python ガイドライン:概要](https://azure.github.io/azure-sdk/python_introduction.html)」を参照してください。

現時点で、このコア ライブラリと共に使用できるライブラリは、以下のとおりです。

- `azure-storage-blob`
- `azure-storage-queue`
- `azure-keyvault-keys`
- `azure-keyvault-secrets`

## <a name="manage-azure-resources"></a>Azure のリソースを管理する

Azure SDK for Python には、Azure リソース自体の作成、プロビジョニング、およびその他の管理を行うのに役立つ多くのライブラリも含まれています。 これらは*管理ライブラリ*と呼ばれています。 各管理ライブラリには `azure-mgmt-<service name>` という名前が付けられています。 管理ライブラリを使用すると、[Azure portal](https://portal.azure.com) または [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) を使用して実行できるのと同じタスクを Python コードを記述して実行できます。

たとえば、SQL Server インスタンスを作成するとします。 まず、適切な管理ライブラリをインストールします。

```bash
pip install azure-mgmt-sql
```

Python コードでライブラリをインポートします。

```python
from azure.mgmt.sql import SqlManagementClient

```

次に、資格情報と Azure サブスクリプション ID を使用して、管理クライアント オブジェクトを作成します。

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

最後に、そのクライアント オブジェクトを使用して、適切なリソース グループ名、サーバー名、場所、および管理者の資格情報を使ってリソースを作成します。

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

クライアント ライブラリの場合と同様に、各管理ライブラリでの操作の詳細については、[GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルで確認できます。

また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api?view=azure-python)で確認できます。 

## <a name="get-help-and-give-feedback"></a>質問とフィードバック

- [Azure SDK for Python のドキュメント](https://aka.ms/python-docs)をご覧ください。
- ご質問は、[Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) のコミュニティに投稿してください。
- [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) でこの SDK に対して問題を開いてください。
- Twitter で [@AzureSDK](https://twitter.com/AzureSdk/) にお知らせください。
