---
title: 手順 4:VS Code を使用して Azure Functions の Python コードをローカルでデバッグする
description: チュートリアルの手順 4、VS Code デバッガーをローカルで実行して Python コードをチェックする。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 51ef666bd529194670279deaae51ee073633fd3a
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441597"
---
# <a name="4-debug-the-azure-functions-python-code-locally"></a>4:Azure Functions の Python コードをローカルでデバッグする

[前の手順: コード ファイルを調べる](tutorial-vs-code-serverless-python-03.md)

Visual Studio Code で Azure Functions の Python コードをローカルでデバッグできます。

1. Functions プロジェクトを作成すると、Visual Studio Code 拡張機能により、**Attach to Python Functions** という名前の構成が 1 つ含まれる起動構成も`.vscode/launch.json` に作成されます。 この構成では、F5 を押すか、デバッグ エクスプローラーを使用し、プロジェクトを開始できます。

    ![Python プロジェクトを開始するためのデバッグ エクスプローラーの構成](media/tutorial-vs-code-serverless-python/configuration-to-start-a-python-project-for-debugging.png)

1. デバッガーを起動すると、ターミナルが開き、利用できるエンドポイントの概要など、Azure Functions からの出力が表示されます。 "HttpExample" 以外の名前を使用した場合は、URL が異なることがあります。

    <pre>
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    </pre>

1. Visual Studio Code の **[出力]** ウィンドウの URL で **Ctrl + クリック**または **Cmd + クリック**を使用し、ブラウザーを開いてそのアドレスを表示するか、ブラウザーを起動して同じ URL を貼り付けます。 いずれの場合でもエンドポイントは `api/<function_name>` であり、今回は `api/HttpExample` となります。 ただし、その URL には name パラメーターが含まれないため、ブラウザー ウィンドウには、コードのそのパスに応じて、"Please pass a name on the query string or in the request body" (クエリ文字列または要求本文で名前を渡してください) とだけ表示されます。

    > [!TIP]
    > この URL にアクセスできず、企業プロキシの内側で実行している (`HTTP_PROXY` および `HTTPS_PROXY` 環境変数が設定されている可能性が高い) 場合は、`NO_PROXY` という環境変数を `localhost,127.0.0.1` に設定して、もう一度お試しください。

1. それでは、`http://localhost:7071/api/HttpExample?name=VS%20Code` のように、使用する name パラメーターを追加してみましょう。ブラウザー ウィンドウに "Hello Visual Studio Code!" というメッセージが表示され、そのコード パスを実行したことがわかります。

1. JSON 要求本文で名前値を渡すには、JSON インラインで curl のようなツールを使用できます。

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data '{"name":"Visual Studio Code"}' http://localhost:7071/api/HttpExample
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```powershell
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    PowerShell では、[Invoke-WebRequest コマンドレット](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-6)を使用することもできます。

    ---

    あるいは、`{"name":"Visual Studio Code"}` を含む "*data.json*" のようなファイルを作成し、コマンド `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample` を使用します。

1. 関数のデバッグをテストするには、`name = req.params.get('name')` と記されている行にブレークポイントを設定し、URL に再度要求を行います。 Visual Studio Code デバッガーはその行で停止するはずです。変数を調べ、コードをステップ実行できます (基本的なデバッグの簡単なチュートリアルは、[Visual Studio Code チュートリアルのデバッガーの構成と実行](https://code.visualstudio.com/docs/python/python-tutorial#configure-and-run-the-debugger)に関するセクションにあります)。

1. 関数をローカルで徹底的にテストできたら、デバッガーを停止します ( **[デバッグ]**  >  **[デバッグの停止]** メニュー コマンドを使用するか、デバッグ ツールバーで **Disconnect** コマンドを使用します)。

> [!div class="nextstepaction"]
> [デバッガーをローカルで実行しました - 手順 5 に進む >>>](tutorial-vs-code-serverless-python-05.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)
