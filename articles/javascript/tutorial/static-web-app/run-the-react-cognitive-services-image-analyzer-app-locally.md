---
title: サンプル React アプリをダウンロードする
description: サンプル アプリ一式が GitHub リポジトリに用意されています。 リポジトリをフォークし、依存関係をインストールして、ローカルで実行します。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5050aca4c34f20f8b4c7cb7a8460815516abd7c7
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687453"
---
# <a name="3-download-and-run-the-react-cognitive-services-image-analyzer-app"></a>3.React Cognitive Services Image Analyzer アプリをダウンロードして実行する

サンプル アプリ一式が GitHub リポジトリに用意されています。 リポジトリをフォークし、依存関係をインストールして、ローカルで実行します。

## <a name="fork-the-sample-repo"></a>サンプル リポジトリをフォークする

変更のプッシュ先となる独自の GitHub リポジトリを作成するには、ローカル コンピューターに単純に複製する代わりにリポジトリをフォークします。 

1. 別のブラウザー ウィンドウまたはタブを開き、<a href="https://github.com/login" target="_blank">GitHub</a> にサインインします。 
1. Web ブラウザーで <a href="https://github.com/Azure-Samples/js-e2e-client-cognitive-services" target="_blank">GitHub サンプル リポジトリ</a>に移動します。 

    ```http
    https://github.com/Azure-Samples/js-e2e-client-cognitive-services
    ```

1. ページの右上のセクションで、 **[Fork]\(フォーク\)** を選択します。 
1. **[Code]\(コード\)** を選択し、フォークの場所の URL をコピーします。 

    :::image type="content" source="../../media/static-web-app/browser-screenshot-clone-github-sample-repository-fork.png" alt-text="GitHub Web サイトの部分的なスクリーンショット。**Code** を選択し、フォークの場所をコピーします。":::    

## <a name="create-local-development-environment"></a>ローカル開発環境を作成する

1. ターミナルまたは bash ウィンドウで、フォークをローカル コンピューターに複製します。 `YOUR-ACCOUNT-NAME` を GitHub アカウント名に置き換えます。

    ```bash
    git clone https://github.com/YOUR-ACCOUNT-NAME/js-e2e-client-cognitive-services
    ```

1. 新しいディレクトリに移動し、依存関係をインストールします。 

    ```bash
    cd js-e2e-client-cognitive-services && npm install
    ```

## <a name="run-sample"></a>サンプルを実行する

1. サンプルを実行します。 

    ```bash
    npm start
    ```

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-cognitive-services-app-before-authentication.png" alt-text="キーとエンドポイントの設定前のイメージ分析のための React Cognitive Service Computer Vision サンプルの部分的なブラウザー スクリーンショット。":::    
    
1. アプリを停止します。 ターミナル ウィンドウを閉じるか、ターミナルで `control+c` を使用します。 
    
## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Computer Vision リソースを作成してコード内で使用する](create-computer-vision-resource-use-in-code.md) 