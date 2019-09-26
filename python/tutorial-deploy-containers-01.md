---
title: Visual Studio Code を使用して Docker コンテナーを Azure App Service にデプロイする
description: チュートリアルの手順 1、概要と前提条件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 0135761f8294b3fbbb8fe821540b46126c107109
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186182"
---
# <a name="deploy-containers-to-azure-app-service"></a>Azure App Service にコンテナーをデプロイする

このチュートリアルでは、Visual Studio Code を使用して、コンテナー イメージをコンテナー レジストリから [Azure App Service](https://azure.microsoft.com/services/app-service/containers/) にデプロイするプロセスについて説明します。これらはすべて Visual Studio Code 内で実行されます。

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
