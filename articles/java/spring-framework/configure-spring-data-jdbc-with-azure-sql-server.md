---
title: Azure SQL Database で Spring Data JDBC を使用する
description: Azure SQL Database で Spring Data JDBC を使用する方法を説明します。
documentationcenter: java
ms.date: 05/18/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: cd0823246482f37822fab6326628e9aab01ebfbd
ms.sourcegitcommit: e9accb9d82b5c633dffffd148974911398f2d096
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2020
ms.locfileid: "86018513"
---
# <a name="use-spring-data-jdbc-with-azure-sql-database"></a>Azure SQL Database で Spring Data JDBC を使用する

このトピックでは、[Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) を使って [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) で情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) は、従来のリレーショナル データベースに接続するための標準の Java API です。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>サンプル アプリケーション

この記事では、サンプル アプリケーションをコーディングします。 より早く進めたい場合は、このアプリケーションは既にコーディングされており、[https://github.com/Azure-Samples/quickstart-spring-data-jdbc-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-jdbc-sql-server) で入手できます。

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

以下のコマンドを実行し、コマンド ラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,sqlserver -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Azure SQL Database を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開き、以下のテキストを追加します。

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:sqlserver://$AZ_DATABASE_NAME.database.windows.net:1433;database=demo;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_SQL_SERVER_PASSWORD

spring.datasource.initialization-mode=always
```

2 つの `$AZ_DATABASE_NAME` 変数と、`$AZ_SQL_SERVER_PASSWORD` 変数を、この記事の冒頭で構成した値に置き換えます。

> [!WARNING]
> 構成プロパティ `spring.datasource.initialization-mode=always` は、サーバーが起動されるたびに、`schema.sql` ファイル (後ほど作成します) を使用して、Spring Boot がデータベース スキーマを自動的に生成することを意味します。 これはテストには適していますが、再起動するたびにデータが削除されるため、運用環境では使用しないでください。

これで、次のように、提供されている Maven Wrapper を使用してアプリケーションを起動できるはずです。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-01.png)](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-01.png#lightbox)

### <a name="create-the-database-schema"></a>データベース スキーマを作成する

Spring Boot は、データベース スキーマを作成するために *src/main/resources/schema.sql* を自動的に実行します。 このファイルを作成し、次の内容を追加します。

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

次のコマンドを使用して、実行中のアプリケーションを停止して再起動します。 これで、アプリケーションは、先ほど作成した `demo` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、JDBC を使用して Azure SQL Database サーバーにデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-02.png)](media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-02.png#lightbox)

お疲れさまでした。 JDBC を使用して、Azure SQL Database でデータを格納および取得する Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data JDBC の詳細については、Spring の[リファレンス ドキュメント](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference)を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
