---
title: Visual Studio Code から Azure Functions アプリケーションを作成する
description: HTTP トリガーを使用する関数を含むローカルの Azure Functions (サーバーレス) アプリケーションを作成します。 Azure Functions アプリには、さまざまなトリガーを持つ多数の関数を含めることができます。 HTTP トリガーは、特に着信 HTTP トラフィックの処理を行います。
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: devx-track-js, contperfq2
ms.openlocfilehash: d7d39018ab9645755c6c9bfe414f7507e9d091c7
ms.sourcegitcommit: 12f80b1e0fe08db707c198271d0c399c3aba343a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2020
ms.locfileid: "94515193"
---
# <a name="2-create-the-local-functions-app-with-the-visual-studio-code-_functions_-extension"></a>2.Visual Studio Code の _Functions_ 拡張機能を使用してローカルの Functions アプリを作成する

[前の手順:概要と前提条件](tutorial-vscode-serverless-node-install.md)

この手順で、[HTTP トリガー](/azure/azure-functions/functions-reference-node#http-triggers-and-bindings)を使用する関数を含むローカルの Azure Functions (サーバーレス) アプリケーションを作成します。 Azure Functions アプリには、[さまざまなトリガー](/azure/azure-functions/functions-triggers-bindings)を持つ多数の関数を含めることができます。 HTTP トリガーは、特に着信 HTTP トラフィックの処理を行います。

1. ターミナルまたはコマンド プロンプトで、プロジェクトの適切なフォルダー内から Visual Studio Code を実行します。

    ```bash
    # Create and navigate to a project folder

    # Run VS Code in that folder
    code .
    ```

1. Visual Studio Code で、Azure ロゴを選択して **Azure Functions** エクスプローラーを開き、 **[プロジェクトの作成]** コマンドを選択します。

    ![VS Code でローカルな Function アプリを作成する](../media/functions-extension/create-function-app-project.png)

1. 最初の 2 つのプロンプトでは、現在のフォルダーを選択し、次に、言語として **[JavaScript]** を選択します。

1. **[Select a template for your project's first function]\(プロジェクトの最初の関数のテンプレートを選択してください\)** というプロンプトでは、 **[HTTP Trigger]\(HTTP トリガー\)** を選択します。

    ![関数のトリガーを選択する](../media/functions-extension/create-function-choose-template.png)

1. **[Provide a function name]\(関数名を指定してください\)** というプロンプトでは、「**HttpExample**」と入力します。 (既定の "HttpTrigger" という名前は使用しないでください。これはトリガーと同じであるため、混乱を招く可能性があります。)

    ![関数名を入力する](../media/functions-extension/create-function-name.png)

1. **[承認レベル]** というプロンプトでは、 **[匿名]** を選択します。

    ![ [承認レベル] というプロンプトで [匿名] を選択する](../media/functions-extension/create-function-anonymous-auth.png)

1. しばらくすると、VS Code によってプロジェクトの作成が完了します。 関数用に指定された *HttpExample* フォルダーが作成され、次の 3 つのファイルが含まれています。

    | ファイル名 | 説明 |
    | --- | --- |
    | *index.js* |  HTTP 要求に応答するソース コード。 |
    | *function.json* | HTTP トリガーの[バインディング構成](/azure/azure-functions/functions-triggers-bindings)。 |
    | *sample.dat* | フォルダー内に他のファイルを含めることができることを示すプレースホルダー データ ファイル。 このファイルは、このチュートリアルでは使用されていないため、必要に応じて削除できます。 |

    ![関数アプリの作成の結果](../media/functions-extension/create-function-app-results.png)

## <a name="http-function-javascript-template-code"></a>HTTP 関数 JavaScript テンプレート コード

HTTP 要求に応答するための基本的なコードが用意されています。 HTTP 要求 (_req_ パラメーター) と応答オブジェクトについてよく理解している場合は、この関数を使い慣れているはずです。 応答情報は、**context** オブジェクトの `res` プロパティで返されます。  

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };
}
```

各関数の[トリガー](/azure/azure-functions/functions-triggers-bindings?tabs=csharp)の種類によってテンプレート関数が提供されるため、アプリケーションのコードにすぐに焦点を当てることができます。 Express.js から Azure Functions に移行する場合は、お使いのアプリケーションに[必要な変更を確認](/azure/azure-functions/shift-expressjs?tabs=javascript)してください。 

> [!div class="nextstepaction"]
> [Functions アプリを作成しました](tutorial-vscode-serverless-node-test-local.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=create-app)
