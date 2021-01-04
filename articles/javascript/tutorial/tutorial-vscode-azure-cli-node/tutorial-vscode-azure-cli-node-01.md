---
title: Azure CLI を使用して Node.js アプリを Azure App Service にデプロイする
description: チュートリアル パート 1、Azure CLI の概要と前提条件。
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: c2cb0bad794a80370b544c2817a3c7f602a19451
ms.sourcegitcommit: 1dfcc022a3098b1a1505e9458eada35f527ef070
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "97658414"
---
# <a name="deploy-to-azure-app-service-using-the-azure-cli"></a>Azure CLI を使用して Azure App Service にデプロイする

このチュートリアルでは、すべてのオペレーティング システムで実行される [Azure コマンド ライン インターフェイス (CLI)](/cli/azure/overview?view=azure-cli-latest&preserve-view=false) を使用して、Node.js アプリケーションを Azure App Service にデプロイします。 この CLI を使用すると、Azure リソースを作成し、Git リポジトリと Azure 間のデプロイ パイプラインを設定し、アプリの `console.log` 出力を表示することができます。

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Node.js と npm 6.x 以上](https://nodejs.org/en/download)、Node.js パッケージ マネージャー。
- [Git](https://git-scm.com/downloads)。インストール後に、コマンド `git --version` を実行するとバージョン番号が表示されます。
[!INCLUDE [Azure CLI](../../../includes/azure-cli-prepare-your-environment-no-header.md)]


### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-node-git&mktingSource=vscode-tutorial-node-git)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

### <a name="sign-in-to-azure-with-azure-cli"></a>Azure CLI を使用して Azure にサインインする

[!INCLUDE [Sign in ](../../../azure-cli/includes/interactive-login.md)]

## <a name="check-npm-version"></a>npm のバージョンを確認する

既に Node.js がインストールされている場合は、`node -v` コマンドを実行して、バージョンが 10.x 以上であることを確認します。 バージョンが古い場合は、最新の LTS ("長期サポート") リリースに[アップグレード](https://nodejs.org/en/download/)してください。

> [!div class="nextstepaction"]
> [前提条件をインストールしました](tutorial-vscode-azure-cli-node-02.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=getting-started)
