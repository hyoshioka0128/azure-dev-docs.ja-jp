---
title: インクルード ファイル tutorial-azure-web-app-mongodb-03.md
description: インクルード ファイル tutorial-azure-web-app-mongodb-03.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: adea87271b1332f77ab254530410787d1a9baa3c
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183932"
---
チュートリアルのこのセクションでは、サンプル アプリケーションを Azure にデプロイします。 その後、リモートで実行されているアプリをブラウザーで表示できます。 

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](../azure-sign-in.md)]

## <a name="create-web-app-resource"></a>Web アプリ リソースを作成する

Visual Studio Code 拡張機能を使用して App Service リソースを作成し、Web アプリをそのリソースにデプロイします。

1. Azure エクスプローラーに移動します。 サブスクリプションを右クリックし、[`Create new web app...`] を選択します。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/create-web-app-with-extension.png" alt-text="Azure App Service 拡張機能を使用して Web アプリを作成する Visual Studio Code の部分的なスクリーンショット。":::

1. 値がどのように使用されるかを理解するために次の表を使用しながら、プロンプトに従います。

    |プロパティ|値|
    |--|--|
    |新しい Web アプリのグローバルに一意の名前を入力します。| App Service リソースに `web-app-with-mongodb-YOUR-NAME` などの値を入力します。 `<YOUR-NAME>` を自分の名前または一意の ID に置き換えます。 この一意の名前は、ブラウザーでリソースにアクセスするために URL の一部としても使用されます。|
    |Linux アプリのランタイムを選択します。|[`Node 12 LTS`] を選択します。|

1. アプリの作成プロセスが完了すると、Visual Studio Code の右下隅にステータス メッセージが表示され、[`Deploy`] または [`View output`] を選択できます。 [`Deploy`] を選択します。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-extension-create-web-app-deploy-web-app.png" alt-text="Azure App Service 拡張機能を使用して Web アプリを作成する Visual Studio Code の部分的なスクリーンショット。":::

    ステータス メッセージが表示されなくなったら、Azure エクスプローラーを選択してデプロイし、リソース名を右クリックして、 **[Web アプリにデプロイ]** を選択します。

1. デプロイ プロセス中、通知が表示され、 **出力ウィンドウ** を表示することを選択できます。  これにより、デプロイのローリング状態が表示されます。 

1. デプロイが完了すると、通知が表示されます。 **[ストリーム ログ]** を選択して、ローリング ログを表示します。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-service-deployed.png" alt-text="Azure App Service 拡張機能を使用して Web アプリを作成する Visual Studio Code の部分的なスクリーンショット。":::

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-app-service-stream-logs.png" alt-text="Azure App Service 拡張機能を使用して Web アプリを作成する Visual Studio Code の部分的なスクリーンショット。":::    

1. ブラウザーで Web サイトを開き、テキスト `YOUR-RESOURCE_NAME` を独自のリソース名 `https://YOUR-RESOURCE_NAME.azurewebsites.net` に置き換えます。
    
    これで Web サイトはローカルでもリモートでも実行できるようになりましたが、データベースにはまだ接続されていません。 

## <a name="want-to-know-more"></a>詳細について

初期の Web サービスはポート 8080 で実行されるように構成されており、公開されています。 このような種類の Web サイト設定は構成可能です。
* [アプリ設定](/azure/app-service/configure-common)
* [認証](/azure/app-service/configure-authentication-provider-microsoft)
* [ネットワークごとにアクセスを制限する](/azure/azure/app-service/app-service-ip-restrictions)

この App Service 拡張機能を使用して Web サイトを Azure クラウドにデプロイする場合は、[そのデプロイの構成](https://github.com/microsoft/vscode-azureappservice/wiki/Configuring-Zip-Deployment#additional-zip-deploy-configuration-settings)方法について理解を深めてください