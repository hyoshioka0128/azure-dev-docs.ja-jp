---
title: Visual Studio Code から Docker コンテナーを Azure App Service にデプロイする
description: チュートリアル パート 1、概要と前提条件。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: bd4110bbbef5e5f65f76cd9329b194646431f91a
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82138794"
---
# <a name="deploy-containers-to-azure-app-service"></a>Azure App Service にコンテナーをデプロイする

このチュートリアルでは、Visual Studio Code を使用して、Docker を使ってコンテナー化された Node.js アプリケーションを作成し、コンテナー イメージをレジストリにプッシュした後、イメージを Azure App Service にデプロイします。

## <a name="walkthrough-video"></a>チュートリアル ビデオ

この記事の内容の完全なチュートリアルについては、こちらのビデオをご覧ください。

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-containers-to-Azure-App-Service/player]

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Docker 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。
- [Azure App Service 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [Node.js と npm](https://nodejs.org/en/download)、Node.js パッケージ マネージャー。
- [Docker](https://www.docker.com/community-edition)。

> <a class="tutorial-install-extension-btn" href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker">Docker 拡張機能をインストールする</a>

> <a class="tutorial-install-extension-btn" href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice">Azure App Service 拡張機能をインストールする</a>

### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="verify-docker-install"></a>Docker のインストールを確認する

ターミナルまたはコマンド プロンプトで次のコマンドを実行して、Docker が正しくインストールされていることを確認します。

```bash
docker --version
```

出力は次のようになります。

<pre>
Docker Version 17.12.0-ce, build c97c6d6
</pre>

> [!div class="nextstepaction"]
> [Docker 拡張機能をインストールしました](tutorial-vscode-docker-node-02.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=getting-started)
