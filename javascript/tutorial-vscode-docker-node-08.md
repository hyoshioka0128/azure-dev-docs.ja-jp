---
title: コンテナー化された Node.js アプリを Visual Studio Code からデプロイした後にリソースをクリーンアップする
description: 'チュートリアル パート 8: リソースをクリーンアップする'
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: dd71ec5affdd1696544ec166ad8517b376958cab
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2020
ms.locfileid: "80741640"
---
# <a name="clean-up-resources"></a>リソースをクリーンアップする

[前の手順:ログのストリーミング](tutorial-vscode-docker-node-07.md)

コンテナー用に作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、 **[Azure: App Service]** エクスプローラーで [App Service] を右クリックし、 **[削除]** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

### <a name="next-steps"></a>次のステップ

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [終わりました](node-howto-deploy-containers.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=clean-up-resources)
