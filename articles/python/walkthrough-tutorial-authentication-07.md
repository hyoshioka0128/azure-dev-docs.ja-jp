---
title: 'チュートリアル、パート 7: Azure サービスで Python アプリを認証する'
description: サードパーティ API エンドポイントを使用して Azure Queue Storage にメッセージを書き込むメイン アプリ API エンドポイントのコードについて説明します。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e026eca0216147c6614582e0cd070cee81daf99c
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379524"
---
# <a name="part-7-main-application-api-endpoint"></a>パート 7: メイン アプリケーション API エンドポイント

[前のパート:メイン アプリのスタートアップ コード](walkthrough-tutorial-authentication-06.md)

アプリの */api/v1/getcode* API は、英数字コードとタイムスタンプを含む JSON 応答を生成します。

まず、`@app.route` デコレータが、`get_code` 関数によって */api/v1/getcode* URL への要求が処理されることを Flask に伝えます。

```python
@app.route('/api/v1/getcode', methods=['GET'])
def get_code():
```

次に、サードパーティ API (この URL は `number_url` にあります) を呼び出します。ヘッダーで Key Vault から取得するアクセス キーを提供します。

```python
    headers = {
        'Content-Type': 'application/json',
        'x-functions-key': access_key
        }

    r = requests.get(url = number_url, headers = headers)

    if (r.status_code != 200):
        return "Could not get you a code.", r.status_code
```

ヘッダーの `x-functions-key` プロパティは、(この例のサードパーティ API がデプロイされている) Azure Functions で、具体的にどのようにアクセス キーがヘッダーに表示されることが想定されているかを示します。 詳細については、[Azure Functions の HTTP トリガー - 認可キー](/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#authorization-keys)に関する記事を参照してください。 何らかの理由で API の呼び出しが失敗した場合は、エラー メッセージと状態コードを返します。

API 呼び出しが成功して数値が返されたと仮定した場合、その数値と (独自の `random_char` 関数を使った) いくつかのランダムな文字を使用して、より複雑なコードを作成します。

```python
    data = r.json()
    chars1 = random_char(3)
    chars2 = random_char(3)
    code_value = f"{chars1}-{data['value']}-{chars2}"
    code = { "code": code_value, "timestamp" : str(datetime.utcnow()) }
```

ここで `code` 変数には、コード値とタイムスタンプを含む、アプリの API に対する完全な JSON 応答が含まれています。 応答例は、`{"code":"ojE-161-pTv","timestamp":"2020-04-15 16:54:48.816549"}` のようになります。

ただし、その応答を返す前に、キュー クライアントの [`send_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python#send-message-content----kwargs-) メソッドを使用して、ストレージ キューにメッセージを書き込みます。

```python
    queue_client.send_message(code)

    return jsonify(code)
```

## <a name="processing-queue-messages"></a>キュー メッセージの処理

キューに格納されているメッセージは、[Azure portal](/azure/storage/queues/storage-quickstart-queues-portal#view-message-properties)、または Azure CLI コマンド [`az storage message get`](/cli/azure/storage/message?view=azure-cli-latest#az-storage-message-get) を使用して表示および管理できます。 サンプル リポジトリには、アプリ エンドポイントからコードを要求し、メッセージ キューを確認するためのスクリプト (*test.cmd* と *test.sh*) が含まれています。 さらに、[`az storage message clear`](/cli/azure/storage/message?view=azure-cli-latest#az-storage-message-clear) コマンドを使用してキューをクリアするスクリプトもあります。

通常、この例のようなアプリでは、後続の処理のためにキューからメッセージを非同期的にプルする別のプロセスがあります。 前述のように、この API エンドポイントによって生成された応答は、2 要素ユーザー認証と共にアプリ内の他の場所で使用される場合があります。 その場合、アプリは、一定期間 (たとえば、10 分) 後にコードを無効にする必要があります。 このタスクを実行する簡単な方法は、ユーザー ログイン手順によって使用される有効な 2 要素認証コードのテーブルを保持することです。 そして、アプリは、(擬似コードで) 次のロジックを使用する単純なキュー監視プロセスを持つことになります。

<pre>
pull a message from the queue and retrieve the code.

if (code is already in the table):
    remove the code from the table, thereby invalidating it
else:
    add the code to the table, making it valid
    call queue_client.send_message(code, visibility_timeout=600)
</pre>

この擬似コードでは、[`send_message`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python#send-message-content----kwargs-) メソッドのオプションの `visibility_timeout` パラメーターを使用して、メッセージがキューに表示されるまでの秒数を指定しています。 既定のタイムアウトはゼロであるため、API エンドポイントによって最初に書き込まれたメッセージは、キュー監視プロセスからすぐに確認できるようになります。 その結果、そのプロセスによって、それらは有効なコード テーブルにすぐに格納されます。 タイムアウトを指定して再度同じメッセージをキューに保持することで、プロセスは 10 分後に再度そのコードを受信することを認識します。その時点で、それはテーブルから削除されます。

## <a name="implementing-the-main-app-api-endpoint-in-azure-functions"></a>Azure Functions でのメイン アプリ API エンドポイントの実装

この記事で前に示したコードでは、Flask Web フレームワークを使用して API エンドポイントを作成しています。 Flask は Web サーバーで実行する必要があるため、このようなコードは Azure App Service または仮想マシンにデプロイする必要があります。

代替のデプロイ オプションは、Azure Functions のサーバーレス環境です。 この場合、すべてのスタートアップ コードと API エンドポイント コードは、HTTP トリガーにバインドされている同じ関数内に含まれます。 App Service と同様に、[関数のアプリケーション設定](/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings)を使用して、コードの環境変数を作成します。

実装において容易になる要素の 1 つは、Queue Storage での認証です。 キューの URL と資格情報オブジェクトを使用して `QueueClient` オブジェクトを取得する代わりに、関数の "*キュー ストレージ バインディング*" を作成します。 このバインディングにより、すべての認証がバックグラウンドで処理されます。 このようなバインディングでは、すぐに使用できるクライアント オブジェクトがパラメーターとして関数に提供されます。 詳細およびコード例については、[Azure Queue Storage への Azure Functions の接続](/azure/azure-functions/functions-add-output-binding-storage-queue-cli?tabs=bash%2Cbrowser&pivots=programming-language-python)に関する記事を参照してください。

## <a name="next-steps"></a>次のステップ

この例を通して、他の Azure サービスでアプリが認証を行う方法と、アプリが Azure Key Vault を使用してサードパーティ API に必要なその他のシークレットを格納する方法について学習しました。

Azure Key Vault と Azure Storage を使用してここで示したのと同じパターンは、他のすべての Azure サービスにも適用されます。 重要な手順は、Azure portal のそのサービスのページ内で、または Azure CLI を使用して、アプリの適切なロール アクセス許可を設定することです。 ([ロールのアクセス許可を割り当てる方法](how-to-assign-role-permissions.md)に関する記事を参照。) 他のアクセス ポリシーを構成する必要があるかどうかについては、必ずサービスのドキュメントを確認してください。

ローカル開発に使用しているどのサービス プリンシパルにも同じロールとアクセス ポリシーを割り当てる必要があることに常に注意してください。

つまり、このチュートリアルを完了すると、その知識を、他の任意の数の Azure サービスや他の任意の数の外部のサービスに適用することができます。

ここでは触れていない 1 つのテーマは、"*ユーザー*" の認証です。 Web アプリのこの領域を調べるには、「[Azure App Service でユーザーをエンドツーエンドで認証および承認する](/azure/app-service/tutorial-auth-aad?pivots=platform-linux)」から始めてください。

## <a name="see-also"></a>関連項目

- [Azure で Python アプリを認証および認可する方法](azure-sdk-authenticate.md)
- チュートリアル サンプル: [github.com/Azure-Samples/python-integrated-authentication](https://github.com/Azure-Samples/python-integrated-authentication)
- [Azure Active Directory のドキュメント](/azure/active-directory)
- [Azure Key Vault のドキュメント](/azure/key-vault)
