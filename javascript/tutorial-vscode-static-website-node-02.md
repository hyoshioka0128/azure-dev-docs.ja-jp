---
title: Visual Studio Code で静的 Node.js アプリを作成する
description: チュートリアル パート 2、サンプル アプリを作成する
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 69c0e7d6f43829546e5f23ec63a4ac35b71d7e78
ms.sourcegitcommit: 44d1abfb836f90b8731d7ea5d5a5af09245b2b89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2020
ms.locfileid: "77422520"
---
# <a name="create-the-app"></a>アプリを作成する

[前の手順:概要と前提条件](tutorial-vscode-static-website-node-01.md)

この手順では、[Angular](https://cli.angular.io/)、[React](https://github.com/facebook/create-react-app)、[Vue](https://cli.vuejs.org/)、または [Svelte](https://github.com/sveltejs/template) のコマンド ライン インターフェイス (CLI) を使用して、Azure にデプロイできる単純なアプリを作成します。 あるいは、静的なファイルのセットを生成する他の JavaScript フレームワークや、HTML、CSS、または JavaScript ファイルを含むフォルダーを使用することもできます。 既にデプロイの準備ができているアプリがある場合は、「[Azure Storage アカウントを作成する](tutorial-vscode-static-website-node-03.md)」に進んでください。

# <a name="angular"></a>[Angular](#tab/angular)

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

1. ブラウザーを開いて [http://localhost:4200](http://localhost:4200) を参照し、アプリが実行されていることを確認します。

    ![実行中のサンプル Angular アプリ](media/static-website/local-app-angular.png)

1. ターミナルまたはコマンド プロンプトで **Ctrl**+**C** を押してサーバーを停止します。

# <a name="react"></a>[React](#tab/react)

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

# <a name="vue"></a>[Vue](#tab/vue)

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

# <a name="svelte"></a>[Svelte](#tab/svelte)

1. CLI を使用して次のコマンドを実行することにより、"my-static-app" という名前の新しいアプリを作成します。

    ```bash
    npx degit sveltejs/template my-static-app
    ```

1. 次に、新しいフォルダーに移動して `npm install` コマンドを実行します。

    ```bash
    cd my-static-app
    npm install
    ```

1. `npm run build` コマンドを実行してアプリケーションをビルドしてみましょう。

    ```bash
    npm run build
    ```

1. これで、_build_ フォルダー内に _public_ フォルダーが作成されました。 _build_ フォルダーには、Azure Storage にデプロイする HTML、CSS、および JavaScript のファイルが含まれています。

1. 次のコマンドを使用してアプリを実行します。

     ```bash
     npm run dev
     ```

1. ブラウザーを開いて [http://localhost:5000](http://localhost:5000) を参照し、アプリが実行されていることを確認します。

    ![実行中のサンプル Vue アプリ](media/static-website/local-app-svelte.png)

1. ターミナルまたはコマンド プロンプトで **Ctrl**+**C** を押してサーバーを停止します。

---

> [!div class="nextstepaction"]
> [アプリを作成しました](tutorial-vscode-static-website-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
