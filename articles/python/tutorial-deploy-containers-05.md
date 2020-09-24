---
title: 手順 5:Azure リソースをクリーンアップする
description: チュートリアルの手順 5、引き続き料金を請求されないように Azure リソースをクリーンアップする。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 9140f5b695fe4b20e1e9417fe1d5e6ad115bb4eb
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831418"
---
# <a name="5-clean-up-azure-resources"></a>5:Azure リソースをクリーンアップする

[前の手順: ログをストリーム配信する](tutorial-deploy-containers-04.md)

このチュートリアルで作成した Azure リソースには、継続的なコストが発生する可能性があります。 そのようなコストを回避するには、これらのすべてのリソースを含むリソース グループを削除します。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>次のステップ

Docker と App Service の拡張機能の詳細については、GitHub のそれぞれのリポジトリ ([vscode-docker](https://github.com/Microsoft/vscode-docker) と [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice)) を参照してください。 問題提起や投稿も歓迎いたします。

データ ストレージのほか、AI や機械学習のサービスなど、Python から使用できる Azure サービスの詳細については、[Azure Python デベロッパー センター](/python/azure/?preserve-view=true&view=azure-python)にアクセスしてください。

VS Code 用の Azure 拡張機能は他にもあります。役に立つ機能が見つかるかもしれません。 拡張機能のエクスプローラーで、"Azure" で検索してください。

![VS Code 用の Azure 拡張機能](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

人気の拡張機能の一部:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure リソース マネージャー ツール](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [完了しました。](/python/azure/?preserve-view=true&view=azure-python)