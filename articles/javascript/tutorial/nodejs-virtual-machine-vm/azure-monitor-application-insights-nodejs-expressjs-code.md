---
title: Application Insights クライアント ライブラリをインストールする
description: Azure SDK クライアント ライブラリを仮想マシン上のコードに追加して、Azure クラウドでのアプリ ログの収集を開始します。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: a6fb29dd059922ead67b4cef5107c9407f40e294
ms.sourcegitcommit: ed749b136f0d6b876fd5866ba4a151c73af5b71f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "94674712"
---
# <a name="5-install-azure-sdk-client-library-to-monitor-web-app"></a>5.Azure SDK クライアント ライブラリをインストールして Web アプリを監視する

このステップでは、Azure SDK クライアント ライブラリを仮想マシン上のコードに追加して、Azure クラウドでのアプリ ログの収集を開始します。

## <a name="edit-indexjs-for-logging-with-azure-monitor-application-insights"></a>Azure Monitor Application Insights を使用したログ記録のために index.js を編集する

1. 仮想マシンに用意されている [Nano](https://www.nano-editor.org/dist/latest/nano.html#Editor-Basics) テキスト エディターを使用して `index.js` を編集します。 

    ```bash
    sudo nano index.js
    ```

1. `index.js` ファイルを編集して、以下で強調表示されているクライアント ライブラリとログのコードを追加します。 多くの bash シェルでは、コピーして直接 nano に貼り付けることができます。 

    :::code language="JavaScript" source="~/../js-e2e-vm/index-logging.js" highlight="5-28" :::

1. 完了したら、`Control+x` キーを使用して終了し、`y` キーを使用して変更内容を保存します。 Web アプリに対する変更は PM2 によって監視されます。この変更によって、VM を再起動しなくても、アプリの再起動が発生しました。 

1. Web ブラウザーで、新しい `trace` ルートを使用してアプリをテストします。

    ```http
    http://REPLACE-WITH-YOUR-IP/trace
    ```

    ブラウザーに応答が表示されます。`tracing...` とお使いの IP アドレスです。

## <a name="viewing-the-vm-logs-for-nginx-and-pm2"></a>NGINX と PM2 の VM ログを表示する

VM によって NGINX と PM2 のログが収集されます。それらを表示することができます。

| サービス | ログの場所|
|--|--|
|NGINX| /var/log/nginx/access.log|
|PM2| /var/log/pm2.log|

1. NGINX プロキシ サービスの VM ログを表示します。 同じ bash シェルで、次のコマンドを使用してログを表示します。

    ```bash
     cat /var/log/nginx/access.log
    ```

    ログには、お使いのローカル コンピューターからの呼び出しが含まれています。 

    ```console
     "GET /trace HTTP/1.1" 200 10 "-"
    ```

1. PM2 サービスの VM ログを表示します。 同じ bash シェルで、次のコマンドを使用してログを表示します。

    ```bash
     cat /var/log/pm2.log
    ```

    ログには、お使いのローカル コンピューターからの呼び出しが含まれています。 

    ```console
    Hello world app listening on port 3000!
    testing from trace route 76.22.73.183
    ```

1. このチュートリアルでは VM にもう一度接続することはありません。 bash シェルで次のコマンドを使用して、ssh 接続を終了します。 

    ```bash
    exit
    ```

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure portal でログを表示する](azure-monitor-application-insights-logs.md) 