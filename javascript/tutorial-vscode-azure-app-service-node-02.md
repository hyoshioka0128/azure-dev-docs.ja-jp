---
title: Visual Studio Code から Azure App Service を作成する
description: チュートリアル パート 2、Node.jp アプリを作成する
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 769ca61ef7716eaa08d45e109a9bf50e5801721c
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686190"
---
# <a name="create-your-nodejs-application"></a>Node.js アプリケーションの作成

[前の手順: 概要と前提条件](tutorial-vscode-azure-app-service-node-01.md)

この手順では、Express アプリケーション ジェネレーターを使用して、Azure にデプロイできる単純な Node.js アプリを作成します。

また、[Visual Studio Code Node.js チュートリアル](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)のアプリを使用することもできます。その場合は、[アプリのデプロイ](tutorial-vscode-azure-app-service-node-03.md)に進んでください。

1. ターミナルまたはコマンド プロンプトで次のコマンドを使用して Express Generator を実行し、"myExpressApp" という新しい Express アプリをスキャフォールディングします。 (`--view pug --git` パラメーターは、[pug](https://pugjs.org/api/getting-started.html) テンプレート エンジン (旧称 Jade) を使用することと、 *.gitignore* ファイルを作成することをジェネレーターに伝えます。)

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. アプリ フォルダーで `npm install` を実行して、アプリケーションの依存関係をインストールします。

    ```bash
    cd myExpressApp
    npm install
    ```

1. `npm start` を実行してサーバーを開始します。

    ```bash
    npm start
    ```

1. ブラウザーを開き、[http://localhost:3000](http://localhost:3000) を参照して、アプリをテストします。 サイトは次のように表示されます。

    ![Express アプリケーションの実行](media/deploy-azure/express.png)

> [!div class="nextstepaction"]
> [Node.js アプリを作成しました](tutorial-vscode-azure-app-service-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
