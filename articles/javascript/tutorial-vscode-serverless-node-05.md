---
title: Azure Functions アプリケーションを Azure にデプロイした後にリソースをクリーンアップする
description: チュートリアル パート 5、リソースをクリーンアップする
ms.topic: conceptual
ms.date: 09/23/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: 67745cdc9ac27b6f8c1bdfbd6f4dbaff0f39ce4a
ms.sourcegitcommit: 815cf2acff71e849735f7afce54723f03ffa5df3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/17/2020
ms.locfileid: "88501494"
---
# <a name="clean-up-resources-after-deploying-an-azure-functions-app-with-visual-studio-code"></a>Visual Studio Code を使用して Azure Functions アプリをデプロイした後にリソースをクリーンアップする

[前の手順:Functions アプリをデプロイする](tutorial-vscode-serverless-node-04.md)

作成した Functions アプリには、最小限のコストが発生する可能性があるリソースが含まれます ([Functions の価格](https://azure.microsoft.com/pricing/details/functions/)に関するページを参照してください)。 リソースをクリーンアップするには、**Azure:Functions** エクスプローラーで Function App を右クリックし、 **[Delete Function App]\(Function App の削除\)** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

## <a name="next-steps"></a>次のステップ

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [終わりました](node-howto-write-serverless-code.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=clean-up-resources)
