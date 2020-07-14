---
title: Azure SQL Database で Spring Data R2DBC を使用する
description: Azure SQL Database で Spring Data R2DBC を使用する方法を説明します。
documentationcenter: java
ms.date: 04/28/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 64316322d509d588f94185452d5c21fdee6e9ef1
ms.sourcegitcommit: e9accb9d82b5c633dffffd148974911398f2d096
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2020
ms.locfileid: "86018651"
---
# <a name="use-spring-data-r2dbc-with-azure-sql-database"></a>Azure SQL Database で Spring Data R2DBC を使用する

このトピックでは、[GitHub の r2dbc-mssql リポジトリ](https://github.com/r2dbc/r2dbc-mssql)の Microsoft SQL Server 用の R2DBC 実装を使用して、[Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) を使って [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) に情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[R2DBC](https://r2dbc.io/) は、従来のリレーショナル データベースにリアクティブ API を提供します。 これを Spring WebFlux と共に使用すると、非ブロッキング API を使用する完全にリアクティブな Spring Boot アプリケーションを作成できます。 "接続ごとに 1 つのスレッド" という従来の手法よりも優れたスケーラビリティが実現されます。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>サンプル アプリケーション

この記事では、サンプル アプリケーションをコーディングします。 より早く進めたい場合は、このアプリケーションは既にコーディングされており、[https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-sql-server) で入手できます。

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

以下のコマンドを実行し、コマンド ラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-azure-sql-database-driver-implementation"></a>リアクティブ Azure SQL Database ドライバーの実装を追加する

生成されたプロジェクトの *pom.xml* ファイルを開き、[GitHub の r2dbc-mssql リポジトリ](https://github.com/r2dbc/r2dbc-mssql)からリアクティブ Azure SQL Database ドライバーを追加します。

`spring-boot-starter-webflux` 依存関係の後に、次のテキストを追加します。

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-mssql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Azure SQL Database を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開き、以下のテキストを追加します。

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:mssql://$AZ_DATABASE_NAME.database.windows.net:1433/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_SQL_SERVER_PASSWORD
```

2 つの `$AZ_DATABASE_NAME` 変数と、`$AZ_SQL_SERVER_PASSWORD` 変数を、この記事の冒頭で構成した値に置き換えます。

> [!NOTE]
> パフォーマンスを向上させるために、`spring.r2dbc.url` プロパティは [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool) を使用して接続プールを使用するように構成されています。

これで、次のように、提供されている Maven Wrapper を使用してアプリケーションを起動できるはずです。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png#lightbox)

### <a name="create-the-database-schema"></a>データベース スキーマを作成する

[!INCLUDE [spring-data-r2dbc-create-schema.md](includes/spring-data-r2dbc-create-schema.md)]

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

次のコマンドを使用して、実行中のアプリケーションを停止して再起動します。 これで、アプリケーションは、先ほど作成した `demo` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

データベース テーブルの作成中のスクリーンショットを次に示します。

[![データベース テーブルの作成](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png#lightbox)

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、R2DBC を使用して Azure SQL Database サーバーにデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png#lightbox)

お疲れさまでした。 R2DBC を使用して Azure SQL Database にデータを格納および取得する、完全なリアクティブ Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data R2DBC の詳細については、Spring の「[リファレンス ドキュメント](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference)」を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
