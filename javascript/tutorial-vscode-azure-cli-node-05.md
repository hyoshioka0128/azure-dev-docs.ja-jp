---
title: Azure App Service からログをストリーム配信する
description: チュートリアル パート5、ログを表示する
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: f96deb992af0d446876265e1b8214879ddff45e6
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "77709879"
---
# <a name="stream-logs-from-app-service"></a>Azure App Service からログをストリーム配信する

[前の手順:アプリケーションをデプロイする](tutorial-vscode-azure-cli-node-04.md)

この手順では、実行中の App Service からログを表示 ("tail") します。 サイト コードでの `console.log` への呼び出しはすべて、ターミナルに表示されます。

1. 次のコマンドを実行して、ログ記録を開始します。`<your_app_name>` は、お使いの App Service の名前に置き換えてください。

    ```azurecli
    az webapp log tail --name <your_app_name>
    ```

1. 数秒後、ログ ストリーミング サービスに接続されていることを示すメッセージが出力に表示されます。

    <pre>
    2019-09-25T13:39:23  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. ブラウザーで数回、ページを最新の情報に更新して追加の出力を生成します。

    <pre>
    GET / 304 2.327 ms - -
    GET / 304 0.957 ms - -
    GET / 304 2.435 ms - -
    </pre>

1. **Ctrl**+**C** を押してログ セッションを終了します。

> [!div class="nextstepaction"]
> [ログを確認しました](tutorial-vscode-azure-cli-node-06.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=tailing-logs)
