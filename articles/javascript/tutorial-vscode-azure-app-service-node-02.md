---
title: Visual Studio Code から Node.js Azure App Service を作成する
description: チュートリアル パート 2、Node.jp アプリを作成してローカルで実行する
ms.topic: conceptual
ms.date: 03/04/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 66299059f03219dc45ff5f2dbcb51e8b61144340
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218541"
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
