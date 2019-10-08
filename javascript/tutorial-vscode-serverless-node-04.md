---
title: Visual Studio Code から Azure Functions アプリケーションをデプロイする
description: チュートリアル パート 4、Functions アプリをクラウドにデプロイする。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 53d0dd11567084d42de71a0f737cf8b9f5fc5249
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685929"
---
# <a name="deploy-the-functions-app"></a>Functions アプリをデプロイする

[前の手順: 関数をローカルでテストする](tutorial-vscode-serverless-node-03.md)

1. VS Code で、Azure ロゴを選択して **Azure エクスプローラーを**を開き、次に、 **[Functions]\(関数\)** で青い上向き矢印を選択してアプリをデプロイします。

    ![Azure Functions にデプロイするコマンド](media/functions-extension/deploy-app.png)

    または、**コマンド パレット**を開き (**F1**)、「deploy to function app」と入力し、**Azure Functions:Deploy to Function App** コマンドを選択します。

1. **[Select Function App in Azure]\(Azure で関数アプリを選択してください\)** というプロンプトでは、 **[Create new Function app in Azure]\(Azure で新しい関数アプリを作成する\)** を選択します。

1. 次のプロンプトでは、関数アプリのグローバルに一意の名前を入力して、**Enter** キーを押します。 関数アプリ名として有効な文字は "a-z"、"0-9"、および "-" です。

1. 次のプロンプトでは、お近くの Azure [リージョン](https://azure.microsoft.com/en-us/regions/)を選択します。

1. **Azure Functions** の VS Code **[出力]** パネルに進行状況が表示されます。

    ![デプロイの進行状況を示す VS Code の出力パネル](media/functions-extension/deploy-progress.png)

1. デプロイが完了したら、**Azure Functions** エクスプローラーに移動し、Azure サブスクリプションのノードを展開し、関数アプリのノードを展開し、 **[Functions (Read only)]\(関数 (読み取り専用)\)** を展開します。 関数名を右クリックし、 **[Copy Function Url]\(関数の URL をコピーする\)** を選択します。

    ![関数の URL をコピーするコマンド](media/functions-extension/copy-function-url-command.png)

1. URL をブラウザーに貼り付けて、`?name=<yourname>` 引数を付加します。 すると、クラウドで実行されている関数がブラウザーに表示されます。

    ![クラウドで実行されている関数](media/functions-extension/remote-test-browser.png)

1. 必要に応じて、*index.js* 内の関数コードに変更を加えるか、他のトリガーで関数を追加します。 ローカルでテストした後、前の手順と同様にコードを再デプロイして、クラウドでこれらの変更をテストします。

    > [!TIP]
    > デプロイ時には関数アプリケーション全体がデプロイされるため、すべての個別の関数に対する変更が一度にデプロイされます。

> [!div class="nextstepaction"]
> [関数アプリをデプロイしました](tutorial-vscode-serverless-node-05.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=deploy-app)
