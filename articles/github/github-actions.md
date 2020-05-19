---
title: GitHub Actions を使用して Azure にデプロイする
description: リポジトリ内にワークフローを作成し、Azure へのビルド、テスト、パッケージ化、リリース、およびデプロイを行います。
ms.author: kaelli
author: KathrynEE
ms.topic: conceptual
ms.service: azure
ms.date: 05/05/2020
ms.openlocfilehash: 16ebc2d8013b2199911c0648f4bb8364c4d6de06
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369921"
---
# <a name="deploy-to-azure-using-github-actions"></a>GitHub Actions を使用して Azure にデプロイする

Azure 向けの GitHub Actions を使用して、Azure にデプロイするワークフローを自動化します。 開始するには、次の GitHub Actions を参照してください。 

- Azure Web Apps と Azure Web App for Containers にデプロイするための [Azure/webapps-deploy](https://github.com/Azure/webapps-deploy) アクション 
- Azure WebApp (Windows または Linux) またはそのデプロイ スロットで JSON 構文を使用して、アプリ設定、接続文字列、その他の一般的な設定を一括して構成する [Azure/appservice-settings](https://github.com/Azure/appservice-settings)

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
  
Azure 向けのすべての GitHub Actions へのリンクについては、次のページを参照してください。 
   
- [Azure のアクション](https://github.com/marketplace?query=Azure&type=actions)  

## <a name="connect-to-azure"></a>Azure に接続する

Azure に接続するサンプル ワークフローについては、次の GitHub アクションを参照してください。  

- [Azure ログイン](https://github.com/Azure/login)  
- [Azure CLI](https://github.com/Azure/CLI)  


## <a name="starter-templates-and-end-to-end-cicd-workflow-samples"></a>スターター テンプレートとエンドツーエンドの CI/CD ワークフローのサンプル 

次のサンプルには、Web アプリを Azure にデプロイするためのエンドツーエンドのワークフローが用意されています。 

- [ASP.NET サポートを使用して Web アプリをデプロイする](https://github.com/Azure-Samples/dotnet-sample)  
- [ASP.NET Core アプリをデプロイする](https://github.com/Azure-Samples/dotnet_core_sample)  
- [Node.js Web アプリをデプロイする](https://github.com/Azure-Samples/node_express_app)  
- [Java Web アプリをデプロイする](https://github.com/Azure-Samples/java-spring-petclinic)  
- [Java Spring アプリをデプロイする](https://github.com/Azure-Samples/Java-application-petstore-ee7)  
- [Python Web アプリをデプロイする](https://github.com/Azure-Samples/pythonSample_thecatsaidno)  
- [Docker を使用してデプロイする](https://github.com/Azure-Samples/Node_express_container)  


## <a name="deploy-a-web-app"></a>Web アプリをデプロイする

- [Azure Web アプリ](https://github.com/Azure/webapps-deploy)  
- [Azure Web App for Containers](https://github.com/Azure/webapps-container-deploy)  
- [Azure App Service 設定](https://github.com/Azure/appservice-settings)  

## <a name="deploy-a-serverless-app"></a>サーバーレス アプリのデプロイ

- [Azure Functions](https://github.com/Azure/functions-action)  
- [コンテナー用の Azure Functions](https://github.com/Azure/webapps-container-deploy)  
 
## <a name="build-and-deploy-containerized-apps"></a>コンテナー化されたアプリのビルドとデプロイ

- [Docker ログイン](https://github.com/Azure/docker-login)  

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

## <a name="trigger-a-run-in-azure-pipelines"></a>Azure Pipelines での実行のトリガー

- [Azure Pipelines](https://github.com/Azure/pipelines)  
 
## <a name="utility-actions"></a>ユーティリティ アクション

- [変数の置換](https://github.com/Microsoft/variable-substitution) 


## <a name="additional-resources"></a>その他のリソース

GitHub を使用した Azure へのアプリのデプロイをサポートするには、次の GitHub リソースを使用できます。  

- [Azure Marketplace 向けの GitHub Actions](https://github.com/marketplace?query=Azure&type=actions)
- [学習ラボ、Azure を使用した継続的デリバリー](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)
- [Azure にデプロイするためのスターター アクション ワークフロー](https://github.com/Azure/actions-workflow-samples)
