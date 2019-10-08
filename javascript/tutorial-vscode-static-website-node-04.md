---
title: Visual Studio Code から静的 Node.js アプリ ファイルを Azure Storage にデプロイする
description: チュートリアル パート 4、Azure Storage にファイルをデプロイする
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: b1295fcb9a90ca26880715296e4214c2a1df6a07
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685944"
---
# <a name="deploy-the-website-to-azure-storage"></a>Azure Storage に Web サイトをデプロイする

[前の手順: Storage アカウントを作成する](tutorial-vscode-static-website-node-03.md)

この手順では、Visual Studio Code を使用して、前の手順で作成した静的 Web サイトのファイルを Azure Storage にデプロイします。ファイルはそこでホストされて提供されます。

1. Visual Studio Code で、**Azure Storage** エクスプローラーに移動し、サブスクリプションを展開し、前の手順で作成した Azure Storage アカウントのノードを展開してから、 **[BLOB コンテナー]** ノードを展開します。 `$web` コンテナーは、アプリ コードをデプロイする場所です。

    ![Azure Storage エクスプローラー内の Azure Storage ノード](media/static-website/storage-nodes.png)

1. **ファイル** エクスプローラーを選択し、*build* フォルダーを右クリックし、 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。

    ![Deploy to Static Website (静的な Web サイトにデプロイする) コマンド](media/static-website/deploy-build.png)

1. メッセージが表示されたら、前に作成した Stoage アカウントを選択します。

1. デプロイが完了すると、 **[Browse to website]\(Web サイトを参照\)** ボタンを含むメッセージが表示されます。 そのボタンを選択すると、デプロイしたアプリ コードの最初のエンドポイントが開きます。

    ![デプロイ完了メッセージ](media/static-website/deployment-complete.png)

    ![Azure で実行されている静的 Web サイト](media/static-website/azure-app.png)

> [!div class="nextstepaction"]
> [サイトは Azure にあります](tutorial-vscode-static-website-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
