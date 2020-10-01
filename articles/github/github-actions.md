---
title: GitHub Actions を使用して Azure にデプロイする
description: リポジトリ内にワークフローを作成し、Azure へのビルド、テスト、パッケージ化、リリース、およびデプロイを行います。
author: N-Usha
ms.author: ushan
ms.topic: conceptual
ms.service: azure
ms.date: 05/05/2020
ms.openlocfilehash: f3245aa3d75510352d89d26c1c61825dbac1e2c8
ms.sourcegitcommit: e97cb81a245ce7dcabeac3260abc3db7c30edd79
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91493090"
---
# <a name="deploy-to-azure-using-github-actions"></a>GitHub Actions を使用して Azure にデプロイする

開発者は、[GitHub Actions](https://help.github.com/articles/about-github-actions) を使用することで、自動化されたソフトウェア開発ライフサイクル ワークフローを構築できます。  

Azure 用 GitHub Actions を使用すると、ビルド、テスト、パッケージ化、リリース、および Azure への**デプロイ**を行うために設定可能なワークフローをリポジトリ内に作成できます。 [Azure との他のすべての統合の詳細については、こちらを参照してください](https://aka.ms/GitHubonAzure)。

[無料の Azure アカウント](https://azure.com/free/open-source)を使用して今すぐ始めましょう。

> [!NOTE]   
> この記事に記載されているリンクは、GitHub の記事または GitHub リポジトリにリンクしています。 

## <a name="key-concepts"></a>主要な概念

GitHub Actions を使用すると、カスタムのソフトウェア開発ライフサイクル (SDLC) ワークフローを GitHub リポジトリに直接作成できます。 GitHub Actions と主要な概念の概要については、次の記事を参照してください。 

- [GitHub Actions について](https://help.github.com/actions/getting-started-with-github-actions/about-github-actions)
- [主要な概念](https://help.github.com/actions/getting-started-with-github-actions/core-concepts-for-github-actions)
- [GitHub Actions によるパッケージ化について](https://help.github.com/en/actions/publishing-packages-with-github-actions/about-packaging-with-github-actions)

## <a name="get-started"></a>はじめに 

GitHub Actions には事前構成されたテンプレートと Marketplace アクションがあります。 

- [構成済みのテンプレートを使用する](https://help.github.com/actions/getting-started-with-github-actions/starting-with-preconfigured-workflow-templates)  
- [GitHub Marketplace からのアクションを使用する](https://help.github.com/en/actions/getting-started-with-github-actions/using-actions-from-github-marketplace)  
- [GitHub Marketplace アクション、Azure へのデプロイ](https://github.com/marketplace?type=actions&query=Azure)  
  
Azure 用 GitHub Actions については、次のページを参照してください。 
   
- [Azure のアクション](https://github.com/marketplace?query=Azure&type=actions)  
- [Azure にデプロイするためのスターター アクション ワークフロー](https://github.com/Azure/actions-workflow-samples)


## <a name="connect-to-azure"></a>Azure に接続する

Azure に接続し、Az CLI または Az PowerShell に基づいてスクリプトを実行するサンプル ワークフローについては、次の GitHub アクションを使用します。  

- [Azure ログイン](https://github.com/Azure/login)  
- [Azure CLI](https://github.com/Azure/CLI)
- [Azure PowerShell](https://github.com/Azure/powershell)


## <a name="sample-apps-with-cicd-workflow-samples"></a>CI/CD ワークフロー サンプルを使用したサンプル アプリ 

次のサンプルでは、任意の言語、任意のエコシステムの Web アプリをビルドして Azure にデプロイするためのエンド ツー エンドのワークフローが提供されています。 

- [ASP.NET サポートを使用して Web アプリをデプロイする](https://github.com/Azure-Samples/dotnet-sample)  
- [ASP.NET Core アプリをデプロイする](https://github.com/Azure-Samples/dotnet_core_sample)  
- [Node.js Web アプリをデプロイする](https://github.com/Azure-Samples/node_express_app)  
- [Java Web アプリをデプロイする](https://github.com/Azure-Samples/java-spring-petclinic)  
- [Java Spring アプリをデプロイする](https://github.com/Azure-Samples/Java-application-petstore-ee7)  
- [Python Web アプリをデプロイする](https://github.com/Azure-Samples/pythonSample_thecatsaidno)  
- [Docker を使用してコンテナー化された Web アプリをデプロイする](https://github.com/Azure-Samples/Node_express_container)


## <a name="deploy-a-web-app"></a>Web アプリをデプロイする

Azure Web Apps および Azure Web App for Containers にデプロイする:

- [Azure/webapps-deploy アクション](https://github.com/Azure/webapps-deploy)

静的 Web アプリをデプロイする:
- [Azure/static-web-apps-deploy](/azure/static-web-apps/getting-started?tabs=angular)


次のアクションを使用して、アプリ設定と接続文字列を構成します。

- [Azure/appservice-settings](https://github.com/Azure/appservice-settings) 
- [Azure App Service 設定](https://github.com/Azure/appservice-settings)  

## <a name="deploy-a-serverless-app"></a>サーバーレス アプリのデプロイ

- [Azure Functions](https://github.com/Azure/functions-action)  
- [コンテナー用の Azure Functions](https://github.com/Azure/webapps-container-deploy)  
 
## <a name="build-and-deploy-containerized-apps"></a>コンテナー化されたアプリのビルドとデプロイ

- [Docker ログイン](https://github.com/Azure/docker-login)  
- [Azure Container Instances へのデプロイ](https://github.com/Azure/aci-deploy)
- [コンテナーのスキャン操作](https://github.com/Azure/container-scan)

## <a name="deploy-to-kubernetes"></a>Kubernetes へのデプロイ

- [Kubectl ツール インストーラー](https://github.com/Azure/setup-kubectl)  
- [Kubernetes コンテキスト設定](https://github.com/Azure/k8s-set-context)  
- [AKS コンテキスト設定](https://github.com/Azure/aks-set-context)  
- [Kubernetes シークレット作成](https://github.com/Azure/k8s-create-secret)  
- [Kubernetes デプロイ](https://github.com/Azure/k8s-deploy)  
- [Helm セットアップ](https://github.com/Azure/setup-helm)  
- [Kubernetes ベイク](https://github.com/Azure/k8s-bake)  

## <a name="train-and-deploy-a-machine-learning-model"></a>機械学習モデルのトレーニングとデプロイ 

- [Login](https://github.com/Azure/aml-workspace) 
- [トレーニング](https://github.com/Azure/aml-run)
- [モデルのデプロイ](https://github.com/Azure/aml-deploy)

## <a name="deploy-to-databases"></a>データベースへのデプロイ

- [Azure SQL データベース](https://github.com/Azure/sql-action)  
- [Azure MySQL アクション](https://github.com/Azure/mysql-action)  

## <a name="azure-policy-integrations"></a>Azure Policy の統合

- [Azure Policy コンプライアンス スキャン](https://github.com/Azure/policy-compliance-scan) 

## <a name="trigger-a-run-in-azure-pipelines"></a>Azure Pipelines での実行のトリガー

- [Azure Pipelines](https://github.com/Azure/pipelines)  
 
## <a name="utility-actions"></a>ユーティリティ アクション

- [変数の置換](https://github.com/Microsoft/variable-substitution) 


## <a name="additional-resources"></a>その他のリソース

GitHub を使用した Azure へのアプリのデプロイをサポートするには、次の GitHub リソースを使用できます。  

- [Azure 用 GitHub Actions のマーケットプレース](https://github.com/marketplace?query=Azure&type=actions)
- [学習ラボ、Azure を使用した継続的デリバリー](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)
- [Azure にデプロイするためのスターター アクション ワークフロー](https://github.com/Azure/actions-workflow-samples)