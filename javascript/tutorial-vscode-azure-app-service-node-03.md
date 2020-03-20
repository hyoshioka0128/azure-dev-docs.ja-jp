---
title: Visual Studio Code から Azure App Service に Node.js アプリをデプロイする
description: チュートリアル パート 3、Web サイトをデプロイする
ms.topic: conceptual
ms.date: 03/04/2020
ms.openlocfilehash: 7e7a6c765b79878ca3ddc86bd16349694cb7ded6
ms.sourcegitcommit: 0cf7703a8b26469bb58840853ce9135b5adf4417
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2020
ms.locfileid: "79510600"
---
# <a name="deploy-the-app-to-azure"></a>Azure にアプリケーションをデプロイする

[前の手順:アプリケーションの作成](tutorial-vscode-azure-app-service-node-02.md)

この手順では、VS コードと Azure App Service 拡張機能を介して git デプロイを使って Node.js アプリをデプロイします。 この目標を達成するには、まずローカル git リポジトリを初期化し、次に Azure で Web アプリを作成した後、git デプロイを使用するように VS Code を構成します。

1. ターミナルで、*expressApp1* フォルダーにいることを確認し、次のコマンドを使用して Visual Studio Code を開始します。

    ```bash
    code .
    ```

1. VS Code で、ソース管理アイコンを選択して、**ソース管理**エクスプローラーを開き、[ **+** ] を選択してローカル git リポジトリを初期化します。

    ![git リポジトリを初期化する](media/deploy-azure/git-init.png)

1. プロンプトで、ワークスペース フォルダーの *[expressApp1]* を選択します。

1. リポジトリが初期化されたら、「Initial commit (初期コミット)」というメッセージを入力し、チェックマークをオンにして、ソース ファイルの初期コミットを作成します。

    ![リポジトリへの初期コミットを完了する](media/deploy-azure/initial-commit.png)

1. コマンド パレット (**Ctrl**+**Shift**+**P**) で、「create web (Web の作成)」と入力して、 **[Azure App Service: Create New Web App...Advanced]\(Azure App Service: 新しい Web アプリの作成\)、[詳細設定]** の順に選択します。 詳細設定コマンドを使って、Linux の既定値を使用する代わりに、リソース グループ、App Service プラン、オペレーティング システムなどのデプロイを完全に制御できます。

1. プロンプトに次のように応答します。

    - **サブスクリプション**のアカウントを選択します。
    - **[Enter a globally unique name]\(グローバルに一意の名前を入力する\)** に、Azure 全体で一意の名前を入力します。 英数字 ('A-Z'、'a-z'、および '0-9') とハイフン ('-') のみを使用します。
    - **[新しいリソース グループの作成]** を選択して、`AppServiceTutorial-rg` のような名前を指定します。
    - オペレーティング システム (Windows または Linux) を選択します。
    - Linux のみ: Node.js バージョンを選択します (Windows の場合、後でアプリ設定を使ってバージョンを設定します)。
    - **[Create a new App Service plan]\(新しい App Service プランの作成\)** を選択して、`AppServiceTutorial-plan` のような名前を指定し、 **[F1 Free]** 価格レベルを選択します。
    - Application Insights リソースに対して **[Skip for now]\(今はしない\)** を選択します。
    - お近くの場所を選択します。

1. しばらくすると、VS Code により作成が完了したことが通知されます。 **[X]** ボタンを使って、通知を閉じます。

    ![Web アプリの作成の完了に関する通知](media/deploy-azure/creation-complete.png)

1. Web アプリを配置したら、次に、ローカル git リポジトリからコードをデプロイするように VS Code に指示します。 Azure アイコンを選択して **Azure App Service** エクスプローラーを開き、サブスクリプション ノードを展開します。作成した Web アプリの名前を右クリックし、 **[Configure Deployment Source]\(デプロイ ソースの構成\)** を選択します。

    ![Web アプリでのデプロイ ソースの構成コマンド](media/deploy-azure/configure-deployment-source.png)

1. メッセージが表示されたら、 **[LocalGit]** を選択します。

1. Windows で App Service にデプロイする場合は、デプロイする前に 2 つの設定を作成する必要があります。

    1. VS Code で、新しい App Service のノードを展開し、 **[アプリケーション設定]** を右クリックして、 **[Add New Setting]\(新しい設定の追加\)** を選択します。

        ![アプリ設定の追加コマンド](media/deploy-azure/add-setting.png)

    1. 設定キーとして「`WEBSITE_NODE_DEFAULT_VERSION`」を、設定値として「`10.15.2`」を入力します。 この設定により、Node.js のバージョンが設定されます。
    1. この手順を繰り返して、値 `1` を持つ `SCM_DO_BUILD_DURING_DEPLOYMENT` のキーを作成します。 この設定により、デプロイ時にサーバーにより `npm install` が実行されるよう強制されます。
    1. **Application Settings\(アプリケーション設定\)** ノードを展開して、設定が有効になっていることを確認します。

1. 青色の上向き矢印アイコンを選択して、Azure にコードをデプロイします。

    ![Web アプリへのデプロイ アイコン](media/deploy-azure/deploy.png)

1. プロンプトで *[expressApp1]* フォルダーを選択し、**サブスクリプション**のアカウントを再度選択し、先ほど作成した Web アプリの名前を選択します。

1. Linux へのデプロイ時にターゲット サーバーで `npm install` を実行するように構成を更新するよう求められたら、 **[はい]** を選択します。

    ![ターゲットの Linux サーバー上で構成を更新するように求めるメッセージ](media/deploy-azure/server-build.png)

1. Linux および Windows の場合、 **[Always deploy the workspace "nodejs-docs-hello-world" to (app name)"]\(ワークスペース "nodejs-docs-hello-world" を常に (アプリ名) にデプロイ\)** というメッセージが表示されたら、 **[はい]** を選択します。 **[はい]** を選択すると、後続のデプロイで同じ App Service Web アプリを自動的に対象とするように VS Code に指示されます。

1. デプロイが完了したら、プロンプトで **[Web サイトの参照]** を選択して、新しくデプロイした Web アプリを表示します。 "Hello World!" がブラウザーに表示されます。

1. (省略可能):コード ファイルに変更を加えてから、デプロイ ボタンを再度使用して Web アプリを更新できます。

> [!div class="nextstepaction"]
> [サイトは Azure にあります](tutorial-vscode-azure-app-service-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
