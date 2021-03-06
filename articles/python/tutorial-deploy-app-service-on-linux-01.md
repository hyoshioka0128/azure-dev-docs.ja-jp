---
title: チュートリアル:Visual Studio Code から Azure App Service on Linux に Python アプリをデプロイする
description: チュートリアルの手順 1、概要、前提条件、Azure にサインインする。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: ed0370e5e4905403ec7917410f68fb4969969a05
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82138758"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>チュートリアル:Visual Studio Code から Azure App Service on Linux に Python アプリをデプロイする

この記事では、Visual Studio Code と [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) 拡張機能を使用して、Azure App Service on Linux に Python アプリケーションをデプロイする手順について説明します。

このチュートリアルのいずれかの手順で問題が発生した場合は、詳細をお知らせください。 フィードバックを送信するには、各記事の最後にある "**問題が発生しました**" リンクを使用します。

> [!TIP]
> [Azure App Service on Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) は、定義済みの Docker コンテナーでソース コードを実行します。 そのコンテナーでは、Python 3.7 と [Gunicorn](https://gunicorn.org) Web サーバーを使用してアプリが実行されます。 このコンテナーの特性は、[App Service on Linux 向けの Python アプリの構成](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python)に関するページで説明されています。 コンテナーの定義自体は [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/3.7) にあります。

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Visual Studio Code と Azure App Service 拡張機能](#visual-studio-code-python-and-the-azure-app-service-extension)。
- Python 環境

### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

### <a name="visual-studio-code-python-and-the-azure-app-service-extension"></a>Visual Studio Code、Python、Azure App Service 拡張機能

次のソフトウェアをインストールします。

- [Visual Studio Code](https://code.visualstudio.com/)。
- Python および [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 拡張機能。[VS Code Python チュートリアルの前提条件](https://code.visualstudio.com/docs/python/python-tutorial)に説明があります。
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) 拡張機能。VS Code 内から Azure App Service と対話できるようにする機能です。 全般的な情報については、[App Service 拡張機能のチュートリアル](https://code.visualstudio.com/tutorials/app-service-extension/getting-started)をご覧のうえ、[vscode-azureappservice GitHub リポジトリ](https://github.com/Microsoft/vscode-azureappservice)にアクセスしてください。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure にサインインしました - 手順 2 に進む >>>](tutorial-deploy-app-service-on-linux-02.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=01-verify-prerequisites)
