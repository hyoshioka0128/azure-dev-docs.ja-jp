---
title: Azure App Service からログをストリーム配信する
description: チュートリアル パート5、ログを表示する
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: ce112d535d1f89a7e153f80b9fb80032a977b8d8
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686150"
---
# <a name="stream-logs-from-app-service"></a>Azure App Service からログをストリーム配信する

[前の手順: アプリをデプロイする](tutorial-vscode-azure-cli-node-04.md)

この手順では、実行中の App Service からログを表示 ("tail") します。 サイト コードでの `console.log` への呼び出しはすべて、ターミナルに表示されます。

1. 次のコマンドを実行して、ログ記録を開始します。`<your_app_name>` は、お使いの App Service の名前に置き換えてください。

    ```bash
    az webapp log tail --name <your_app_name>
    ```

1. 数秒後、ログ ストリーミング サービスに接続されていることを示すメッセージが表示されます。

    ```bash
    2019-09-25T13:39:23  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    ```

1. ブラウザーで数回、ページを最新の情報に更新して追加の出力を生成します。

    ```bash
    GET / 304 2.327 ms - -
    GET / 304 0.957 ms - -
    GET / 304 2.435 ms - -
    ```

1. **Ctrl**+**C** を押してログ セッションを終了します。

> [!div class="nextstepaction"]
> [ログを確認しました](tutorial-vscode-azure-cli-node-06.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=tailing-logs)
