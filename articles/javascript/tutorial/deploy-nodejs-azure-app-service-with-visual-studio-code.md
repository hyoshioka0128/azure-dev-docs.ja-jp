---
title: Visual Studio Code から Azure App Service に Node.js アプリをデプロイする
description: Visual Studio Code App Service 拡張機能を使用して Azure App Service に Express.js Node.js アプリケーションをデプロイします。
ms.topic: tutorial
ms.date: 01/20/2021
ms.custom: devx-track-js
ms.openlocfilehash: 8f6ab806262bee9684cc2c37d05aa1f21695c806
ms.sourcegitcommit: 681ea211dc8e1809dd299b5cd64eebd974c073d7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98658777"
---
# <a name="deploy-nodejs-to-azure-app-service-using-visual-studio-code"></a>Visual Studio Code を使用して Node.js を Azure App Service にデプロイする

Visual Studio Code [App Service 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)を使用して、Node.js アプリケーションを (Linux または Windows 上の) Azure App Service にデプロイします。

Git と Azure App Service 拡張機能を使用して、Node.js アプリを Azure にデプロイします。 この目標を達成するには:

* Express.js アプリを作成します
* ローカル Git リポジトリを初期化します
* アプリをホストするための Web アプリ リソースを作成します
* アプリをリソースにデプロイします
* リモート ログをローカルで表示します

## <a name="walkthrough-video"></a>チュートリアル ビデオ

この記事の内容の完全なチュートリアルについては、こちらのビデオをご覧ください。

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-to-Azure-App-Service-using-Visual-Studio-Code/player]

## <a name="1-set-up-your-development-environment"></a>1. 開発環境を設定する

- アクティブなサブスクリプションが含まれる Azure アカウント。 [無料で作成できます](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- VS Code 用の [Azure App Service 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) (VS Code 内からインストール)。
- [Node.js 12 以降と npm](https://nodejs.org/en/download)。

## <a name="2-sign-in-to-azure"></a>2. Azure へのサインイン

[!INCLUDE [azure-sign-in](../includes/azure-sign-in.md)]

## <a name="3-create-a-local-expressjs-app"></a>3. ローカル環境で Express.js アプリを作成する

[!INCLUDE [Create a local Express.js app](../includes/create-node-app.md)]

## <a name="4-run-your-local-expressjs-app"></a>4. ローカル環境で Express.js アプリを実行する

[!INCLUDE [Run your local Express.js app](../includes/run-node-app.md)]

## <a name="5-initialize-git-in-visual-studio-code-for-current-app"></a>5. Visual Studio Code で Git を現在のアプリ用に初期化する

1. ターミナルで、*expressApp1* フォルダーにいることを確認し、次のコマンドを使用して Visual Studio Code を開始します。

    ```bash
    code .
    ```

1. Visual Studio Code で、ソース管理アイコンを選択して、**ソース管理** エクスプローラーを開き、 **[リポジトリの初期化]** を選択してローカル Git リポジトリを初期化します。

    ![git リポジトリを初期化する](../media/deploy-azure/git-init.png)

1. プロンプトで、ワークスペース フォルダーの *[expressApp1]* を選択します。

1. リポジトリが初期化されたら、「Initial commit (初期コミット)」というメッセージを入力し、チェックマークをオンにして、ソース ファイルの初期コミットを作成します。

    ![リポジトリへの初期コミットを完了する](../media/deploy-azure/initial-commit.png)

## <a name="6-create-app-service-resource-in-visual-studio-code"></a>6. Visual Studio Code で App Service リソースを作成する

1. コマンド パレット (**Ctrl**+**Shift**+**P**) で、「create web (Web の作成)」と入力して、 **[Azure App Service: Create New Web App...Advanced]\(Azure App Service: 新しい Web アプリの作成\)、[詳細設定]** の順に選択します。 詳細設定コマンドを使って、Linux の既定値を使用する代わりに、リソース グループ、App Service プラン、オペレーティング システムなどのデプロイを完全に制御できます。

1. プロンプトに次のように応答します。

    - **サブスクリプション** アカウントを選択します。
    - **[Enter a globally unique name]\(グローバルに一意の名前を入力する\)** に、Azure 全体で一意の名前を入力します。 英数字 ('A-Z'、'a-z'、および '0-9') とハイフン ('-') のみを使用します。
    - **[新しいリソース グループの作成]** を選択して、`AppServiceTutorial-rg` のような名前を指定します。
    - オペレーティング システム (Windows または Linux) を選択します。
    - Linux のみ: Node.js バージョンを選択します (Windows の場合、後でアプリ設定を使ってバージョンを設定します)。
    - **[新しい App Service プランの作成]** を選択し、名前を指定して (例: `AppServiceTutorial-plan`)、 **[F1 Free]** [価格レベル](../core/what-is-azure-for-javascript-development.md#free-tier-resources)を選択します。
    - Application Insights リソースに対して **[Skip for now]\(今はしない\)** を選択します。
    - お近くの場所を選択します。

1. しばらくすると、VS Code により作成が完了したことが通知されます。 **[X]** ボタンを使って、通知を閉じます。

    ![Web アプリの作成の完了に関する通知](../media/deploy-azure/creation-complete.png)

1. Web アプリを配置したら、次に、ローカル Git リポジトリからコードをデプロイするように VS Code に指示します。 Azure アイコンを選択して **Azure App Service** エクスプローラーを開き、サブスクリプション ノードを展開します。作成した Web アプリの名前を右クリックし、 **[Configure Deployment Source]\(デプロイ ソースの構成\)** を選択します。

    ![Web アプリでのデプロイ ソースの構成コマンド](../media/deploy-azure/configure-deployment-source.png)

1. メッセージが表示されたら、 **[LocalGit]** を選択します。

1. Windows で App Service にデプロイする場合は、デプロイする前に 2 つの設定を作成する必要があります。

    1. VS Code で、新しい App Service のノードを展開し、 **[アプリケーション設定]** を右クリックして、 **[Add New Setting]\(新しい設定の追加\)** を選択します。

        ![アプリ設定の追加コマンド](../media/deploy-azure/add-setting.png)

    1. 設定キーとして「`WEBSITE_NODE_DEFAULT_VERSION`」を、設定値として「`10.15.2`」を入力します。 この設定により、Node.js のバージョンが設定されます。
    1. この手順を繰り返して、値 `1` を持つ `SCM_DO_BUILD_DURING_DEPLOYMENT` のキーを作成します。 この設定により、デプロイ時にサーバーにより `npm install` が実行されるよう強制されます。
    1. **Application Settings\(アプリケーション設定\)** ノードを展開して、設定が有効になっていることを確認します。

1. 青色の上向き矢印アイコンを選択して、Azure にコードをデプロイします。

    ![Web アプリへのデプロイ アイコン](../media/deploy-azure/deploy.png)

1. プロンプトで *[expressApp1]* フォルダーを選択し、**サブスクリプション** のアカウントを再度選択し、先ほど作成した Web アプリの名前を選択します。

1. Linux へのデプロイ時にターゲット サーバーで `npm install` を実行するように構成を更新するよう求められたら、 **[はい]** を選択します。

    ![ターゲットの Linux サーバー上で構成を更新するように求めるメッセージ](../media/deploy-azure/server-build.png)

1. Linux および Windows の場合、 **[Always deploy the workspace "nodejs-docs-hello-world" to (app name)"]\(ワークスペース "nodejs-docs-hello-world" を常に (アプリ名) にデプロイ\)** というメッセージが表示されたら、 **[はい]** を選択します。 **[はい]** を選択すると、後続のデプロイで同じ App Service Web アプリを自動的に対象とするように VS Code に指示されます。

1. デプロイが完了したら、プロンプトで **[Web サイトの参照]** を選択して、新しくデプロイした Web アプリを表示します。 "Hello World!" がブラウザーに表示されます。

1. (省略可能):コード ファイルに変更を加えてから、デプロイ ボタンを再度使用して Web アプリを更新できます。

## <a name="7-stream-remote-service-logs-in-visual-studio-code"></a>7. Visual Studio Code でリモート サービス ログをストリーミングする

実行中のアプリで `console.log` を呼び出すことによって生成される出力を表示 (テール) します。 この出力は、Visual Studio Code の **[出力]** ウィンドウに表示されます。

1. **Azure App Service** エクスプローラーで、アプリ ノードを右クリックし、 **[Start Streaming Logs]\(ストリーミング ログの開始\)** を選択します。

    ![ストリーミング ログの表示](../media/deploy-azure/start-streaming-logs.png)

1. 確認を求められたら、ログを有効にしてアプリケーションを再起動します。

    ![ログの有効化と再起動のプロンプト](../media/deploy-azure/enable-restart.png)

1. アプリが再起動すると、VS Code の **[出力]** ウィンドウが開き、ログ ストリームへの接続が示され、出力が表示されます。

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. ブラウザーで数回、Web ページを最新の情報に更新して追加のログ出力を確認します。

## <a name="8-make-changes-and-redeploy"></a>8.変更を加えて再デプロイする

1. アプリに小さな変更を加えます。 `Welcome to Express` を `Welcome to Express with Visual Studio Code` に変更します。 

1. App Service 拡張機能のアプリ サービスの一覧から、使用するアプリ サービスを右クリックし、 **[Web アプリにデプロイ...]** を選択します。 

    :::image type="content" source="../media/deploy-azure/deploy-or-redeploy-app-service.png" alt-text="Visual Studio Code を使用して App Service にデプロイまたは再デプロイする":::


## <a name="9-clean-up-resources"></a>9.リソースをクリーンアップする

リソースをクリーンアップする必要があるときは、Visual Studio Code の App Service 拡張機能で App Service を右クリックし、 **[削除]** を選択します。

:::image type="content" source="../media/deploy-azure/delete-azure-app-service-with-visual-studio-code-extension.png" alt-text="リソースをクリーンアップする必要があるときは、Visual Studio Code の App Service 拡張機能で App Service を右クリックし、**[削除]** を選択する。":::

## <a name="next-steps"></a>次の手順

* [アプリの設定を構成する方法について確認する](../how-to/configure-web-app-settings.md)

[!INCLUDE [tutorial-next-steps](../includes/tutorial-next-steps.md)]
