---
title: Computer Vision を使用する React コード
description: チュートリアルのこのセクションでは、手順とコードを "_確認_" します。 このチュートリアルでは、こちらの手順を実行する必要はありません。
ms.topic: tutorial
ms.date: 12/17/2020
ms.custom: devx-track-js
ms.openlocfilehash: 84140472c4bb57e208cc0e2c0665e72680664a2f
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687486"
---
# <a name="6-review-how-to-add-computer-vision-to-the-react-app"></a>6.Computer Vision を React アプリに追加する方法を確認する

このサンプルには、Computer Vision を React アプリに追加するために必要なすべての TypeScript コードが含まれています。 チュートリアルのこのセクションでは、手順とコードを "_確認_" します。 このチュートリアルでは、こちらの手順を実行する必要はありません。 

* [サンプル コード](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)
* Azure サービス
    * [静的 Web アプリ](https://docs.microsoft.com/azure/static-web-apps)
    * [Cognitive Services の Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="add-computer-vision-to-local-react-app"></a>Computer Vision をローカルの React アプリに追加する

npm を使用して、Computer Vision を package.json ファイルに追加します。 

```bash
npm install @azure/cognitiveservices-computervision 
```

## <a name="add-computer-vision-code-as-separate-module"></a>Computer Vision コードを別のモジュールとして追加する

Computer Vision コードは、`./src/azure-cognitiveservices-computervision.js` という名前の別のファイルに含まれています。 このモジュールの主要な関数が強調表示されています。 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/azure-cognitiveservices-computervision.js" highlight="55-75" :::

## <a name="add-catalog-of-images-as-separate-module"></a>イメージのカタログを個別のモジュールとして追加する

ユーザーがイメージの URL を入力しない場合、カタログからランダムなイメージが選択されます。 ランダム選択関数が強調表示されています 

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/DefaultImages.js" highlight="33-35" :::

## <a name="add-custom-computer-vision-module-to-react-app"></a>カスタムの Computer Vision モジュールを React アプリに追加する

メソッドを React `app.js` に追加します。 イメージの分析と結果の表示が強調表示されています。

:::code language="javascript" source="~/../js-e2e-client-cognitive-services/src/App.js" highlight="20-25, 29-42" :::

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [リソースをクリーンアップする](clean-up-resources.md) 