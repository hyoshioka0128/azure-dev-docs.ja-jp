---
author: burkeholland
ms.service: app-service
ms.topic: include
ms.date: 03/31/2020
ms.author: buhollan
ms.openlocfilehash: 59eea52aab3f2b1941a3accb5848bc4ad92d2992
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740497"
---
1. ターミナルまたはコマンド プロンプトで、アプリ フォルダーを作成する場所に移動します。

1. 次のコマンドを使用して、Express ジェネレーターを使って、*myexpressapp* という名前の新しい Express アプリを作成します。 `--git --view pug` パラメーターは、ジェネレーターに対して *.gitignore* ファイルを作成することと、 [pug](https://pugjs.org/api/getting-started.html) テンプレート エンジン (旧称 Jade) を使用することを指示します。

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

1. ブラウザーを開き、[http://localhost:3000](http://localhost:3000) を参照して、アプリをテストします。 サイトは次のように表示されます。

    ![Express アプリケーションの実行](../media/deploy-azure/express.png)

1. ターミナルで **Ctrl** + **C** キーを押してサーバーを停止します。
