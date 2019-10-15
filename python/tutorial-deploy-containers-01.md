---
title: チュートリアル:Visual Studio Code を使用して Docker コンテナーを Azure App Service にデプロイする
description: チュートリアルの手順 1、概要と前提条件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: f6cdd345fddf0123cb26549ddbc498f156737799
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172297"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>チュートリアル:Visual Studio Code を使用して Docker コンテナーを Azure App Service にデプロイする

この記事では、Visual Studio Code を使用して、コンテナー イメージをコンテナー レジストリから [Azure App Service](https://azure.microsoft.com/services/app-service/containers/) にデプロイするプロセスについて説明します。これらはすべて Visual Studio Code 内で実行されます。

このチュートリアルのいずれかの手順で問題が発生した場合は、詳細をお知らせください。 フィードバックを送信するには、各記事の最後にある "**問題が発生しました**" リンクを使用します。

## <a name="prerequisites"></a>前提条件

- [Azure アカウント](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- コンテナー レジストリにアップロードされている適切なコンテナー。 たとえば、Python Web アプリを使用してコンテナーを作成し、それをレジストリに追加する方法の詳細については、[コンテナーの作成](https://code.visualstudio.com/docs/python/tutorial-create-containers)に関するページを参照してください。
- [VS Code 用 Azure App Service 拡張情報](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [VS Code 用 Docker 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure にサインインしました](tutorial-deploy-containers-02.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)