---
title: チュートリアル:Azure portal を使用して PostgreSQL で Django アプリをデプロイする
description: Azure で Web アプリと PostgreSQL データベースをプロビジョニングし、GitHub からアプリ コードをデプロイします。
ms.devlang: python
ms.topic: tutorial
ms.date: 07/23/2020
ms.custom: tracking-python
ms.openlocfilehash: f3c667ab720a3e8c82b59dc439b8033697b4fc2a
ms.sourcegitcommit: b224b276a950b1d173812f16c0577f90ca2fbff4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810595"
---
# <a name="tutorial-deploy-a-django-web-app-with-postgresql-using-the-azure-portal"></a>チュートリアル:Azure portal を使用して PostgreSQL で Django Web アプリをデプロイする

Azure portal を使用して、データ ドリブンの Python [Django](https://www.djangoproject.com/) Web アプリを [Azure App Service](/azure/app-service/containers/app-service-linux-intro) にデプロイし、それを [Azure Database for PostgreSQL](/azure/postgresql/) データベースに接続できます。 Free 価格レベルで始めて、後からいつでもスケールアップすることができます。

この場合の Web アプリ コードは、GitHub リポジトリから取得されます。GitHub からの継続的なデプロイのために Web アプリを構成します。 構成の完了後、ローカル コンピューターでさらに開発を行い、変更をリポジトリにコミットできます。 Azure の Web アプリは、これらの変更を自動的にデプロイします。

このチュートリアルでは、Azure portal を使用して次のタスクを実了します。

> [!div class="checklist"]
> - GitHub リポジトリからデプロイする Web アプリを Azure でプロビジョニングする
> - Azure で PostgreSQL サーバーとデータベースをプロビジョニングし、Web アプリに接続する
> - GitHub から自動的に再デプロイするために、コードを更新し、変更をコミットする
> - 診断ログを表示する
> - Azure portal で Web アプリを管理する

[このチュートリアルの Azure CLI ベースのバージョン](/azure/app-service/containers/tutorial-python-postgresql-app?tabs=bash%2Cclone)も使用できます。

## <a name="fork-the-sample-repository"></a>サンプル リポジトリをフォークする

ブラウザーで、[https://github.com/Azure-Samples/djangoapp](https://github.com/Azure-Samples/djangoapp) にアクセスし、ご自身の GitHub アカウントにリポジトリをフォークします。

このリポジトリのフォークを作成すると、後の手順で変更を行ったり、コードを再デプロイしたりすることができます。

**(省略可能) サンプルについて:** djangoapp サンプルには、データ ドリブンの Django 投票アプリが含まれます。これを、Django ドキュメントの「[はじめての Django アプリ作成](https://docs.djangoproject.com/en/2.1/intro/tutorial01/)」に従って取得します。 また、このサンプルは、[Django デプロイ チェックリスト](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/)を使用して変更され、Azure App Service などの運用環境で実行されます。 (これらの変更は運用環境に対して行われ、Azure に固有ではありません)。

- 運用環境の設定は、*azuresite/production.py* ファイルにあります。 開発の詳細は *azuresite/settings.py* にあります。

- `DJANGO_ENV` 環境変数を "production" に設定した場合に、アプリで運用環境の設定が使用されます。 この環境変数は、PostgreSQL データベース構成に使用する他のものと共に、チュートリアルの後半で作成します。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="provision-the-web-app-in-azure"></a>Azure で Web アプリをプロビジョニングする

1. [Azure portal](https://portal.azure.com) を開きます。

1. **[リソースの作成]** を選択します。

1. **[新規]** ページの **[人気]** 列で **[Web アプリ]** を選択します ( **[Web アプリ]** がすぐに表示されない場合は、 **[Azure Marketplace]** で **[Web]** を選択し、 **[おすすめ]** で **[Web アプリ]** を選択します)。 

1. **[Web アプリの作成]** ページで、次の情報を入力します。

    | フィールド | 値 |
    | --- | --- |
    | サブスクリプション | 使用するサブスクリプションを選択します (既定のものとは異なる場合)。 |
    | Resource group | **[新規作成]** を選択し、「DjangoPostgres-Tutorial-rg」と入力します。 |
    | アプリの名前 | Azure 全体で一意となる Web アプリの名前 (アプリの URL は `https://\<app-name>.azurewebsites.net` です)。 有効な文字は、`A`-`Z`、`0`-`9`、および `-` です。 会社名とアプリ識別子を組み合わせて使用すると、適切なパターンになります。 |
    | 発行 | **[コード]** を選択します。 |
    | ランタイム スタック | ドロップダウン リストから **[Python 3.8]** を選択します。 |
    | リージョン | お近くの場所を選択します。 |
    | Linux プラン | ポータルでは、このフィールドに、お使いのリソース グループに基づく App Service プラン名が入力されます。 名前を変更する場合は、 **[新規作成]** を選択します。 |
    | SKU とサイズ | 最適なパフォーマンスを得るには、サブスクリプションで料金が発生しますが、既定のプランを使用します。 料金が発生しないようにするには、 **[サイズの変更]** を選択し、 **[Dev/Test]** を選択し、 **[B1]** (30 日間無料) を選択し、 **[適用]** を選択します。 パフォーマンスを向上させるために、後でプランを拡張できます。 |

1. **[確認および作成]** を選択し、 **[作成]** を選択します。 Azure での Web アプリのプロビジョニングには数分かかります。

1. プロビジョニングが完了したら、 **[リソースに移動]** を選択して、Web アプリの概要ページを開きます。 後の手順のために、このブラウザー ウィンドウまたはタブは開いたままにしてください。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="provision-the-postgresql-database-server-in-azure"></a>Azure で PostgreSQL データベース サーバーをプロビジョニングする

1. [Azure portal](https://portal.azure.com) で新しいブラウザー ウィンドウまたはタブを開きます。 データベースをプロビジョニングするために新しいタブを使用します。これは、データベース ページから、前のセクションから開いたままの Web アプリ ページに情報を転送する必要があるためです。

1. **[リソースの作成]** を選択します。

1. **[新規]** ページの **[Azure Marketplace]** で **[データベース]** を選択し、 **[おすすめ]** で **[Azure Database for PostgreSQL]** を選択します。

1. 次のページの **[単一サーバー]** で **[作成]** を選択します。

1. **[単一サーバー]** ページで、次の情報を入力します。

    | フィールド | 値 |
    | --- | --- |
    | サブスクリプション | 使用するサブスクリプションを選択します (既定のものとは異なる場合)。 |
    | Resource group | 前のセクションで作成した "DjangoPostgres-Tutorial-rg" グループを選択します。 |
    | サーバー名 | Azure 全体で一意となるデータベース サーバーの名前 (アプリの URL は `https://\<server-name>.postgres.database.azure.com` です)。 有効な文字は、`A`-`Z`、`0`-`9`、および `-` です。 会社名とサーバー識別子を組み合わせて使用すると、適切なパターンになります。 |
    | データ ソース | **なし** |
    | 場所 | お近くの場所を選択します。 |
    | Version | 既定値 (最新バージョン) のままにします。 |
    | コンピューティングとストレージ | **[サーバーの構成]** を選択し、 **[Basic]** および **[Gen 5]** を選択します。 **[仮想コア]** を 1 に設定し、 **[ストレージ]** を 5 GB に設定し、 **[OK]** を選択します。 これらの選択により、Azure で PostgreSQL 用に使用できる最もコストの低いサーバーがプロビジョニングされます。 |
    | 管理者ユーザー名、パスワード、パスワードの確認入力 | データベース サーバーの管理者アカウントの資格情報を入力します。 これらの資格情報は、このチュートリアルの後半で必要になるため、記録しておいてください。 |

1. **[確認および作成]** 、 **[作成]** の順に選択します。 Azure での Web アプリのプロビジョニングには数分かかります。

1. プロビジョニングが完了したら、 **[リソースに移動]** を選択して、データベース サーバーの概要ページを開きます。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="create-the-pollsdb-database-on-the-postgresql-server"></a>PostgreSQL サーバーで pollsdb データベースを作成する

このセクションでは、Azure Cloud Shell でデータベース サーバーに接続し、PostgreSQL コマンドを使用してサーバー上に "pollsdb" データベースを作成します。 このデータベースは、サンプル アプリのコードで想定されています。

1. PostgreSQL サーバーの概要ページで、(左側の **[設定]** から) **[接続のセキュリティ]** を選択します。

    ![ファイアウォール規則のためのポータル接続セキュリティ ページ](media/tutorial-python-postgresql-app-portal/server-firewall-rules.png)

1. **[0.0.0.0 から 255.255.255.255 を追加する]** というラベルのボタンを選択し、表示されるポップアップ メッセージで **[続行]** を選択し、ページの上部にある **[保存]** を選択します。 これらのアクションにより、(Django データ モデルの移行を実行する後のセクションで行うように) Cloud Shell と SSH からデータベース サーバーに接続できる規則が追加されます。

1. Azure portal から、ウィンドウの上部にある [Cloud Shell] アイコンを選択して、Azure Cloud Shell を開きます。

    ![Azure portal ツールバーの [Cloud Shell] ボタン](media/tutorial-python-postgresql-app-portal/portal-launch-icon.png)

1. Cloud Shell で、次のコマンドを実行します。

    ```bash
    psql --host=<server-name>.postgres.database.azure.com --port=5432 --username=<user-name>@<server-name> --dbname=postgres
    ```

    `<server-name>` と `<user-name>` を、サーバーの構成時に前のセクションで使用した名前に置き換えます。 完全なユーザー名の値は `<user-name>@<server-name>` であることに注意してください。

    上のコマンドをコピーし、右クリックして **[貼り付け]** を選択することで、Cloud Shell に貼り付けることができます。

    管理者のパスワードを求められたら、入力します。

1. シェルが正常に接続されると、`postgres=>` プロンプトが表示されます。 このプロンプトにより、"postgres" という名前の既定の管理データベースに接続されていることが示されます ("postgres" データベースは、アプリの使用を目的としていません)。

1. プロンプトで、`CREATE DATABASE pollsdb;` コマンドを実行します。 必ず最後にセミコロンを含めて、コマンドを完成させてください。

1. データベースが正常に作成されると、コマンドにより `CREATE DATABASE` が表示されます。 データベースが作成されたことを確認するには、`\c pollsdb` を実行します。 このコマンドにより、プロンプトが `pollsdb=>` に変更されます。これは、成功を示します。

1. `exit` コマンドを実行して psql を終了します。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="connect-the-database"></a>データベースに接続する

このセクションでは、`pollsdb` データベースに接続するために必要な Web アプリの設定を作成します。 これらの設定は、環境変数としてアプリ コードに出現します (詳細については、「[環境変数へのアクセス](/azure/app-service/containers/how-to-configure-python#access-environment-variables)」を参照してください)。

1. 前のセクションで作成した Web アプリのブラウザー タブまたはウィンドウに戻ります。

1. (左側の **[設定]** で) **[構成]** を選択し、ページの上部にある **[アプリケーションの設定]** を選択します。

    ![Web アプリのためのポータル設定の構成](media/tutorial-python-postgresql-app-portal/web-app-settings.png)

1. **[新しいアプリケーション設定]** ボタンを使用して、次の各値の設定を作成します。

    | 設定名 | 値 |
    | --- | --- |
    | DJANGO_ENV | `production` (この値は、上記の[サンプルの概要](#fork-the-sample-repository)で説明したように、運用環境の構成を使用するようにアプリに指示します)。 |
    | DBHOST | 前のセクションのデータベース サーバーの URL (`<server-name>.postgres.database.azure.com` 形式)。 データベース サーバーの [概要] ページから URL 全体をコピーできます。 |
    | DBNAME | `pollsdb` |
    | DBUSER | 前のセクションで使用した完全な管理者ユーザー名。 完全なユーザー名は、この場合も `<user-name>@<server-name>` です。 |
    | DBPASS | 前に作成した管理者パスワード。 |

1. **[保存]** を選択し、 **[続行]** を選択して設定を適用します。

    > [!IMPORTANT]
    > 設定を変更した後に **[保存]** を選択することは必須です。 **[新しいアプリケーション設定]** ボタンを使用して作成した設定は、 **[保存]** を使用するまで適用されません。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="deploy-app-code-to-the-web-app-from-a-repository"></a>リポジトリから Web アプリにアプリ コードをデプロイする

データベースと接続の設定が整ったら、GitHub リポジトリから直接コードをデプロイするように Web アプリを構成できます。

1. Web アプリのブラウザー ウィンドウまたはタブで、(左側の **[デプロイ]** の下の) **[デプロイ センター]** を選択します。

1. **[ソース管理]** ステップで、 **[GitHub]** を選択し、サインイン プロンプトに従うことを選択するか、 **[続行]** を選択して現在の GitHub ログインを使用します。

1. **[ビルド プロバイダー]** ステップで、 **[App Service のビルド サービス]** を選択し、 **[続行]** を選択します。

1. **[構成]** ステップで、次の値を選択します。

    | フィールド | 値 |
    | --- | --- |
    | Organization | サンプルリ ポジトリをフォークした対象の GitHub アカウント。 |
    | リポジトリ | djangoapp |
    | [Branch]\(ブランチ) | master |

1. **[続行]** を選択してリポジトリを選択し、 **[完了]** を選択します。 Azure で、数秒以内にコードがデプロイされ、アプリが開始されます。

    App Service では、各サブフォルダー内で *wsgi.py* ファイルを探すことにより、Django プロジェクトが検出されます。 App Service でそのファイルが見つかると、Django Web アプリが読み込まれます。 詳細については、[組み込み Python イメージの構成](/azure/app-service/containers/how-to-configure-python)に関する記事を参照してください。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="run-django-database-migrations"></a>Django データベースの移行を実行する

コードをデプロイし、データベースを配置すると、アプリを使用する準備はほぼ完了です。 残りの作業は、データベース自体に必要なスキーマを確立することだけです。 これを行うには、Django アプリのデータ モデルをデータベースに "移行" します。

1. Web アプリのブラウザー ウィンドウまたはタブで、(左側の **[開発ツール]** の下の) **[SSH]** を選択します。 Web アプリ サーバーで SSH コンソールを開きます。 Web アプリ コンテナーを開始する必要があるため、初回の接続には数分かかることがあります。

1. コンソールで、Web アプリのフォルダーに移動します。

    ```bash
    cd site/wwwroot
    ```

1. Python パッケージをインストールする:

    ```bash
    pip install -r requirements.txt
    ```

1. データベースの移行を実行します。

    ```bash
    python manage.py migrate
    ```

1. アプリの管理者ログインを作成します。

    ```bash
    python manage.py createsuperuser
   ```

    `createsuperuser` コマンドを実行すると、Web アプリ内で使用される、Django スーパーユーザー (または管理者) の資格情報を入力するように求められます。 このチュートリアルでは、既定のユーザー名である `root` を使用し、**Enter** キーを押してメール アドレスを空白のままにして、パスワードとして `Pollsdb1` を入力します。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

### <a name="create-a-poll-question-in-the-app"></a>アプリで投票の質問を作成する

これで、アプリが PostgreSQL データベースで動作していることを示すための簡単なテストを実行する準備ができました。

1. Web アプリのブラウザー ウィンドウまたはタブで、 **[概要]** ページに戻り、Web アプリの **URL** (`http:\//\<app-name>.azurewebsites.net` 形式) を選択します。

1. データベース内に特定の投票がまだないため、アプリには "No polls are available" (投票は利用できません) というメッセージが表示されます。

1. `http:\//\<app-name>.azurewebsites.net/admin` ([Django Administration]\(Django 管理\) ページ) に移動し、前のセクションのスーパーユーザーの資格情報 (`root` と `Pollsdb1`) を使用してサインインします。

1. **[Polls]\(投票\)** で、 **[Questions]\(質問\)** の横の **[Add]\(追加\)** を選択し、いくつかの選択肢がある投票の質問を作成します。

1. もう一度 `http:\//\<app-name>.azurewebsites.net/` に移動し、質問がユーザーに表示されるようになったことを確認します。 質問に自由に回答してデータベースにデータを生成します。

**お疲れさまでした。** アクティブな PostgreSQL データベースを使用して、Azure App Service for Linux で Python Django Web アプリが実行されています。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="update-the-app-and-redeploy"></a>アプリを更新して再デプロイする

このチュートリアルで既に説明したように、GitHub リポジトリに変更をコミットするたびに、Azure によりアプリ コードが再デプロイされます。

ただし、Django アプリのデータ モデルを変更する場合は、それらの変更をデータベースに移行する必要があります。

1. 「[Django データベースの移行を実行する](#run-django-database-migrations)」の説明に従って、SSH 経由で再度 Web アプリに接続します。

1. `cd site/wwwroot` を使用してアプリ フォルダーに移動します。

1. `python manage.py migrate` を使用して移行を再実行します。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="view-diagnostic-logs"></a>診断ログを表示する

Azure 上でアプリをホストするコンテナー内から生成されたコンソール ログにアクセスできます。

Azure portal の Web アプリのページで、(左側の **[監視]** の下の) **[ログ ストリーム]** を選択します。 コンソール出力としてログが表示されます。

`https://<app-name>.scm.azurewebsites.net/api/logs/docker` で、ブラウザーからログ ファイルを検査することもできます。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="clean-up-resources"></a>リソースをクリーンアップする

今後の開発作業のために、アプリとデータベースを必要なだけ実行したままにしておくことができます。 それ以外の場合は、継続的な課金が発生しないようにするために、このチュートリアルのために作成したリソース グループを削除します。これにより、それに含まれるすべてのリソースが削除されます。

1. Azure portal で、ウィンドウの上部にある検索バーに「DjangoPostgres-Tutorial-rg」と入力し、 **[リソース グループ]** で同じ名前を選択します。

1. [リソース グループ] ページで、 **[リソース グループの削除]** を選択します。

1. プロンプトが表示されたら、リソース グループの名前を入力し、 **[削除]** を選択します。

[問題がある場合は、お知らせください。](https://aka.ms/DjangoPortalTutorialHelp)

## <a name="next-steps"></a>次のステップ

App Service で Python アプリが実行される方法を確認する:

> [!div class="nextstepaction"]
> [Python アプリの構成](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python)