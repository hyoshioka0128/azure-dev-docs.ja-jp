---
title: Azure App Service から Visual Studio Code にログをストリーム配信する
description: チュートリアル パート 4、ログを表示または tail する。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 4048fd1d5d288d88cadf0a865c2c5b0ddd517daf
ms.sourcegitcommit: aa2c66b0fecce51862cc9115f68d39c770f0b2ae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77709809"
---
# <a name="stream-logs-from-azure-app-service"></a>Azure App Service からログをストリーム配信する

[前の手順:Web サイトのデプロイ](tutorial-vscode-azure-app-service-node-03.md)

この手順では、実行中の Web サイトが `console.log` への呼び出しを介して生成する出力を表示または "tail" する方法について説明します。 この出力は、Visual Studio Code の **[出力]** ウィンドウに表示されます。

1. **Azure App Service** エクスプローラーで、アプリ ノードを右クリックし、 **[Start Streaming Logs]\(ストリーミング ログの開始\)** を選択します。

    ![ストリーミング ログの表示](media/deploy-azure/view-logs.png)

1. 確認を求められたら、ログを有効にしてアプリケーションを再起動します。

    ![ログの有効化と再起動のプロンプト](media/deploy-azure/enable-restart.png)

1. アプリが再起動すると、VS Code の **[出力]** ウィンドウが開き、ログ ストリームへの接続が示され、出力が表示されます。

    <pre>
    Connecting to log-streaming service...
    2019-09-20 17:33:51.428 INFO  - Container msdocs-vscode-node_2 for site msdocs-vscode-node initialized successfully.
    2019-09-20 17:33:56.500 INFO  - Container logs
    </pre>

1. ブラウザーで数回、Web ページを最新の情報に更新して追加のログ出力を確認します。

> [!div class="nextstepaction"]
> [ログを確認しました](tutorial-vscode-azure-app-service-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
