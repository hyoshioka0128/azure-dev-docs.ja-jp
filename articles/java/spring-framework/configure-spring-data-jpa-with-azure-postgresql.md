---
title: Azure PostgreSQL で Spring Data JPA を使用する方法
description: Azure Database for PostgreSQL データベース で Spring Data JPA を構成して使用する方法について説明します。
documentationcenter: java
ms.date: 12/19/2018
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.topic: conceptual
ms.openlocfilehash: 723d98afd5f82a733011db207f8f462888c95e57
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369794"
---
# <a name="how-to-use-spring-data-jpa-with-azure-postgresql"></a>Azure PostgreSQL で Spring Data JPA を使用する方法

この記事では、[Spring Data] を使用して、[Java Persistence API (JPA)](https://docs.oracle.com/javaee/7/tutorial/persistence-intro.htm) を使って [Azure Database for PostgreSQL](/azure/postgresql/) データベース内の情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* [Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。
* [psql](https://www.postgresql.org/docs/current/app-psql.html) コマンドライン ユーティリティ。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="create-a-postgresql-database-for-azure"></a>Azure 用に PostgreSQL データベースを作成する

### <a name="create-a-postgresql-database-server-using-the-azure-portal"></a>Azure portal を使用して PostgreSQL データベース サーバーを作成する

> [!NOTE]
> 
> PostgreSQL データベースの作成に関する詳細については、[Azure Portal を使用した Azure Database for PostgreSQL サーバーの作成](/azure/postgresql/quickstart-create-server-database-portal)に関するページを参照してください。

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[+ リソースの作成]** をクリックし、 **[データベース]** 、 **[Azure Database for PostgreSQL]** の順にクリックします。

   ![PostgreSQL データベースを作成する][POSTGRESQL01]

1. **[単一サーバー]** または **[Hyperscale server group]\(Hyperscale サーバー グループ\)** を選択します。

1. **Create** をクリックしてください。

1. 次の情報を入力します。

   - **サブスクリプション**:使用する Azure サブスクリプションを指定します。
   - **[リソース グループ]** :新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。
   - **[サーバー名]** : PostgreSQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoyspostgresql.postgres.database.azure.com* のような完全修飾ドメイン名の作成に使用されます。
   - **データ ソース**:このチュートリアルでは、`Blank` を選択して新しいデータベースを作成します。
   - **管理者ユーザー名**:データベース管理者名を指定します。
   - **[パスワード]** と **[パスワードの確認]** :データベース管理者のパスワードを指定します。
   - **[場所]** :データベースに最も近い地理的リージョンを指定します。
   - **バージョン**:最新のデータベース バージョンを指定します。

   ![PostgreSQL データベースのプロパティを作成する][POSTGRESQL02]

1. 上記の情報をすべて入力したら、 **[確認と作成]** をクリックします。

1. 選択内容を見直して確認し、 **[作成]** をクリックします。

### <a name="configure-a-firewall-rule-for-your-postgresql-database-server-using-the-azure-portal"></a>Azure portal を使用して PostgreSQL データベース サーバーのファイアウォール規則を構成する

1. デプロイが完了したら、 **[リソースに移動]** をクリックします。

1. **[接続のセキュリティ]** をクリックします。

1. 規則に一意の名前を指定することで新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、 **[保存]** をクリックします。 (この演習では、クライアントである開発用マシンの IP アドレスです。  これを **[開始 IP アドレス]** と **[終了 IP アドレス]** の両方に使用できます。)

   ![PostgreSQL データベースを選択する][POSTGRESQL03]

### <a name="retrieve-the-connection-string-for-your-postgresql-server-using-the-azure-portal"></a>Azure portal を使用して PostgreSQL サーバーの接続文字列を取得する

1. **[すべてのリソース]** ページで、先ほど作成した PostgreSQL データベースをクリックします。

1. **[接続文字列]** をクリックし、 **[JDBC]** テキスト フィールド内の値をコピーします。

   ![JDBC 接続文字列を取得する][POSTGRESQL05]

### <a name="create-postgresql-database-using-the-psql-command-line-utility"></a>`psql` コマンド ライン ユーティリティを使用して PostgreSQL データベースを作成する

1. コマンド シェルを開き、次の例のように `psql` コマンドを入力して PostgreSQL サーバーに接続します。

   ```shell
   psql --host=wingtiptoyspostgresql.postgres.database.azure.com --port=5432 --username=wingtiptoysuser@wingtiptoyspostgresql --dbname=postgres
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `host` | この記事の前半の完全修飾 PostgreSQL サーバー名を指定します。 |
   | `port` | PostgreSQL サーバー ポートを指定します。既定では `5432` です。 |
   | `username` | この記事の前半の PostgreSQL 管理者と短縮サーバー名を指定します。 |
   | `dbname` | 差し当たり既定の `postgres` データベースを使用することを指定します。 |

   PostgreSQL サーバーは、次の例のような表示で応答します。

   ```shell
   psql (9.3.24, server 10.5)
   SSL connection (cipher: ECDHE-RSA-AES256-SHA384, bits: 256)
   Type "help" for help.
   
   postgres=>
   ```
   > 注:サーバーがこの IP アドレスを認識しないというエラーが表示される場合は、クライアントが使用している IP アドレスがエラーに示されます。  前に戻って、前の手順で説明したようにこれを割り当てます。*Azure portal を使用してサーバーのファイアウォール規則を構成する*。

1. 次の例のように `psql` コマンドを入力して、*mypgsqldb* という名前のデータベースを作成します。

   ```SQL
   CREATE DATABASE mypgsqldb;
   ```

   PostgreSQL サーバーは、次の例のような表示で応答します。

   ```shell
   CREATE DATABASE
   ```

1. 省略可能:データベースが作成されたことは、`psql` で `\l` を入力して確認できます。PostgreSQL サーバーは次の例のように応答します。

   ```shell
                   List of databases
          Name        |      Owner      | Encoding
   -------------------+-----------------+----------
    azure_maintenance | azure_superuser | UTF8
    azure_sys         | azure_superuser | UTF8
    mypgsqldb         | wingtiptoysuser | UTF8
    postgres          | azure_superuser | UTF8
    template0         | azure_superuser | UTF8
    template1         | azure_superuser | UTF8
   (6 rows)
   ```

1. `\q` を入力して `psql` ユーティリティを終了します。

## <a name="configure-the-sample-application"></a>サンプル アプリケーションを構成する

1. コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jpa-on-azure.git
   ```

1. サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。

1. テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。

   ```yaml
   spring.datasource.url=jdbc:postgresql://wingtiptoyspostgresql.postgres.database.azure.com:5432/mypgsqldb?ssl=true&sslmode=prefer
   spring.datasource.username=wingtiptoysuser@wingtiptoyspostgresql
   spring.datasource.password=********
    ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `spring.datasource.url` | この記事の前半の PostgreSQL JDBC 文字列を指定します。 |
   | `spring.datasource.username` | この記事の前半の PostgreSQL 管理者名を指定し、その後に短縮サーバー名を追加します。 |
   | `spring.datasource.password` | この記事の前半の PostgreSQL 管理者パスワードを指定します。 |

1. *application.properties* ファイルを保存して閉じます。

> [!NOTE]
> *application.properties* ファイルの最初のプロパティは `spring.jpa.hibernate.ddl-auto=create` です。これは、アプリケーションの起動時にデータベース スキーマを自動的に削除して再作成する休止プロパティです。
> この構成は開発およびテスト時に有用ですが、運用環境では使用しないでください。

## <a name="package-and-test-the-sample-application"></a>サンプル アプリケーションをパッケージ化してテストする

1. サンプル アプリケーションを Maven でビルドします。次に例を示します。

   ```shell
   mvn clean package -P postgresql
   ```

1. サンプル アプリケーションを開始します。次に例を示します。

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d '{"name":"cat","species":"feline"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   アプリケーションから次のような値が返されます。

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して既存のすべてのレコードを取得します。

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   アプリケーションから次のような値が返されます。

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>まとめ

このチュートリアルでは、Spring Data を使用して、JPA を使って Azure PostgreSQL データベース 内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

<!-- URL List -->

[Java 開発者向けの Azure]: /azure/developer/java/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[POSTGRESQL01]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-01.png
[POSTGRESQL02]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-02.png
[POSTGRESQL03]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-03.png
[POSTGRESQL04]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-04.png
[POSTGRESQL05]: media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-05.png
