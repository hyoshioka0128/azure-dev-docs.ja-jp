---
title: Azure CLI を使用して Azure に Node.js アプリをデプロイした後にリソースをクリーンアップする
description: チュートリアル パート 7、リソースをクリーンアップする
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 7998eb641090b252455613a46ae41e45e5cd1c1d
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466751"
---
# <a name="clean-up-resources"></a>リソースのクリーンアップ

[前の手順:変更を加えて再デプロイする](tutorial-vscode-docker-node-06.md)

作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、ターミナルまたはコマンド プロンプトで次のコマンドを実行します。

```bash
az group delete --name myResourceGroup
```

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

> [!div class="nextstepaction"]
> [終わりました](node-howto-deploy-web-app.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
