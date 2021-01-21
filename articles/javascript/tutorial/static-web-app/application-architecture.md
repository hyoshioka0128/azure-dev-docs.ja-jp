---
title: Computer Vision アプリのアーキテクチャ
description: チュートリアルのこのセクションでは、クライアント アプリとデプロイ プロセスについて説明します。
ms.topic: tutorial
ms.date: 12/17/2020
ms.custom: devx-track-js
ms.openlocfilehash: b8949b355ec30775dfacf5ea77db53edb595578c
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561648"
---
# <a name="2-application-architecture-for-static-web-app-with-computer-vision"></a>2.Computer Vision を使用した静的 Web アプリのアプリケーション アーキテクチャ

クライアント アプリケーションとデプロイ プロセスについて説明します。

## <a name="what-is-an-azure-static-web-app"></a>Azure Static Web アプリとは

静的 Web アプリを構築する場合、Azure ではユーザーが関心を持つ機能と制御のレベルによって、いくつかの選択肢があります。 このチュートリアルでは、最も簡単なサービスについて重点的に説明します。選択の多くが自動的に行われるため、ユーザーはホスティング環境ではなくフロントエンド コードに集中できます。

## <a name="client-application-architecture"></a>クライアント アプリケーションのアーキテクチャ

React (create-react-app) は次の機能を提供します。 
* Cognitive Services [**Computer Vision**](/azure/cognitive-services/computer-vision/) の Azure キーとエンドポイントが見つからない場合にメッセージを表示します
* Cognitive Services Computer Vision を使用してイメージを分析できるようにします
    * パブリック イメージの URL を入力するか、コレクション内のイメージを分析します
    * 分析が完了したとき
        * 表示する画像
        * Computer Vision JSON の結果を表示する 

:::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-image-analysis-result.png" alt-text="React Cognitive Service Computer Vision サンプル結果の部分的なブラウザー スクリーンショット。":::

## <a name="deploy-to-azure-with-github-action"></a>GitHub アクションを使用して Azure にデプロイする

特定のブランチへのプッシュが発生したときに、GitHub アクションが開始されます。
* Computer Vision キーとエンドポイントの GitHub シークレットをビルドに挿入します
* React (create-react-app) クライアントを構築します
* 生成されたファイルを Azure [**静的 Web アプリ**](/azure/static-web-apps) リソースに移動します

> [!div class="nextstepaction"]
> [React Cognitive Services Image Analyzer アプリをダウンロードしてローカルで実行する](run-the-react-cognitive-services-image-analyzer-app-locally.md)