---
title: Visual Studio Code から静的 Node.js アプリ ファイルを Azure Storage にデプロイする
description: 静的 Web アプリのチュートリアル パート 4、Azure Storage にファイルをデプロイする
ms.topic: tutorial
ms.date: 09/24/2019
ms.author: buhollan
ms.custom: devx-track-js
ms.openlocfilehash: e7c1ca3390f8161c7323103868c591e8c02f539e
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365295"
---
# <a name="deploy-the-website-to-azure-storage"></a>Azure Storage に Web サイトをデプロイする

[前の手順:Storage アカウントの作成](tutorial-vscode-static-website-node-03.md)

この手順では、Visual Studio Code を使用して、前の手順で作成した静的 Web サイトのファイルを Azure Storage にデプロイします。ファイルはそこでホストされて提供されます。

# <a name="angular"></a>[Angular](#tab/angular)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![Azure Storage エクスプローラー内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_dist/my-static-app_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Angular - Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-angular.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![Angular - デプロイ完了メッセージ](media/static-website/deployment-complete.png)

    ![Angular - Azure で実行されている静的 Web サイト](media/static-website/azure-app-angular.png)

# <a name="react"></a>[React](#tab/react)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![React - Azure Storage Explorer 内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_build_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![React - Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-react.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![React - デプロイ完了メッセージ](media/static-website/deployment-complete.png)

    ![React - Azure で実行されている静的 Web サイト](media/static-website/azure-app-react.png)

# <a name="vue"></a>[Vue](#tab/vue)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![Vue - Azure Storage Explorer 内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_dist_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Vue - Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-vue.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![Vue - デプロイ完了メッセージ](media/static-website/deployment-complete.png)

    ![Vue - Azure で実行されている静的 Web サイト](media/static-website/azure-app-vue.png)

# <a name="svelte"></a>[Svelte](#tab/svelte)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![Svelte - Azure Storage Explorer 内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_public_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Svelte - Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-svelte.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![Svelte - デプロイ完了メッセージ](media/static-website/deployment-complete-svelte.png)

    ![Svelte - Azure で実行されている静的 Web サイト](media/static-website/azure-app-svelte.png)

---

> [!div class="nextstepaction"]
> [サイトは Azure にあります](tutorial-vscode-static-website-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
