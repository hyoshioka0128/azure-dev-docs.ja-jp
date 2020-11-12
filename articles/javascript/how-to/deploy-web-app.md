---
title: JavaScript アプリを Azure にデプロイする
description: ホスティング オプションとデプロイ シナリオには、Azure 用のサービスとツールがいくつか含まれています。 アプリを発行し、Azure でサービスを提供します。
ms.topic: how-to
ms.date: 10/28/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js, contperfq2
ms.openlocfilehash: e0c3df78dd44e6879f24773dbbce86a5af76a35c
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94333810"
---
# <a name="deploy-and-host-your-nodejs-apps-on-azure"></a>Azure で Node.js アプリをデプロイしてホストする

ホスティング オプションとデプロイ シナリオには、Azure 用のサービスとツールがいくつか含まれています。 Azure には、ホスティングの多数のオプションと、ローカルまたはクラウドのリポジトリから Azure へのアプリの移動に役立つ多数のツールが用意されています。 

## <a name="choose-a-hosting-provider-from-azure"></a>Azure からホスティング プロバイダーを選択する

Azure でのクライアント、サーバー、またはバックグラウンド タスク アプリのホスティングには、さまざまなソリューションが用意されており、その中から選択することができます。 次の表を使用して選択してください。 ほとんどのユース ケースで推奨されるソリューションは、[Azure App Service](/azure/app-service/overview) です。 

| サービス | 推奨される対象 |
|--|--|
|[App Service](/azure/app-service/overview) - **推奨**|コードまたはコンテナーからアプリをホストします。 これにより、基盤となる環境を管理することなく、Web サーバーを管理できます。|
|[静的 Web アプリ](/azure/static-web-apps/)|静的クライアント アプリ (Angular、Vue、React など) をホストします。 必要に応じて、フルスタック アプリをホストするサーバーレス関数エンドポイントを追加します。 この単純なサービスにより Web サーバーの多くが抽象化されるため、ユーザーはクライアント アプリケーションにとって重要な機能に専念できます。 |
|[関数](/azure/azure-functions/)|サーバーレス アプリケーション エンドポイントをホストします。|
|[Storage](/azure/storage/blobs/storage-blob-static-website-how-to?tabs=azure-portal)|Azure Storage は、静的 Web アプリをホストすることもできます。 これは、堅牢な Storage とクライアント アプリケーションの間で緊密な統合が必要な場合に役立ちます。|
|[Container Instances](/azure/container-instances/)|1 つのコンテナーをすばやく設定します。|
|[Kubernetes Service](/azure/aks/)|複数コンテナーのオーケストレーション。|
|[仮想マシン](/azure/virtual-machines) (VM)|Windows または Linux VM のフル コントロール。 [動作保証済みの Linux ディストリビューションを見つける](/azure/virtual-machines/linux/endorsed-distros?toc=/azure/virtual-machines/linux/toc.json)か、Azure Marketplace で Linux VM イメージを[検索する方法を確認](/azure/virtual-machines/linux/cli-ps-findimage)してください。|
|[Content Delivery Network](/azure/cdn/) (CDN)|最も近いポイント オブ プレゼンス (POP) サーバーを使用して、Azure BLOB ストレージ、Web アプリケーション、またはパブリックにアクセスできる任意の Web サーバーから読み込まれた静的オブジェクトをキャッシュします。 さらに Azure CDN では、各種のネットワーク最適化とルーティング最適化を利用して、キャッシュできない動的なコンテンツも高速化できます。|

さまざまなホスティング方法を包括的に把握するには、「[Azure コンピューティング サービスのデシジョン ツリー](/azure/architecture/guide/technology-choices/compute-decision-tree)」のほか、Microsoft Learn の「[コア Cloud Services - Azure コンピューティング オプション](/learn/modules/intro-to-azure-compute)」モジュールを参照してください。

## <a name="choose-your-deployment-process-for-azure"></a>Azure に対するデプロイ プロセスを選択する

アプリケーションをホストするサービスを選択したら、デプロイ プロセスとツールを選択します。 クライアントおよびサーバーのアプリを Azure サービスにデプロイするということは、ファイルまたはファイルのセットを Azure に移動して、HTTP エンドポイント経由でサービスを提供することを意味します。 

次の表に、Azure クラウドにファイルを移動するための一般的な方法を示します。

| メソッド | 詳細 |
|--|--|
|[GitHub のアクション](/azure/app-service/deploy-github-actions?tabs=applevel)|これは、自動化またはトリガーによる継続的なデプロイに使用します。|
|[Visual Studio Code 拡張機能](https://marketplace.visualstudio.com/search?term=azure&target=VSCode&category=All%20categories&sortBy=Relevance)|これは、手動の場合やテスト、まれにデプロイに使用されます。 サービスの拡張機能がローカルにインストールされている必要があります。|
|[Azure CLI](../tutorial-vscode-azure-cli-node-04.md)|これは、手動の場合か、まれにデプロイに使用されます。 サービスの拡張機能がローカルにインストールされている必要があります。|

特定のサービスに基づいて他のデプロイ方法が存在する場合があります。 たとえば、Azure App Service では、多種多様なデプロイ方法がサポートされています。
* [ZIP ファイルから](/azure/app-service/deploy-zip)
* [FTP を使用](/azure/app-service/deploy-ftp)
* [Dropbox または OneDrive](/azure/app-service/deploy-content-sync)
* [ローカル Git](/azure/app-service/deploy-local-git)

## <a name="verify-your-deployment-with-your-http-endpoint"></a>HTTP エンドポイントでデプロイを確認する

デプロイを確認するには、HTTP エンドポイントにアクセスします。 HTTP エンドポイントは、 **[概要]** ページですべてのサービスに表示されます。 

### <a name="view-http-endpoint-in-azure-portal"></a>Azure portal で Azure エンドポイントを表示する

Azure portal のサービスの [概要] ページで HTTP エンドポイントを表示します。 

:::image type="content" source="../media/howto-deploy/azure-portal-hosting-url.png" alt-text="Azure portal のサービスの [概要] ページで HTTP エンドポイントを表示する。":::

## <a name="next-steps"></a>次のステップ

* [コンテナーを使用したデプロイ](deploy-containers.md)
