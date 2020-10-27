---
title: インクルード ファイル tutorial-azure-web-app-mongodb-02.md
description: インクルード ファイル tutorial-azure-web-app-mongodb-02.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: fafb2b452e9e1bfab34d9c2d9d46adaabf4fd022
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183916"
---
チュートリアルのこのセクションでは、サンプル アプリケーションをローカル コンピューターにダウンロードして、Visual Studio Code ターミナルから実行します。 その後、ローカルで実行されているアプリをブラウザーで表示できます。 

## <a name="download-and-run-the-initial-expressjs-app"></a>初期 Express.js アプリをダウンロードして実行する

初期 Express.js Web アプリは、開始点として提供されています。 この手順では、アプリをダウンロードし、依存関係をインストールして、アプリを実行します。 初期アプリは、データベースへの接続を試みます (使用可能な場合)。 使用できない場合でも、Web サイトは要求に正常に応答します。 

1. ローカル コンピューターに [zip 圧縮された GitHub リポジトリをダウンロード](https://github.com/Azure-Samples/js-e2e-express-mongo.git)し、フォルダーに展開します。 
1. Visual Studio Code でそのフォルダーを開きます。 フォルダーを右クリックして **[Open with Code]\(コードで開く\)** を選択するか、フォルダー内で同等の CLI を使用することができます。

    ```console
    code .
    ```

1. Visual Studio Code で、ターミナル ウィンドウを開き、次のコマンドを実行して、サンプルの依存関係をインストールします。

    ```javascript
    npm install
    ```

1. 同じターミナル ウィンドウで、Web アプリを実行するコマンドを実行します。

    ```javascript
    npm start
    ```

1. Web ブラウザーを開き、次の URL を使用して、ローカル コンピューターで Web アプリを表示します。

    ```url
    http://localhost:8080/
    ```

    データベースが見つかりませんというテキストを含む単純な Web アプリがブラウザーに表示されたら、チュートリアルのこのセクションは成功しています。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="MongoDB データベースに接続された単純な Node.js アプリ。":::