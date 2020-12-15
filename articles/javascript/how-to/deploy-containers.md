---
title: Node.js コンテナーを Azure にデプロイする
description: Docker コンテナーを使用して Node.js アプリを Azure にデプロイする
ms.topic: how-to
ms.date: 12/07/2020
ms.custom: seo-javascript-september2019, devx-track-js
ms.openlocfilehash: 6bf8acb66a708433966bdfe90cc358a56d2d3b42
ms.sourcegitcommit: ae2fa266a36958c04625bb0ab212e6f2db98e026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857810"
---
# <a name="deploy-nodejs-container-to-azure"></a>Node.js コンテナーを Azure にデプロイする 

コンテナーを使用したアプリの構築は、スケーラビリティのための一般的なパターンです。 Azure では、コンテナーをデプロイする方法に関していくつかの選択肢が用意されています。

## <a name="host-your-container-app-on-azure"></a>Azure でコンテナー アプリをホストする

次のホスティングを選択すると、コンテナー化されたアプリケーションをデプロイできます。

| サービス | 推奨される対象 |
|--|--|
|[App Service](/azure/app-service/quickstart-custom-container?pivots=container-linux)|Azure App Service でカスタム コンテナーをデプロイして実行します。|
|[Container Instances](/azure/container-instances/)|1 つのコンテナーをすばやく設定します。|
|[コンテナー レジストリ](/azure/container-registry/)|カスタムまたはプライベートのコンテナー イメージを構築、保存、管理します。|
|[Kubernetes Service](/azure/aks/)|複数コンテナーのオーケストレーション。|
|[仮想マシン](/azure/virtual-machines) (VM)|Windows または Linux VM のフル コントロール。 [動作保証済みの Linux ディストリビューションを見つける](/azure/virtual-machines/linux/endorsed-distros?toc=/azure/virtual-machines/linux/toc.json)か、Azure Marketplace で Linux VM イメージを[検索する方法を確認](/azure/virtual-machines/linux/cli-ps-findimage)してください。|

## <a name="build-containerize-and-deploy-app-to-azure"></a>アプリをビルドおよびコンテナー化して Azure にデプロイする

開始するには、次の方法を示すこちらの[チュートリアル](develop-nodejs-on-azure.md)に従ってください。

* サンプル コードのダウンロード
* Node.js アプリを実行する
* Visual Studio Code でアプリをデバッグする
* Node.js の MEAN アプリをコンテナー化する
* Azure CLI コマンドを使用してアプリをデプロイする
* CosmosDB リソース上に MongoDB サーバーを作成する
* コンテナー イメージをプライベート コンテナー レジストリに追加する
* カスタム ドメイン名を Web アプリに追加する
* Web アプリをより大きなサイズにスケールアウトする
* すべてのリソース用のリソース グループを作成および削除する

## <a name="next-steps"></a>次のステップ

Microsoft Learn モジュール：

- [Azure Container Instances で Docker コンテナーを実行する](/learn/modules/run-docker-with-azure-container-instances/)

- [Azure Container Registry を使用してコンテナー イメージをビルドして格納する](/learn/modules/build-and-store-container-images/)
