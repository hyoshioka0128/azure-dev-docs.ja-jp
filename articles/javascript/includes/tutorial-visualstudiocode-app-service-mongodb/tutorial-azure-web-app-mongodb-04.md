---
title: インクルード ファイル tutorial-azure-web-app-mongodb-04.md
description: インクルード ファイル tutorial-azure-web-app-mongodb-04.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 73e629427359a625ea3ec4796c2ec7000a21536a
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183940"
---
チュートリアルのこのセクションでは、Visual Studio Code の拡張機能と Docker コンテナーを使用して、ローカルの MongoDB データベースをアプリケーションに追加します。

## <a name="configure-visual-studio-code-to-run-containers"></a>コンテナーを実行するように Visual Studio Code を構成する

このセクションでは、2 つのコンテナー (1 つは Node.js プロジェクト用で、もう 1 つは MongoDB コンテナー用) を実行するように開発環境を構成します。 このセクションでは Visual Studio Code 開発コンテナーを使用するため、コンテナー構成は **.devcontainer** フォルダーに保存されます。 これをソース管理にコミットして、チームの他のユーザーもローカルの MongoDB にアクセス可能にすることができます。  

1. Visual Studio Code で、コマンド パレット (Ctrl + Shift + P) を使用して **[Remote-Containers: Add Development Container Configuration Files]\(Remote-Containers: 開発コンテナー構成ファイルの追加\)** を選択します。 

1. 一覧から **[Node.js & Mongo DB]\(Node.js と Mongo DB\)** を選択します。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-configure-development-container.png" alt-text="Visual Studio Code のコマンド パレットの部分的なスクリーンショット。"::: 

1. **\.devcontainer\devcontainer.json** ファイルで **forwardPorts** プロパティを見つけてコメント解除し、配列に `8080` を追加します。 また、シェルを使用して MongoDB コンテナーにアクセスできるようにするには、ポート `27017` を配列に追加します。  

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-dev-container-configuration-forward-ports.png" alt-text="Visual Studio Code のコマンド パレットの部分的なスクリーンショット。"::: 

## <a name="run-web-app-locally-with-database"></a>データベースでローカルに Web アプリを実行する

このセクションでは、両方のコンテナーを使用して開発環境を実行し、Web サイトを表示します。 

1. Visual Studio Code の左下隅にある緑色の **[リモート コンテナー]** アイコンを選択します。 コマンド パレットが開きます。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-remote-container-icon.png" alt-text="Visual Studio Code のコマンド パレットの部分的なスクリーンショット。"::: 

1. コマンド パレットで、 **[Remote-Containers: Reopen in Container]\(Remote-Containers: コンテナーでもう一度開く\)** を選択します。 コンテナーでプロジェクトを初めて開くと、Node.js と MongoDB のイメージがプルダウンされ、開始されます。 これには数分かかることがあります。 

    コンテナーが実行されている場合、Visual Studio Code ターミナルに Node.js コンテナーのターミナルが表示されます。 

    必要に応じて、`ls` コマンドを使用してファイルを表示できます。 ファイルによってローカル コンピューターの共有ボリュームが使用されていることに注目してください。 Node.js コンテナー内でコード ファイルに加えた変更は、ローカル ファイルに保存されます。

1. 次のコマンドを使用して、ターミナルでプロジェクトを開始します。

    ```console
    npm start
    ```

1. ローカル Web アプリの URL を使用してブラウザーを開きます。

    ```
    http://localhost:8080/
    ```

1. フィールドにデータを入力し、フォームを送信します。 データは、サーバー側の React のレンダリングを使用して表示されます。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="Visual Studio Code のコマンド パレットの部分的なスクリーンショット。":::

1. アプリの探索が完了したら、コマンドパレットを使用してコンテナーを停止し、 **[Remote-Containers:Reopen Locally]\(Remote-Containers: ローカルでもう一度開く\)** を選択します。これにより、コンテナーは停止しますが、ローカル コンピューターに残ります。 

## <a name="want-to-know-more"></a>詳細について 

Visual Studio Code の拡張機能 **[MongoDB for VS Code](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)** を使用して MongoDB コンテナーに接続し、データを表示します。

**mongo** シェルを使用する場合は、新しい Visual Studio Code ウィンドウを開いてから、 **[Remote-Containers: Attach to Running Container]\(Remote-Containers: 実行中のコンテナーにアタッチする\)** を使用して、Visual Studio Code ターミナルで MongoDB コンテナーに接続し、`-db` で終わるコンテナーを選択します。 ウィンドウがコンテナーにアタッチされたら、Visual Studio Code ターミナルを開きます。 次のコマンドを使用して、Mongo シェルにすぐにアクセスできます。

```console
mongo
```

コンテナーをクリーンアップする場合は、Visual Studio Code の拡張機能である **[Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)** を使用します。