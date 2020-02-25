---
title: 手順 5:Azure リソースをクリーンアップする
description: チュートリアルの手順 5、引き続き料金を請求されないように Azure リソースをクリーンアップする。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: df785e68de26fe4414430289800fdabfa8757eef
ms.sourcegitcommit: 44d1abfb836f90b8731d7ea5d5a5af09245b2b89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2020
ms.locfileid: "77422231"
---
# <a name="5-clean-up-azure-resources"></a>5:Azure リソースをクリーンアップする

[前の手順: ログをストリーム配信する](tutorial-deploy-containers-04.md)

このチュートリアルで作成した Azure リソースには、継続的なコストが発生する可能性があります。 そのようなコストを回避するには、これらのすべてのリソースを含むリソース グループを削除します。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>次のステップ

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
> [完了しました。](https://docs.microsoft.com/python/azure/?view=azure-python)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=07-clean-up-resources)
