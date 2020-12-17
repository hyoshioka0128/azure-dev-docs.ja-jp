---
title: Azure 用語集のチート シート
description: Microsoft Azure を利用する際に理解しておく必要がある、最も重要な用語と概念の簡単な一覧です。
ms.date: 12/07/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: f79864b39bb70c7ae703468f205e420417698200
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2020
ms.locfileid: "97521979"
---
# <a name="azure-terminology-in-brief"></a>Azure 用語の概略

| 項目 | 簡単な説明 |
| --- | --- |
| [アカウントとサブスクリプション](#account-and-subscriptions) | Azure 上のリソースを管理するための課金情報と基本的な組織構造。 [詳細情報](#account-and-subscriptions)
| [リソース](#resource) | Azure データ センター内での特定の機能の割り当ての総称。 [詳細情報](#resource) |
| [リソース グループ](#resource-group) | 1 つの単位として管理できるその他のリソースの論理コンテナー。 [詳細情報](#resource-group) |
| [リージョン](#region-location) | リソースが割り当てられている特定の Azure データ センターへの参照。 [詳細情報](#region-location) |
| [Azure App Service](#azure-app-service) | Web アプリケーション用の Azure のマネージド ホスティング サービス。 [詳細情報](#azure-app-service) |
| [App Service プラン](#app-service-plan) | Azure App Service によって使用される仮想マシンを定義するリソース。 [詳細情報](#app-service-plan) |
| [Azure Portal](#azure-portal) | Azure リソースを作成および管理するための Web ベースの UI。 [詳細情報](#azure-portal) |
| [Azure CLI](#azure-command-line-interface-cli) | Azure リソースを作成および管理するために使用するテキスト ベースのコマンドのセット。 [詳細情報](#azure-command-line-interface-cli) |
| [az webapp、az webapp up](#az-webapp-az-webapp-up) | Azure App Service を操作するための Azure CLI コマンド。 [詳細....](#az-webapp-az-webapp-up) |

## <a name="account-and-subscriptions"></a>アカウントとサブスクリプション

**Azure アカウント** には、お客様の基本的な連絡先情報 (電話番号、メール アドレス) と課金情報 (クレジット カード) が含まれています。

1 つの課金アカウント内で、アクティビティを 1 つ以上の **サブスクリプション** に整理できます。 各サブスクリプションには独自のアクセス許可があり、同じアカウントの下にある個人または部署に対して個別のサブスクリプションを作成することができます。 1 人のユーザーが任意の数のサブスクリプションにアクセスできます。

Azure でのリソースの作成は、常にサブスクリプションのコンテキスト内で行われます。 [サブスクリプション間でリソースを移動](/azure/azure-resource-manager/management/move-resource-group-and-subscription)することができますが、アカウント間では移動できません。

## <a name="resource"></a>リソース

**リソース** とは、Azure データ センター内での特定の機能の割り当ての総称です。 リソースには、仮想マシン、仮想ネットワーク、さまざまなレベルのストレージ、データベース、機械学習モデル、IoT インジェスト ハブなどがあります。

リソースには実際のコンピューティング能力が割り当てられるため、必要なパフォーマンス レベルに応じて、各リソースに継続的なコストが発生する可能性があります。 開発とテストのために、無料のサービス レベルで多くのリソースを作成できます。 詳しくは、[料金計算ツール](https://azure.microsoft.com/pricing/calculator/)に関するページをご覧ください。

## <a name="resource-group"></a>リソース グループ

1 つのサブスクリプション内において、"*リソース グループ*" とは、1 つの単位として管理できるその他のリソースの論理コンテナーです。

リソース グループは、通常、特定のプロジェクトに関連付けられており、リソースのプロビジョニング時には常にリソース グループを指定する必要があります。 通常、新しいプロジェクトで最初に行う手順は、適切なリソース グループの作成です。

リソース グループを削除すると、それに含まれるすべてのリソースが割り当て解除され、削除されます。これは、各リソースを個別に削除するよりも便利です。

## <a name="region-location"></a>リージョン (場所)

**リージョン** は、リソースがプロビジョニングされている Azure データ センターの特定の場所を識別します。

さまざまなリソースは常にリージョン間で通信できますが、リソースが同じリージョンに配置されている場合は、その効率が向上します。

グローバルなお客様にサービスを提供するアプリケーションでは、Azure で複数のリージョンにリソースを自動的にレプリケートさせることで、アプリケーションの全体的な応答性とパフォーマンスを向上させることができます。

## <a name="azure-app-service"></a>Azure App Service

**App Service** は、Web アプリケーション用の Azure のマネージド ホスティング サービスです。 Azure により、基盤となるすべてのハードウェアおよびサーバー インフラストラクチャが管理されます。ユーザーはコードと構成を指定します。

App Service では、App Service **Web アプリ** と呼ばれるホストをプロビジョニングし、コードをアップロードします。 また、負荷分散、スケーリング、サーバー側の環境変数など、ホストのさまざまな特性を構成します。

App Service Web アプリの直接 URL は常に `<web-app-name>.azurewebsites.net` です。 カスタム ドメインを使用するように Web アプリを構成することもできます。

## <a name="app-service-plan"></a>App Service プラン

**App Service プラン** は、Azure App Service によって使用される仮想マシンを定義するリソースであり、その後、アプリケーションをホストするためのコア コストが決まります。 コードをデプロイする前に、App Service のプロビジョニングの一部として App Service プランを定義します。

## <a name="azure-portal"></a>Azure portal

[Azure portal](https://portal.azure.com) は Web ベースのユーザー インターフェイスであり、Azure のアカウント、サブスクリプション、リソースを操作できます。

## <a name="azure-command-line-interface-cli"></a>Azure コマンド ライン インターフェイス (CLI)

[Azure CLI](/cli/azure/what-is-azure-cli) は、Azure リソースを作成および管理するためのコマンド セットです。特に自動化の役に立ちます。 Azure CLI は、すべてのオペレーティング システムで使用でき、ほとんどの Azure サービスで動作します。

PowerShell を使用する場合は、[Azure PowerShell モジュール](/powershell/azure)を使用することもできます。

## <a name="az-webapp-az-webapp-up"></a>az webapp、az webapp up

**az webapp** コマンドは、Azure CLI を通じて Azure App Service のすべての側面を操作する方法です。

**az webapp up** コマンドは、特に Web アプリケーションのデプロイを簡略化します。 この単一のコマンドで、リソース グループ、App Service プラン、App Service Web アプリのプロビジョニングと、その後のコードのアップロードをすべての 1 回の操作で行うことができます。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [クラウド開発の概要 >>>](cloud-development-overview.md)
