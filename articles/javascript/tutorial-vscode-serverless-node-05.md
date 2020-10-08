---
title: Azure Functions アプリケーションをデプロイした後にリソースをクリーンアップする
description: 'サーバーレス - リソースをクリーンアップするには、Azure: Functions エクスプローラーで Function アプリを右クリックし、\関数アプリの削除\ を選択します。'
ms.topic: tutorial
ms.date: 08/31/2020
ms.custom: devx-track-js
ms.openlocfilehash: 30ce0f0f5d79c6fd7a69172fbdfbeb27d12c7a81
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364575"
---
# <a name="tutorial-for-azure-functions-clean-up-resources"></a>Azure Functions のチュートリアル: リソースをクリーンアップする

[前の手順:Functions アプリをデプロイする](tutorial-vscode-serverless-node-04.md)

作成した Functions アプリには、最小限のコストが発生する可能性があるリソースが含まれます ([Functions の価格](https://azure.microsoft.com/pricing/details/functions/)に関するページを参照してください)。 リソースをクリーンアップするには、**Azure:Functions** エクスプローラーで Function App を右クリックし、 **[Delete Function App]\(関数アプリの削除\)** を選択します。

[Azure portal](https://portal.azure.com) にアクセスし、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用することもできます。

[!INCLUDE [Next steps for using VSCode extensions](includes/tutorial-next-steps-vscode-extensions.md)]

[!INCLUDE [Next steps for using JavaScript on Azure](includes/tutorial-next-steps-js-azure.md)]

## <a name="learn-more-about-azure-functions"></a>Azure Functions の詳細について

* [Azure Functions 開発者ガイド](/azure/azure-functions/functions-reference)
* [Azure Functions の JavaScript 開発者向けガイド](/azure/azure-functions/functions-reference-node)
* [Azure Functions のセキュリティ保護](/azure/azure-functions/security-concepts)
* [ストレージ](/azure/azure-functions/storage-considerations)と[パフォーマンス](/azure/azure-functions/functions-best-practices)の考慮事項

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [終わりました](node-howto-write-serverless-code.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=clean-up-resources)