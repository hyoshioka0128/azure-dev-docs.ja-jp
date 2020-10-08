---
title: 手順 7:Azure App Service on Linux にデプロイしたリソースを Visual Studio Code を使用してクリーンアップします。
description: チュートリアルの手順 7、Azure のリソースをクリーンアップする
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: cbecb9771a1b4dcc8460b7c7296ae06928930a34
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764753"
---
# <a name="7-clean-up-resources-after-deploying-to-azure-app-service-on-linux-from-visual-studio-code"></a>7:Azure App Service on Linux にデプロイしたリソースを Visual Studio Code を使用してクリーンアップします。

[前の手順: ログをストリーム配信する](tutorial-deploy-app-service-on-linux-06.md)

作成した Azure App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 そのようなコストを回避するには、これらのすべてのリソースをまとめて含むリソース グループを削除します。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>次のステップ

おつかれさまです。このチュートリアルはこれで完了です。App Service on Linux に Python コードをデプロイしました。

前述のように、App Service 拡張機能の詳細は GitHub リポジトリの [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice) で参照できます。 問題提起や投稿も歓迎いたします。

データ ストレージのほか、AI や機械学習のサービスなど、Python から使用できる Azure サービスの詳細については、[Azure Python デベロッパー センター](/python/azure/)にアクセスしてください。

VS Code 用の Azure 拡張機能は他にもあります。役に立つ機能が見つかるかもしれません。 拡張機能のエクスプローラーで、"Azure" で検索してください。

![Visual Studio Code 用の Azure 拡張機能](media/deploy-azure/azure-extensions-for-visual-studio-code.png)

人気の拡張機能の一部:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager (ARM) Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [完了しました。](/python/azure/) 

[問題がある場合は、お知らせください。](https://aka.ms/FlaskVSCQuickstartHelp)
