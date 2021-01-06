---
title: 概要と前提条件
description: GitHub アクションを使用して、ローカル環境で React/TypeScript クライアント アプリケーションを構築し、Azure Static Web アプリにデプロイします。
ms.topic: tutorial
ms.date: 12/17/2020
ms.custom: devx-track-js
ms.openlocfilehash: dfd5803fe79a0a000173d2d4e3fe52b3c4f029c5
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687445"
---
# <a name="1-build-and-deploy-a-static-web-app-to-azure"></a>1.静的 Webアプリを構築して Azure にデプロイする

このチュートリアルでは、GitHub アクションを使用して、React/TypeScript クライアント アプリケーションをローカル環境で構築し、Azure Static Web アプリにデプロイします。 React アプリにより、ユーザーは Cognitive Services Computer Vision を使用してイメージを分析できます。

* [**サンプル コード**](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

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
> [GitHub アクションを使用したクライアント アプリのアーキテクチャとデプロイ プロセスを理解する](./application-architecture.md) 
