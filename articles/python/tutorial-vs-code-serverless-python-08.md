---
title: 手順 8:Azure Functions の Python コードで使用されるリソースをクリーンアップする
description: チュートリアルの手順 8、引き続き料金を請求されないように Azure リソースをクリーンアップする。
ms.topic: conceptual
ms.date: 09/17/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: ec97faa440dcfb79240b0805e44de7d3d3447bad
ms.sourcegitcommit: 050c898df76a1af5feffe99e392a073b8ac9c19c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2020
ms.locfileid: "92137211"
---
# <a name="8-clean-up-azure-resources-for-azure-functions"></a>8:Azure Functions の Azure リソースをクリーンアップする

[前の手順: ストレージ バインドを追加する](tutorial-vs-code-serverless-python-07.md)

この記事では、このチュートリアルで作成した Azure リソースを削除する方法について説明します。 Visual Studio Code で作成した Azure 関数アプリには、最小限のコストが発生する可能性があるリソースが含まれます。 (詳細については、[Functions の価格](https://azure.microsoft.com/pricing/details/functions/)に関するページを参照してください。)

リソースをクリーンアップする最善の方法は、このチュートリアルで使用されている個々のリソースをすべて含むリソース グループを削除することです。 リソースには、関数アプリ、ストレージ アカウント、および バッキング App Service プランが含まれています。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

Visual Studio Code では、 **[Azure: Functions]** エクスプローラーの Function App に **[Delete Function App]\(関数アプリの削除\)** コマンドというコンテキスト メニューがあります。 ただし、このコマンドによって削除されるのは関数アプリのみであり、他のリソースは配置されたままになるため、継続的なコストが発生する可能性があります。

## <a name="next-steps"></a>次のステップ

このチュートリアルを完了し、Azure Functions に Python コードをデプロイしました。おめでとうございます! これで、よりたくさんのサーバーレス関数を作成できる準備が整いました。

前述のように、Functions 拡張機能の詳細は GitHub リポジトリの [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions) で参照できます。 問題提起や投稿も歓迎いたします。

「[Azure Functions の概要](/azure/azure-functions/functions-overview)」では、使用できるさまざまなトリガーについて説明されています。

データ ストレージのほか、AI や機械学習のサービスなど、Python から使用できる Azure サービスの詳細については、[Azure Python デベロッパー センター](./index.yml)にアクセスしてください。

Visual Studio Code 用の Azure 拡張機能は他にもあります。役に立つ機能が見つかるかもしれません。 拡張機能のエクスプローラーで、"Azure" で検索してください。

![Visual Studio Code 用の Azure 拡張機能](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

人気の拡張機能の一部:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。 [App Service へのデプロイに関するチュートリアル](tutorial-deploy-app-service-on-linux-01.md)を参照してください。
- [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure リソース マネージャー ツール](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [完了しました。](/python/azure/?preserve-view=true&view=azure-python)

[問題がある場合は、お知らせください。](https://aka.ms/python-functions-qs-ms-survey)
