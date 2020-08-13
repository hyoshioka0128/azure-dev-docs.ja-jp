---
title: チュートリアル:Visual Studio Code を使用して Docker コンテナーを Azure App Service にデプロイする
description: チュートリアルの手順 1、概要と前提条件。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: fe98ff2007be9f817cc8e9bd0210761579eef536
ms.sourcegitcommit: 5051b25ad32be891800b23fc7ae12a4ca85cbb73
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "88147413"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>チュートリアル:Visual Studio Code を使用して Docker コンテナーを Azure App Service にデプロイする

この記事では、Visual Studio Code を使用して、コンテナー イメージをコンテナー レジストリから [Azure App Service](/azure/app-service/) にデプロイするプロセスについて説明します。これらはすべて Visual Studio Code 内で実行されます。

このチュートリアルのいずれかの手順で問題が発生した場合は、詳細をお知らせください。 フィードバックを送信するには、各記事の最後にある "**問題が発生しました**" リンクを使用します。

関連するデモ ビデオについては、仮想 PyCon 2020 の「<a href="https://www.youtube.com/watch?v=t79HDLC5kQA&feature=youtu.be&ocid=AID3006292" target="_blank">Django Apps in VS Code dev containers</a>」 (VS Code 開発コンテナーの Django Apps) (youtube.com) をご覧ください。

## <a name="prerequisites"></a>前提条件

- [Azure アカウント](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- コンテナー レジストリにアップロードされている適切なコンテナー。 Python Web アプリを使用してコンテナーを作成する方法の詳細については、[コンテナーでの Python](https://code.visualstudio.com/docs/containers/quickstart-python) に関する記事を参照してください。
- [VS Code 用 Azure App Service 拡張情報](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [VS Code 用 Docker 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure にサインインしました - 手順 2 に進む >>>](tutorial-deploy-containers-02.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)
