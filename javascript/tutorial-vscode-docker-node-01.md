---
title: Visual Studio Code から Docker コンテナーを Azure App Service にデプロイする
description: チュートリアル パート 1、概要と前提条件。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 1a14010d362ed3858d319a141fd24e5ea1b0e714
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740571"
---
# <a name="deploy-containers-to-azure-app-service"></a>Azure App Service にコンテナーをデプロイする

このチュートリアルでは、Visual Studio Code を使用して、Docker を使ってコンテナー化された Node.js アプリケーションを作成し、コンテナー イメージをレジストリにプッシュした後、イメージを Azure App Service にデプロイします。

## <a name="walkthrough-video"></a>チュートリアル ビデオ

この記事の内容の完全なチュートリアルについては、こちらのビデオをご覧ください。

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-containers-to-Azure-App-Service/player]

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Docker 拡張機能](vscode:extension/ms-azuretools.vscode-docker)。
- [Azure App Service 拡張機能](vscode:extension/ms-azuretools.vscode-azureappservice)。
- [Node.js と npm](https://nodejs.org/en/download)、Node.js パッケージ マネージャー。
- [Docker](https://www.docker.com/community-edition)。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-docker">Docker 拡張機能をインストールする</a>

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">Azure App Service 拡張機能をインストールする</a>

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
