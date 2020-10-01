---
title: Azure で開発するためのローカル JavaScript 環境を構成する
description: エディター、Azure SDK ライブラリ、オプションのツール、ライブラリ認証に必要な資格情報など、Azure で作業するためのローカル JavaScript 開発環境を設定する方法。
ms.date: 09/22/2020
ms.topic: conceptual
ms.custom: devx-track-js
ms.openlocfilehash: 6d9f86b026a7104e79228d78d5ac27649049a8a4
ms.sourcegitcommit: 4af22924a0eaf01e6902631c0714045c02557de4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91208683"
---
# <a name="configure-your-local-javascript-dev-environment-for-azure"></a>Azure 用のローカル JavaScript 開発環境を構成する

クラウド アプリケーションを作成する場合、開発者は通常、Azure などのクラウド環境にコードをデプロイする前に、ローカル ワークステーションでそのコードをテストしたいと考えます。 ローカル開発には、使い慣れた開発環境と共に幅広いツールを利用できるという利点があります。

この記事では、Azure での JavaScript に適したローカル開発環境を作成および検証するためのセットアップ手順について説明します。

## <a name="one-time-subscription-creation"></a>1 回限りのサブスクリプションの作成

Azure リソースは、Azure を使用するための課金単位であるサブスクリプション内に作成されます。 無料のリソースを作成することはできますが (各サブスクリプションには、ほとんどのサービスに無料のリソースが用意されています)、リソースを運用環境にデプロイする予定の場合は、有料階層のリソースを作成する必要があります。

* 既にサブスクリプションをお持ちの場合は、新しいものを作成する必要はありません。 [Azure portal](https://portal.azure.com) を使用して、既存のサブスクリプションにアクセスしてください。
* [無料試用版サブスクリプションを開始する](https://azure.microsoft.com/free/cognitive-services)

## <a name="one-time-installation"></a>1 回限りのインストール

ローカル ワークステーションで JavaScript と共に Azure リソースを使用して開発するには、次のものを 1 回インストールする必要があります。

|名前またはインストーラー|説明|
|--|--|
|[Node.js](https://www.npmjs.com/)|ローカル ワークステーション開発用の最新の長期サポート (LTS) ランタイム環境をインストールします。 |
| NPM (最新バージョンの Node.js と共にインストールされます) または [Yarn](https://yarnpkg.com/)|Azure SDK ライブラリをインストールするためのパッケージ マネージャー。|
|[Visual Studio Code](https://code.visualstudio.com/)| Visual Studio Code は、優れた JavaScript 統合とコーディングのエクスペリエンスを提供しますが、必須ではありません。 任意のコード エディターを使用することができます。 このドキュメントにおいては、別のエディターを使用している場合、Azure との統合を確認するか、Azure CLI を使用してください。|
|[Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)|Azure CLI を使用すると、コマンド ライン、ターミナル、または bash シェルから Azure リソースを再作成したり管理したりすることができます。|

> [!CAUTION]
> Azure Web アプリや Azure Container Instance など、コードのランタイム環境として Azure リソースを使用する予定がある場合は、ローカルの Node.js 開発環境が、使用する予定の Azure リソース ランタイムと一致していることを確認する必要があります。

### <a name="optional-local-installations"></a>オプションのローカル インストール

次の一般的なローカル ワークステーションのインストールは、ローカルの開発タスクに役立つオプションです。

|名前またはインストーラー|説明|
|--|--|
| [git](https://git-scm.com/downloads) | ソース管理用のコマンドライン ツール。 必要に応じて、別のソース管理ツールを使用できます。 |

## <a name="one-time-configuration-of-service-principal"></a>サービス プリンシパルの 1 回限りの構成

各 Azure サービスには認証メカニズムがあります。 これには、キーとエンドポイント、接続文字列、またはその他のメカニズムが含まれる場合があります。 ベスト プラクティスに準拠するには、リソースを作成し、サービス プリンシパルを使用してリソースに対して認証を行います。 サービス プリンシパルを使用すると、当面の開発ニーズに対するアクセス スコープを具体的に定義できます。

概念的には、サービス プリンシパルを作成して使用する手順は次のとおりです。

* joe@microsoft.com など、個々のユーザー アカウントを使用して Azure にログインします。
* 特定のスコープを持つ名前付きサービス プリンシパルを作成します。 ほとんどのクイックスタートでは Azure リソースを作成するように求められるため、サービス プリンシパルにはリソースを作成する機能が必要です。
* 自分のユーザー アカウントで Azure からログオフします。
* サービス プリンシパルを使用して、プログラムで Azure に対する認証を行います。
* サービス プリンシパルは、Azure リソースを作成し、該当のサービスに関連付けられているサービスを使用します。

### <a name="create-service-principal"></a>サービス プリンシパルの作成

サービス プリンシパルを[作成する方法](node-sdk-azure-authenticate-principal.md)について学習します。 作成ステップからの応答を忘れずに保存してください。 サービス プリンシパルを使用するには、`appId`、`tenant`、および `password` の値が必要です。

## <a name="steps-for-each-new-development-project-setup"></a>新しい開発プロジェクトのセットアップごとの手順

Azure SDK ライブラリはサービスごとに個別に提供されるため、すべての Azure リソースにアクセスするための、単一のダウンロード可能なパッケージはありません。 各ライブラリは、使用する Azure サービスに基づいてインストールします。

Azure を使用する新しいプロジェクトでは、それぞれ次のことを行う必要があります。
- Azure リソースを作成するか、既存の Azure リソースの認証情報を検索する
- Azure SDK ライブラリを NPM または Yarn からインストールする。 [ライブラリのバージョン](#library-versions)について確認してください。
- プロジェクト内の認証情報を安全に管理する。 一般的な方法の 1 つとして、 **[Dotenv](https://www.npmjs.com/package/dotenv)** を使用して、`.env` ファイルから環境変数を読み取る方法があります。 `.env` ファイルがソース管理にチェックインされないように、必ず `.env` ファイルを `.gitignore` ファイルに追加してください。

### <a name="library-versions"></a>ライブラリのバージョン

[Azure ライブラリ](azure-sdk-library-package-index.md)では、一般的に `@azure` スコープが使用されます。

最新のライブラリでは、`@azure` スコープが使用されます。 通常、Microsoft の古いパッケージは `azure-` で始まります。 多くのパッケージはこの名前で始まりますが、それらは Microsoft によって生成されたものではありません。 パッケージの所有者が Microsoft または Azure のいずれかであることを確認してください。

## <a name="create-azure-resource-with-service-principal"></a>サービス プリンシパルを使用して Azure リソースを作成する

Azure CLI で、[サービス プリンシパルを使用して Azure リソースを作成します](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-a-resource-using-service-principal)。

## <a name="use-service-principal-in-javascript"></a>JavaScript でサービス プリンシパルを使用する

個人用ユーザー アカウントではなく、Azure クライアント ライブラリに対して認証を行う場合に、[サービス プリンシパルを使用します](node-sdk-azure-authenticate-principal.md#using-the-service-principal)。

## <a name="create-environment-variables-for-the-azure-libraries"></a>Azure ライブラリの環境変数を作成する

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

## <a name="install-npm-packages"></a>NPM パッケージのインストール

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