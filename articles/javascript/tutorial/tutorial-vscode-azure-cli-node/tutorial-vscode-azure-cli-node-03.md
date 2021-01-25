---
title: アプリをホストするための Azure App Service を Azure CLI から作成する
description: チュートリアル パート 3、Azure CLI で App Service を作成する
ms.topic: tutorial
ms.date: 01/13/2021
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: ad8e0836d62102521b557fd45b24291d52db447c
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561518"
---
# <a name="create-the-app-service"></a>App Service の作成

[前の手順:アプリケーションの作成](tutorial-vscode-azure-cli-node-02.md)

この手順では、Azure CLI を使用して、アプリ コードをホストするための Azure App Service を作成します。

<a name="create-resource-group"></a>

## <a name="create-resource-group-and-set-as-default-value"></a>リソース グループを作成し、既定値として設定する

1. ターミナルまたはコマンド プロンプトで次のコマンドを使用して、App Service の **リソース グループ** を作成します。 リソース グループは、基本的に、Web サイト、データベース、Azure Functions など、Azure におけるアプリ リソースの名前付きコレクションです。

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

    上の Azure CLI コマンド [`az group create`](/cli/azure/group#az_group_create) は、`westus` データ センターに `myResourceGroup` というリソース グループを作成します。 これらの値は、必要に応じて変更できます。

    コマンドが正常に実行されると、リソース グループの詳細を含む JSON 出力が表示されます。

1. 次の Azure CLI コマンド [`az configure`](/cli/azure/config) を実行して、後続コマンド用の既定のリソース グループとリージョンを設定します。 これにより、毎回これらの値を指定する必要がなくなります。 (このコマンドには、成功時の出力はありません。)

    ```azurecli
    az configure --defaults group=myResourceGroup location=westus
    ```

## <a name="create-and-deploy-web-app-service-with-azure-cli-command"></a>Azure CLI コマンドを使用して Web アプリ サービスを作成してデプロイする

次の Azure CLI コマンド [`az webapp up`](/cli/azure/webapp#az_webapp_up) を実行して、App Service アプリを作成してデプロイします。 `<your_app_name>` は、URL になる一意の名前 `http://<your_app_name>.azurewebsites.net` に置き換えます。 

```azurecli
az webapp up --name <your_app_name> --logs --launch-browser
```

`--logs` コマンドは、Web アプリの起動直後にログ ストリームを表示します。 `--launch-browser` コマンドは、新しいアプリに対して既定のブラウザーを開きます。 同じコマンドを使用して、アプリ全体を再デプロイできます。 

## <a name="troubleshooting"></a>トラブルシューティング

* 欠落しているが必須のパラメーターである `--resource-group` に関するエラーが表示された場合は、この記事の先頭に戻り、既定値を設定するか、パラメーターと値を指定してください。 

## <a name="next-steps"></a>次のステップ

Azure [webapp](/cli/azure/webapp) コマンド グループまたは Azure [App service](/cli/azure/appservice) コマンド グループを使用して、Web アプリのコマンドの詳細を確認してください。 

> [!div class="nextstepaction"]
> [App Service を作成しました](tutorial-vscode-azure-cli-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
