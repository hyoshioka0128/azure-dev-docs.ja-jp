---
title: Azure SQL Database で Spring Data JDBC を使用する方法
description: Azure SQL Database で Spring Data JDBC を使用する方法を説明します。
services: sql-database
documentationcenter: java
ms.date: 12/19/2018
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: ccbc2c78877e5c687cd463e49b84475495368fa3
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81668798"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-sql-database"></a>Azure SQL Database で Spring Data JDBC を使用する方法

## <a name="overview"></a>概要

この記事では、[Spring Data] を使用して、[Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) を使って [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 内の情報を格納および取得するサンプル アプリケーションの作成方法を説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* [Curl](https://curl.haxx.se/) または機能をテストするための類似の HTTP ユーティリティ。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="create-an-azure-sql-database"></a>Azure SQL Database の作成

### <a name="create-a-sql-database-server-using-the-azure-portal"></a>Azure portal を使用して SQL データベース サーバーを作成する

> [!NOTE]
> 
> Azure SQL データベースの作成に関する詳細については、[Azure portal での Azure SQL データベースの作成](/azure/sql-database/sql-database-get-started-portal)に関するページを参照してください。

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[+ リソースの作成]** をクリックし、 **[データベース]** 、 **[SQL Database]** の順にクリックします。

   ![SQL データベースを作成する][SQL01]

1. 次の情報を指定します。

   * **データベース名**: SQL データベース用に一意の名前を選択します。このデータベースが、指定した SQL Server に後で作成されます。
   * **サブスクリプション**:使用する Azure サブスクリプションを指定します。
   * **[リソース グループ]** :新しいリソース グループを作成するのか、既存のリソース グループを選択するのかを指定します。
   * **ソースの選択**:このチュートリアルでは、`Blank database` を選択して新しいデータベースを作成します。

   ![SQL データベースのプロパティを指定する][SQL02]
   
1. **[サーバー]** 、 **[新規作成]** の順にクリックして、次の情報を指定します。

   - **[サーバー名]** : SQL サーバー用に一意の名前を選択します。この名前は、*wingtiptoyssql.database.windows.net* のような完全修飾ドメイン名の作成に使用されます。
   - **サーバー管理者ログイン**:データベース管理者名を指定します。
   - **[パスワード]** と **[パスワードの確認]** :データベース管理者のパスワードを指定します。
   - **[場所]** :データベースに最も近い地理的リージョンを指定します。


1. 上記の情報をすべて入力したら、 **[OK]** をクリックします。

1. **[確認と作成]** をクリックします。

1. 設定を確認し、 **[作成]** をクリックします。

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a>Azure portal を使用して SQL サーバーのファイアウォール規則を構成する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックし、先ほど作成した SQL サーバーをクリックします。

1. 左側のナビゲーション ウィンドウで、 **[概要]** セクションをクリックし、 **[サーバー ファイアウォールの設定]** をクリックします

   ![ファイアウォール設定の表示][SQL06]

1. **[ファイアウォールと仮想ネットワーク]** セクションで、規則に一意の名前を指定することで新しい規則を作成し、データベースへのアクセス権を必要とする IP アドレスの範囲を入力して、 **[保存]** をクリックします。 (この演習では、クライアントである開発用マシンの IP アドレスです。  これを **[開始 IP アドレス]** と **[終了 IP アドレス]** の両方に使用できます。)

   ![ファイアウォール設定を構成する][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a>Azure portal を使用して SQL サーバーの接続文字列を取得する

1. Azure portal (<https://portal.azure.com/>) を参照し、サインインします。

1. **[すべてのリソース]** をクリックし、先ほど作成した SQL データベースをクリックします。

1. **[接続文字列]** 、 **[JDBC]** の順にクリックし、JDBC のテキスト フィールド内の値をコピーします。

   ![JDBC 接続文字列を取得する][SQL09]

### <a name="create-test-table-in-database"></a>データベースにテスト テーブルを作成する
このデータベースに対してクライアント アプリケーションを実行するために、次の SQL コマンドを使用して新しいテーブルを作成します。

``` SQL
IF NOT EXISTS (SELECT 1 FROM sysobjects WHERE NAME='pet' and XTYPE='U')
  CREATE TABLE pet (
    id      INT           IDENTITY  PRIMARY KEY,
    name    VARCHAR(255),
    species VARCHAR(255)
  );

```

## <a name="configure-the-sample-application"></a>サンプル アプリケーションを構成する

1. コマンド シェルを開き、次の例のように git コマンドを使用してサンプル プロジェクトを複製します。

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. POM ファイルを変更して、次の依存関係を含めます。

```
 <dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.4.1.jre11</version>
 </dependency>
```
1. サンプル プロジェクトの *resources* ディレクトリ内で *application.properties* ファイルを探すか、まだ存在しない場合はファイルを作成します。

1. テキスト エディターで *application.properties* ファイルを開き、このファイルに次の行を追加するか構成して、サンプルの値を前半の該当する値に置き換えます。

   ```yaml
   spring.datasource.driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `spring.datasource.url` | この記事の前半の SQL JDBC 文字列の編集されたバージョンを指定します。 |
   | `spring.datasource.username` | この記事の前半の SQL 管理者名を指定し、その後に短縮サーバー名を追加します。 |
   | `spring.datasource.password` | この記事の前半の SQL 管理者パスワードを指定します。 |

1. *application.properties* ファイルを保存して閉じます。

## <a name="package-and-test-the-sample-application"></a>サンプル アプリケーションをパッケージ化してテストする 

1. サンプル アプリケーションを Maven でビルドします。次に例を示します。

   ```shell
   mvn clean package -P sql
   ```

1. サンプル アプリケーションを開始します。次に例を示します。

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 次の例のように、コマンド プロンプトから `curl` を使用して新しいレコードを作成します。

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   または

``` shell
   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
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

このチュートリアルでは、Spring Data を使用して、JDBC を使って Azure SQL データベース内の情報を格納および取得する Java のサンプル アプリケーションを作成しました。

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

[SQL01]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-09.png
