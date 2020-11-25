---
title: Azure Monitor リソースの作成
description: すべての Azure リソース用の Azure リソース グループと、Web アプリのログ ファイルを Azure クラウドに収集するための Monitor リソースを作成します。 Azure Monitor は Azure サービスの名前ですが、Application Insights はこのチュートリアルで使用するクライアント ライブラリの名前です。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: b401953c0e1c972efa0f5d90817f461b858cf04b
ms.sourcegitcommit: a2a51e0c6530eb5794a2fe667cf4c9a60b2a7470
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625041"
---
# <a name="2-create-application-insights-resource-for-web-pages"></a>2.Web ページ用の Application Insights リソースを作成する

チュートリアルのこのステップでは、すべての Azure リソース用の Azure リソース グループと、Web アプリのログ ファイルを Azure クラウドに収集するための Monitor リソースを作成します。 Azure Monitor は Azure サービスの名前ですが、Application Insights はこのチュートリアルで使用するクライアント ライブラリの名前です。 

## <a name="create-a-resource-group-for-your-virtual-machine-resources"></a>仮想マシン リソース用のリソース グループを作成する

このチュートリアルにはいくつかの Azure リソースが含まれています。 リソース グループを作成することにより、リソースを見つけやすくなり、完了時に削除しやすくなります。

1. ターミナルまたは bash シェルで、[Azure リソース グループを作成する Azure CLI コマンドを入力](/cli/azure/group?view=azure-cli-latest#az_group_create)して、`rg-demo-vm-eastus` という名前を指定します。

    ```azurecli
    az group create \
        --location eastus \
        --name rg-demo-vm-eastus 
    ```

## <a name="create-azure-monitor-resource-with-azure-cli"></a>Azure CLI を使用して Azure Monitor リソースを作成する

1. Azure CLI に Application Insights 拡張機能をインストールします。

    ```azurecli
    az extension add -n application-insights
    ```

1. 次のコマンドを使用して、[監視リソースを作成します](/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest#ext_application_insights_az_monitor_app_insights_component_create)。


    ```azurecli
    az monitor app-insights component create \
      --app demoWebAppMonitor \
      --location eastus \
      --resource-group rg-demo-vm-eastus
    ```

    その結果から `instrumentationKey` を見つけてコピーします。 この情報は後で必要になります。 

1. このターミナルは開いたままにしてください。次のステップで使用します。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Linux 仮想マシンの作成](create-linux-virtual-machine-azure-cli.md) 
