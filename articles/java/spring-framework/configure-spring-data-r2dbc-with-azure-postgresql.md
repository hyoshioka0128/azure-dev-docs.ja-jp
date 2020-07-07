---
title: Azure Database for PostgreSQL で Spring Data R2DBC を使用する
description: Azure Database for PostgreSQL データベースで Spring Data R2DBC を使用する方法を説明します。
documentationcenter: java
ms.date: 03/18/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: bd3208cc68b863afe2ce39e83d62d358a4baea96
ms.sourcegitcommit: 0d492c9cc9b5295285ab75da55e5ab0577576287
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2020
ms.locfileid: "85370733"
---
# <a name="use-spring-data-r2dbc-with-azure-database-for-postgresql"></a>Azure Database for PostgreSQL で Spring Data R2DBC を使用する

このトピックでは、[Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) を使って [Azure Database for PostgreSQL](/azure/postgresql/) データベースで情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。 このサンプルでは、GitHub の [r2dbc-postgresql](https://github.com/pgjdbc/r2dbc-postgresql) リポジトリにある、PostgreSQL 用の R2DBC 実装を使用します。

[R2DBC](https://r2dbc.io/) は、従来のリレーショナル データベースにリアクティブ API を提供します。 これを Spring WebFlux と共に使用すると、非ブロッキング API を使用する完全にリアクティブな Spring Boot アプリケーションを作成できます。 "接続ごとに 1 つのスレッド" という従来の手法よりも優れたスケーラビリティが実現されます。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

[!INCLUDE [spring-data-postgresql-setup.md](includes/spring-data-postgresql-setup.md)]

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

以下のコマンドを使用して、コマンド ラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.0.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-postgresql-driver-implementation"></a>リアクティブ PostgreSQL ドライバー実装を追加する

生成されたプロジェクトの *pom.xml* ファイルを開いてから、[GitHub の r2dbc-postgresql リポジトリ](https://github.com/pgjdbc/r2dbc-postgresql)にあるリアクティブ PostgreSQL ドライバーを追加します。 `spring-boot-starter-webflux` 依存関係の後に、次のテキストを追加します。

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>Azure Database for PostgreSQL を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開き、以下のテキストを追加します。

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:postgres://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_POSTGRESQL_PASSWORD
spring.r2dbc.properties.sslMode=REQUIRE
```

> [!WARNING]
> セキュリティ上の理由から、Azure Database for PostgreSQL では SSL 接続を使用する必要があります。 このため、`spring.r2dbc.properties.sslMode=REQUIRE` 構成プロパティを追加する必要があります。そうしないと、R2DBC PostgreSQL ドライバーはセキュリティで保護されていない接続を使用して接続を試み、これは失敗します。

2 つの `$AZ_DATABASE_NAME` 変数と、`$AZ_POSTGRESQL_PASSWORD` 変数を、この記事の冒頭で構成した値に置き換えます。

> [!NOTE]
> パフォーマンスを向上させるために、`spring.r2dbc.url` プロパティは [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool) を使用して接続プールを使用するように構成されています。

これで、次のように、提供されている Maven Wrapper を使用してアプリケーションを起動できるはずです。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-01.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-01.png#lightbox)

### <a name="create-the-database-schema"></a>データベース スキーマを作成する

[!INCLUDE [spring-data-r2dbc-create-schema.md](includes/spring-data-r2dbc-create-schema.md)]

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

次のコマンドを使用して、実行中のアプリケーションを停止して再起動します。 これで、アプリケーションは、先ほど作成した `demo` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

データベース テーブルの作成中のスクリーンショットを次に示します。

[![データベース テーブルの作成](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-02.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-02.png#lightbox)

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、R2DBC を使用して PostgreSQL サーバーにデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-03.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-03.png#lightbox)

お疲れさまでした。 R2DBC を使用して Azure Database for PostgreSQL にデータを格納および取得する、完全なリアクティブ Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data R2DBC の詳細については、Spring の「[リファレンス ドキュメント](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference)」を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
