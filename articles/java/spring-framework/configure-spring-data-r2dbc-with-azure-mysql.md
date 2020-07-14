---
title: Azure Database for MySQL で Spring Data R2DBC を使用する
description: Azure Database for MySQL データベースで Spring Data R2DBC を使用する方法を説明します。
documentationcenter: java
ms.date: 03/18/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: af45b63bba4dcca817ebbe38600ae4a348dc1562
ms.sourcegitcommit: e9accb9d82b5c633dffffd148974911398f2d096
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2020
ms.locfileid: "86018585"
---
# <a name="use-spring-data-r2dbc-with-azure-database-for-mysql"></a>Azure Database for MySQL で Spring Data R2DBC を使用する

このトピックでは、[GitHub の r2dbc-mysql リポジトリ](https://github.com/mirromutth/r2dbc-mysql)の MySQL 用の R2DBC 実装を使用して、[Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) を使って [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) に情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[R2DBC](https://r2dbc.io/) は、従来のリレーショナル データベースにリアクティブ API を提供します。 これを Spring WebFlux と共に使用すると、非ブロッキング API を使用する完全にリアクティブな Spring Boot アプリケーションを作成できます。 "接続ごとに 1 つのスレッド" という従来の手法よりも優れたスケーラビリティが実現されます。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>サンプル アプリケーション

この記事では、サンプル アプリケーションをコーディングします。 より早く進めたい場合は、このアプリケーションは既にコーディングされており、[https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-mysql](https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-mysql) で入手できます。

[!INCLUDE [spring-data-mysql-setup.md](includes/spring-data-mysql-setup.md)]

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

次のように入力して、コマン ドラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-mysql-driver-implementation"></a>リアクティブ MySQL ドライバー実装を追加する

生成されたプロジェクトの *pom.xml* ファイルを開き、[GitHub の r2dbc-mysql リポジトリ](https://github.com/mirromutth/r2dbc-mysql)からリアクティブ MySQL ドライバーを追加します。

`spring-boot-starter-webflux` 依存関係の後に、次のスニペットを追加します。

```xml
<dependency>
   <groupId>dev.miku</groupId>
   <artifactId>r2dbc-mysql</artifactId>
   <version>0.8.1.RELEASE</version>
   <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>Azure Database for MySQL を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開いて、以下を追加します。

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_MYSQL_PASSWORD
```

- 2 つの `$AZ_DATABASE_NAME` 変数を、この記事の冒頭で構成した値に置き換えます。
- `$AZ_MYSQL_PASSWORD` 変数を、この記事の冒頭で構成した値で置き換えます。

> [!NOTE]
> パフォーマンスを向上させるために、`spring.r2dbc.url` プロパティは [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool) を使用して接続プールを使用するように構成されています。

これで、提供されている Maven Wrapper を使用してアプリケーションを起動できるようになりました。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-01.png)](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-01.png#lightbox)

### <a name="create-the-database-schema"></a>データベース スキーマを作成する

[!INCLUDE [spring-data-r2dbc-create-schema.md](includes/spring-data-r2dbc-create-schema.md)]

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

実行中のアプリケーションを停止して再起動します。 これで、アプリケーションは、先ほど作成した `demo` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

データベース テーブルの作成中のスクリーンショットを次に示します。

[![データベース テーブルの作成](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-02.png)](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-02.png#lightbox)

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、R2DBC を使用して MySQL サーバーにデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-03.png)](media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-03.png#lightbox)

お疲れさまでした。 R2DBC を使用して Azure Database for MySQL にデータを格納および取得する、完全なリアクティブ Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data R2DBC の詳細については、Spring の「[リファレンス ドキュメント](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference)」を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
