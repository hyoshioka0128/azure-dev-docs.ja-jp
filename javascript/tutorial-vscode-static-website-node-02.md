---
title: Visual Studio Code で静的 Node.js アプリを作成する
description: チュートリアル パート 2、サンプル アプリを作成する
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: bc930b7c6a3454ca335fec5a955113a45fa2f0f2
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466554"
---
# <a name="create-the-app"></a>アプリケーションの作成

[前の手順:概要と前提条件](tutorial-vscode-static-website-node-01.md)

この手順では、[Angular](https://cli.angular.io/)、[React](https://github.com/facebook/create-react-app)、または [Vue](https://cli.vuejs.org/) のコマンド ライン インターフェイス (CLI) を使用して、Azure にデプロイできる単純なアプリを作成します。 あるいは、静的なファイルのセットを生成する他の JavaScript フレームワークや、HTML、CSS、または JavaScript ファイルを含むフォルダーを使用することもできます。 既にデプロイの準備ができているアプリがある場合は、「[Azure Storage アカウントを作成する](tutorial-vscode-static-website-node-03.md)」に進んでください。

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. CLI を使用して次のコマンドを実行することにより、"my-static-app" という名前の新しいアプリを作成します。

    ```bash
    npx @angular/cli new my-static-app
    ```

    CLI によって構成に関する質問が表示された場合は、Enter キーを押して既定のオプションを選択してください。

1. 新しいフォルダーに切り替え、`npm run build` を実行してアプリケーションをビルドします。

    ```bash
    cd my-static-app
    npm run build
    ```

1. _my-static-app_ フォルダーに _dist_ フォルダーが作成されています。 _dist_ フォルダー内には、プロジェクトと同じ名前のフォルダー (_my-static-app_) があります。 _build/my-static-app_ フォルダーには、Azure Storage にデプロイする HTML、CSS、および JavaScript のファイルが含まれています。

1. 次のコマンドを使用してアプリを実行します。

    ```bash
    npm start
    ```

1. ブラウザーを開いて [http://localhost:3000](http://localhost:3000) を参照し、アプリが実行されていることを確認します。

    ![実行中のサンプル Angular アプリ](media/static-website/local-app-angular.png)

1. ターミナルまたはコマンド プロンプトで **Ctrl**+**C** を押してサーバーを停止します。

# <a name="reacttabreact"></a>[React](#tab/react)

1. CLI を使用して次のコマンドを実行することにより、"my-static-app" という名前の新しいアプリを作成します。

    ```bash
    npx create-react-app my-static-app
    ```

1. 新しいフォルダーに切り替え、`npm run build` を実行してアプリケーションをビルドします。

    ```bash
    cd my-static-app
    npm run build
    ```

1. _my-static-app_ フォルダーに _build_ フォルダーが作成されています。 _build_ フォルダーには、Azure Storage にデプロイする HTML、CSS、および JavaScript のファイルが含まれています。

1. 次のコマンドを使用してアプリを実行します。

    ```bash
    npm start
    ```

1. ブラウザーを開いて [http://localhost:3000](http://localhost:3000) を参照し、アプリが実行されていることを確認します。

    ![実行中のサンプル React アプリ](media/static-website/local-app-react.png)

1. ターミナルまたはコマンド プロンプトで **Ctrl**+**C** を押してサーバーを停止します。

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. CLI を使用して次のコマンドを実行することにより、"my-static-app" という名前の新しいアプリを作成します。

    ```bash
    npx @vue/cli create my-static-app
    ```

CLI によって構成に関する質問が表示された場合は、Enter キーを押して既定のオプションを選択してください。

1. 新しいフォルダーに切り替え、`npm run build` を実行してアプリケーションをビルドします。

    ```bash
    cd my-static-app
    npm run build
    ```

1. _my-static-app_ フォルダーに _dist_ フォルダーが作成されています。 _dist_ フォルダーには、Azure Storage にデプロイする HTML、CSS、および JavaScript のファイルが含まれています。

1. 次のコマンドを使用してアプリを実行します。

     ```bash
     npm run serve
     ```

1. ブラウザーを開いて [http://localhost:8080](http://localhost:8080) を参照し、アプリが実行されていることを確認します。

    ![実行中のサンプル Vue アプリ](media/static-website/local-app-vue.png)

1. ターミナルまたはコマンド プロンプトで **Ctrl**+**C** を押してサーバーを停止します。

---

> [!div class="nextstepaction"]
> [アプリを作成しました](tutorial-vscode-static-website-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
