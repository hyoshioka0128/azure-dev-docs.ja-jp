---
title: VS Code を使用して Azure App Service on Linux に Python Web アプリをデプロイする
description: チュートリアルの手順 5、Web アプリのコードをデプロイする
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: e9b85928bd2b1308ab57747d7b0c32c085274cde
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019680"
---
# <a name="deploy-your-app"></a>アプリをデプロイする

[前の手順: カスタム スタートアップ ファイルを構成する](tutorial-deploy-app-service-on-linux-04.md)

1. Visual Studio Code で **[Azure: App Service]** エクスプローラーを開いて、青色の上矢印を選択します。

   ![Web アプリへのデプロイ コマンド](media/deploy-azure/deploy-to-web-app-command.png)

    または、App Service の名前を右クリックして **[Web アプリにデプロイ]** コマンドを選択する方法もあります。

1. 表示されたプロンプトで、次の情報を入力します。

    - "Select the folder to deploy (デプロイ先のフォルダーを選択してください)" では、現在のアプリ フォルダーを選択します。
    - "Select Web App (Web アプリを選択してください)" では、前の手順で作成した App Service を選択します。

1. デプロイ プロセスの進行中は、VS Code の**出力**ウィンドウで進行状況を確認できます。

    ![VS Code の出力ウィンドウに表示されるデプロイの進行状況](media/deploy-azure/deployment-progress.png)

1. 数分後 (インストールする必要のある依存関係の数によって異なります) にデプロイが完了すると、次のメッセージが表示されます。 **[Web サイトの参照]** ボタンを選択すると、実行中のサイトが表示されます。

    ![デプロイの完了メッセージ](media/deploy-azure/deployment-complete.png)

    ![App Service でアプリが正常に実行されている](media/deploy-azure/running-app.png)

1. ファイルがデプロイされたことを確認するには、 **[Azure:App Service]** エクスプローラーで App Service を展開し、 **[ファイル]** を展開します。

    ![App Service エクスプローラーでデプロイ ファイルを確認する](media/deploy-azure/expand-files-node.png)

    *antenv* は、依存関係を含んだ仮想環境が App Service によって作成されるフォルダーです。 このノードを展開すると、*requirements.txt* に指定したパッケージが *antenv/lib/python3.7/site-packages* にインストールされていることを確認できます。

> [!div class="nextstepaction"]
> [アプリをデプロイしました](tutorial-deploy-app-service-on-linux-06.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=05-deploy-app)
