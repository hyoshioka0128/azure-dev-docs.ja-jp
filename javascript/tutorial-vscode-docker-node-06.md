---
title: コンテナー化された Node.js アプリからのログを Visual Studio Code からストリーム配信する
description: チュートリアル パート 5、Visual Studio Code にログをストリーム配信する
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 2ac930996bd910014565c4e329bec93015bd2a3a
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466520"
---
# <a name="stream-logs-into-visual-studio-code"></a>Visual Studio Code にログをストリーム配信する

[前の手順:変更を加えて再デプロイする](tutorial-vscode-docker-node-05.md)

この手順では、実行中の Web サイトが `console.log` への呼び出しを介して生成する出力を表示または "tail" する方法について説明します。 この出力は、Visual Studio Code の **[出力]** ウィンドウに表示されます。

1. **Azure App Service** エクスプローラーで、アプリ ノードを右クリックし、 **[Start Streaming Logs]\(ストリーミング ログの開始\)** を選択します。

    ![ストリーミング ログの表示](media/deploy-containers/stream-logs-command.png)

1. 確認を求められたら、ログを有効にしてアプリケーションを再起動します。

    ![ログの有効化と再起動のプロンプト](media/deploy-azure/enable-restart.png)

1. アプリが再起動されると、Visual Studio Code の **[出力]** パネルが開き、`Starting Live Log Stream` というメッセージで始まる、ログ ストリームへの接続が表示されます。

> [!div class="nextstepaction"]
> [ログを確認しました](tutorial-vscode-docker-node-07.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=tailing-logs)
