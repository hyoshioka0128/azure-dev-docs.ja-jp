---
title: チュートリアル:Visual Studio Code を使用して Docker コンテナーを Azure App Service にデプロイする
description: チュートリアル ステップ 1、コンテナーの環境を構成する
ms.topic: conceptual
ms.date: 09/17/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 447643662ef3c839823d81d29cfb55cc2ee442b2
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2020
ms.locfileid: "90772611"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>チュートリアル:Visual Studio Code を使用して Docker コンテナーを Azure App Service にデプロイする

この記事では、Visual Studio Code を使用して、コンテナー イメージをコンテナー レジストリから [Azure App Service](/azure/app-service/) にデプロイするプロセスについて説明します。

このチュートリアルのいずれかの手順で問題が発生した場合は、詳細をお知らせください。 各記事の最後にある **[このページ]** フィードバック ボタンを使用してください。

関連するデモ ビデオについては、仮想 PyCon 2020 の「<a href="https://www.youtube.com/watch?v=t79HDLC5kQA&feature=youtu.be&ocid=AID3006292" target="_blank">Django Apps in VS Code dev containers</a>」 (VS Code 開発コンテナーの Django Apps) (youtube.com) をご覧ください。

## <a name="configure-your-environment"></a>環境を構成する

- [Azure アカウント](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- コンテナー レジストリにアップロードされている適切なコンテナー。 Python Web アプリを使用してコンテナーを作成する方法の詳細については、[コンテナーでの Python](https://code.visualstudio.com/docs/containers/quickstart-python) に関する記事を参照してください。
- [VS Code 用 Azure App Service 拡張情報](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [VS Code 用 Docker 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [Azure にサインインしました - 手順 2 に進む >>>](tutorial-deploy-containers-02.md)
