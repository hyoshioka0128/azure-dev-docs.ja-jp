---
title: Azure SQL Database で Spring Data JPA を使用する
description: Azure SQL Database で Spring Data JPA を使用する方法を学習します。
documentationcenter: java
ms.date: 06/19/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: bb25b52bd68c71d9a51a4a33fe278fe5a0148a05
ms.sourcegitcommit: e9accb9d82b5c633dffffd148974911398f2d096
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2020
ms.locfileid: "86018532"
---
# <a name="use-spring-data-jpa-with-azure-sql-database"></a>Azure SQL Database で Spring Data JPA を使用する

このトピックでは、[Spring Data JPA](https://spring.io/projects/spring-data-jpa) を使って [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) で情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[Java Persistence API (JPA)](https://en.wikipedia.org/wiki/Java_Persistence_API) は、オブジェクト リレーショナル マッピング用の標準的な Java API です。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>サンプル アプリケーション

この記事では、サンプル アプリケーションをコーディングします。 より早く進めたい場合は、このアプリケーションは既にコーディングされており、[https://github.com/Azure-Samples/quickstart-spring-data-jpa-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-jpa-sql-server) で入手できます。

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

次のように入力して、コマン ドラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jpa,sqlserver -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Azure SQL Database を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開き、以下を追加します。 必ず、2 つの `$AZ_DATABASE_NAME` 変数と、`$AZ_SQL_SERVER_PASSWORD` 変数を、この記事の冒頭で構成した値に置き換えてください。

```properties
logging.level.org.hibernate.SQL=DEBUG

spring.datasource.url=jdbc:sqlserver://$AZ_DATABASE_NAME.database.windows.net:1433;database=demo;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_SQL_SERVER_PASSWORD

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop
```

> [!WARNING]
> 構成プロパティ `spring.jpa.hibernate.ddl-auto=create-drop` は、アプリケーションの起動時に Spring Boot によってデータベース スキーマが自動的に作成され、シャットダウン時にその削除が試行されることを意味します。 これはテストには最適ですが、運用環境では使用できません。

これで、提供されている Maven Wrapper を使用してアプリケーションを起動できるようになりました。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-01.png)](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-01.png#lightbox)

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、JPA を使用して Azure SQL Database でデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-jpa-create-application.md](includes/spring-data-jpa-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-02.png)](media/configure-spring-data-jpa-with-azure-sql-server/create-sql-server-02.png#lightbox)

お疲れさまでした。 JPA を使用して、Azure SQL Database でデータを格納および取得する Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data JPA の詳細については、Spring の[リファレンス ドキュメント](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference)を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
