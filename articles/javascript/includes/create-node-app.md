---
author: burkeholland
ms.service: app-service
ms.topic: include
ms.date: 03/31/2020
ms.author: buhollan
ms.custom: devx-track-js
ms.openlocfilehash: 66e71a8632b622a090c9899e0354d3e70c768c21
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364712"
---
1. ターミナル コマンド プロンプトで、アプリ フォルダーを作成する場所に移動します。

1. 次のコマンドを実行して、Express ジェネレーターを使用して `myexpressapp` という名前の新しい Express アプリを作成します。 `--git --view pug` パラメーターは、ジェネレーターに対して、.gitignore ファイルを作成し、[Pug](https://pugjs.org/api/getting-started.html) テンプレート エンジン (旧称 Jade) を使用するよう指示します。

    ```bash
    npx express-generator myexpressapp --git --view pug
    ```

1. アプリ フォルダーに移動します。

    ```bash
    cd myexpressapp
    ```

1. アプリケーションの依存関係をインストールします。

    ```bash
    npm install
    ```

1. サーバーを起動します。

    ```bash
    npm start
    ```

1. ブラウザーを開き、`http://localhost:3000` に移動して、アプリをテストします。 サイトは次のように表示されます。

    ![Express アプリケーションの実行](../media/deploy-azure/express.png)

1. ターミナルで **Ctrl** + **C** キーを選択してサーバーを停止します。
 
