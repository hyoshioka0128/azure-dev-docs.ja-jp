---
title: Jenkins と Azure の概要
description: Azure で Jenkins ビルドをホストし、オートメーション サーバーをデプロイします。また、Azure のコンピューティング リソースとストレージ リソースを使用することで、継続的インテグレーションとデプロイ (CI/CD) パイプラインを拡張します。
keywords: Jenkins, Azure, 開発, 概要
ms.topic: overview
ms.date: 08/08/2020
ms.custom: devx-track-jenkins
ms.openlocfilehash: 43e38bd609f00257859c5976dc0b4e0fa4ae9e8f
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "88240634"
---
# <a name="azure-and-jenkins"></a>Azure と Jenkins

[Jenkins](https://jenkins.io/) は、ソフトウェア プロジェクトの継続的インテグレーションと配信 (CI/CD) を設定する際に使用される、広く普及しているオープンソースのオートメーション サーバーです。 Azure で Jenkins デプロイをホストしたり、Azure リソースを使用して、既存の Jenkins 構成を拡張したりできます。 Azure へのアプリケーションの CI/CD を簡素化する Jenkins プラグインも使用できます。

この記事は、Jenkins での Azure 使用の概要のほか、Jenkins ユーザーが利用できる重要な Azure コア機能も扱っています。 お使いの Jenkins サーバーの Azure での共有については、[Azure での Jenkins サーバーの作成](configure-on-linux-vm.md)に関する記事を参照してください。

## <a name="host-your-jenkins-servers-in-azure"></a>Azure で Jenkins サーバーをホストする

Azure で Jenkins をホストすることで、ビルドの自動化を集中管理し、ソフトウェア プロジェクト拡大のニーズに合わせてデプロイを拡大縮小します。 Linux VM に Jenkins をインストールして構成する方法については、[Jenkins の使用を開始するためのクイックスタート](configure-on-linux-vm.md)を参照してください。 Azure Jenkins のデプロイは、[Azure Monitor ログ](/azure/log-analytics/log-analytics-overview)と [Azure CLI](/cli/azure) を使用して監視および管理します。

## <a name="scale-your-build-automation-on-demand"></a>ビルド自動化をオン デマンドで拡大縮小する

ジョブとパイプラインのビルド数と複雑性の増加に応じて、既存の Jenkins デプロイにビルド エージェントを追加して、Jenkins のビルド容量を拡張します。 こうしたビルド エージェントを Azure 仮想マシンで実行するには、[Azure VM Agents プラグイン](https://plugins.jenkins.io/azure-vm-agents)を使用します。 詳細については、こちらの[チュートリアル](/azure/jenkins/jenkins-azure-vm-agents)を参照してください。

[Azure サービス プリンシパル](/azure/azure-resource-manager/resource-group-overview)で構成したら、Jenkins のジョブとパイプラインでは、その資格情報を使用することで次が実現します。

- [Azure Storage プラグイン](https://plugins.jenkins.io/windows-azure-storage)を使用して、ビルド アーティファクトを [Azure Storage](/azure/storage/common/storage-introduction) に安全に格納およびアーカイブする。 詳細については、[Jenkins ストレージの操作方法](azure-storage-blobs-as-build-artifact-repository.md)に関するページをご覧ください。
- [Azure CLI](deploy-to-azure-app-service-using-azure-cli.md) を使用して Azure リソースを管理および構成する。

## <a name="deploy-your-code-into-azure-services"></a>Azure サービスにコードをデプロイする

Jenkins プラグインを使用して、アプリケーションを Jenkins CI/CD パイプラインの一部として Azure にデプロイします。 [Azure App Service](/azure/app-service/) および [Azure Container Service](/azure/container-service/kubernetes/) にデプロイすることで、基盤となるインフラストラクチャを管理することなく、アプリケーションの更新プログラムをステージング、テスト、およびリリースできます。

 次のサービスおよび環境にデプロイするためのプラグインが用意されています。

- [Azure App Service on Linux](/azure/app-service/containers/app-service-linux-intro)。 作業を開始するには、こちらの[チュートリアル](deploy-from-github-to-azure-app-service.md)を参照してください。
- [Azure App Service](/azure/app-service/overview)。 作業を開始するには、こちらの[操作方法](deploy-to-azure-app-service-using-plugin.md)を参照してください。