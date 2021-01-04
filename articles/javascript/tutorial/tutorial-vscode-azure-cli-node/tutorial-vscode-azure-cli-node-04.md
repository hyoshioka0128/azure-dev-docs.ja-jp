---
title: Azure CLI を使用して Azure App Service にアプリ コードをデプロイする
description: チュートリアル パート 4、Azure CLI で Web サイトをデプロイする
ms.topic: tutorial
ms.date: 12/14/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 7dc0369615f58e8677b479b28c2223d3fa865b19
ms.sourcegitcommit: 1dfcc022a3098b1a1505e9458eada35f527ef070
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "97658411"
---
# <a name="deploy-the-app-to-app-service"></a>アプリを App Service にデプロイする

[前の手順:App Service の作成](tutorial-vscode-azure-cli-node-03.md)

この手順では、ローカル Git リポジトリを Azure にプッシュする基本的なプロセスを使用して、Azure App Service に Node.js アプリ コードをデプロイします。

1. ターミナルまたはコマンド プロンプトで、次のコマンドを実行してローカル Git リポジトリを初期化し、初期コミットを行います。 (*node_modules* フォルダーは無視されます。これは、Express Generator を以前に実行したときに作成された *.gitignore* ファイルに指定されているからです。)

    ```bash
    git init
    git add -A
    git commit -m "Initial Commit"
    ```

1. 次のコマンドを実行し、[Azure CLI でユーザー レベルのデプロイ資格情報を設定](/azure/app-service/deploy-configure-credentials)します。`username` と `password` は、デプロイ専用の新しい資格情報に置き換えてください。 これらの資格情報は Azure サブスクリプションの資格情報とは異なります。 

    ```azurecli
    az webapp deployment user set --user-name <username> --password <password>
    ```

1. 次のコマンドを実行し、アプリ コードのプッシュ先である [Git エンドポイントを Azure CLI で取得](/cli/azure/webapp/deployment/source?view=azure-cli-latest&preserve-view=false)します。`<your_app_name>` は、前のステップで App Service を作成するときに使用した名前に置き換えてください。

    ```azurecli
    az webapp deployment source config-local-git --name <your_app_name>
    ```

    このコマンドの出力は次のようになります。

    <pre>
    {
      "url": "https://username@msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git"
    }
    </pre>

1. 次のコマンドを実行して、`azure` という名前の新しいリモートを Git に設定します。前の手順の URL から *ユーザー名を省略* したものを使用します。 前の手順の例を使用すると、コマンドは次のようになります。

    ```bash
    git remote add azure https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
    ```

1. 次のコマンドを実行して、Git リポジトリから App Service にアプリ コードをデプロイします。 コマンドでは、資格情報の入力が求められます。

    ```bash
    git push azure <DEFAULT-BRANCH-NAME>
    ```

1. コマンドを実行すると、リモート ホストからの一連のメッセージが表示されます。 プロセスが完了したら、アプリの URL が開いているブラウザーを更新して、実行中のコードを確認します。

    ![Azure で実行されているアプリ コード](../../media/azure-cli/remote-app.png)

> [!div class="nextstepaction"]
> [アプリをデプロイしました](tutorial-vscode-azure-cli-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=deploy-website)
