---
title: Azure App Service on Linux にデプロイしたリソースを Visual Studio Code を使用してクリーンアップします。
description: チュートリアルの手順 7、Azure のリソースをクリーンアップする
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: a86207477deb4652ef1d98c1130329bc3e0c91fc
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019610"
---
# <a name="clean-up-resources"></a>リソースのクリーンアップ

[前の手順: ログをストリーム配信する](tutorial-deploy-app-service-on-linux-06.md)

作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、 **[Azure: App Service]** エクスプローラーで [App Service] を右クリックし、 **[削除]** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

## <a name="next-steps"></a>次の手順

おつかれさまです。このチュートリアルはこれで完了です。App Service on Linux に Python コードをデプロイしました。

前述のように、App Service 拡張機能の詳細は GitHub リポジトリの [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice) で参照できます。 問題提起や投稿も歓迎いたします。

データ ストレージのほか、AI や機械学習のサービスなど、Python から使用できる Azure サービスの詳細については、[Azure Python デベロッパー センター](https://docs.microsoft.com/python/azure/?view=azure-python)にアクセスしてください。

VS Code 用の Azure 拡張機能は他にもあります。役に立つ機能が見つかるかもしれません。 拡張機能のエクスプローラーで、"Azure" で検索してください。

![VS Code 用の Azure 拡張機能](media/deploy-containers/azure-extensions.png)

人気の拡張機能の一部:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager (ARM) Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [終わりました](https://docs.microsoft.com/python/azure/?view=azure-python) 

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=07-clean-up-resources)
