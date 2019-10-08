---
title: Visual Studio Code から静的 Node.js Web サイト用の Azure Storage アカウントを作成する
description: チュートリアル パート 3、Azure Storage アカウントを作成する
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 44e6479379fff3ddf1012cdb61cf73440cad346e
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685966"
---
# <a name="create-an-azure-storage-account"></a>Azure Storage アカウントの作成

[前の手順: アプリを作成する](tutorial-vscode-static-website-node-02.md)

この手順では、組み込み Web サーバーを使った単純なファイル ストア (CDN) として機能する Azure Storage アカウントを作成します。 この組み込みサーバーにより、Azure Storage は、静的サイトを素早くホストするための最適な選択肢になります。

1. 前の手順で作成された `my-react-app` フォルダーから Visual Studio Code を開始します。それにより、そのフォルダーが自動的に開かれます。

    ```bash
    code .
    ```

1. VS Code で、Azure ロゴを選択して **Azure** エクスプローラーを開きます。 **Azure Storage** の下で Azure サブスクリプションを右クリックし、 **[ストレージ アカウントの作成]** を選択します。

    ![VS Code で Storage アカウントを作成する](media/static-website/create-storage-account.png)

1. [Enter the name of the new storage account]\(新しいストレージ アカウントの名前を入力してください\) というプロンプトでは、Storage アカウントのグローバルに一意の名前を入力して、Enter キーを押します。 アプリの名前として有効な文字は、'a-z' と '0-9' です。

1. [Select a resource group]\(リソース グループを選択してください\) というプロンプトでは、 **[Create a new Resource Group]\(新しいリソース グループを作成する\)** を選択し、既定の名前をそのまま使用します。

1. [Select a location]\(場所を選択してください\) というプロンプトでは、近くの[リージョン](https://azure.microsoft.com/regions/)を選択します。

1. Storage アカウントが作成されている間、VS Code の **[出力]** パネルに進行状況が表示されます。

1. Storage アカウントが完成したら、そのアカウントを右クリックし、 **[Configure Static Website]\(静的 Web サイトの構成\)** を選択します。 静的 Web サイトのホスティングを有効にするとは、インデックス ドキュメントやその他の静的な資産が、Azure Storage によって自動的に提供されることを意味します。

    ![[ストレージ アカウントを作成]](media/static-website/configure-static-website.png)

1. プロンプトが表示されたら、インデックス ドキュメント名と 404 エラー ドキュメント名の両方に対して *index.html* と入力します。 エラー ドキュメントには *index.html* を使用します。これは、React、Angular、および Vue などの最新のシングルページ アプリケーション (SPA) ではクライアントでエラーが処理されるからです。 従来の静的 Web サイトには、カスタム 404 エラー ページを使用します。

> [!div class="nextstepaction"]
> [ストレージ コンテナーを作成しました](tutorial-vscode-static-website-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
