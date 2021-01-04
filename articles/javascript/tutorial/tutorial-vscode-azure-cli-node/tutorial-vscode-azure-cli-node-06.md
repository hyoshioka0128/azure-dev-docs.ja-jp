---
title: アプリ コードに変更を加えて Azure に再デプロイする
description: チュートリアル パート 6、Azure CLI で変更を加えて再デプロイする
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: ddd03bb06d1b6b91626e4db979f0a0ca8ea7f124
ms.sourcegitcommit: 1dfcc022a3098b1a1505e9458eada35f527ef070
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/17/2020
ms.locfileid: "97658406"
---
# <a name="part-6-make-changes-and-redeploy"></a>ハート 6 変更を加えて再デプロイする

[前の手順:ログのストリーミング](tutorial-vscode-azure-cli-node-05.md)

この手順では、アプリ コードに変更を加えてローカル Git リポジトリにコミットした後、Azure にプッシュしてサイトを再デプロイします。

1. `myExpressApp` フォルダーで *views/index.pug* ファイルを開き、5 行目のメッセージを `p Welcome to Azure!` に変更します。

    ![index.pug ファイルの編集](../../media/azure-cli/editpugfile.png)

1. ファイルを保存します。

1. ターミナルまたはコマンド プロンプトで、次のコマンドを実行して変更を Git にコミットします。

    ```bash
    git commit -a -m "Edited message"
    ```

1. 先ほど作成した Azure という名前の Git リモートに変更をプッシュします。

    ```bash
    git push azure <DEFAULT-BRANCH-NAME>
    ```

1. App Service は既に Git リポジトリに接続されているため、コマンドからの出力は、変更が Azure に自動的に発行されていることを示しています。 

    ```output
    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 405 bytes | 202.00 KiB/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id '9fd89a25b6'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling node.js deployment.
    remote: Creating app_offline.htm
    remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
    remote: Copying file: 'views\index.pug'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the 
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. ブラウザーでアプリを更新して、次の変更を確認します。

    ![発行された変更がブラウザーに表示されている](../../media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [変更を確認しました](tutorial-vscode-azure-cli-node-07.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)
