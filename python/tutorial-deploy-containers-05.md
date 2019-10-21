---
title: チュートリアル:Azure リソースをクリーンアップする
description: チュートリアルの手順 5、引き続き料金を請求されないように Azure リソースをクリーンアップする。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 0a3e04759573769d1ed00e59a294caddfc4ef0cc
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278717"
---
# <a name="tutorial-clean-up-azure-resources"></a>チュートリアル:Azure リソースをクリーンアップする

[前の手順: ログをストリーム配信する](tutorial-deploy-containers-04.md)

この記事では、Visual Studio Code で Azure App Service にアプリをデプロイするときに作成した Azure リソースを削除する方法について説明します。

このチュートリアルで作成したさまざまな Azure リソースには、継続的なコストが発生する可能性があります。 それらをクリーンアップするには、[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウの **[リソース グループ]** を選択して、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することをお勧めします。

## <a name="next-steps"></a>次の手順

Docker と App Service の拡張機能の詳細については、GitHub のそれぞれのリポジトリ ([vscode-docker](https://github.com/Microsoft/vscode-docker) と [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice)) を参照してください。 問題提起や投稿も歓迎いたします。

データ ストレージのほか、AI や機械学習のサービスなど、Python から使用できる Azure サービスの詳細については、[Azure Python デベロッパー センター](https://docs.microsoft.com/python/azure/?view=azure-python)にアクセスしてください。

VS Code 用の Azure 拡張機能は他にもあります。役に立つ機能が見つかるかもしれません。 拡張機能のエクスプローラーで、"Azure" で検索してください。

![VS Code 用の Azure 拡張機能](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

人気の拡張機能の一部:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure リソース マネージャー ツール](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [終わりました](https://docs.microsoft.com/python/azure/?view=azure-python)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=07-clean-up-resources)
