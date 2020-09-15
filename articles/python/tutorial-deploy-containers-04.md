---
title: 手順 4:コンテナー用の Azure App Service から Visual Studio Code にログをストリーム配信する
description: チュートリアルの手順 4、Azure App Service のログを表示してその動作を監視する。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 92af617452aa8892f61e110a69be53a9208f3883
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473507"
---
# <a name="4-stream-logs-from-azure-app-service-for-a-container"></a>4:コンテナー用の Azure App Service からログをストリーム配信する

[前の手順: 変更を加えて再デプロイする](tutorial-deploy-containers-03.md)

この手順を使用して、コンテナー用の Azure App Service から Visual Studio Code にログをストリーム配信します。

VS Code 内から、Azure App Service で実行中のサイトのログを表示 (または追跡) できます。これにより、コンソールへの出力が `print` ステートメントの場合と同様にキャプチャされ、VS Code の **[出力]** パネルにルーティングされます。

1. アプリを **[Azure:App Service]** エクスプローラーで見つけて、そのアプリを右クリックし、 **[Start Streaming Logs]\(ログのストリーム配信を開始する\)** を選択します。

1. ログを有効にしてアプリを再起動するよう求められたら、 **[はい]** と回答します。 アプリが再起動すると、VS Code の [出力] パネルが開き、ログ ストリームに接続されます。

1. 数秒後、ログストリーミング サービスに接続されていることを示すメッセージが出力に表示されます。

    <pre>
    Connecting to log stream...
    2018-09-27T20:14:26  Welcome, you are now connected to log-streaming service.

    2018-09-27 20:14:59.269 INFO  - Starting container for site

    2018-09-27 20:14:59.270 INFO  - docker run -d -p 24138:8000 --name vsdocs-django-sample-container_0 -e WEBSITES_PORT=8000 -e WEBSITE_SITE_NAME=vsdocs-django-sample-container -e WEBSITE_AUTH_ENABLED=False -e WEBSITE_ROLE_INSTANCE_ID=0 -e WEBSITE_INSTANCE_ID=02c705ae24eaf5f298e553a9c2724b9fe4485707c2d1c36137cd02931091e561 -e HTTP_LOGGING_ENABLED=1 vsdocsregistry.azurecr.io/python-sample-vscode-django-tutorial:latest

    2018-09-27 20:15:06.216 INFO  - Container vsdocs-django-sample-container_0 for site vsdocs-django-sample-container initialized successfully.
    </pre>

1. そのアプリ内を移動して、さまざまな HTTP 要求に関する追加出力を確認します。

> [!div class="nextstepaction"]
> [ログを確認しました - 手順 5 に進む >>>](tutorial-deploy-containers-05.md)

問題がある場合 ページの下部にある [このページ] へのフィードバックを使用して、GitHub の問題を送信します。
