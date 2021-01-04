---
title: Computer Vision を使用する React コード
description: このサンプルには、Computer Vision を React アプリに追加するために必要なすべてのコードが含まれています。 チュートリアルのこのセクションでは、手順とコードを "_確認_" します。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 44999c92ef2a938327a379d0d4993b000466fb3f
ms.sourcegitcommit: 525c4b41d85aae9c3026a070b07e00c2241ea716
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97394066"
---
# <a name="5-review-how-to-add-computer-vision-to-the-react-app"></a>5.Computer Vision を React アプリに追加する方法を確認する

このサンプルには、Computer Vision を React アプリに追加するために必要なすべての TypeScript コードが含まれています。 チュートリアルのこのセクションでは、手順とコードを "_確認_" します。 このチュートリアルでは、こちらの手順を実行する必要はありません。 

## <a name="add-computer-vision-to-local-react-app"></a>Computer Vision をローカルの React アプリに追加する

npm を使用して、Computer Vision を package.json ファイルに追加します。 

```bash
npm install @azure/cognitiveservices-computervision 
```

## <a name="add-computer-vision-code-as-separate-module"></a>Computer Vision コードを別のモジュールとして追加する

Computer Vision コードは、`./src/VisualAI.js` という名前の別のファイルに含まれています。 このモジュールの主要な関数が強調表示されています。 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/VisualAI.js" highlight="55-75" :::

## <a name="add-catalog-of-images-as-separate-module"></a>イメージのカタログを個別のモジュールとして追加する

ユーザーがイメージの URL を入力しない場合、カタログからランダムなイメージが選択されます。 ランダム選択関数が強調表示されています 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/DefaultImages.js" highlight="33-35" :::

## <a name="add-custom-computer-vision-module-to-react-app"></a>カスタムの Computer Vision モジュールを React アプリに追加する

メソッドを React `app.js` に追加します。 イメージの分析と結果の表示が強調表示されています。

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/App.js" highlight="20-25, 29-42" :::

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [リソースをクリーンアップする](clean-up-resources.md) 