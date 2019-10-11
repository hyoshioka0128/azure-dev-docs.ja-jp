---
title: チュートリアル:Visual Studio Code を使用してコンテナー イメージを Azure App Service にデプロイする
description: チュートリアルの手順 2、実際の Docker イメージをコンテナー レジストリから Azure App Service にデプロイする。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: a099e25134615fc05508cde3c0f128a3be813402
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172271"
---
# <a name="tutorial-deploy-a-container-image-to-azure-app-service"></a>チュートリアル:コンテナー イメージを Azure App Service にデプロイする

[前の手順: 前提条件](tutorial-deploy-containers-01.md)

コンテナー イメージがレジストリ内にある場合は、VS Code の Docker 拡張機能を使用して、コンテナーを実行している Azure App Service を簡単に設定できます。

1. **Docker** エクスプローラーで **[レジストリ]** を展開し、お使いのレジストリ (**Azure** など) のノードを展開して、お使いのイメージ名のノードを、`:latest` タグが付いたイメージが表示されるまで展開します。

    ![Docker エクスプローラー内でイメージを検索する](media/deploy-containers/deploy-find-image.png)

1. そのイメージを右クリックし、 **[Deploy Image to Azure App Service]\(Azure App Service にイメージをデプロイ\)** を選択します。

    ![デプロイ メニュー コマンドを選択する](media/deploy-containers/deploy-menu.png)

1. 画面の指示に従って、Azure サブスクリプションの選択、リソース グループの選択または指定、リージョンの指定、App Service プランの構成 (B1 が最も低コスト)、サイト名の指定を行います。 下のアニメーションでこのプロセスを示します。

    ![モデルを作成およびデプロイする](media/deploy-containers/deploy-to-app-service.gif)

    **リソース グループ**は、アプリを構成するさまざまなリソースの名前付きコレクションです。 アプリのリソースすべてを 1 つのグループに割り当てることで、それらのリソースを 1 つのユニットとして簡単に管理できます (詳細については、Azure ドキュメントの「[Azure Resource Manager の概要](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)」を参照してください)。

    **App Service プラン**は、実行中のコンテナーをホストする物理リソース (基になる仮想マシン) を定義します。 このチュートリアルでは、B1 が、Docker コンテナーをサポートする最も低コストのプランです (詳細については、Azure ドキュメントの [App Service プランの概要](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)に関する記事を参照してください)。

    App Service の名前は Azure 全体で一意である必要があるため、通常は会社名または個人名を使用します。 運用サイトでは、通常、App Service を構成する際に、個別に登録されたドメイン名を使用します。

1. App Service の作成には数分かかり、進行状況は VS Code の [出力] パネルに表示されます。

1. 完了したら、App Service に `WEBSITES_PORT` という名前の設定を追加して ("WEBSITES" と複数形になっていることに注意してください)、コンテナーがリッスンしているポートを指定することも**必要です** (たとえば、[VS Code での Python コンテナーの作成](https://code.visualstudio.com/docs/python/tutorial-create-container)に関するチュートリアルにあるイメージを使用している場合、Flask のポートは 5000、Django のポートは 8000 になります)。 `WEBSITES_PORT` を設定するには、 **[Azure: App Service]** エクスプローラーに切り替え、新しい App Service のノードを展開します (必要に応じて更新します)。次に、 **[Application Settings]\(アプリケーション設定\)** を右クリックし、 **[新しい設定の追加]** を選択します。 プロンプトで、キーとして「`WEBSITES_PORT`」を入力し、その値にポート番号を入力します。

    ![新しい設定を追加するための App Service のコンテキスト メニュー コマンド](media/deploy-containers/add-app-service-setting.png)

1. 設定を変更すると、App Service は自動的に再起動されます。 また、いつでも App Service を右クリックして **[再起動]** を選択できます。

1. サービスが再起動したら、`http://<name>.azurewebsites.net` でサイトを参照します。 [出力] パネル内の URL を **Ctrl** キーを押しながらクリック (macOS の場合は **Cmd** キーを押しながらクリック) するか、 **[Azure: App Service]** エクスプローラー内で [App Service] を右クリックして **[Web サイトの参照]** を選択します。

> [!div class="nextstepaction"]
> [イメージをデプロイしました](tutorial-deploy-containers-03.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=02-deploy-container)
