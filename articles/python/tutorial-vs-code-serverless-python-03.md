---
title: 手順 3:VS Code でサーバーレス Azure Functions の Python コード ファイルを調べる
description: チュートリアルの手順 3、Azure Functions が提供するテンプレートのサーバーレス Python コードを理解する。
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: a57d7bee924082a187b08c4326ae4de1d7cca998
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442195"
---
# <a name="3-examine-the-python-code-files-in-visual-studio-code"></a>3:Visual Studio Code で Python コード ファイルを調べる

[前の手順: 関数を作成する](tutorial-vs-code-serverless-python-02.md)

新しく作成された **HttpExample** 関数にはファイルが 3 つあります。 *\_\_init\_\_.py* には関数のコードが含まれ、*function.json* では Azure Functions 向けに関数が記述されており、*sample.dat* はサンプル データ ファイルです。 "*sample.dat*" は、他のファイルをサブフォルダーに追加できることを示すためだけに存在します。必要に応じて削除できます。

## <a name="functionjson"></a>function.json

*function.json* ファイルには、Azure Functions エンドポイントに必要な構成情報が記述されています。

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "anonymous",
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
      "name": "$return"
    }
  ]
}
```

`scriptFile` プロパティで、コードのスタートアップ ファイルを特定します。そのコードには、`main` という名前の Python 関数が含まれている必要があります。 ここに指定されているファイルに `main` 関数が含まれている限り、コードを複数のファイルに分割できます。

`bindings` 要素には、2 つのオブジェクトが含まれています。1 つは受信要求を説明するものであり、もう 1 つは HTTP 応答を説明するものです。 受信要求 (`"direction": "in"`) の場合、関数は HTTP GET または POST 要求に応答し、認証を必要としません。 応答 (`"direction": "out"`) は、`main` Python 関数から返される値が何であれ、それを返す HTTP 応答です。

*functions.json* ファイルの詳細については、「[Azure Functions の開発者向けガイド](/azure/azure-functions/functions-reference)」と、[Azure Functions のトリガーとバインドのリファレンス](/azure/azure-functions/functions-triggers-bindings?tabs=python)を参照してください。

## <a name="__init__py"></a>\_\_init\_\_.py

新しい関数を作成すると、Azure Functions によって " *\_\_init\_\_.py*" に既定の Python コードが与えられます。

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello, {name}. This HTTP triggered function executed successfully.")
    else:
        return func.HttpResponse(
             "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.",
             status_code=200
        )
```

コードの重要な部分は次のとおりです。

- `azure.functions` モジュールをインポートする必要があります。ログ記録モジュールのインポートは任意ですが、お勧めします。
- 必須の `main` Python 関数は、`req` という名前の `func.HttpRequest` オブジェクトを受け取り、`func.HttpResponse` 型の値を返します。 これらのオブジェクトの機能については、[func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest) と [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse) のリファレンスで詳細を確認できます。
- 次に、`main` の本文で要求が処理され、応答が生成されます。 今回のコードでは、URL の `name` パラメーターを探します。 見つからなかった場合、要求本文に JSON が含まれるかどうかが確認され (`func.HttpRequest.get_json` を使用)、また、JSON に `name` 値が含まれることが確認されます (`get_json` によって返される JSON オブジェクトの `get` メソッドを使用)。
- 名前が見つかった場合、コードにより、名前が付加された文字列 "Hello" と関数の成功の通知が返されます。 それ以外の場合、コードによって汎用メッセージが返されます。

> [!div class="nextstepaction"]
> [コード ファイルを確認しました - 手順 4 に進む >>>](tutorial-vs-code-serverless-python-04.md)

[問題がある場合は、お知らせください。](https://aka.ms/python-functions-qs-ms-survey)
