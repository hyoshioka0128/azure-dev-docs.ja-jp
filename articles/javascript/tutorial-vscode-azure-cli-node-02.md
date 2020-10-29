---
title: Node.js アプリを作成し、Azure CLI を使用して Azure にデプロイする
description: チュートリアル パート 2、Azure CLI で アプリ コードを作成する。
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 89e979dabca91e47083f48e42543453facc57130
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689148"
---
# <a name="create-the-app-code-using-express"></a>Express を使用してアプリ コードを作成する

[前の手順:概要と前提条件](tutorial-vscode-azure-cli-node-01.md)

この手順では、[Express Generator](https://expressjs.com/en/starter/generator.html) を使用して [Express](https://www.expressjs.com) で簡単な Nodo.js アプリを作成します。

1. 次のコマンドを使用して Express Generator を実行し、"myExpressApp" という新しい Express アプリをスキャフォールディングします。 (`--view pug --git` パラメーターは、 [pug](https://pugjs.org/api/getting-started.html) テンプレート エンジン (旧称 Jade) を使用することと、 *.gitignore* ファイルを作成することをジェネレーターに指示します。)

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. 次のコマンドを実行して、アプリ フォルダーに移動し、アプリの依存関係をインストールします。

    ```bash
    cd myExpressApp
    npm install
    ```

1. 次のコマンドを実行して、アプリ サーバーを開始します。

    ```bash
    npm start
    ```

1. ブラウザーを開いて `http://localhost:3000` に移動し、実行中のアプリを表示します。

    ![Express アプリをローカルで実行](media/azure-cli/local-app.png)

1. アプリのテストが完了したら、`npm start` を実行したターミナルで **Ctrl**+**C** を押してサーバーを停止します。

> [!div class="nextstepaction"]
> [アプリを作成します](tutorial-vscode-azure-cli-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=express)
