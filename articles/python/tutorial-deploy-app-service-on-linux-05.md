---
title: 手順 5:VS Code を使用して Azure App Service on Linux に Python Web アプリをデプロイする
description: チュートリアルの手順 5、Web アプリのコードをデプロイする
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 3f17fe3190857d1cc4faca84c9319e1c514cea4c
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441207"
---
# <a name="5-deploy-your-python-web-app-to-azure-app-service-on-linux"></a>5:Azure App Service on Linux に Python Web アプリをデプロイする

[前の手順: カスタム スタートアップ ファイルを構成する](tutorial-deploy-app-service-on-linux-04.md)

この手順を使用して、Azure App Service に Python アプリをデプロイします。

1. Visual Studio Code で **[Azure:App Service]** エクスプローラーを開き、青色の上矢印を選択します。

   ![App Service エクスプローラーで Web アプリを App Service にデプロイする](media/deploy-azure/deploy-web-app-to-app-service-in-app-service-explorer.png)

    または、App Service の名前を右クリックして **[Web アプリにデプロイ]** コマンドを選択する方法もあります。

1. 表示されたプロンプトで、次の情報を入力します。

    - "Select the folder to deploy (デプロイ先のフォルダーを選択してください)" では、現在のアプリ フォルダーを選択します。
    - "Select Web App (Web アプリを選択してください)" では、前の手順で作成した App Service を選択します。

1. デプロイ プロセスの進行中は、VS Code の**出力**ウィンドウで進行状況を確認できます。

    ![VS Code の出力ウィンドウに表示されるデプロイの進行状況](media/deploy-azure/view-deployment-progress-in-visual-studio-code-output.png)

1. 数分後 (インストールする必要のある依存関係の数によって異なります) にデプロイが完了すると、次のメッセージが表示されます。 **[Web サイトの参照]** ボタンを選択すると、実行中のサイトが表示されます。

    ![[Web サイトの参照] ボタンを使用したデプロイの完了](media/deploy-azure/web-app-deployment-complete-with-browse-website-button.png)

    ![App Service でアプリが正常に実行されている](media/deploy-azure/web-app-running-successfully-on-app-service.png)

1. ファイルがデプロイされていることを確認するには、 **[Azure: App Service]** エクスプローラーで App Service を選択し、 **[ファイル]** を展開します。

    ![App Service エクスプローラーでデプロイ ファイルを確認する](media/deploy-azure/expand-files-node-to-check-deployment-of-web-app-files.png)

    *antenv* は、依存関係を含んだ仮想環境が App Service によって作成されるフォルダーです。 このノードを展開すると、*requirements.txt* に指定したパッケージが *antenv/lib/python3.7/site-packages* にインストールされていることを確認できます。

> [!div class="nextstepaction"]
> [アプリをデプロイしました - 手順 6 に進む >>>](tutorial-deploy-app-service-on-linux-06.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=05-deploy-app)
