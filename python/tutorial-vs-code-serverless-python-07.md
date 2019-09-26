---
title: Visual Studio Code を使用して Python で Azure Functions のストレージ バインドを追加する
description: チュートリアルの手順 7、メッセージを Azure Storage に書き込むバインドを Python で追加する。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: 8fdb78024cc6ac1df0ee961fc424878f5f634cf2
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019940"
---
# <a name="add-a-binding-to-write-messages-to-azure-storage"></a>Azure Storage にメッセージを書き込む目的でバインドを追加する

[前の手順: 2 つ目の関数をデプロイする](tutorial-vs-code-serverless-python-06.md)

"_バインド_" を追加すると、データ アクセス コードを記述しなくても、Azure Storage などのリソースに関数コードを接続できます。 バインドは "*function.json*" ファイルに定義され、入力と出力の両方を表すことができます。 関数では複数の入力と出力のバインドを使用できますが、トリガーは 1 つだけです。 詳細については、「[Azure Functions でのトリガーとバインドの概念](/azure/azure-functions/functions-triggers-bindings.md)」を参照してください。

このセクションでは、このチュートリアルで前に作成した HttpExample 関数にストレージ バインドを追加します。 関数でこのバインドを使用し、要求ごとにストレージにメッセージを書き込みます。 ストレージでは、関数アプリで使用されているものと同じ既定のストレージ アカウントが使用されます。 ただし、ストレージを頻繁に利用する予定がある場合は、別アカウントの作成を検討してください。

1. Azure Functions プロジェクトのリモート設定を "*local.settings.json*" ファイルに同期します。その際、コマンド パレットを開き、 **[Azure Functions:リモート設定のダウンロード]** を選択します。 "*local.settings.json*" を開き、`AzureWebJobsStorage` の値が含まれていることを確認します。 その値はストレージ アカウントの接続文字列です。

1. `HttpExample` フォルダーで、"*function.json*" を右クリックし、 **[バインドの追加]** を選択します。

    ![Visual Studio Code エクスプローラーの [バインドの追加] コマンド](media/tutorial-vs-code-serverless-python/add-binding-command.png)

1. Visual Studio Code の後続のプロンプトで、次の値を選択するか、指定します。

    | Prompt | 指定する値 |
    | --- | --- |
    | Set binding direction (バインド方向を設定してください) | out |
    | Select binding with direction out (外方向のバインドを選択してください) | Azure Queue Storage |
    | The name used to identify this binding in your code (コードでこのバインドの識別に使用する名前) | msg |
    | The queue to which the message will be sent (メッセージの送信先のキュー) | outqueue |
    | Select setting from "*local.settings.json*" (asking for the storage connection) ("local.settings.json" から設定を選択してください (ストレージ接続を要求)) | AzureWebJobsStorage |

1. 以上の選択を行ったら、次のバインドが "*function.json*" ファイルに追加されていることを確認します。

    ```json
        {
          "type": "queue",
          "direction": "out",
          "name": "msg",
          "queueName": "outqueue",
          "connection": "AzureWebJobsStorage"
        }
    ```

1. これでバインドが構成されました。バインドを関数コードで使用できます。 繰り返しになりますが、新しく定義されたバインドは、" *\_\_init\_\_.py*" の `main` 関数の引数としてコードに表示されます。 たとえば、次のコードに一致するように、HttpExample で " *\_\_init\_\_.py*" ファイルを変更できます。次のコードでは、`msg` 引数を使用することで、要求に使用されている名前でタイムスタンプ付きのメッセージが書かれます。 コメントからは、特定の変更について説明されます。

    ```python
    import logging
    import datetime  # MODIFICATION: added import
    import azure.functions as func

    # MODIFICATION: the added binding appears as an argument; func.Out[func.QueueMessage]
    # is the appropriate type for an output binding with "type": "queue" (in function.json).
    def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
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
            # MODIFICATION: write the a message to the message queue, using msg.set
            msg.set(f"Request made for {name} at {datetime.datetime.now()}")

            return func.HttpResponse(f"Hello {name}!")
        else:
            return func.HttpResponse(
                 "Please pass a name on the query string or in the request body",
                 status_code=400
            )
    ```

1. このような変更をローカルでテストするには、F5 を押すか、 **[デバッグ]**  >  **[デバッグの開始]** メニュー コマンドを選択し、Visual Studio Code で再度、デバッガーを起動します。 前と同じように、 **[出力]** ウィンドウには、プロンプトのエンドポイントが表示されるはずです。

1. ブラウザーで、URL `http://localhost:7071/api/HttpExample?name=VS%20Code` にアクセスし、HttpExample エンドポイントへの要求を作成します。これにより、メッセージのキューへの書き込みも行われるはずです。

1. メッセージが "outqueue" キュー (バインドで名前を設定) に書き込まれたことを確認するには、3 つのメソッドのいずれかを使用できます。

    1. [Azure portal](https://portal.azure.com) にサインインし、関数プロジェクトが含まれるリソース グループに移動します。 そのリソース グループ内で、プロジェクトのストレージ アカウントを見つけて移動し、 **[キュー]** に進みます。 そのページで "outqueue" に進みます。ログに記録されているメッセージがすべて表示されるはずです。

    1. Azure Storage Explorer でキューに移動し、キューを調べます。Azure Storage Explorer は Visual Studio と統合されますが、詳細は「[Visual Studio Code を使用して関数を Azure Storage に接続する](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code.md)」にあります。特に「[出力キューを確認する](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code.md#examine-the-output-queue)」セクションをご覧ください。

    1. Azure CLI を使用し、ストレージ キューにクエリを実行します。詳細は「[ストレージ キューに対するクエリを実行する](/azure/azure-functions/functions-add-output-binding-storage-queue-python.md#query-the-storage-queue)」にあります。

1. クラウドでテストするには、 **[Deploy to Function App]\(Function App にデプロイする\)** を **[Azure:Functions]** エクスプローラーで使用し、コードを再デプロイします。 プロンプトが表示されたら、前に作成した Function App を選択します。 デプロイが完了したら (数分かかります!)、繰り返しになりますが、 **[出力]** ウィンドウにテストを繰り返すことができる公開エンドポイントが表示されます。

> [!div class="nextstepaction"]
> [ストレージ バインドを追加しました](tutorial-vs-code-serverless-python-08.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=07-storage-binding)
