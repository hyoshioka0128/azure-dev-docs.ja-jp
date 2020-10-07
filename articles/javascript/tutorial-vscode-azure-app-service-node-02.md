---
title: Visual Studio Code から Node.js Azure App Service を作成する
description: Node.js チュートリアル パート 2、Node.jp アプリを作成してローカルで実行する
ms.topic: tutorial
ms.date: 03/04/2020
ms.custom: devx-track-js
ms.openlocfilehash: 7d627d97987ed41f90972362ab38d436a6157d33
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365245"
---
# <a name="create-and-run-a-local-nodejs-app"></a>ローカル Node.js アプリを作成して実行する

[前の手順:概要と前提条件](tutorial-vscode-azure-app-service-node-01.md)

この手順では、Express アプリケーション ジェネレーターを使用して簡単な Nodo.js アプリを作成します。 次に、アプリをローカルで実行します。

1. ターミナルまたはコマンド プロンプトで、アプリ フォルダーを作成する場所に移動します。

1. 次のコマンドを使用して、Express ジェネレーターを使って、*expressApp1* という名前の新しい Express アプリを作成します。 (`--view pug --git` パラメーターは、[pug](https://pugjs.org/api/getting-started.html) テンプレート エンジン (旧称 Jade) を使用することと、 *.gitignore* ファイルを作成することをジェネレーターに指示します。)

    ```bash
    npx express-generator expressApp1 --view pug -–git
    ```

1. アプリ フォルダーに移動します。

    ```bash
    cd expressApp1
    ```

1. アプリケーションの依存関係をインストールします。

    ```bash
    npm install
    ```

1. サーバーを起動します。

    ```bash
    npm start
    ```

1. ブラウザーを開き、`http://localhost:3000` を参照して、アプリをテストします。 サイトは次のように表示されます。

    ![Express アプリケーションの実行](media/deploy-azure/express.png)

1. ターミナルで **Ctrl**+**C** を押してサーバーを停止します。

> [!div class="nextstepaction"]
> [Node.js アプリを作成しました](tutorial-vscode-azure-app-service-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
