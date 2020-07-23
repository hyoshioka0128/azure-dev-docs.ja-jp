---
title: Azure Database for MySQL で Spring Data JDBC を使用する
description: Azure Database for MySQL データベースで Spring Data JDBC を使用する方法を説明します。
documentationcenter: java
ms.date: 04/07/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 481b3fce5d7d5f62cf4639d53d86c092e364a74f
ms.sourcegitcommit: 04ee2325e3efd9b7797102b4cd9d5db009c38a42
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86945810"
---
# <a name="use-spring-data-jdbc-with-azure-database-for-mysql"></a>Azure Database for MySQL で Spring Data JDBC を使用する

このトピックでは、[Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) を使って [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) から情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[JDBC](https://jcp.org/en/jsr/detail?id=221) は、従来のリレーショナル データベースに接続するための標準の Java API です。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>サンプル アプリケーション

この記事では、サンプル アプリケーションをコーディングします。 より早く進めたい場合は、このアプリケーションは既にコーディングされており、[https://github.com/Azure-Samples/quickstart-spring-data-jdbc-mysql](https://github.com/Azure-Samples/quickstart-spring-data-jdbc-mysql) で入手できます。

[!INCLUDE [spring-data-mysql-setup.md](includes/spring-data-mysql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

次のように入力して、コマン ドラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,mysql -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>Azure Database for MySQL を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開いて、以下を追加します。

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo?serverTimezone=UTC
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_MYSQL_PASSWORD

spring.datasource.initialization-mode=always
```

- 2 つの `$AZ_DATABASE_NAME` 変数を、この記事の冒頭で構成した値に置き換えます。
- `$AZ_MYSQL_PASSWORD` 変数を、この記事の冒頭で構成した値で置き換えます。

> [!WARNING]
> 構成プロパティ `spring.datasource.initialization-mode=always` は、サーバーが起動されるたびに、`schema.sql` ファイル (後ほど作成します) を使用して、Spring Boot がデータベース スキーマを自動的に生成することを意味します。 これはテストには適していますが、再起動するたびにデータが削除されるため、運用環境では使用しないでください。

> [!NOTE]
> 構成プロパティ `spring.datasource.url` に `?serverTimezone=UTC` を追加して、データベースへの接続時に UTC 日付形式 (協定世界時) を使用するように JDBC ドライバーに指示します。 そうしないと、Java サーバーではデータベースと同じ日付形式が使用されず、エラーが発生します。

これで、提供されている Maven Wrapper を使用してアプリケーションを起動できるようになりました。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png)](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png#lightbox)

### <a name="create-the-database-schema"></a>データベース スキーマを作成する

Spring Boot は、データベース スキーマを作成するために *src/main/resources/`schema.sql`* を自動的に実行します。 このファイルを次の内容で作成します。

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

実行中のアプリケーションを停止して再起動します。 これで、アプリケーションは、先ほど作成した `demo` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、JDBC を使用して MySQL サーバーにデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png)](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png#lightbox)

お疲れさまでした。 JDBC を使用して Azure Database for MySQL にデータを格納および取得する Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data JDBC の詳細については、Spring の[リファレンス ドキュメント](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference)を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
