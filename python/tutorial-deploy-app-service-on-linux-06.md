---
title: Azure App Service から VS Code にログをストリーム配信する
description: チュートリアルの手順 6、アプリのログを Visual Studio Code にストリーム配信する
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 39ae534580e5057b1ddcf93588be94ad67f2a9b4
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71020170"
---
# <a name="stream-logs-from-the-running-app"></a>実行中のアプリからログをストリーム配信する

[前の手順: アプリをデプロイする](tutorial-deploy-app-service-on-linux-05.md)

1. Visual Studio Code で **[Azure:App Service]** エクスプローラーを開き、App Service を右クリックして、 **[Start streaming logs]\(ログのストリーム配信を開始する\)** を選択します。

   ![[Start streaming logs]\(ログのストリーム配信を開始する\) コマンド](media/deploy-azure/start-streaming-logs-command.png)

1. ファイルのログ記録を有効にして Web アプリを再起動するよう求められたら、 **[はい]** を選択します。 アプリの再起動中、VS Code の**出力**ウィンドウに進行状況が表示されます。 ログの有効化は、1 回限りのプロセスです。

1. ログを有効にしたら、App Service を右クリックし、再度、 **[Start streaming logs]\(ログのストリーム配信を開始する\)** を選択します。 VS Code の**出力**ウィンドウに "Starting Live Log Stream (ライブ ログ ストリームを開始しています)" と表示され、ログ出力が見え始めます。 ブラウザーで Web アプリを最新の情報に更新してみてください。さらに多くのログ情報が生成されます。

1. (ログを無効にせずに) ログのストリーム配信を停止するには、 **[Azure:App Service]** エクスプローラーでアプリを右クリックし、 **[Stop streaming logs]\(ログのストリーム配信を停止する\)** を選択します。

> [!div class="nextstepaction"]
> [ログを確認しました](tutorial-deploy-app-service-on-linux-07.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=06-stream-logs)
