---
title: 仮想マシンに SSH 接続する
description: SSH を使用して Linux 仮想マシンに接続します。  最新の Mac、Windows、または Linux オペレーティング システムを使用している場合は、ターミナル ベースのクライアント SSH が既にインストールされているはずです。
ms.topic: tutorial
ms.date: 01/05/2021
ms.custom: devx-track-js
ms.openlocfilehash: c4b9577c93e37c28145abe8e976a93cd6ff39197
ms.sourcegitcommit: 075f39972e390e79ed09a3fcfdbfc776727e08fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952494"
---
# <a name="4-connect-to-linux-virtual-machine-using-ssh"></a>4.SSH を使用して Linux 仮想マシンに接続する

チュートリアルのこのセクションでは、ターミナルで SSH を使用して仮想マシンに接続します。 [SSH](https://www.ssh.com/ssh/) は、Azure Cloud Shell を含む多くの最新シェルに用意されている一般的なツールです。 

## <a name="connect-with-ssh-and-change-web-app"></a>SSH を使用して接続して Web アプリを変更する

前のステップと同じターミナルまたはシェル ウィンドウを使用します。 

1. 以下のコマンドを使用して、リモート仮想マシンに接続します。 このプロセスでは、お使いの SSH クライアントが SSH キーを検出でき、VM 作成の一環として作成され、ローカル コンピューター上に配置されていることを前提としています。 接続を続行するかどうかを確認するメッセージが表示されたら、「`yes`」と応答します。 接続が完了すると、リモート仮想マシンを示すようにターミナルのプロンプトが変更されます。 

    `YOUR-PUBLIC-IP-ADDRESS` を独自の仮想マシンのパブリック IP に置き換えます。 

    ```console
    ssh azureuser@YOUR-PUBLIC-IP-ADDRESS
    ``` 

1. 次のコマンドを使用して、仮想マシン上の自分の場所を把握します。 azureuser のルート `/home/azureuser` にいるはずです。 

    ```bash
    pwd
    ```

1. Web アプリはサブ ディレクトリ `myapp` にあります。 `myapp` ディレクトリに移動し、内容を一覧表示します。

    ```bash
    cd myapp && ls -l
    ```

    仮想マシンにクローンされた GitHub リポジトリと npm パッケージ ファイルを表す、次のような内容が表示されます。
    
    ```console
    -rw-r--r--   1 root root   891 Nov 11 20:23 cloud-init-github.txt
    -rw-r--r--   1 root root  1347 Nov 11 20:23 index-logging.js
    -rw-r--r--   1 root root   282 Nov 11 20:23 index.js
    drwxr-xr-x 190 root root  4096 Nov 11 20:23 node_modules
    -rw-r--r--   1 root root 84115 Nov 11 20:23 package-lock.json
    -rw-r--r--   1 root root   329 Nov 11 20:23 package.json
    -rw-r--r--   1 root root   697 Nov 11 20:23 readme.md
    ```

## <a name="install-monitoring-sdk"></a>Monitoring SDK のインストール

[Application Insights 用の Azure SDK クライアント ライブラリ](https://www.npmjs.com/package/applicationinsights)をインストールします。

```bash
sudo npm install --save applicationinsights
```

## <a name="add-monitoring-instrumentation-key"></a>監視インストルメンテーション キーの追加

1. [Nano](https://www.nano-editor.org/dist/latest/nano.html#Editor-Basics) エディターを使用して `package.json` ファイルを変更します。

    ```bash
    sudo nano package.json
    ```

1. ファイルの start スクリプトを編集して、環境変数を含めます。 `REPLACE-WITH-YOUR-KEY` をインストルメンテーション キーの値に置き換えてください。

    ```json
    "start": "APPINSIGHTS_INSTRUMENTATIONKEY=REPLACE-WITH-YOUR-KEY pm2 start index.js --watch --log /var/log/pm2.log"
    ```

1. 次のコマンドを使用して PM2 を停止し、再起動します。

    ```bash
    sudo npm run-script stop && sudo npm start
    ```

    これで Azure クライアント ライブラリは _node_modules_ に配置され、キーは環境変数としてアプリに渡されました。 次のステップでは、`index.js` に必要なコードを追加します。 

1. ターミナルは開いたままで、VM に接続されたままにしてください。次のステップで使用します。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure クラウドにログを記録するための Azure SDK クライアント コードを追加する](azure-monitor-application-insights-nodejs-expressjs-code.md) 