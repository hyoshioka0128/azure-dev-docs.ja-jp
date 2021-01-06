---
title: Azure 向けの GitHub Actions とは
description: リポジトリ内にワークフローを作成し、ビルド、テスト、パッケージ化、リリース、および Azure へのデプロイを行います。
author: N-Usha
ms.author: ushan
ms.topic: conceptual
ms.service: azure
ms.date: 10/30/2020
ms.custom: github-actions-azure
ms.openlocfilehash: c952410231ae57b0d127e1b6fb05e03f88403640
ms.sourcegitcommit: 0d2ea78f18430c845a32e0d2311427ab81033465
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/23/2020
ms.locfileid: "97754068"
---
# <a name="what-is-github-actions-for-azure"></a>Azure 向けの GitHub Actions とは

[GitHub Actions](https://help.github.com/articles/about-github-actions) は、GitHub 内からソフトウェア開発ワークフローを自動化するのに役立ちます。 コードを保存する場所と同じ場所にワークフローをデプロイし、プル要求や問題に対して共同作業を行うことができます。

GitHub Actions の[ワークフロー](https://help.github.com/articles/about-github-actions#workflow)とは、お使いの GitHub リポジトリで設定する自動化されたプロセスです。 ワークフローを使用すると、任意のプロジェクトを GitHub でビルド、テスト、パッケージ化、リリース、またはデプロイできます。

各ワークフローは、特定のイベント (プル要求など) が発生した後に実行される個々の[アクション](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)で構成されます。  個々のアクションは、ソフトウェア開発タスクを自動化する、パッケージ化されたスクリプトです。

Azure 向けの GitHub Actions を使用すると、ビルド、テスト、パッケージ化、リリース、および Azure へのデプロイを行うために設定可能なワークフローをリポジトリ内に作成できます。 Azure 向けの GitHub Actions では、Azure App Service、Azure Functions、Azure Key Vault などの Azure サービスがサポートされています。

GitHub Actions には、Azure Resource Manager テンプレート、Azure CLI、Azure Policy などのユーティリティのサポートも含まれています。

GitHub Actions を使用した継続的デリバリーの詳細については、GitHub Universe 2020 のこちらの動画をご覧ください。  

> [!VIDEO https://www.youtube.com/embed/36hY0-O4STg]

## <a name="why-should-i-use-github-actions-for-azure"></a>Azure 向けの GitHub Actions を使用する理由

Azure 向けの GitHub Actions は、Microsoft によって開発されたものであり、Azure で使用するように設計されています。 Azure 向けのすべての GitHub Actions は、[GitHub Marketplace](https://github.com/marketplace?query=Azure&type=actions) で確認できます。 アクションをワークフローに組み込む方法の詳細については、「[アクションの検索とカスタマイズ](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/finding-and-customizing-actions)」をご覧ください。

## <a name="what-is-the-difference-between-github-actions-and-azure-pipelines"></a>GitHub Actions と Azure Pipelines の違いは何ですか

Azure Pipelines と GitHub Actions はどちらも、ソフトウェア開発ワークフローの自動化に役立ちます。 これらのサービスの違いと、Azure Pipelines から GitHub Actions への移行方法に関する[詳細情報](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/migrating-from-azure-pipelines-to-github-actions)をご覧ください。

## <a name="what-do-i-need-to-use-github-actions-for-azure"></a>Azure 向けの GitHub Actions を使用するには何が必要ですか

次の Azure と GitHub のアカウントが必要です。

* アクティブなサブスクリプションが含まれる Azure アカウント。 [無料でアカウントを作成できます](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* GitHub アカウント。 お持ちでない場合は、[無料](https://github.com/join)でサインアップしてください。  

## <a name="how-do-i-connect-github-actions-and-azure"></a>GitHub Actions と Azure の接続方法

アクションに応じて、サービス プリンシパルまたは発行プロファイルを使用して、GitHub から Azure に接続します。 [Azure login](https://github.com/marketplace/actions/azure-login) アクションを使用するたびに、サービス プリンシパルを使用します。 [Azure App Service アクション](https://github.com/marketplace/actions/azure-webapp)は、発行プロファイルまたはサービス プリンシパルの使用をサポートしています。 サービス プリンシパルの詳細については、「[Azure Active Directory のアプリケーション オブジェクトとサービス プリンシパル オブジェクト](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object)」を参照してください。  

Azure login アクションは、[Azure CLI](https://github.com/marketplace/actions/azure-cli-action) と [Azure PowerShell](https://github.com/marketplace/actions/azure-powershell-action) のアクションの両方と組み合わせて使用できます。 Azure login アクションは、Azure 向けのその他のほとんどの GitHub Actions ([Web アプリへのデプロイ](https://github.com/marketplace/actions/azure-webapp)、[キー コンテナー シークレットへのアクセス](https://github.com/marketplace/actions/azure-key-vault-get-secrets)など) でも機能します。

## <a name="what-is-included-in-a-github-actions-workflow"></a>GitHub Actions ワークフローに含まれる内容

ワークフローは、1 つ以上のジョブで構成されます。 ジョブ内には、個々のアクションで構成されるステップがあります。 GitHub Actions の概念の詳細については、「[GitHub Actions の概要](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)」をご覧ください。  

## <a name="where-can-i-see-complete-workflow-examples"></a>完成したワークフローの例はどこで確認できますか

[Azure スターター アクション ワークフロー リポジトリ](https://github.com/Azure/actions-workflow-samples)では、任意の言語、任意のエコシステムの Web アプリをビルドして Azure にデプロイするためのエンド ツー エンドのワークフローが提供されています。

## <a name="where-can-i-see-all-the-available-actions"></a>使用可能なすべてのアクションを確認できる場所

[Azure 向けの GitHub Actions のマーケットプレース](https://github.com/marketplace?query=Azure&type=actions)にアクセスして、Azure で利用可能なすべての GitHub Actions をご覧ください。

* [静的 Web アプリへのデプロイ](/azure/static-web-apps/getting-started?tabs=angular)
* [Azure App Service 設定](https://github.com/Azure/appservice-settings)  
* [Azure Functions へのデプロイ](https://github.com/Azure/functions-action)  
* [コンテナー用の Azure Functions へのデプロイ](https://github.com/Azure/webapps-container-deploy)  
* [Docker ログイン](https://github.com/Azure/docker-login)  
* [Azure Container Instances へのデプロイ](https://github.com/Azure/aci-deploy)
* [コンテナーのスキャン操作](https://github.com/Azure/container-scan)
* [Kubectl ツール インストーラー](https://github.com/Azure/setup-kubectl)  
* [Kubernetes コンテキスト設定](https://github.com/Azure/k8s-set-context)  
* [AKS コンテキスト設定](https://github.com/Azure/aks-set-context)  
* [Kubernetes シークレット作成](https://github.com/Azure/k8s-create-secret)  
* [Kubernetes デプロイ](https://github.com/Azure/k8s-deploy)  
* [Helm セットアップ](https://github.com/Azure/setup-helm)  
* [Kubernetes ベイク](https://github.com/Azure/k8s-bake)  
* [Azure 仮想マシン イメージの作成](https://github.com/Azure/build-vm-image)
* [Machine Learning へのログイン](https://github.com/Azure/aml-workspace)
* [機械学習のトレーニング](https://github.com/Azure/aml-run)
* [機械学習 - モデルのデプロイ](https://github.com/Azure/aml-deploy)
* [Azure SQL データベースへのデプロイ](https://github.com/Azure/sql-action)  
* [Azure MySQL へのデプロイ アクション](https://github.com/Azure/mysql-action)  
* [Azure Policy コンプライアンス スキャン](https://github.com/Azure/policy-compliance-scan)
* [Azure Policy の管理](https://github.com/Azure/manage-azure-policy)
* [Azure Pipelines の実行のトリガー](https://github.com/Azure/pipelines)  
* [変数の置換](https://github.com/Microsoft/variable-substitution)

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [ラーニング パス、GitHub Actions を使用したワークフローの自動化](https://docs.microsoft.com/learn/modules/github-actions-automate-tasks/)

> [!div class="nextstepaction"]
> [学習ラボ、Azure を使用した継続的デリバリー](https://lab.github.com/githubtraining/github-actions:-continuous-delivery-with-azure)
