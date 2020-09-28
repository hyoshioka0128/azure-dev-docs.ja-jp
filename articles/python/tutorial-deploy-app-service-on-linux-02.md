---
title: 手順 2:Visual Studio Code から Azure App Service on Linux にデプロイするアプリを準備する
description: チュートリアルの手順 2、アプリケーションの設定
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 36e2c57f556718a0c2d83de90995ce52602a34ad
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90832028"
---
# <a name="2-prepare-your-app-for-deployment-to-azure-app-service"></a>2:Azure App Service にデプロイするためにアプリを準備する

[前のステップ: 環境を構成する](tutorial-deploy-app-service-on-linux-01.md)

この記事では、このチュートリアルの Azure App Service にデプロイするアプリを準備します。 既存のアプリを使用することも、アプリを作成またはダウンロードすることもできます。

作業対象のアプリが既にある場合は、フレームワーク (Flask、Django など) を含む依存関係を記述した *requirements.txt* ファイルがあることを確認してください。 任意のフレームワークを使用できます。

まだアプリがない場合は、次のいずれかの方法を使用します。 アプリがローカルで動作することを確認してください。

## <a name="option-1-vs-code-flask-tutorial-sample"></a>オプション 1: VS Code Flask チュートリアル サンプル

[https://github.com/Microsoft/python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) をダウンロードまたは複製します。これは [Flask チュートリアル](https://code.visualstudio.com/docs/python/tutorial-flask)に従って得られた結果です。

## <a name="option-2-vs-code-django-tutorial-sample"></a>オプション 2:VS Code Django チュートリアル サンプル

[https://github.com/Microsoft/python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial) をダウンロードまたは複製します。これは [Django チュートリアル](https://code.visualstudio.com/docs/python/tutorial-django)に従って得られた結果です。

このサンプルのようにローカル SQLite データベースが Django アプリに使用されている場合、事前初期化済みかつ事前設定済みの *db.sqlite3* ファイルのコピーをリポジトリに追加する必要があります。 これは、現在 App Service for Linux には、デプロイの過程で Django の `migrate` コマンドを実行する手段が用意されていないため、あらかじめ作成しておいたデータベースを自分でデプロイしなければならないためです。 その場合でも、データベースは事実上読み取り専用であり、また、データベースに書き込みを行うとエラーが発生します。

いずれのケースも最善の方法は、アプリのコードとは無関係にデプロイおよび初期化された別個のデータベースを使用することです。

## <a name="option-3-create-a-minimal-flask-app"></a>オプション 3:最小限の Flask アプリを作成する

このセクションでは、このチュートリアルで使用する最小限の Flask アプリについて説明します。

1. 新しいフォルダーを作成して VS Code で開き、以下の内容を含んだ *hello.py* という名前のファイルを追加します。 App Service のスタートアップ コマンドで名前がどのように使用されるか (後述) を説明するために、このアプリ オブジェクトには意図的に `myapp` という名前を付けています。

    ```python
    from flask import Flask
    myapp = Flask(__name__)

    @myapp.route("/")
    def hello():
        return "Hello Flask, on Azure App Service for Linux"
    ```

1. *requirements.txt* という名前のファイルを作成し、内容を次のようにします。

    ```text
    Flask==1.1.2
    ```

1. メニュー コマンド **Terminal** > **New Terminal** を使用してターミナルを開きます。

1. 次に、`env` という名前の仮想環境を作成してアクティブ化します。

    # <a name="macoslinux"></a>[macOS/Linux](#tab/linux)

    ```bash
    sudo apt-get install python3-venv    # If needed
    python3 -m venv env
    source env/bin/activate
    ```

    # <a name="windows"></a>[Windows](#tab/windows)

    ```cmd
    python -m venv env
    env\scripts\activate
    ```

    ---

1. アプリの依存関係をインストールします:

    ```cmd
    pip install -r requirements.txt
    ```

1. アプリ オブジェクトの検索先を Flask に指示する FLASK_APP 環境変数を設定します。

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set FLASK_APP=hello:myapp
    ```

    # <a name="powershell"></a>[PowerShell](#tab/powershell)

    ```ps
    $env:FLASK_APP = "hello:myapp"
    ```

   # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    export FLASK_APP=hello:myapp
    ```

    ---

1. アプリを実行します。

    ```cmd
    flask run
    ```

1. その後、`http://127.0.0.1:5000/` という URL を使用してアプリをブラウザーで開くことができます。

> [!div class="nextstepaction"]
> [アプリの準備ができました - 手順 3 に進む >>>](tutorial-deploy-app-service-on-linux-03.md)

[問題がある場合は、お知らせください。](https://aka.ms/FlaskVSCQuickstartHelp)
