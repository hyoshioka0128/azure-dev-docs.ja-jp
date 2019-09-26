---
title: Visual Studio Code を使用して Azure Functions の Python コードをローカルでデバッグする
description: チュートリアルの手順 4、VS Code デバッガーをローカルで実行して Python コードをチェックする。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: 28df4c9a8a8b3a6ab6308449e9ae2e1ebd2cc6e4
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186143"
---
# <a name="debug-the-function-code-locally"></a>関数コードをローカルでデバッグする

[前の手順: コード ファイルを調べる](tutorial-vs-code-serverless-python-03.md)

1. Functions プロジェクトを作成すると、Visual Studio Code 拡張機能により、**Attach to Python Functions** という名前の構成が 1 つ含まれる起動構成も`.vscode/launch.json` に作成されます。 この構成では、F5 を押すか、デバッグ エクスプローラーを使用し、プロジェクトを開始できます。

    ![Functions 起動構成が表示されたデバッグ エクスプローラー](media/tutorial-vs-code-serverless-python/launch-configuration.png)

1. デバッガーを起動すると、ターミナルが開き、利用できるエンドポイントの概要など、Azure Functions からの出力が表示されます。 "HttpExample" 以外の名前を使用した場合は、URL が異なることがあります。

    ```output
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Visual Studio Code の **[出力]** ウィンドウの URL で **Ctrl + クリック**または **Cmd + クリック**を使用し、ブラウザーを開いてそのアドレスを表示するか、ブラウザーを起動して同じ URL を貼り付けます。 いずれの場合でもエンドポイントは `api/<function_name>` であり、今回は `api/HttpExample` となります。 ただし、その URL には name パラメーターが含まれないため、ブラウザー ウィンドウには、コードのそのパスに応じて、"Please pass a name on the query string or in the request body" (クエリ文字列または要求本文で名前を渡してください) とだけ表示されます。

1. それでは、`http://localhost:7071/api/HttpExample?name=VS%20Code` のように、使用する name パラメーターを追加してみましょう。ブラウザー ウィンドウに "Hello Visual Studio Code!" というメッセージが表示され、そのコード パスを実行したことがわかります。

1. JSON 要求本文で名前値を渡すには、JSON インラインで curl のようなツールを使用できます。

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data {"name":"Visual Studio Code"} http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    あるいは、`{"name":"Visual Studio Code"}` を含む "*data.json*" のようなファイルを作成し、コマンド `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample` を使用します。

1. 関数のデバッグをテストするには、`name = req.params.get('name')` と記されている行にブレークポイントを設定し、URL に再度要求を行います。 Visual Studio Code デバッガーはその行で停止するはずです。変数を調べ、コードをステップ実行できます (基本的なデバッグの簡単なチュートリアルは、[Visual Studio Code チュートリアルのデバッガーの構成と実行](https://code.visualstudio.com/docs/python/python-tutorial#configure-and-run-the-debugger)に関するセクションにあります)。

1. 関数をローカルで徹底的にテストできたら、デバッガーを停止します ( **[デバッグ]**  >  **[デバッグの停止]** メニュー コマンドを使用するか、デバッグ ツールバーで **Disconnect** コマンドを使用します)。

> [!div class="nextstepaction"]
> [デバッガーをローカルで実行しました](tutorial-vs-code-serverless-python-05.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)
