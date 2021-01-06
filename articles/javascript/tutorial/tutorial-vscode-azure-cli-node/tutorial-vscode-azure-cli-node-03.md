---
title: アプリをホストするための Azure App Service を Azure CLI から作成する
description: チュートリアル パート 3、Azure CLI で App Service を作成する
ms.topic: tutorial
ms.date: 12/18/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: abd434d2222e5bdd758be42856a569e24def08f8
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687436"
---
# <a name="create-the-app-service"></a>App Service の作成

[前の手順:アプリケーションの作成](tutorial-vscode-azure-cli-node-02.md)

この手順では、Azure CLI を使用して、アプリ コードをホストするための Azure App Service を作成します。

1. ターミナルまたはコマンド プロンプトで次のコマンドを使用して、App Service の **リソース グループ** を作成します。 リソース グループは、基本的に、Web サイト、データベース、Azure Functions など、Azure におけるアプリ リソースの名前付きコレクションです。

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

    上の Azure CLI コマンド [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) は、`westus` データ センターに `myResourceGroup` というリソース グループを作成します。 これらの値は、必要に応じて変更できます。

    コマンドが正常に実行されると、リソース グループの詳細を含む JSON 出力が表示されます。

1. 次の Azure CLI コマンド [`az configure`](/cli/azure/config?view=azure-cli-latest) を実行して、後続コマンド用の既定のリソース グループとリージョンを設定します。 これにより、毎回これらの値を指定する必要がなくなります。 (このコマンドには、成功時の出力はありません。)

    ```azurecli
    az configure --defaults group=myResourceGroup location=westus
    ```

1. 次の Azure CLI コマンド [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) を実行して、App Service で使用される基の仮想マシンを定義する **App Service プラン** を作成します。

    ```azurecli
    az appservice plan create --name myPlan --sku F1
    ```

    上のコマンドを使用すると、共有仮想マシンを使用する [Free ホスティング プラン](../../core/what-is-azure-for-javascript-development.md#free-tier-resources) (`--sku F1`) が指定され、プランに `myPlan` という名前が設定されます。 

1. 次の Azure CLI コマンド [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) を実行して、App Service を作成します。`<your_app_name>` は、[最新の Node.js ランタイム](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes&preserve-view=false)を使用する URL `http://<your_app_name>.azurewebsites.net` になる一意の名前に置き換えます。 

    ```azurecli
    az webapp create --name <your_app_name> --plan myPlan -g --runtime "node|12-lts"
    ```


1. 次の Azure CLI コマンド [`az webapp browse`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_browse) を実行して、新しく作成された App Service をブラウザーで開きます。ここでも、`<your_app_name>` は、使用した名前に置き換えます。

    ```azurecli
    az webapp browse --name <your_app_name>
    ```

1. アプリのカスタム コードをデプロイしていないので、ブラウザーには既定のページが表示されます。

    ![既定の App Service ページ](../../media/azure-cli/azure-default-page.png)

## <a name="troubleshooting"></a>トラブルシューティング

* 欠落しているが必須のパラメーターである `--resource-group` に関するエラーが表示された場合は、この記事の先頭に戻り、既定値を設定してください。 

> [!div class="nextstepaction"]
> [App Service を作成しました](tutorial-vscode-azure-cli-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
