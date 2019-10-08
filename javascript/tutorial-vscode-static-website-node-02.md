---
title: Visual Studio Code で静的 Node.js アプリを作成する
description: チュートリアル パート 2、サンプル アプリを作成する
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 566d166a69bbbee59726b8e381bee4a24077d8c6
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685978"
---
# <a name="create-the-app"></a>アプリケーションの作成

[前の手順: 概要と前提条件](tutorial-vscode-static-website-node-01.md)

この手順では、React ユーティリティ CLI の [create-react-app](https://github.com/facebook/create-react-app) を使用して、Azure にデプロイできる簡単な React アプリを作成します。 代わりに、Angular、Vue、別のフレームワーク、またはいくつかの HTML ファイルが含まれた任意のフォルダーを使用することもできます。 既にデプロイの準備ができているアプリがある場合は、「[Azure Storage アカウントを作成する](tutorial-vscode-static-website-node-03.md)」に進んでください。

1. 次のコマンドを実行することにより、create-react-app ツールを使用して、`my-react-app` という新しい React アプリをスキャフォールディングします。

    ```bash
    npm create react-app my-react-app
    ```

1. 新しいフォルダーに切り替え、`npm run build` を実行してアプリケーションをビルドします。

    ```bash
    cd my-react-app
    npm run build
    ```

1. *my-react-app* フォルダーに *build* フォルダーが作成されています。 *build* フォルダーには、Azure Storage にデプロイする HTML、CSS、および JavaScript のファイルが含まれています。

1. 次のコマンドを使用してアプリを実行します。

    ```bash
    npm start
    ```

1. ブラウザーを開いて [http://localhost:3000](http://localhost:3000) を参照し、アプリが実行されていることを確認します。

    ![実行中のサンプル React アプリ](media/static-website/local-app.png)

1. ターミナルまたはコマンド プロンプトで **Ctrl**+**C** を押してサーバーを停止します。

> [!div class="nextstepaction"]
> [アプリを作成しました](tutorial-vscode-static-website-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
