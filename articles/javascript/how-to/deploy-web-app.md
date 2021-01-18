---
title: JavaScript アプリを Azure にデプロイする
description: ホスティング オプションとデプロイ シナリオには、Azure 用のサービスとツールがいくつか含まれています。 アプリを発行し、Azure でサービスを提供します。
ms.topic: how-to
ms.date: 01/12/2021
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js, contperf-fy21q2
ms.openlocfilehash: c1ca1f3dcf99ce96c52eec7ebccee9860608b273
ms.sourcegitcommit: 657f43a5048cd17b080b40b5090d575c8d7f5eaf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/13/2021
ms.locfileid: "98173212"
---
# <a name="deploy-and-host-your-nodejs-apps-on-azure"></a>Azure で Node.js アプリをデプロイしてホストする

ホスティング オプションとデプロイ シナリオには、Azure 用のサービスとツールがいくつか含まれています。 Azure には、ホスティングの多数のオプションと、ローカルまたはクラウドのリポジトリから Azure へのアプリの移動に役立つ多数のツールが用意されています。 

## <a name="choose-a-recommended-azure-host-provider"></a>推奨される Azure ホスト プロバイダーの選択

Azure でのクライアント、サーバー、またはバックグラウンド タスク アプリのホスティングには、さまざまなソリューションが用意されており、その中から選択することができます。 次の表を使用して選択してください。 ほとんどのユース ケースで推奨されるソリューションは、[Azure App Service](/azure/app-service/overview) です。 

さまざまなホスティング方法を包括的に把握するには、「[Azure コンピューティング サービスのデシジョン ツリー](/azure/architecture/guide/technology-choices/compute-decision-tree)」のほか、Microsoft Learn の「[コア Cloud Services - Azure コンピューティング オプション](/learn/modules/intro-to-azure-compute)」モジュールを参照してください。


 サービス |サポートされているアプリの種類| 推奨される対象 |
|--|--|--|
|[*App Service](/azure/app-service/overview) - **推奨**|クライアント、サーバー、クライアント/サーバー、API、サーバー レンダー|コードまたはコンテナーからアプリをホストします。 これにより、基盤となる環境を管理することなく、Web サーバーを管理できます。|
|[(プレビュー) Static Web Apps](/azure/static-web-apps/)|静的フロントエンド、事前レンダー、サーバー API を使用した静的フロントエンド|静的クライアント アプリ (Angular、Vue、React など) をホストします。 必要に応じて、フルスタック アプリをホストするサーバーレス関数エンドポイントを追加します。 この単純なサービスにより Web サーバーの多くが抽象化されるため、ユーザーはクライアント アプリケーションにとって重要な機能に専念できます。 |
|[関数](/azure/azure-functions/)|サーバー API|サーバーレス API エンドポイントをホストします。 Azure には、一般的なシナリオをブートストラップするためのトリガーと呼ばれる多数のテンプレートが用意されています。|

## <a name="host-web-apps-with-more-control"></a>より詳細な制御で Web アプリをホストする

次の選択肢を使用すると、アプリケーション環境をより細かく制御できます。 

| サービス | 推奨される対象 |
|--|--|
|[仮想マシン](/azure/virtual-machines) (VM)|Windows または Linux VM のフル コントロール。 [動作保証済みの Linux ディストリビューションを見つける](/azure/virtual-machines/linux/endorsed-distros?toc=/azure/virtual-machines/linux/toc.json)か、Azure Marketplace で Linux VM イメージを[検索する方法を確認](/azure/virtual-machines/linux/cli-ps-findimage)してください。|
|[Container Instances](/azure/container-instances/)|1 つのコンテナーをすばやく設定します。|
|[Kubernetes Service](/azure/aks/)|複数コンテナーのオーケストレーション。|

## <a name="alternative-choices-for-web-app-hosting-on-azure"></a>Azure での Web アプリ ホスティングの代替選択肢

これらの選択は、特定のユース ケースに合わせて調整されます。 

| サービス | 推奨される対象 |
|--|--|
|[Storage](/azure/storage/blobs/storage-blob-static-website-how-to?tabs=azure-portal)|Azure Storage は、静的 Web アプリをホストすることもできます。 これは、堅牢な Storage とクライアント アプリケーションの間で緊密な統合が必要な場合に役立ちます。|
|[Content Delivery Network](/azure/cdn/) (CDN)|事前にレンダリングされた Web サイトを配信します。 最も近いポイント オブ プレゼンス (POP) サーバーを使用して、Azure BLOB ストレージ、Web アプリケーション、またはパブリックにアクセスできる任意の Web サーバーから読み込まれた静的オブジェクトをキャッシュします。 さらに Azure CDN では、各種のネットワーク最適化とルーティング最適化を利用して、キャッシュできない動的なコンテンツも高速化できます。|

## <a name="deploy-your-web-app-to-azure"></a>Azure への Web アプリのデプロイ

アプリケーションをホストするサービスを選択したら、デプロイ プロセスとツールを選択します。 クライアントおよびサーバーのアプリを Azure サービスにデプロイするということは、ファイルまたはファイルのセットを Azure に移動して、HTTP エンドポイント経由でサービスを提供することを意味します。 

Azure クラウドにファイルを移動する一般的な方法は次のとおりです。

| メソッド | 詳細 |
|--|--|
|[GitHub のアクション](/azure/app-service/deploy-github-actions?tabs=applevel)|これは、自動化またはトリガーによる継続的なデプロイに使用します。|
|[Visual Studio Code 拡張機能](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance)|これは、手動の場合やテスト、まれにデプロイに使用されます。 サービスの拡張機能がローカルにインストールされている必要があります。|
|[Azure CLI](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-04.md)|これは、手動の場合か、まれにデプロイに使用されます。 サービスの拡張機能がローカルにインストールされている必要があります。|

特定のサービスに基づいて他のデプロイ方法が存在する場合があります。 たとえば、Azure App Service では、多種多様なデプロイ方法がサポートされています。
* [ZIP ファイルから](/azure/app-service/deploy-zip)
* [FTP を使用](/azure/app-service/deploy-ftp)
* [Dropbox または OneDrive](/azure/app-service/deploy-content-sync)
* [ローカル Git](/azure/app-service/deploy-local-git)
* [cURL](/azure/app-service/deploy-zip#with-curl)
* [SSH](/azure/app-service/configure-linux-open-ssh-session)

最初のデプロイ時に使用しなかったメソッドであっても、[提供されている任意のメソッド](#deploy-your-web-app-to-azure)を使用して、App Service に再デプロイできます。 メソッドを切り替える場合は、再デプロイ前に構成を行うことができます。 

### <a name="deploy-or-redeploy-to-app-service-with-visual-studio-code"></a>Visual Studio Code を使用して App Service にデプロイまたは再デプロイする

Visual Studio Code を使用して App Service アプリをデプロイまたは再デプロイするには、App Service 拡張機能のサービスの一覧から App Service を右クリックし、 **[Web アプリにデプロイ]** を選択します。 

:::image type="content" source="../media/azure-app-service-vscode-extensions/deploy-or-redeploy-app-service.png" alt-text="Visual Studio Code を使用して App Service にデプロイまたは再デプロイする":::

## <a name="verify-your-deployment-with-your-http-endpoint"></a>HTTP エンドポイントでデプロイを確認する

デプロイを確認するには、HTTP エンドポイントにアクセスします。 HTTP エンドポイントは、 **[概要]** ページですべてのサービスに表示されます。 

### <a name="view-http-endpoint-in-azure-portal"></a>Azure portal で Azure エンドポイントを表示する

Azure portal のサービスの [概要] ページで HTTP エンドポイントを表示します。 

:::image type="content" source="../media/howto-deploy/azure-portal-hosting-url.png" alt-text="Azure portal のサービスの [概要] ページで HTTP エンドポイントを表示する。":::

## <a name="next-steps"></a>次のステップ

* [コンテナーを使用したデプロイ](deploy-containers.md)
* [Visual Studio Code を使用したその他のデプロイ チュートリアル](https://code.visualstudio.com/docs/azure/deployment)
