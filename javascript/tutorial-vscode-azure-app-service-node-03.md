---
title: Visual Studio Code から Azure App Service に Node.js アプリをデプロイする
description: チュートリアル パート 3、Web サイトをデプロイする
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 5891c5a9dafe87987f725b38147957fb6a961ca5
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686254"
---
# <a name="deploy-the-website"></a>Web サイトのデプロイ

[前の手順: アプリを作成する](tutorial-vscode-azure-app-service-node-02.md)

この手順では、Visual Studio Code と Azure App Service 拡張機能を使用して Node.js Web サイトをデプロイします。 このチュートリアルでは、アプリが圧縮されて Azure App Service on Linux にデプロイされる、最も基本的なデプロイ モデルを使用します。

1. アプリをデプロイする前に、それが、`PORT` 環境変数 `process.env.PORT` で指定されたポートでリッスンしていることを確認してください。

1. アプリ フォルダー (前の手順の `myExpressApp` など) で、次のコマンドを使用してそのフォルダー内の VS Code を起動します。

    ```bash
    code .
    ```

1. VS Code で、Azure アイコンを選択して **Azure App Service** エクスプローラーを開き、青い上矢印アイコンを選択してアプリを Azure にデプロイします。

    ![Web アプリへのデプロイ](media/deploy-azure/deploy.png)

    > [!TIP]
    > または、**コマンド パレット** を開き (**F1**)、「deploy to web app」と入力し、**Azure App Service:Deploy to Web App** コマンドを実行します。

1. プロンプトで、次の情報を入力します。

    a. アプリ (このチュートリアルで使用されている `myExpressApp`) の現在のフォルダーを選択します。

    a. **[新しい Web アプリを作成する]** を選択します。

    a. アプリのグローバルに一意の名前を入力し、**Enter** キーを押します。 アプリ名に使用できる有効な文字は "a-z"、"0-9"、"-" です。

    a. 自分の近く、またはアクセスする必要がある他のサービスの近くにある Azure リージョン内の場所 ([リージョン](https://azure.microsoft.com/regions/)に関するページで説明) を選択します。

    a. **Node.js のバージョン**を選択します (LTS を推奨)。

1. 拡張機能によってアプリが作成されると、VS Code の **[出力]** ウィンドウに進行状況が表示されます (**Azure App Service** の出力を選択しなければならない場合があります)。

    ![Azure App Service に対する Visual Studio Code の [出力] ウィンドウ](media/deploy-azure/output-window.png)

1. ターゲット サーバーで `npm install` を実行するように構成を更新するよう求められたら、 **[はい]** を選択します。 その後、アプリがデプロイされます。

    ![構成済みのデプロイ](media/deploy-azure/server-build.png)

1. デプロイが開始されると、すべての後続のデプロイで、同じ App Service Web アプリが自動的にターゲットになるようにワークスペースを更新するよう求められます。 自分の変更が適切なアプリにデプロイされるよう **[はい]** を選択してください。

    ![構成済みのデプロイ](media/deploy-azure/save-configuration.png)

1. デプロイが完了したら、プロンプトで **[Web サイトの参照]** を選択して、新しくデプロイした Web サイトを表示します。

## <a name="troubleshooting"></a>トラブルシューティング

"このディレクトリまたはページを表示するアクセス許可がありません" というエラーは、おそらくアプリが正常に起動できなかったことを示しています。 [次の手順](tutorial-vscode-azure-app-service-node-04.md)に記載されているように、ログを表示すると、問題を診断して修正するのに役立ちます。 ご自身で解決できない場合は、以下の **[問題が発生しました]** ボタンをクリックしてお問い合わせください。 喜んでお手伝いします。

## <a name="updating-the-website"></a>Web サイトの更新

同じプロセスを使用し、新しいアプリを作成する代わりに既存のものを選択することで、このアプリに対する変更をデプロイできます。

----

> [!div class="nextstepaction"]
> [サイトは Azure にあります](tutorial-vscode-azure-app-service-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
