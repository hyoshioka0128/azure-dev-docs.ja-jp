---
title: Azure CLI を使用して Azure App Service にアプリ コードをデプロイする
description: チュートリアル パート 4、Web サイトをデプロイする
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 668d055a56eae2eb365884a41fcc515aae5fb229
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "77709799"
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

1. 次のコマンドを実行して、Azure にデプロイ資格情報を設定します。`username` と `pPassword` は、ご自分の資格情報に置き換えてください。 このコマンドは、成功すると JSON 出力を表示します。

    ```azurecli
    az webapp deployment user set --user-name <username> --password <password>
    ```

1. 次のコマンドを実行して、アプリ コードをプッシュする Git エンドポイントを取得します。`<your_app_name>` は、前の手順で App Service を作成するときに使用した名前に置き換えてください。

    ```azurecli
    az webapp deployment source config-local-git --name <your_app_name>
    ```

    このコマンドの出力は次のようになります。

    <pre>
    {
      "url": "https://username@msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git"
    }
    </pre>

1. 次のコマンドを実行して、`azure` という名前の新しいリモートを Git に設定します。前の手順の URL から*ユーザー名を省略*したものを使用します。 前の手順の例を使用すると、コマンドは次のようになります。

    ```bash
    git remote add azure https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
    ```

1. 次のコマンドを実行して、Git リポジトリから App Service にアプリ コードをデプロイします。 コマンドでは、資格情報の入力が求められます。

    ```bash
    git push azure master
    ```

1. コマンドを実行すると、リモート ホストからの一連のメッセージが表示されます。 プロセスが完了したら、アプリの URL が開いているブラウザーを更新して、実行中のコードを確認します。

    ![Azure で実行されているアプリ コード](media/azure-cli/remote-app.png)

> [!TIP]
> エラー `Object #<eventemitter> has no method 'hrtime'` が発生した場合は、サイトでノードのランタイム バージョンを設定することが必要な可能性があります。 次のコマンドは、ノード バージョン `6.9.1` を使用するようにサイトに指示します。 サイトで、ノードの異なるバージョンまたはより新しいバージョンが必要な場合は、完全なセマンティック バージョン `major.minor.patch` を指定します。
>
> ```azurecli
> az webapp config appsettings set --name <your_app_name> --settings
> ```

> [!div class="nextstepaction"]
> [アプリをデプロイしました](tutorial-vscode-azure-cli-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=deploy-website)
