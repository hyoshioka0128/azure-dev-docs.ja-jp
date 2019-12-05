---
title: チュートリアル:Visual Studio Code を使用して 2 つ目の Python 関数を Azure Functions に追加する
description: チュートリアルの手順 6、2 つ目の関数を追加して Azure Functions プロジェクトを展開する。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 84130e3bc4d5a85046e21b66338ccc07bba8cad1
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74465911"
---
# <a name="tutorial-add-a-second-python-function-to-azure-functions"></a>チュートリアル:2 つ目の Python 関数を Azure Functions に追加する

[前の手順: Azure にデプロイする](tutorial-vs-code-serverless-python-05.md)

最初のデプロイ後、Python 関数を追加するなど、コードを変更し、同じ Azure Functions アプリに再デプロイできます。

1. **[Azure: Functions]** エクスプローラーで **[関数の作成]** コマンドを選択するか、コマンド パレットの **[Azure Functions:関数の作成]** を使用します。 関数には次の詳細を指定します。

    - テンプレート:HTTP トリガー
    - 名前:"DigitsOfPi"
    - 承認レベル:Anonymous

1. Visual Studio Code では、ファイル エクスプローラーは関数名を持つサブフォルダーになりますが、ここにも " *\_\_init\_\_.py*"、"*function.json*"、"*sample.dat*" という名前のファイルが含まれています。

1. " *\_\_init\_\_.py*" の内容を次のコードに一致するように置換します。次のコードでは、URL に指定されている桁数まで PI の値を含む文字列が生成されます (このコードでは、URL パラメーターのみが使用されます)。

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. このコードでは HTTP GET のみがサポートされるため、`"methods"` コレクションに `"get"` だけが含まれるように "*function.json*" を変更します (つまり、`"post"` を削除します)。 ファイル全体は次のようになります。

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
            "get"
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

1. F5 を押すか、 **[デバッグ]**  >  **[デバッグの開始]** メニュー コマンドを選択し、デバッガーを開始します。 **[出力]** ウィンドウには、プロジェクトの両方のエンドポイントが表示されるはずです。

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. ブラウザーで、あるいは curl から、`http://localhost:7071/api/DigitsOfPi?digits=125` に要求を行い、出力を確認します (コード アルゴリズムは完全には正しくないことに気付かれるかもしれませんが、改善は皆様におまかせします!)完了したらデバッガーを停止します。

1. **[Deploy to Function App]\(Function App にデプロイする\)** を **[Azure:Functions]** エクスプローラーで使用し、コードを再デプロイします。 プロンプトが表示されたら、前に作成した Function App を選択します。

1. デプロイが完了したら (数分かかります!)、 **[出力]** ウィンドウに、テストを繰り返すことができる公開エンドポイントが表示されます。

> [!div class="nextstepaction"]
> [2 つ目の関数を追加しました](tutorial-vs-code-serverless-python-07.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=06-second-function)
