---
title: コンテナー化された Node.js アプリを Visual Studio Code からデプロイした後にリソースをクリーンアップする
description: チュートリアル パート 6、リソースをクリーンアップする
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 53fbfc2c3dc04ed30e940a680eb4c65de6591504
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686004"
---
# <a name="clean-up-resources"></a>リソースのクリーンアップ

[前の手順: ログをストリーミングする](tutorial-vscode-docker-node-05.md)

コンテナー用に作成した App Service には、バッキング App Service プランが含まれていて料金が発生する可能性があります。 リソースをクリーンアップするには、 **[Azure: App Service]** エクスプローラーで [App Service] を右クリックし、 **[削除]** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

## <a name="next-steps"></a>次の手順

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [終わりました](node-howto-deploy-containers.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=clean-up-resources)
