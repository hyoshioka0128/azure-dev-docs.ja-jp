---
title: 手順 2:Visual Studio Code から Azure App Service on Linux にデプロイするアプリを準備する
description: チュートリアルの手順 2、アプリケーションの設定
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 7197f8afc28bd62e7247c3955c888199ee69c509
ms.sourcegitcommit: 09b4a2dbe13601fdf16fcc4082a5075b46ad3459
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2020
ms.locfileid: "96559196"
---
# <a name="2-prepare-your-app-for-deployment-to-azure-app-service"></a>2:Azure App Service にデプロイするためにアプリを準備する

[前のステップ: 環境を構成する](tutorial-deploy-app-service-on-linux-01.md)

この記事では、このチュートリアルの Azure App Service にデプロイするアプリを準備します。 既存のアプリを使用することも、アプリを作成またはダウンロードすることもできます。

## <a name="if-you-already-have-an-app"></a>アプリが既にある場合

作業対象のアプリが既にある場合は、フレームワーク (Flask、Django など) を含む依存関係を一覧表示した *requirements.txt* ファイルがプロジェクト ルートにあることを確認してください。 任意のフレームワークを使用できます。

> [!div class="nextstepaction"]
> [独自のアプリの準備ができました - 手順 3 に進む >>>](tutorial-deploy-app-service-on-linux-03.md)

## <a name="if-you-dont-already-have-an-app"></a>まだアプリがない場合

まだアプリがない場合は、次の "*いずれか*" の方法を使用します。 アプリがローカルで動作することを確認してください。

このチュートリアルの残りの部分では、[オプション 3](#option-3-create-a-minimal-flask-app) に示されているコードを使用します。

### <a name="option-1-use-the-vs-code-flask-tutorial-sample"></a>オプション 1: VS Code Flask チュートリアル サンプルを使用する

[https://github.com/Microsoft/python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) をダウンロードまたは複製します。これは [Flask チュートリアル](https://code.visualstudio.com/docs/python/tutorial-flask)に従って得られた結果です。 アプリコードは、具体的には *hello_app* フォルダー内にあることに注意してください。 アプリをローカルで実行する方法については、サンプルの *readme.md* ファイルを確認してください。

### <a name="option-2-use-the-vs-code-django-tutorial-sample"></a>オプション 2:VS Code Django チュートリアル サンプルを使用する

[https://github.com/Microsoft/python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial) をダウンロードまたは複製します。これは [Django チュートリアル](https://code.visualstudio.com/docs/python/tutorial-django)に従って得られた結果です。

クラウドにデプロイされた Django アプリでは、Azure 用 PostgreSQL などのクラウドベースのデータベースも使用するのが理想的です。 詳細については、[Azure portal を使用して PostgreSQL で Django Web アプリをデプロイする](tutorial-python-postgresql-app-portal.md)に関するページを参照してください。

このサンプルのようにローカル SQLite データベースが Django アプリに使用されている場合、このチュートリアルでは事前初期化済みかつ事前設定済みの *db.sqlite3* ファイルのコピーをリポジトリに追加するのが最も簡単です。 それ以外の場合は、アプリがデプロイされているコンテナーで Django の `migrate` コマンドを実行するようにビルド後のコマンドを構成する必要があります。 詳細については、[App Service の構成 - ビルドの自動化のカスタマイズ](/azure/app-service/configure-language-python#customize-build-automation)に関する記事を参照してください。

### <a name="option-3-create-a-minimal-flask-app"></a>オプション 3:最小限の Flask アプリを作成する

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
    Flask
    ```

1. メニュー コマンド **Terminal** > **New Terminal** を使用してターミナルを開きます。

1. ターミナルで、`.venv` という名前の仮想環境を作成してアクティブ化します。 

    # <a name="macoslinux"></a>[macOS/Linux](#tab/linux)

    ```bash
    sudo apt-get install python3-venv    # If needed
    python3 -m venv .venv
    source .venv/bin/activate
    ```

    # <a name="windows"></a>[Windows](#tab/windows)

    ```cmd
    py -3 -m venv .venv
    .venv\scripts\activate
    ```

    ---

1. VS Code で、新しく作成された環境をアクティブ化することの確認を求められたら **[Yes]\(はい\)** と答えます。

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

1. URL `http://127.0.0.1:5000/` を使用してアプリをブラウザーで開きます。 "Hello Flask, on Azure App Service on Linux." というメッセージが表示されます。

1. ターミナルで **Ctrl**+**C** を押して Flask サーバーを停止します。

> [!div class="nextstepaction"]
> [アプリの準備ができました - 手順 3 に進む >>>](tutorial-deploy-app-service-on-linux-03.md)

[問題がある場合は、お知らせください。](https://aka.ms/FlaskVSCQuickstartHelp)
