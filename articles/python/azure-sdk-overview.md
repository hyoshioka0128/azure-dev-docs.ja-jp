---
title: Python 用 Azure ライブラリ (SDK) を使用する
description: Azure リソースのプロビジョニング、使用、管理を行う際に開発者の生産性を向上させる、Python 用 Azure ライブラリの機能の概要。
ms.date: 09/19/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 3c1cd0deb0c1df31ef3b191a9526bc99ad01fbc1
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764695"
---
# <a name="use-the-azure-libraries-sdk-for-python"></a>Python 用 Azure ライブラリ (SDK) を使用する

オープンソースである Python 用 Azure ライブラリによって、Python アプリケーション コードからの Azure リソースのプロビジョニング、管理、使用が容易になります。

## <a name="the-details-you-really-want-to-know"></a>重要情報

- Azure ライブラリは、ローカルまたはクラウドで実行する Python コード "*から*" の Azure サービスとの通信手段です (特定のサービスのスコープ内で Python コードを実行できるかどうかは、そのサービス自体で Python がサポートされるかどうかによります)。

- このライブラリでは Python 2.7 と Python 3.5.3 以降がサポートされており、PyPy 5.4 以降でもテストされています。

- Azure SDK for Python は、特定の Azure サービスに関連した 180 を超える個別の Python ライブラリのみで構成されています。 "SDK" には他のツールはありません。

- コードをローカルで実行する場合、Azure による認証は、[ローカル開発環境の構成](configure-local-development-environment.md)に関するページで説明されている環境変数に依存します。 

- 必要なライブラリ パッケージは、`pip install <library_name>` を使ってインストールします ([Python SDK パッケージ インデックス](azure-sdk-library-package-index.md)に関するページにあるライブラリ名を使用)。 詳細については、[Azure ライブラリのインストール](azure-sdk-install.md)に関するページを参照してください。

- "管理" と "クライアント" という別個のライブラリが存在します ("管理プレーン" ライブラリおよび "データ プレーン" ライブラリと呼ばれることもあります)。 各セットはさまざまな目的を果たし、さまざまな種類のコードで使用されます。 詳細については、この記事の次のセクションを参照してください。
  - [管理ライブラリを使用して Azure リソースをプロビジョニングし、管理する](#provision-and-manage-azure-resources-with-management-libraries)
  - [クライアント ライブラリを使用して Azure リソースに接続し、そのリソースを使用する](#connect-to-and-use-azure-resources-with-client-libraries)

- ライブラリのドキュメントは、[Azure for Python のリファレンス](/python/api/overview/azure/) (Azure サービスごとに編成) または [Python API ブラウザー](/python/api/) (パッケージ名ごとに編成) でご覧いただけます。 現時点では、多くの場合、目的のクラスやメソッドにたどりつくために多数のレイヤーをクリックしなければなりません。 ご不便をおかけして申し訳ありません。 改善に向けて取り組んでまいります。

- 自分でライブラリを試すには、まず、[ローカルの開発環境を設定する](configure-local-development-environment.md)ことをお勧めします。 その後、次のいずれかのスタンドアロンの例を (任意の順序) で試すことができます。[例:リソース グループをプロビジョニングする](azure-sdk-example-resource-group.md)、[例: Azure Storage をプロビジョニングして使用する](azure-sdk-example-storage.md)、[例: Web アプリをプロビジョニングしてコードをデプロイする](azure-sdk-example-web-app.md)、[例: MySQL データベースをプロビジョニングして使用する](azure-sdk-example-database.md)、および[例: 仮想マシンをプロビジョニングする](azure-sdk-example-virtual-machines.md)。

- デモ ビデオについては、仮想 PyCon 2020 の「<a href="https://www.youtube.com/watch?v=M1pVxItg2Mg&feature=youtu.be&ocid=AID3006292" target="_blank">Using Azure SDKs to interact with Azure resource</a>」 (Azure SDK を使用して Azure リソースを操作する) (youtube.com) をご覧ください。

### <a name="non-essential-but-still-interesting-details"></a>必須ではないが興味深い詳細情報

- Azure CLI は管理ライブラリを使用して Python で書かれているため、Azure CLI のコマンドでできることはすべて、Python スクリプトからも実行できます。 とはいえ、CLI コマンドには、複数のタスクを同時に実行する、非同期操作を自動的に処理する、接続文字列のように出力の書式を設定するといった、便利な機能が数多く用意されています。 結論として、目的のプロセスに対するより厳格な制御が要求される場合を除けば、プロビジョニング スクリプトや管理スクリプトの自動化には、CLI (または同等の Azure PowerShell) を使用した方が、同等の Python コードを記述するよりもはるかに簡単です。

- Python 用 Azure ライブラリは、基になる Azure REST API の上に構築されているため、使い慣れた Python のパラダイムからそれらの API を使用することができます。 ただし、REST API は、必要に応じていつでも Python コードから直接使用できます。

- Azure ライブラリのソース コードは、[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) にあります。 オープンソース プロジェクトとして、皆さんの参加を歓迎します。

- ライブラリは、IronPython や Jython などのインタープリターと共に使用できますが、それらに対するテストは行われていません。個別の問題や非互換性が発生する場合があります。

- ライブラリ API リファレンス ドキュメントのソース リポジトリは、[https://github.com/MicrosoftDocs/azure-docs-sdk-python/](https://github.com/MicrosoftDocs/azure-docs-sdk-python/) にあります。

- 現在、認証プロトコル、ログ記録、トレース、トランスポート プロトコル、バッファーされた応答、再試行などの一般的なクラウド パターンを共有するために、Python 用 Azure ライブラリを更新しています。

  - この共有機能は、[azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) ライブラリに含まれています。

  - Core ライブラリで現在動作しているライブラリは、[Azure SDK for Python の最新リリース](azure-sdk-library-package-index.md#libraries-using-azurecore) ページに記載されています。 これらのライブラリ (主にクライアント ライブラリ) は、"track 2" と呼ばれることがあります。

  - 管理ライブラリと、まだ更新されていないその他のものは、"track 1" と呼ばれることがあります。

- ライブラリに適用されているガイドラインについて詳しくは、「[Python ガイドライン: 概要](https://azure.github.io/azure-sdk/python_introduction.html)」を参照してください。

## <a name="provision-and-manage-azure-resources-with-management-libraries"></a>管理ライブラリを使用して Azure リソースをプロビジョニングし、管理する

SDK の "*管理*" ("管理プレーン") ライブラリは、いずれも名前が `azure-mgmt-` で始まります。これは、Azure リソースの作成とプロビジョニング、その他管理を Python スクリプトから行うのに役立ちます。 すべての Azure サービスには、対応する管理ライブラリがあります。

管理ライブラリを使用すると、[Azure portal](https://portal.azure.com) または [Azure CLI](/cli/azure/install-azure-cli) で行うのと同じタスクを実行する構成またはデプロイのスクリプトを作成することができます。 (既に述べたように、Azure CLI は Python で記述されており、その各種コマンドは管理ライブラリを使用して実装されています。)

各管理ライブラリでの操作の詳細については、[SDK GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)内のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルを参照してください。 また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api)および [Azure のサンプル](/samples/browse/?languages=python&products=azure)で確認できます。

## <a name="connect-to-and-use-azure-resources-with-client-libraries"></a>クライアント ライブラリを使用して Azure リソースに接続し、そのリソースを使用する

SDK の "*クライアント*" ("データ プレーン") ライブラリを使用すると、既にプロビジョニングされたサービスとやり取りする Python アプリケーション コードを記述するのに役立ちます。 クライアント ライブラリは、クライアント API をサポートするサービスに対してのみ存在します。

各クライアント ライブラリでの操作の詳細については、[SDK の GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)内のライブラリのプロジェクト フォルダーにある *README.md* または *README.rst* ファイルを参照してください。 また、その他のコード スニペットについては、[リファレンス ドキュメント](/python/api)および [Azure のサンプル](/samples/browse/?languages=python&products=azure)で確認できます。

## <a name="get-help-and-connect-with-the-sdk-team"></a>SDK チームによるサポートと連絡先

- [Python 用 Azure ライブラリのドキュメント](https://aka.ms/python-docs)をご覧ください。
- ご質問は、[Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) のコミュニティに投稿してください。
- [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) でこの SDK に対して問題を開いてください。
- Twitter で [@AzureSDK](https://twitter.com/AzureSdk/) について言及する

## <a name="next-step"></a>次のステップ

任意の Python 用 Azure ライブラリを簡単に使用できるように、ローカル開発環境の 1 回限りの設定を行うことを強くお勧めします。

> [!div class="nextstepaction"]
> [ローカル開発環境を設定する >>>](configure-local-development-environment.md)
