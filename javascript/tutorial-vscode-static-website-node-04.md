---
title: Visual Studio Code から静的 Node.js アプリ ファイルを Azure Storage にデプロイする
description: チュートリアル パート 4、Azure Storage にファイルをデプロイする
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 53578d829167512877c2359d02c3e70d88afe77f
ms.sourcegitcommit: aceed8548ad4529a81d83eb15a095edc8607cac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/18/2020
ms.locfileid: "77440899"
---
# <a name="deploy-the-website-to-azure-storage"></a>Azure Storage に Web サイトをデプロイする

[前の手順:Storage アカウントの作成](tutorial-vscode-static-website-node-03.md)

この手順では、Visual Studio Code を使用して、前の手順で作成した静的 Web サイトのファイルを Azure Storage にデプロイします。ファイルはそこでホストされて提供されます。

# <a name="angular"></a>[Angular](#tab/angular)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![Azure Storage エクスプローラー内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_dist/my-static-app_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-angular.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![デプロイ完了メッセージ](media/static-website/deployment-complete.png)

    ![Azure で実行されている静的 Web サイト](media/static-website/azure-app-angular.png)

# <a name="react"></a>[React](#tab/react)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![Azure Storage エクスプローラー内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_build_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-react.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![デプロイ完了メッセージ](media/static-website/deployment-complete.png)

    ![Azure で実行されている静的 Web サイト](media/static-website/azure-app-react.png)

# <a name="vue"></a>[Vue](#tab/vue)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![Azure Storage エクスプローラー内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_dist_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-vue.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![デプロイ完了メッセージ](media/static-website/deployment-complete.png)

    ![Azure で実行されている静的 Web サイト](media/static-website/azure-app-vue.png)

# <a name="svelte"></a>[Svelte](#tab/svelte)

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

   ![Azure Storage エクスプローラー内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、_public_ フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build-svelte.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![デプロイ完了メッセージ](media/static-website/deployment-complete-svelte.png)

    ![Azure で実行されている静的 Web サイト](media/static-website/azure-app-svelte.png)

---

> [!div class="nextstepaction"]
> [サイトは Azure にあります](tutorial-vscode-static-website-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
