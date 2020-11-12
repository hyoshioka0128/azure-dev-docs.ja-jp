---
title: コンテナー化された Node.js アプリを Visual Studio Code からデプロイした後にリソースをクリーンアップする
description: Docker チュートリアル パート 8、リソースをクリーンアップする
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: devx-track-js
ms.openlocfilehash: c50934c303b314b000ab5006a12f94009d417429
ms.sourcegitcommit: cbcde17e91e7262a596d813243fd713ce5e97d06
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "93405981"
---
# <a name="part-8-clean-up-resources"></a>パート 8 リソースをクリーンアップする

[前の手順:ログのストリーミング](tutorial-vscode-docker-node-07.md)

コンテナー用に作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、 **[Azure: App Service]** エクスプローラーで [App Service] を右クリックし、 **[削除]** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

### <a name="next-steps"></a>次のステップ

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [終わりました](./how-to/deploy-containers.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=clean-up-resources)