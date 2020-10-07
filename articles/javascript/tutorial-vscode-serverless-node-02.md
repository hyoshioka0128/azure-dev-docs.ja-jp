---
title: Visual Studio Code から Azure Functions アプリケーションを作成する
description: サーバーレス チュートリアル パート2、Azure Functions アプリを作成する
ms.topic: tutorial
ms.date: 09/23/2019
ms.custom: devx-track-js
ms.openlocfilehash: 39481fc2b5472bd2f9e54946d3ab1dd6051f00d1
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365115"
---
# <a name="create-the-local-functions-app"></a>ローカルの Functions アプリを作成する

[前の手順:概要と前提条件](tutorial-vscode-serverless-node-01.md)

この手順で、[HTTP トリガー](/azure/azure-functions/functions-reference-node#http-triggers-and-bindings)を使用する関数を含むローカルの Azure Functions アプリケーションを作成します。 Azure Functions アプリには、[さまざまなトリガー](/azure/azure-functions/functions-triggers-bindings)を持つ多数の関数を含めることができます。 HTTP トリガーは、特に着信 HTTP トラフィックの処理を行います。

1. ターミナルまたはコマンド プロンプトで、プロジェクトの適切なフォルダー内から Visual Studio Code を実行します。

    ```bash
    # Create and navigate to a project folder

    # Run VS Code in that folder
    code .
    ```

1. VS Code で、Azure ロゴを選択して **Azure Functions** エクスプローラーを開き、 **[プロジェクトの作成]** コマンドを選択します。

    ![VS Code でローカルな Function アプリを作成する](media/functions-extension/create-function-app-project.png)

1. 最初の 2 つのプロンプトでは、現在のフォルダーを選択し、次に、言語として **[JavaScript]** を選択します。

1. **[Select a template for your project's first function]\(プロジェクトの最初の関数のテンプレートを選択してください\)** というプロンプトでは、 **[HTTP Trigger]\(HTTP トリガー\)** を選択します。

    ![関数のトリガーを選択する](media/functions-extension/create-function-choose-template.png)

1. **[Provide a function name]\(関数名を指定してください\)** というプロンプトでは、「**HttpExample**」と入力します。 (既定の "HttpTrigger" という名前は使用しないでください。これはトリガーと同じであるため、混乱を招く可能性があります。)

    ![関数名を入力する](media/functions-extension/create-function-name.png)

1. **[承認レベル]** というプロンプトでは、 **[匿名]** を選択します。

    ![ [承認レベル] というプロンプトで [匿名] を選択する](media/functions-extension/create-function-anonymous-auth.png)

1. しばらくすると、VS Code によってプロジェクトの作成が完了します。 関数用に指定された *HttpExample* フォルダーが作成され、次の 3 つのファイルが含まれています。

    | ファイル名 | 説明 |
    | --- | --- |
    | *index.js* |  HTTP 要求に応答するソース コード。 |
    | *function.json* | HTTP トリガーの[バインディング構成](/azure/azure-functions/functions-triggers-bindings)。 |
    | *sample.dat* | フォルダー内に他のファイルを含めることができることを示すプレースホルダー データ ファイル。 このファイルは、このチュートリアルでは使用されていないため、必要に応じて削除できます。 |

    ![関数アプリの作成の結果](media/functions-extension/create-function-app-results.png)

> [!div class="nextstepaction"]
> [Functions アプリを作成しました](tutorial-vscode-serverless-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=create-app)