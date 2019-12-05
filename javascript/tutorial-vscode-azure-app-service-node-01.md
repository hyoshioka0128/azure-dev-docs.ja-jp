---
title: Visual Studio Code から Azure App Service に Node.js アプリをデプロイする
description: チュートリアル パート 1、概要と前提条件。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: a02cb282722447516b034869fa8b614ddb45dc5f
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467209"
---
# <a name="deploy-to-azure-app-service-using-visual-studio-code"></a>Visual Studio Code を使用して Azure App Service にデプロイする

このチュートリアルでは、[App Service 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)を使用して、Node.js アプリケーションを Azure App Service on Linux にデプロイします。

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure App Service 拡張機能](vscode:extension/ms-azuretools.vscode-azureappservice)
- [Node.js と npm](https://nodejs.org/en/download)、Node.js パッケージ マネージャー。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">Azure App Service 拡張機能をインストールする</a>

### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure 拡張機能をインストールしました](tutorial-vscode-azure-app-service-node-02.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=getting-started)
