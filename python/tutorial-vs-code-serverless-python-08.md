---
title: チュートリアル:Azure リソースをクリーンアップする - Python の Azure Functions
description: チュートリアルの手順 8、引き続き料金を請求されないように Azure リソースをクリーンアップする。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: aac220a57c83b274c46886f4548e41b41ec8327e
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172419"
---
# <a name="tutorial-clean-up-azure-resources-for-azure-functions"></a>チュートリアル:Azure Functions の Azure リソースをクリーンアップする

[前の手順: ストレージ バインドを追加する](tutorial-vs-code-serverless-python-07.md)

作成した Function App には、最小限のコストが発生する可能性があるリソースが含まれます ([Functions の価格](https://azure.microsoft.com/pricing/details/functions/)ページを参照してください)。 リソースをクリーンアップするには、**Azure:Functions** エクスプローラーで Function App を右クリックし、 **[Delete Function App]\(関数アプリの削除\)** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

## <a name="next-steps"></a>次の手順

このチュートリアルを完了し、Azure Functions に Python コードをデプロイしました。おめでとうございます! これで、よりたくさんのサーバーレス関数を作成できる準備が整いました。

前述のように、Functions 拡張機能の詳細は GitHub リポジトリの [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions) で参照できます。 問題提起や投稿も歓迎いたします。

「[Azure Functions の概要](/azure/azure-functions/functions-overview)」では、使用できるさまざまなトリガーについて説明されています。

データ ストレージのほか、AI や機械学習のサービスなど、Python から使用できる Azure サービスの詳細については、[Azure Python デベロッパー センター](/azure/python/?view=azure-python)にアクセスしてください。

Visual Studio Code 用の Azure 拡張機能は他にもあります。役に立つ機能が見つかるかもしれません。 拡張機能のエクスプローラーで、"Azure" で検索してください。

![Visual Studio Code 用の Azure 拡張機能](media/tutorial-vs-code-serverless-python/azure-extensions.png)

人気の拡張機能の一部:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。 [App Service へのデプロイに関するチュートリアル](tutorial-deploy-app-service-on-linux-01.md)を参照してください。
- [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure リソース マネージャー ツール](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [終わりました](https://docs.microsoft.com/python/azure/?view=azure-python)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
