---
title: Visual Studio Code で Azure Functions の Python コード ファイルを調べる
description: チュートリアルの手順 3、Azure Functions が提供するテンプレートの Python コードを理解する。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: b1ed7f69656797822b17c11bcf0f2ece0caa09e2
ms.sourcegitcommit: 86aec15e2d25b5b706092feb4271fb958c29d5fa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71707344"
---
# <a name="examine-the-code-files"></a>コード ファイルを調べる

[前の手順: 関数を作成する](tutorial-vs-code-serverless-python-02.md)

新しく作成された関数のサブフォルダーにファイルが 3 つあります。" *\_\_init\_\_.py*" には関数のコードが含まれます。"*function.json*" は、Azure Functions の関数を説明するものです。"*sample.dat*" はサンプル データ ファイルです。 "*sample.dat*" は、他のファイルをサブフォルダーに追加できることを示すためだけに存在します。必要に応じて削除できます。

まず、"*function.json*" を確認し、次に " *\_\_init\_\_.py*" のコードを確認しましょう。

## <a name="functionjson"></a>function.json

function.json ファイルには、Azure Functions エンドポイントに必要な構成情報があります。

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

## <a name="__initpy__"></a>\_\_init.py\_\_

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
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

コードの重要な部分は次のとおりです。

- `azure.functions` から `func` をインポートする必要があります。ログ記録モジュールのインポートは任意ですが、お勧めします。
- 必須の `main` Python 関数は、`req` という名前の `func.HttpRequest` オブジェクトを受け取り、`func.HttpResponse` 型の値を返します。 これらのオブジェクトの機能については、[func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) と [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python) のリファレンスで詳細を確認できます。
- 次に、`main` の本文で要求が処理され、応答が生成されます。 今回のコードでは、URL の `name` パラメーターを探します。 見つからなかった場合、要求本文に JSON が含まれるかどうかが確認され (`func.HttpRequest.get_json` を使用)、また、JSON に `name` 値が含まれることが確認されます (`get_json` によって返される JSON オブジェクトの `get` メソッドを使用)。
- コードにより、名前が見つかった場合は、文字列 "Hello" の後ろに名前が追加されて返され、見つからなかった場合は、エラー メッセージが返されます。

> [!div class="nextstepaction"]
> [コード ファイルを調べました](tutorial-vs-code-serverless-python-04.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=03-examine-code-files)
