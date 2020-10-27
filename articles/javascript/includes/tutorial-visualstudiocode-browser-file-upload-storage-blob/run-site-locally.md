---
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
title: インクルード ファイル run-site-locally.md
description: インクルード ファイル run-site-locally.md
ms.openlocfilehash: 129546c7f6a845c335405c6fc615f907848138f6
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344208"
---
チュートリアルのこのセクションでは、サンプル アプリケーションをローカル コンピューターにダウンロードして、Visual Studio Code ターミナルから実行します。 次に、ローカルで実行されているアプリをブラウザーで表示します。

## <a name="clone-and-run-the-initial-react-app"></a>最初の React アプリを複製して実行する

完全な React アプリが用意されています。 この手順では、アプリを複製し、依存関係をインストールして、アプリを実行します。 コードで構成されている場合、最初のアプリによって Azure Storage への接続が試行されます。使用できない場合は、`Storage is not configured` というメッセージが表示されます。 

1. Visual Studio Code を開きます。
1. Git アイコンを選択し、次に **[リポジトリの複製]** を選択して、GitHub リポジトリを複製します。 このプロセスを実行するには、GitHub にログインする必要があります。 リポジトリ URL `https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob` を使用し、サンプルの複製先となるローカル コンピューター上のフォルダーを選択します。 メッセージが表示されたら、複製されたリポジトリを開きます。 

    :::image type="content" source="../../media/tutorial-browser-file-upload/vscode-git-clone-repository.png" alt-text="Git アイコンを選択し、次に [リポジトリの複製] を選択して、GitHub リポジトリを複製します。":::

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
    http://localhost:3000/
    ```

    Storage が構成されていないというテキストを含む単純な Web アプリがブラウザーに表示されたら、チュートリアルのこのセクションは成功しています。

    :::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-no-azure-storage-resource-configured.png" alt-text="Git アイコンを選択し、次に [リポジトリの複製] を選択して、GitHub リポジトリを複製します。":::

1. Visual Studio Code ターミナルで、Ctrl + C を使用してコードを停止します。
