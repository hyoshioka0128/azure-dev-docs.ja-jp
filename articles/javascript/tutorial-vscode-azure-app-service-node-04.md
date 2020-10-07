---
title: Azure App Service から Visual Studio Code にログをストリーム配信する
description: Node.js チュートリアル パート 4、ログを表示または tail する。
ms.topic: tutorial
ms.date: 03/04/2020
ms.custom: devx-track-js
ms.openlocfilehash: eed2663f32571dc40fb402553424c1303d43f7b9
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365235"
---
# <a name="stream-logs-from-azure-app-service"></a>Azure App Service からログをストリーム配信する

[前の手順:Web サイトのデプロイ](tutorial-vscode-azure-app-service-node-03.md)

この手順では、実行中のアプリによって `console.log` への呼び出しを介して生成される出力を表示または "tail" する方法について説明します。 この出力は、Visual Studio Code の **[出力]** ウィンドウに表示されます。

1. **Azure App Service** エクスプローラーで、アプリ ノードを右クリックし、 **[Start Streaming Logs]\(ストリーミング ログの開始\)** を選択します。

    ![ストリーミング ログの表示](media/deploy-azure/start-streaming-logs.png)

1. 確認を求められたら、ログを有効にしてアプリケーションを再起動します。

    ![ログの有効化と再起動のプロンプト](media/deploy-azure/enable-restart.png)

1. アプリが再起動すると、VS Code の **[出力]** ウィンドウが開き、ログ ストリームへの接続が示され、出力が表示されます。

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. ブラウザーで数回、Web ページを最新の情報に更新して追加のログ出力を確認します。

> [!div class="nextstepaction"]
> [ログを確認しました](tutorial-vscode-azure-app-service-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
