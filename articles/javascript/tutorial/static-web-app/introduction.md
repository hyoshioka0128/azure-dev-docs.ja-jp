---
title: 概要と前提条件
description: GitHub アクションを使用して、React クライアント アプリケーションをローカルで構築し、Azure Static Web アプリにデプロイします。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 1a034d2746fae453019325d01f20c7073a6ce9a3
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2020
ms.locfileid: "96559257"
---
# <a name="1-build-and-deploy-a-static-web-app-to-azure"></a>1.静的 Webアプリを構築して Azure にデプロイする

このチュートリアルでは、GitHub アクションを使用して、React クライアント アプリケーションをローカルで構築し、Azure Static Web アプリにデプロイします。 

React (create-react-app) は次の機能を提供します。 
* Cognitive Services Computer Vision の Azure キーとエンドポイントが見つからない場合にメッセージを表示します
* Cognitive Services Computer Vision を使用してイメージを分析できるようにします
    * パブリック イメージの URL を入力するか、コレクション内のイメージを分析します
    * 分析が完了したとき
        * 表示する画像
        * Computer Vision JSON の結果を表示する 

特定のブランチへのプッシュが発生したときに、GitHub アクションが開始されます。
* Computer Vision キーとエンドポイントの GitHub シークレットをビルドに挿入します
* React (create-react-app) クライアントを構築します
* 生成されたファイルを Azure Static Web アプリ リソースに移動します

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="what-is-an-azure-static-web-app"></a>Azure Static Web アプリとは

静的 Web アプリを構築する場合、Azure ではユーザーが関心を持つ機能と制御のレベルによって、いくつかの選択肢があります。 このチュートリアルでは、最も簡単なサービスについて重点的に説明します。選択の多くが自動的に行われるため、ユーザーはホスティング環境ではなくフロントエンド コードに集中できます。

## <a name="prerequisites"></a>前提条件

- [Azure CLI をインストールする](/cli/azure/install-azure-cli)か、[Azure Cloud Shell](https://shell.azure.com) を使用します
- [Node.js および npm](https://nodejs.org/en/download) - ローカル コンピューターにインストール済み。
- [Visual Studio Code](https://code.visualstudio.com/) - ローカル コンピューターにインストール済み。 
    - [Azure Static Web Apps (プレビュー)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps) - React アプリを Azure Static Web アプリにデプロイするために使用されます。
- [Git](https://git-scm.com/downloads) - GitHub にプッシュするために使用されます。これにより GitHub アクションがアクティブになります。
- [GitHub アカウント](https://github.com/join) - フォークしてリポジトリにプッシュするため

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [React Cognitive Services Image Analyzer アプリをダウンロードしてローカルで実行する](run-the-react-cognitive-services-image-analyzer-app-locally.md) 