---
title: Azure で開発するためのローカル JavaScript 環境を構成する
description: エディター、Azure SDK ライブラリ、オプションのツール、ライブラリ認証に必要な資格情報など、Azure で作業するためのローカル JavaScript 開発環境を設定する方法。
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: devx-track-js, azure-sdk-ai-text-analytics-5.0.0
ms.openlocfilehash: d7f179898c974b1eaf00916bde6ce9df107e0083
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846713"
---
# <a name="configure-your-local-javascript-dev-environment-for-azure"></a>Azure 用のローカル JavaScript 開発環境を構成する

クラウド アプリケーションを作成する場合、開発者は通常、Azure などのクラウド環境にコードをデプロイする前に、ローカル ワークステーションでそのコードをテストしたいと考えます。 ローカル開発には、使い慣れた開発環境と共に幅広いツールを利用できるという利点があります。

この記事では、Azure での JavaScript に適したローカル開発環境を作成および検証するためのセットアップ手順について説明します。

## <a name="one-time-subscription-creation"></a>1 回限りのサブスクリプションの作成

Azure リソースは、Azure を使用するための課金単位であるサブスクリプション内に作成されます。 無料のリソースを作成することはできますが (各サブスクリプションには、ほとんどのサービスに無料のリソースが用意されています)、リソースを運用環境にデプロイする予定の場合は、有料階層のリソースを作成する必要があります。

|Type|説明|
|--|--|
|[無料試用版のサブスクリプション](https://azure.microsoft.com/free/cognitive-services)|_無料_試用版のサブスクリプションを作成します。|
|[既存のサブスクリプション](https://portal.azure.com)|既にサブスクリプションを持っている場合は、Azure portal、Azure CLI、または JavaScript で既存のサブスクリプションにアクセスします。|
|[複数のサブスクリプションにまたがる](/azure/governance/management-groups/create-management-group-javascript)|複数のサブスクリプションを管理する必要がある場合は、JavaScript を使用して管理グループを作成する方法を参照してください。|

## <a name="one-time-installation"></a>1 回限りのインストール

ローカル ワークステーションで JavaScript と共に Azure リソースを使用して開発するには、次のものを 1 回インストールする必要があります。

|名前またはインストーラー|説明|
|--|--|
|[Node.js](https://www.npmjs.com/)|ローカル ワークステーション開発用の最新の長期サポート (LTS) ランタイム環境をインストールします。 |
| NPM (最新バージョンの Node.js と共にインストールされます) または [Yarn](https://yarnpkg.com/)|Azure SDK ライブラリをインストールするためのパッケージ マネージャー。|
|[Visual Studio Code](https://code.visualstudio.com/)| Visual Studio Code は、優れた JavaScript 統合とコーディングのエクスペリエンスを提供しますが、必須ではありません。 任意のコード エディターを使用することができます。 このドキュメントにおいては、別のエディターを使用している場合、Azure との統合を確認するか、Azure CLI を使用してください。|
|[Azure CLI](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)|Azure CLI を使用すると、コマンド ライン、ターミナル、または bash シェルから Azure リソースを再作成したり管理したりすることができます。|

> [!CAUTION]
> Azure Web アプリや Azure Container Instance など、コードのランタイム環境として Azure リソースを使用する予定がある場合は、ローカルの Node.js 開発環境が、使用する予定の Azure リソース ランタイムと一致していることを確認する必要があります。

### <a name="optional-local-installations"></a>オプションのローカル インストール

次の一般的なローカル ワークステーションのインストールは、ローカルの開発タスクに役立つオプションです。

|名前またはインストーラー|説明|
|--|--|
| [git](https://git-scm.com/downloads) | ソース管理用のコマンドライン ツール。 必要に応じて、別のソース管理ツールを使用できます。 |

## <a name="one-time-configuration-of-service-principal"></a>サービス プリンシパルの 1 回限りの構成

各 Azure サービスには認証メカニズムがあります。 これには、キーとエンドポイント、接続文字列、またはその他のメカニズムが含まれる場合があります。 ベスト プラクティスに準拠するには、リソースを作成し、[サービス プリンシパル](node-sdk-azure-authenticate-principal.md)を使用してリソースに対して認証を行います。 サービス プリンシパルを使用すると、当面の開発ニーズに対するアクセス スコープを具体的に定義できます。

**サービス プリンシパルを作成する**手順: 

1. 個々のユーザー アカウントを使用して Azure にサインインします。
1. 特定のスコープを持つ "_名前付き_" サービス プリンシパルを作成します。 ほとんどのクイックスタートでは Azure リソースを作成するように求められるため、サービス プリンシパルにはリソースを作成する機能が必要です。
1. 個々のユーザー アカウントを使用して Azure からログオフします。

**サービス プリンシパルを使用する**手順:

1. 証明書、環境変数、または `.json` ファイルを使用して、サービス プリンシパルをプログラムで Azure に対して認証します。 
1. サービス プリンシパルを使用して Azure リソースを作成し、サービスを使用します。

[サービス プリンシパルを作成する方法](node-sdk-azure-authenticate-principal.md)について学習してください。 作成ステップからの応答を忘れずに保存してください。 サービス プリンシパルを使用するには、応答の `appId`、`tenant`、および `password` の値が必要です。

[サービス プリンシパルを使用して Azure リソースを作成します](/cli/azure/create-an-azure-service-principal-azure-cli#create-a-resource-using-service-principal)。

## <a name="steps-for-each-new-development-project-setup"></a>新しい開発プロジェクトのセットアップごとの手順

[Azure SDK ライブラリ](azure-sdk-library-package-index.md) は、各サービスに対して個別に提供されます。 各ライブラリは、使用する必要がある Azure サービスに基づいてインストールします。

Azure を使用する新しいプロジェクトでは、それぞれ次のことを行う必要があります。
- Azure リソースを作成し、関連付けられているキーまたは構成を[安全な場所]()に保存します。
- Azure SDK ライブラリを NPM または Yarn からインストールする。 
- サービス プリンシパルを使用して Azure SDK に対する認証を行い、構成情報を使用して特定のサービスにアクセスします。

## <a name="securing-configuration-information"></a>構成情報の保護

構成情報を保存するには、いくつかのオプションがあります。
- [Dotenv](https://www.npmjs.com/package/dotenv) は、`.env` ファイルから環境変数を読み取るための一般的な npm パッケージです。 `.env` ファイルがソース管理にチェックインされないように、必ず `.env` ファイルを `.gitignore` ファイルに追加してください。
- Azure [Key Vault](https://docs.microsoft.com/azure/key-vault/) を使用して、クラウドのリソース、アプリ、ソリューションにアクセスして暗号化するキーを作成および管理します。

### <a name="create-environment-variables-for-the-azure-libraries"></a>Azure ライブラリの環境変数を作成する

Azure SDK ライブラリに必要な Azure 設定を使用して Azure クラウドにアクセスするには、最も一般的な値を環境変数に設定します。 次のコマンドは、環境変数をローカル ワークステーションに設定します。 もう 1 つの一般的なメカニズムは、`DOTENV` NPM パッケージを使用して、これらの設定用の `.env` ファイルを作成することです。 `.env` を使用する場合は、このファイルをソース管理にチェックインしないようにしてください。 これらの設定がソース管理にチェックインされるようにするための標準的な方法は、`.env` ファイルを git の `.ignore` ファイルに追加することです。

次の例では、クライアント ID は、サービス プリンシパル ID とサービス プリンシパル シークレットです。

# <a name="bash"></a>[bash](#tab/bash)

```bash
AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
AZURE_TENANT_ID="00112233-7777-8888-9999-aabbccddeeff"
AZURE_CLIENT_ID="12345678-1111-2222-3333-1234567890ab"
AZURE_CLIENT_SECRET="abcdef00-4444-5555-6666-1234567890ab"
```

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
set AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
set AZURE_TENANT_ID=00112233-7777-8888-9999-aabbccddeeff
set AZURE_CLIENT_ID=12345678-1111-2222-3333-1234567890ab
set AZURE_CLIENT_SECRET=abcdef00-4444-5555-6666-1234567890ab
```

---

これらのコマンドに示されている値を、お客様固有のサービス プリンシパルの値で置き換えます。

## <a name="install-npm-packages"></a>npm パッケージをインストールする

すべてのプロジェクトについて、次の手順に従って、常に個別のフォルダーと独自の `package.json` ファイルを作成することをお勧めします。

1. ターミナル、コマンド プロンプト、または bash シェルを開き、プロジェクトに新しいフォルダーを作成します。 その後、その新しいフォルダーに移動します。

    ```console
    mkdir MY-NEW-PROJECT && cd MY-NEW-PROJECT
    ```

1. パッケージ ファイルを初期化します。

    ```console
    npm init -y
    ```

    このコマンドは、NPM コマンドを実行して、package.json ファイルを作成し、最小プロパティを初期化します。 NPM または Yarn を使用して Azure SDK ライブラリ パッケージをインストールすると、package.json ファイルにそのインストール情報が取り込まれます。

1. クイックスタートに必要な Azure SDK ライブラリをインストールします。 次のコマンドは例です。

    ```console
    npm install @azure/ai-text-analytics@5.0.0
    ```

## <a name="use-source-control"></a>ソース管理を使用する

プロジェクトを開始するときは常にソース管理リポジトリを作成するという習慣を身に付けることをお勧めします。 Git がインストールされている場合は、次のコマンドを実行します。

```bash
git init
```

そこから、`git add` や `git commit` などのコマンドを実行して変更をコミットできます。 変更を定期的にコミットすることにより、コミット履歴が作成されるので、以前の状態に戻すことができるようになります。

プロジェクトのオンライン バックアップを作成するには、リポジトリを [GitHub](https://github.com) または [Azure DevOps](/azure/devops/user-guide/code-with-git?view=azure-devops) にアップロードすることをお勧めします。 最初にローカル リポジトリを初期化した場合は、`git remote add` を使用して、ローカル リポジトリを GitHub または Azure DevOps にアタッチします。

Git のドキュメントは、[git-scm.com/docs](https://git-scm.com/docs) と、インターネット上のさまざまな場所で参照できます。

Visual Studio Code には、数多くの組み込み Git 機能が含まれています。 詳細については、「[VS Code でのバージョン管理の使用](https://code.visualstudio.com/docs/editor/versioncontrol)」を参照してください。

他の任意のソース管理ツールを使用することもできます。Git は最も広く使用され、サポートされているソース管理ツールの 1 つにすぎません。

## <a name="next-steps"></a>次のステップ

* [サービス プリンシパルを作成して使用する](node-sdk-azure-authenticate-principal.md)
* [Node.js 用 Azure モジュールを使った認証](node-sdk-azure-authenticate.md)
* [Visual Studio Code から静的 Web サイトを Azure にデプロイする](tutorial-vscode-static-website-node-01.md)