---
title: Visual Studio Code から Node.js アプリのコンテナー イメージをデプロイする
description: Docker チュートリアル パート 5、Azure App Service にイメージをデプロイする
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: devx-track-js
ms.openlocfilehash: 18a1b616a14571923ec52528e21227be703b3aac
ms.sourcegitcommit: f723980ade4cbc13548a5d8ac3f3fa681b8a2dbd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97609374"
---
# <a name="deploy-the-image-to-azure-app-service"></a>Azure App Service にイメージをデプロイする

[前の手順:アプリ イメージを作成する](tutorial-vscode-docker-node-04.md)

この手順では、レジストリにプッシュしたイメージを Visual Studio Code から直接 [Azure App Service](https://azure.microsoft.com/services/app-service/) にデプロイします。

## <a name="enable-admin-access-on-the-registry"></a>レジストリの管理者アクセスを有効にする

イメージを Web アプリに配置するには、Azure portal でレジストリに対して "管理者" アクセスを有効にする必要があります。

1. **Docker** エクスプローラーで、レジストリ名を右クリックし、[ポータルで開く] を選択します。 

    ![VS Code の [ポータルで開く] コマンド](../../media/deploy-containers/open-in-portal.png)

    これにより、Azure portal でレジストリが開きます。

1. サイドバーで [アクセス キー] をクリックし、[管理者ユーザー] 設定を [有効] に切り替えます。  
    
    ![Azure portal で管理者ユーザー設定を有効にする](../../media/deploy-containers/access-keys.png)

## <a name="deploy-image"></a>イメージをデプロイする

1. **DOCKER** エクスプローラーで、 **[Registries]\(レジストリ\)** でイメージのノードを展開し、`:latest` を右クリックし、 **[Deploy Image to Azure App Service]\(Azure App Service にイメージをデプロイする\)** を選択します。

    ![エクスプローラーからデプロイする](../../media/deploy-containers/deploy-image-command.png)

1. 入力を求められたら、App Service の値を指定します。

    - この名前は Azure 全体で一意である必要があります。
    - 既存のリソース グループを選択するか、新しいリソース グループを作成します。 (**リソース グループ** は、基本的に、Azure のアプリケーション リソースの名前付きコレクションです。)
    - 既存の App Service プランを選択するか、新しいプランを作成します。 (Web サイトをホストする物理リソースは、**App Service プラン** によって定義されます。 このチュートリアルでは、Basic または無料プラン サービス レベルを使用できます)。

1. デプロイが完了すると、Visual Studio Code は、Web サイトの URL を示す通知を表示します。

    ![デプロイ成功のメッセージ](../../media/deploy-containers/deploy-successful.png)

1. 結果は、Visual Studio Code の **[出力]** パネルの **[Docker]** セクションでも確認することができます。

    ![デプロイ成功の出力](../../media/deploy-containers/deploy-output.png)

1. デプロイされた Web サイトを参照するには、 **[出力]** パネルの URL を **Ctrl**+**クリック** することもできます。 新しい App Service は、Visual Studio Code で **AZURE** エクスプローラーの **[APP SERVICE]** セクションにも表示されます。そこで、Web サイトを右クリックして、 **[Web サイトの参照]** を選択できます。

> [!div class="nextstepaction"]
> [サイトは Azure にあります](tutorial-vscode-docker-node-06.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
