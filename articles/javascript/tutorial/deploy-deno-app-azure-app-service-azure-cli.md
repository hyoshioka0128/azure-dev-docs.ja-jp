---
title: Azure CLI から Azure App Service に Deno アプリをデプロイする
description: このチュートリアルでは、Azure CLI を使用して、Deno アプリケーションを Azure App Service (Linux または Windows 上) にデプロイします。
ms.topic: tutorial
ms.date: 10/13/2020
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-javascript
ms.openlocfilehash: ba2e0a42b6d2dedd2192629562a8415a0d6d7167
ms.sourcegitcommit: 0cda024089784b92c1db3a4506c1dccd6bfe6339
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772617"
---
# <a name="deploy-deno-apps-to-azure-app-service-from-the-azure-cli"></a>Azure CLI から Azure App Service に Deno アプリをデプロイする

Azure CLI を使用して、Deno アプリケーションを Azure App Service (Linux または Windows 上) にデプロイします。 Azure App Service 用の Deno ランタイムには、実験用の Docker イメージが用意されています。 

![デモ サーバーの実行](../media/deploy-azure/deno-hello-world.png)

## <a name="1-prepare-your-environment"></a>1.環境を準備する

- アクティブなサブスクリプションが含まれる Azure アカウント。 [無料で作成できます](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-deno&mktingSource=vscode-tutorial-appservice-deno)
- [Visual Studio Code](https://code.visualstudio.com/) をインストールします
- [Deno](https://deno.land/#installation) をインストールします
- Bash 環境で [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) を使用します。

   [![埋め込みの起動](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell を起動する")](https://shell.azure.com)   
- 必要に応じて、Azure CLI を[インストール](/cli/azure/install-azure-cli)して、CLI リファレンス コマンドを実行します。
   - ローカル インストールを使用する場合は、[az login](/cli/azure/reference-index#az-login) コマンドを使用して Azure CLI でサインインします。  認証プロセスを完了するには、ターミナルに表示される手順に従います。  追加のサインイン オプションについては、「[Azure CLI を使用してサインインする](/cli/azure/authenticate-azure-cli)」を参照してください。
  - 初回使用時にインストールを求められたら、Azure CLI 拡張機能をインストールします。  拡張機能の詳細については、「[Azure CLI で拡張機能を使用する](/cli/azure/azure-cli-extensions-overview)」を参照してください。
  - [az version](/cli/azure/reference-index?#az_version) を実行し、インストールされているバージョンおよび依存ライブラリを検索します。 最新バージョンにアップグレードするには、[az upgrade](/cli/azure/reference-index?#az_upgrade) を実行します。

## <a name="2-sign-in-to-azure-cli"></a>2.Azure CLI へのサインイン

ローカルのコマンド ラインから Azure CLI を使用する場合は、CLI コマンドを使用する前に [az login](/cli/azure/reference-index#az-login) を使用してサインインする必要があります。

[!INCLUDE [interactive-login](../../azure-cli/includes/interactive-login.md)]

3. ログインすると、ご使用のアカウントに関連付けられているサブスクリプションの一覧が表示されます。 `isDefault: true` が示されているサブスクリプション情報が、ログイン後に、現在アクティブになっているサブスクリプションです。 

4. 別のサブスクリプションを選択する必要がある場合、切り替え先のサブスクリプション ID を指定して [az account set](/cli/azure/account#az-account-set) コマンドを実行します。 サブスクリプションの選択の詳細については、「[複数の Azure サブスクリプションの使用](/cli/azure/manage-azure-subscriptions-azure-cli)」を参照してください。

## <a name="3-create-local-deno-api-app"></a>3.ローカル Deno API アプリを作成する

Deno の組み込み Web サーバーを使用して Deno アプリを作成します。 次に、アプリをローカルで実行します。

1. ターミナルまたはコマンド プロンプトで、アプリ フォルダーを作成する場所に移動し、`deno-demo` という名前の新しいフォルダーを作成します。

1. `demo.ts` という名前の新しいファイルを作成します。
1. Deno では、URL から直接、実行中のコードを受け取ります。 すべての要求に "Hello World" で答える HTTP サーバーを作成します。 次のコードを使用します。

    ```typescript
    import { serve } from "https://deno.land/std@0.54.0/http/server.ts"
    const handler = serve({ port: 80 })

    console.log("Serving at 80")

    for await (const req of handler) {
     req.respond({ body: "Hello World!\n" })
    }
    ```

1. 次のスクリプトを実行し、アプリを実行します。

    ```bash
    deno run --allow-net ./demo.ts
    ```

1. ブラウザーを開き、`http://localhost:80` を参照して、アプリをテストします。 サイトは次のように表示されます。

    ![デモ サーバーの実行](../media/deploy-azure/deno-hello-world.png)

    「`deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts`」と入力し、このコードを実行することもできます。

1. ターミナルで **Ctrl** + **C** キーを押してサーバーを停止します。

## <a name="4-deploy-deno-app-to-azure"></a>4.Deno アプリを Azure にデプロイする

Azure CLI を使用して Azure に Deno アプリをデプロイします。

1. 次のコマンドを使用して `deno-quickstart` という名前のリソース グループを作成します。

    ```azurecli
    az group create --name deno-quickstart --location eastus
    ```

    リソース グループの名前を変更することにした場合、次の手順で必ずすべての `-g` フラグを更新してください。

1. このコマンドを使用し、Web サイトを保持する `deno-plan` という名前の AppService Plan を作成します。

    ```azurecli
    az appservice plan create --resource-group deno-quickstart --name deno-plan --is-linux
    ```

1. 次に、Web アプリ自体を作成します。 このコマンドによって新しい AppService が作成され、以前に作成した Plan にバインドされます。 Web アプリに付ける名前の `<your-app-name>` タグを変更します。これは一意にする必要があります。

    ```azurecli
    az webapp create -n <your-app-name> --resource-group deno-quickstart -p deno-plan -i anthonychu/azure-webapps-deno:1.0.2
    ```

    この AppService では、Deno コードを実行するための基本機能を提供する `anthonychu/azure-webapps-deno:1.0.2` Docker イメージを実行します。 この処理は、完了するまでに数秒かかる場合があります。

## <a name="5-configure-the-azure-app-service-webapp"></a>5.Azure App Service の Web アプリを構成する

1. 実験用の Deno イメージ名に対する Docker コンテナー イメージを取得する場所を Web アプリに指示します。

    ```azurecli
    az webapp config container set --name <your-app-name> --resource-group deno-quickstart -i anthonychu/azure-webapps-deno:1.0.2 -r 'https://index.docker.io' -u '' -p  '' -t true
    ```

1. スタートアップ ファイルがないように Web アプリを設定します。

    ```azurecli
    az webapp config set --name <your-app-name> --resource-group deno-quickstart --startup-file ''

1. Set the webapp to have runtime environment variables:

    ```azurecli
    az webapp config appsettings set --name <your-app-name> --resource-group deno-quickstart --settings WEBSITE_RUN_FROM_PACKAGE=1 WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
    ```

## <a name="6-configure-deno-app-deployment-to-web-app"></a>6.Web アプリへの Deno アプリのデプロイを構成する 

Azure Web アプリが構成されて、使用の準備が整っていますが、Deno パッケージがまだ存在しません。 アプリを `.zip` パッケージにパッケージ化し、ファイルがローカル コンピューター上にあることを Web アプリに指示し、zip ファイル内のスタートアップ ファイルを設定します。 

1. `deno-demo` フォルダーに移動します。

    ```bash
    cd deno-demo
    ```

1. `zip` コマンドを実行します

    ```bash
    zip demo demo.ts
    ```

    このコマンドの結果、`demo.ts` ファイルと同じフォルダーに `demo.zip` という名前のファイルが生成されます。

1. デプロイのコード ソースとしてパッケージを構成します。

    ```azurecli
    az webapp deployment source config-zip --name <your-app-name> --resource-group deno-quickstart --src ./demo.zip
    ```

1. パッケージ内のファイルが実行するように構成します。

    ```azurecli
    az webapp config set --name <your-app-name> --resource-group deno-quickstart --startup-file 'deno run --allow-net demo.ts'
    ```

1. `https://<your-app-name>.azurewebsites.net` に進んでアプリケーションをテストします。 

## <a name="7-clean-up-resources"></a>7.リソースをクリーンアップする

次のコマンドを使用して、リソース グループを削除します。これにより、Web アプリのリソースも削除されます。

```azurecli
az group delete deno-quickstart
```

## <a name="next-steps"></a>次のステップ

各項目の詳細情報
* Visual Studio Code 拡張機能を使用して [App Service にデプロイする](../tutorial-vscode-azure-app-service-node-01.md)
* [仮想マシンにデプロイする](./nodejs-virtual-machine-vm/introduction.md)
* [カスタム ハンドラー](/azure/azure-functions/functions-custom-handlers)として [Deno 関数をデプロイする](https://github.com/anthonychu/azure-functions-deno-worker)