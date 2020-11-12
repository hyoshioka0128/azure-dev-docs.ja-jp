---
title: Azure CLI を使用して Azure に Node.js アプリをデプロイした後にリソースをクリーンアップする
description: チュートリアル パート 7、 Azure CLI で ソースをクリーンアップする
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 0c3404067f6324a37d7863568e715b806511a226
ms.sourcegitcommit: cbcde17e91e7262a596d813243fd713ce5e97d06
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "93405991"
---
# <a name="part-7-clean-up-resources"></a>パート 7: リソースをクリーンアップする

[前の手順:変更を加えて再デプロイする](tutorial-vscode-docker-node-06.md)

作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、ターミナルまたはコマンド プロンプトで次のコマンドを実行します。

```azurecli
az group delete --name myResourceGroup
```

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

> [!div class="nextstepaction"]
> [終わりました](./how-to/deploy-web-app.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)