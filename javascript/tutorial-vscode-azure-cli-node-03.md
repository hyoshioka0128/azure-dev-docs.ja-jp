---
title: アプリをホストするための Azure App Service を Azure CLI から作成する
description: チュートリアル パート 3、App Service を作成する
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 827bcde79336304f424a283ccbdc4a4282a50bf7
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "77709889"
---
# <a name="create-the-app-service"></a>App Service の作成

[前の手順:アプリケーションの作成](tutorial-vscode-azure-cli-node-02.md)

この手順では、Azure CLI を使用して、アプリ コードをホストするための Azure App Service を作成します。

1. ターミナルまたはコマンド プロンプトで次のコマンドを使用して、App Service の**リソース グループ**を作成します。 リソース グループは、基本的に、Web サイト、データベース、Azure Functions など、Azure におけるアプリ リソースの名前付きコレクションです。

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

    上のコマンドは、`westus` データ センターに `myResourceGroup` というリソース グループを作成します。 これらの値は、必要に応じて変更できます。

    コマンドが正常に実行されると、リソース グループの詳細を含む JSON 出力が表示されます。

1. 次のコマンドを実行して、後続コマンド用の既定のリソース グループとリージョンを設定します。 これにより、毎回これらの値を指定する必要がなくなります。 (このコマンドには、成功時の出力はありません。)

    ```azurecli
    az configure --defaults group=myResourceGroup location=westus
    ```

1. 次のコマンドを実行して、App Service で使用される基の仮想マシンを定義する **App Service プラン**を作成します。

    ```azurecli
    az appservice plan create --name myPlan --sku F1
    ```

    上のコマンドは、共有仮想マシンを使用する無料ホスティング プラン (`--sku F1`) を指定し、プランに `myPlan` という名前を指定します。 この場合も、コマンドは成功時に JSON 出力を表示します。

1. 次のコマンドを実行して、App Service を作成します。`<your_app_name>` は、URL `http://<your_app_name>.azurewebsites.net` になる一意の名前に置き換えます。 PowerShell コマンドは少し異なることに注意してください。 `--runtime "node|6.9"` 引数は、サーバーでノード バージョン 6.9.x を使用するように Azure に指示します。

    ```azurecli
    az webapp create --name <your_app_name> --plan myPlan --runtime "node|6.9"
    ```

    > [!TIP]
    > また、`package.json` に目的のノード バージョンを指定することもできます。 Azure は、デプロイ時にこの設定を適用します。 たとえば、次の `package.json` エントリは、少なくともノード 7.0.0 を使用するように Azure に指示します。
    >
    > ``` json
    > "engines": {
    >     "node": ">7.0.0"
    > },
    > ```

1. 次のコマンドを実行して、新しく作成された App Service をブラウザーで開きます。ここでも、`<your_app_name>` は、使用した名前に置き換えます。

    ```azurecli
    az webapp browse --name <your_app_name>
    ```

1. アプリのカスタム コードをデプロイしていないので、ブラウザーには既定のページが表示されます。

    ![既定の App Service ページ](media/azure-cli/azure-default-page.png)

> [!div class="nextstepaction"]
> [App Service を作成しました](tutorial-vscode-azure-cli-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
