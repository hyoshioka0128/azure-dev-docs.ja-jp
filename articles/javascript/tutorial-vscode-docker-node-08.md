---
title: コンテナー化された Node.js アプリを Visual Studio Code からデプロイした後にリソースをクリーンアップする
description: 'チュートリアル パート 8: リソースをクリーンアップする'
ms.topic: conceptual
ms.date: 09/20/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: d05766092022772c96fa745f9a52aa4901b27f82
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "90772705"
---
# <a name="part-8-clean-up-resources"></a>パート 8 リソースをクリーンアップする

[前の手順:ログのストリーミング](tutorial-vscode-docker-node-07.md)

コンテナー用に作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、 **[Azure: App Service]** エクスプローラーで [App Service] を右クリックし、 **[削除]** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

### <a name="next-steps"></a>次のステップ

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [終わりました](node-howto-deploy-containers.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=clean-up-resources)
