---
title: JavaScript 関数の作成とデプロイ
description: このチュートリアルでは、拡張機能と JavaScript を使用して Visual Studio Code で新しいサーバーレス アプリを作成し、パブリック HTTP エンドポイントを使用してホストするために Azure クラウドにこのアプリケーションをデプロイします。
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: devx-track-js, contperfq2
ms.openlocfilehash: cba66f22c3750e33d8de04fd35e32e38fc08d744
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94338518"
---
# <a name="1-create-and-deploy-azure-functions-from-visual-studio-code"></a>1.Visual Studio Code で Azure Functions を作成してデプロイする

このチュートリアルでは、拡張機能と JavaScript を使用して Visual Studio Code で新しいサーバーレス アプリを作成し、パブリック HTTP エンドポイントを使用してホストするために Azure クラウドにこのアプリケーションをデプロイします。

## <a name="walkthrough-video"></a>チュートリアル ビデオ

この記事の内容の完全なチュートリアルについては、こちらのビデオをご覧ください。

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-Azure-Functions-Visual-Studio-Code/player]

[!INCLUDE [azure subscription](../includes/environment-subscription-h2.md)]

## <a name="install-software-to-local-workstation"></a>ローカル ワークステーションへのソフトウェアのインストール

[!INCLUDE [Node.js version text as bullet list](../includes/environment-nodejs-bullet-list.md)]
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure Functions 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)

> <a class="tutorial-install-extension-btn" href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions">Azure Functions 拡張機能をインストールする</a>

## <a name="sign-in-to-azure-in-visual-studio-code"></a>Visual Studio Code で Azure にサインインする

[!INCLUDE [azure-sign-in](../includes/azure-sign-in-vscode.md)]

[!INCLUDE [Install Azure Functions core tools](../includes/environment-functions-core-tools.md)]

> [!div class="nextstepaction"]
> [前提条件をインストールしました](tutorial-vscode-serverless-node-create-local.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=getting-started)
