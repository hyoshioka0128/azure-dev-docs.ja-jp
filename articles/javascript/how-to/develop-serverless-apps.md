---
title: Azure Functions を使用したサーバーレス Node.js コード
description: Azure Functions が提供するサーバーレス コードのインフラストラクチャにより、応答性の高いオンデマンドの HTTP エンドポイントを作成できます。
ms.topic: how-to
ms.date: 10/27/2020
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js, contperfq2
ms.openlocfilehash: eb33717761d492051737b0c4ec86a93a6e2b6256
ms.sourcegitcommit: e1175aa94709b14b283645986a34a385999fb3f7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93192544"
---
# <a name="use-azure-functions-to-develop-nodejs-serverless-code"></a>Azure Functions を使用して Node.js サーバーレス コードを開発する

Azure Functions が提供するサーバーレス コードのインフラストラクチャにより、応答性の高いオンデマンドの HTTP エンドポイントを作成できます。 サーバーレス コードは、さまざまなイベントに反応して作動する、JavaScript または TypeScript コードから成ります。 

関数は、コードまたは Docker コンテナーとして Web サービスの上で実行されます。これは、抽象化されているため、ユーザーはエンドポイントのコードに集中することができます。 関数を使用すると、別の関数をトリガーすることもできます。これにより、関数の作業ストリームで、既存のホスト型バックエンド サーバーの機能を置き換えることができ、そのサーバーを管理する必要がなくなります。 

## <a name="what-is-a-function-resource"></a>関数リソースとは

Azure の関数リソースとは、1 つの Azure の地理的な場所にあるすべての関連する関数の論理単位です。 リソースには、1 つまたは複数の関数を含めることができます。これらは、互いに独立している場合もあれば、入力や出力のトリガーと関連付けられる場合もあります。 多くの一般的な関数から選択することも、独自に作成することもできます。

:::image type="content" source="../media/howto-serverless/portal-screenshot-new-azure-function-type.png" alt-text="多くの一般的な関数から選択することも、独自に作成することもできます。":::

関数リソースの設定には、環境変数、認証、ログ、CORS など、一般的なサーバーレス構成が含まれます。  

## <a name="durable-stateful-functions"></a>永続的なステートフル関数 

[Durable Functions](/azure/azure-functions/durable/durable-functions-overview) では " *状態* " が保持されます。つまり、Azure で長時間実行される関数を管理することができます。 [JavaScript で最初の Durable Functions を作成する](/azure/azure-functions/durable/quickstart-js-vscode)。

## <a name="static-web-apps-include-functions"></a>静的 Web アプリに関数を含める 

サーバーレス API も必要とする静的なフロントエンド クライアント アプリケーション (Angular、React、Vue など) を開発する場合は[静的 Web アプリ](/azure/static-web-apps/getting-started?tabs=react)を関数と共に使用して、両方をまとめてバンドルします。 

## <a name="a-simple-javascript-function-for-http-requests"></a>HTTP 要求の単純な JavaScript 関数

関数は、要求およびコンテキスト情報を含むエクスポートされた非同期関数です。 次の Azure portal の部分的なスクリーンショットに、関数コードを示します。 

:::image type="content" source="../media/howto-serverless/portal-screenshot-azure-function-http.png" alt-text="Azure portal の Azure 関数の部分的なスクリーンショット。":::

## <a name="develop-functions-locally-with-visual-studio-code-and-extensions"></a>Visual Studio Code と拡張機能を使用してローカルで関数を開発する

Visual Studio Code を使用して[初めての関数](/azure/azure-functions/functions-create-first-function-vs-code)を作成します。 Visual Studio Code では、[Azure Functions 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)を使用して、詳細の多くが簡略化されます。

この拡張機能により、一般的なテンプレートを使用して JavaScript および TypeScript の関数を作成することができます。 

Azure の HTTP 関数の JavaScript の例を次に示します。 

```nodejs
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

Azure の HTTP 関数の TypeScript の例を次に示します。 

```typescript
import { AzureFunction, Context, HttpRequest } from "@azure/functions"

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
    context.log('HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };

};

export default httpTrigger;
```

## <a name="configuring-the-function"></a>関数の構成

関数は、 **function.json** ファイルで構成されます。 この構成を使用すると、関数のトリガー方法 ("direction": in) と、関数が返すもの ("direction": out) を構成できます。 また、環境変数、および関数が動作するために必要なその他の情報を設定することもできます。 [トリガーとバインド](/azure/azure-functions/functions-triggers-bindings?tabs=javascript.md)の詳細については、こちらを参照してください。 

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

## <a name="develop-functions-remotely-using-the-azure-portal"></a>Azure portal を使用して関数をリモートで開発する

[Azure portal を使用して Azure 関数を作成する](https://ms.portal.azure.com/#create/Microsoft.FunctionApp)と、関数を構成し、事前に設定されたテンプレート内にコードを記述して、関数をテストすることができます。 

ポータルでは、TypeScript ではなく、JavaScript 関数のみが作成されます。 TypeScript を使用して開発する場合は、関数をダウンロードするか、関数の拡張機能を使用して Visual Studio Code で関数をローカルに作成します。 

## <a name="next-steps"></a>次のステップ

[Azure Functions の開発者向け JavaScript ガイド](/azure/azure-functions/functions-reference-node)は、お勧めの出発点です。 

Microsoft Learn モジュールで、[Azure Functions と SignalR Service を使って、Web アプリの自動更新を有効にする](/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)方法について学びます。

* [タイマーでコードを実行する](/azure/azure-functions/functions-create-scheduled-function)
* [Azure BLOB ストレージにファイルがアップロードされたりファイルが更新されたときにコードを実行する](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
* [Azure Queue storage にメッセージが書き込まれたときにコードを実行する](/azure/azure-functions/functions-create-storage-queue-triggered-function)
* [Azure Functions と Azure Cosmos DB を使用して非構造化データを格納する](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb?tabs=javascript)
* [Node.js と Azure Functions のサンプル](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)