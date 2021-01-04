---
title: 概要と前提条件
description: GitHub アクションを使用して、ローカル環境で React/TypeScript クライアント アプリケーションを構築し、Azure Static Web アプリにデプロイします。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: dd35ca0925b014d2fddeb064c8cbdbdea703697b
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522353"
---
# <a name="1-build-and-deploy-a-static-web-app-to-azure"></a>1.静的 Webアプリを構築して Azure にデプロイする

このチュートリアルでは、GitHub アクションを使用して、React/TypeScript クライアント アプリケーションをローカル環境で構築し、Azure Static Web アプリにデプロイします。 

React (create-react-app) は次の機能を提供します。 
* Cognitive Services Computer Vision の Azure キーとエンドポイントが見つからない場合にメッセージを表示します
* Cognitive Services Computer Vision を使用してイメージを分析できるようにします
    * パブリック イメージの URL を入力するか、コレクション内のイメージを分析します
    * 分析が完了したとき
        * 表示する画像
        * Computer Vision JSON の結果を表示する 

特定のブランチへのプッシュが発生したときに、GitHub アクションが開始されます。
* Computer Vision キーとエンドポイントの GitHub シークレットをビルドに挿入します
* React (create-react-app) クライアントを構築します
* 生成されたファイルを Azure Static Web アプリ リソースに移動します

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="what-is-an-azure-static-web-app"></a>Azure Static Web アプリとは

静的 Web アプリを構築する場合、Azure ではユーザーが関心を持つ機能と制御のレベルによって、いくつかの選択肢があります。 このチュートリアルでは、最も簡単なサービスについて重点的に説明します。選択の多くが自動的に行われるため、ユーザーはホスティング環境ではなくフロントエンド コードに集中できます。

## <a name="prerequisites"></a>前提条件

- [Node.js および npm](https://nodejs.org/en/download) - ローカル コンピューターにインストール済み。
- [Visual Studio Code](https://code.visualstudio.com/) - ローカル コンピューターにインストール済み。 
    - [Azure Static Web Apps (プレビュー)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps) - React アプリを Azure Static Web アプリにデプロイするために使用されます。
- [Git](https://git-scm.com/downloads) - GitHub にプッシュするために使用されます。これにより GitHub アクションがアクティブになります。
- [GitHub アカウント](https://github.com/join) - フォークしてリポジトリにプッシュするため
- Bash 環境で [Azure Cloud Shell](/azure/cloud-shell/quickstart) を使用します。

   [![埋め込みの起動](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell を起動する")](https://shell.azure.com)   
- 必要に応じて、Azure CLI を[インストール](/cli/azure/install-azure-cli)して、CLI リファレンス コマンドを実行します。
   - ローカル インストールを使用する場合は、[az login](/cli/azure/reference-index#az-login) コマンドを使用して Azure CLI でサインインします。  認証プロセスを完了するには、ターミナルに表示される手順に従います。  追加のサインイン オプションについては、「[Azure CLI を使用してサインインする](/cli/azure/authenticate-azure-cli)」を参照してください。
  - 初回使用時にインストールを求められたら、Azure CLI 拡張機能をインストールします。  拡張機能の詳細については、「[Azure CLI で拡張機能を使用する](/cli/azure/azure-cli-extensions-overview)」を参照してください。
  - [az version](/cli/azure/reference-index?#az_version) を実行し、インストールされているバージョンおよび依存ライブラリを検索します。 最新バージョンにアップグレードするには、[az upgrade](/cli/azure/reference-index?#az_upgrade) を実行します。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [React Cognitive Services Image Analyzer アプリをダウンロードしてローカルで実行する](run-the-react-cognitive-services-image-analyzer-app-locally.md) 