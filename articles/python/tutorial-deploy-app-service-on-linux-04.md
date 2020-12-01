---
title: 手順 4:Azure App Service on Linux の Python アプリ用のカスタム スタートアップ ファイルを構成する
description: チュートリアルの手順 4. では、Web アプリの起動方法を App Service に指定します (Django や Flask などのフレームワークごとの手順を含む)。
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 0c7fd378fa1166113812c4748c10676030941e13
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485651"
---
# <a name="4-configure-a-custom-startup-file-for-python-apps-on-azure-app-service"></a>4:Azure App Service の Python アプリ用のカスタム スタートアップ ファイルを構成する

[前の手順: App Service を作成する](tutorial-deploy-app-service-on-linux-03.md)

この手順では、Azure App Service の Python アプリに対して必要な場合にカスタム スタートアップ ファイルを構成します。

カスタム スタートアップ ファイルは次の場合に必要です。

- 既定値 (`--bind=0.0.0.0 --timeout 600`) 以外の引数を使って Gunicorn Web サーバーを起動したい。

- Flask や Django 以外のフレームワークを使用してアプリが作成されているか、または異なる Web サーバーを使用したい。

- Flask アプリのメイン コード ファイルに *app.py* または *application.py** **以外** の名前が付けられているか、アプリ オブジェクトに `app` **以外** の名前が付けられている。

    つまり、プロジェクトのルート フォルダーに *app.py* または *application.py* が存在し、"*かつ*" Flask アプリ オブジェクトの名前が `app` である場合以外は、カスタム スタートアップ コマンドが必要となります。

    たとえば、[手順 2](tutorial-deploy-app-service-on-linux-02.md) のコード例のメイン ファイルの名前は *hello.py* で、アプリ オブジェクトの名前は `myapp` であるため、この記事に示すようにカスタム スタートアップ ファイルが必要です。

詳細については、[Python アプリの構成 - コンテナーのスタートアップ プロセス](/azure/app-service/configure-language-python#container-startup-process)に関する記事を参照してください。

## <a name="create-a-startup-file"></a>スタートアップ ファイルを作成する

カスタム スタートアップ ファイルが必要な場合は、次の手順を使用します。

1. スタートアップ コマンドを含んだ *startup.txt* や *startup.sh* (または他の任意の名前) というファイルをプロジェクトに作成します。 Django や Flask などのフレームワークについて詳しくは、この記事の後続のセクションを参照してください。

    スタートアップ ファイルには、必要に応じて複数のコマンドを含めることができます。

1. このファイルを、アプリの他のファイルと一緒にデプロイできるようコード リポジトリにコミットします。

1. **[Azure: App Service]** エクスプローラーで [App Service] を展開し、 **[Application Settings]\(アプリケーション設定\)** を右クリックして、 **[ポータルで開く]** を選択します。

    ![App Service エクスプローラーでアプリケーション設定をポータルで開く](media/deploy-azure/open-application-settings-in-portal-for-app-service.png)

1. Azure portal で必要に応じてサインインし、 **[Configuration]\(構成\)** ページの **[General settings]\(全般設定\)** を選択します。 **[Stack settings]\(スタックの設定\)**  >  **[スタートアップ コマンド]** でスタートアップ ファイルの名前 (*startup.txt*、*startup.sh* など) を入力し、 **[保存]** を選択します。

    ![Azure portal でスタートアップ コマンド ファイルの名前を設定する](media/deploy-azure/enter-startup-file-for-app-service-in-the-azure-portal.png)

    > [!NOTE]
    > スタートアップ コマンド ファイルを使用する代わりに、Azure portal の **[スタートアップ コマンド]** フィールドにスタートアップ コマンド自体を直接指定できます。 ただし、コマンド ファイルの使用をお勧めします。その理由は、これにより構成のこの部分をリポジトリに保存し、そこで変更を監査したり、別の App Service インスタンスにまとめて再デプロイしたりできるためです。

1. 変更を保存すると、App Service が再起動されます。

    ただし、まだアプリのコードをデプロイしていないため、この時点でサイトにアクセスしても "アプリケーション エラー" が表示されます。 このメッセージは、Gunicorn サーバーは起動したものの、アプリが見つからないために、何も HTTP 要求に応答していないことを意味します。 アプリのコードは次の手順でデプロイします。

## <a name="django-startup-commands"></a>Django のスタートアップ コマンド

既定では、*wsgi.py* ファイルが格納されているフォルダーを App Service が自動的に特定し、次のコマンドで Gunicorn を起動します。

```sh
# <module> is the folder that contains wsgi.py. If you need to use a subfolder,
# specify the parent of <module> using --chdir.
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Gunicorn のいずれかの引数を変更したい場合は (`--timeout 1200` を使用するなど)、それらの変更を含んだコマンド ファイルを作成します。

## <a name="flask-startup-commands"></a>Flask のスタートアップ コマンド

既定では、Flask アプリの WSGI 呼び出し可能型が `app` という名前であり、アプリのルート フォルダーに *application.py* または *app.py* というファイル名で存在することを App Service on Linux コンテナーは想定しています。

次のいずれかの種類を使用している場合は、カスタム スタートアップ コマンドでアプリ オブジェクトの場所を *file:app_object* 形式で指定する必要があります。

- **ファイル名またはアプリ オブジェクト名が異なる**: たとえば、アプリのメイン コード ファイルが *hello.py* で、アプリ オブジェクトの名前が `myapp` である場合、スタートアップ コマンドは次のようになります。

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```

- **スタートアップ ファイルがサブフォルダーに存在する**: たとえば、スタートアップ ファイルが *myapp/website.py* で、アプリ オブジェクトが `app` である場合、Gunicorn の `--chdir` 引数を使用してそのフォルダーを指定した後、通常どおりにスタートアップ ファイルとアプリ オブジェクトの名前を指定します。

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir myapp website:app
    ```

- **スタートアップ ファイルがモジュール内に存在する**: [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) コードでは、*webapp.py* スタートアップ ファイルが *hello_app* フォルダーに格納されています。このフォルダーは、それ自体が *\_\_init\_\_.py* ファイルを含んだモジュールとなっています。 このアプリ オブジェクトは `app` という名前で、 *\_\_init\_\_.py* で定義されています。また、*webapp.py* には、相対インポートが使用されています。

    このような配置のため、Gunicorn から `webapp:app` を参照すると "Attempted relative import in non-package (非パッケージでの相対インポートが試行されました)" というエラーが表示され、アプリは起動に失敗します。

    この状況では、モジュールからアプリ オブジェクトをインポートする単純な shim ファイルを作成し、その shim を使用してアプリを起動するよう Gunicorn に命令します。 たとえば、[python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) コードには、次の内容を含んだ *startup.py* が存在します。

    ```python
    # startup.py shim
    from hello_app.webapp import app
    ```

    スタートアップ コマンドは次のとおりです。

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 startup:app
    ```


## <a name="startup-commands-for-other-frameworks-and-web-servers"></a>他のフレームワークと Web サーバーのスタートアップ コマンド

Python アプリを実行する App Service コンテナーには、Django と Flask が gunicorn Web サーバーと共に既定でインストールされています。

Django や Flask 以外のフレームワーク (Falcon、FastAPI など) を使用する場合、または異なる Web サーバーを使用する場合は、次の手順を実行します。

- 対象のフレームワークまたは Web サーバーを *requirements.txt* ファイルに追加します。

- [先ほど Flask に関するセクション](#flask-startup-commands)で取り上げた WSGI の呼び出し可能型をスタートアップ コマンドで特定します。

- gunicorn 以外の Web サーバーを起動するには、サーバーを直接呼び出す代わりに `python -m` コマンドを使用します。 たとえば、次のコマンドは、uvicorn サーバーを起動するものです。WSGI の呼び出し可能型は `app` という名前で、*application.py* に存在することを想定しています。

    ```sh
    python -m uvicorn application:app --host 0.0.0.0
    ```

    `python -m` を使用するのは、*requirements.txt* 経由でインストールされる Web サーバーは Python のグローバル環境には追加されず、直接呼び出すことができないためです。 `python -m` コマンドでは、現在の仮想環境内からサーバーが呼び出されます。

> [!div class="nextstepaction"]
> [スタートアップ ファイルを構成しました - 手順 5 に進む >>>](tutorial-deploy-app-service-on-linux-05.md)

[問題がある場合は、お知らせください。](https://aka.ms/FlaskVSCQuickstartHelp)
