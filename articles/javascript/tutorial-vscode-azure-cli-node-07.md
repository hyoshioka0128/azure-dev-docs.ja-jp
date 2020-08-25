---
title: Azure CLI を使用して Azure に Node.js アプリをデプロイした後にリソースをクリーンアップする
description: チュートリアル パート 7、リソースをクリーンアップする
ms.topic: conceptual
ms.date: 09/24/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: fb78f369d80ff40fef1d4a21df68ea79fafa2dbc
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218529"
---
# <a name="part-7-clean-up-resources"></a>パート 7: リソースをクリーンアップする

[前の手順:変更を加えて再デプロイする](tutorial-vscode-docker-node-06.md)

作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、ターミナルまたはコマンド プロンプトで次のコマンドを実行します。

```azurecli
az group delete --name myResourceGroup
```

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

> [!div class="nextstepaction"]
> [終わりました](node-howto-deploy-web-app.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
