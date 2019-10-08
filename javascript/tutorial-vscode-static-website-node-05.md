---
title: Visual Studio Code から静的 Node.js Web サイトに変更をデプロイする
description: チュートリアル パート 5、変更を加えて再デプロイする
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 986d2a0f8999d79dfd1d856ed20a053c495a3765
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685939"
---
# <a name="make-changes-and-redeploy"></a>変更を加えて再デプロイする

[前の手順: Azure Storage にデプロイする](tutorial-vscode-static-website-node-04.md)

この手順では、アプリのソース コードに簡単な変更を行ってサイトを再デプロイして、エンドツーエンドのデプロイ ワークフローを体験します。

1. Visual Studio Code で *src/app.js* ファイルを開き、次に一致するように 11 行目を変更します。

    ```js
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. ターミナルまたはコマンド プロンプトで `npm run build` を実行します。

1. VS Code で、更新した *build* フォルダーを右クリックし、もう一度 **[Deploy to Static Website]\(静的な Web サイトにデプロイする\)** を選択します。 Storage アカウントを選択し、変更をデプロイすることを確認します。 (Azure 拡張機能では、キャッシュの問題を回避するために、変更をデプロイする前に古いファイルが自動的に削除されます。)

1. デプロイが完了したら、ブラウザーでサイトを更新して変更を確認します。

    ![再デプロイ後のアプリの変更](media/static-website/updated-azure-app.png)

> [!div class="nextstepaction"]
> [変更をデプロイしました](tutorial-vscode-static-website-node-06.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
