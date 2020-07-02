---
title: Visual Studio Code から静的 Node.js Web サイト用の Azure Storage アカウントを作成する
description: チュートリアル パート 3、Azure Storage アカウントを作成する
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 4adca67f850497777abce7d550e39532e59257d9
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2020
ms.locfileid: "85792722"
---
# <a name="create-an-azure-storage-account"></a>Azure Storage アカウントの作成

[前の手順:アプリケーションの作成](tutorial-vscode-static-website-node-02.md)

この手順では、組み込み Web サーバーを使った単純なファイル ストア (CDN) として機能する Azure Storage アカウントを作成します。 この組み込みサーバーにより、Azure Storage は、静的サイトを素早くホストするための最適な選択肢になります。

1. 前の手順で作成された `my-static-app` フォルダーから Visual Studio Code を開始します。それにより、そのフォルダーが自動的に開かれます。

    ```bash
    code .
    ```

1. VS Code で、Azure ロゴを選択して **Azure** エクスプローラーを開きます。 **Azure Storage** の下で Azure サブスクリプションを右クリックし、 **[ストレージ アカウントの作成]** を選択します。

    ![VS Code で Storage アカウントを作成する](media/static-website/create-storage-account.png)

1. [Enter the name of the new storage account]\(新しいストレージ アカウントの名前を入力してください\) というプロンプトでは、Storage アカウントのグローバルに一意の名前を入力して、Enter キーを押します。 アプリの名前として有効な文字は、'a-z' と '0-9' です。

    > [!NOTE]
    > これにより、ストレージ アカウントと、同じ名前のリソース グループが作成されます。 このストレージ アカウントは、米国西部に自動的に配置されます。 リソース グループと場所を指定する場合は、コンテキスト メニューの [ストレージ アカウントの作成 (詳細)] オプションを選択します。

1. Storage アカウントが作成されている間、VS Code の **[出力]** パネルに進行状況が表示されます。

    ![VS Code の出力ウィンドウ ](media/static-website/output-storage.png)

1. Storage アカウントが完了すると、ストレージ アカウントに対して静的な Web サイトのホスティングが有効になっていることを示すメッセージが表示されます。

    ![ストレージ アカウントの作成](media/static-website/static-website-enabled-notification.png)

    > [!IMPORTANT]
    > エラー ドキュメントには *index.html* を使用します。これは、React、Angular、および Vue などの最新のシングルページ アプリケーション (SPA) ではクライアントでルート エラーが処理されるためです。 従来の静的 Web サイトには、カスタム 404 エラー ページを使用します。

> [!div class="nextstepaction"]
> [ストレージ コンテナーを作成しました](tutorial-vscode-static-website-node-04.md)[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
