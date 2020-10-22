---
title: Azure Database for MySQL で Spring Data JPA を使用する
description: Azure Database for MySQL データベースで Spring Data JPA を使用する方法を学習します。
documentationcenter: java
ms.date: 10/12/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 5df2d48a10c3d4c312c4106a73fb719943f12b85
ms.sourcegitcommit: 76f1a47c58810486856e0d128bd154cf7d355e65
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92200577"
---
# <a name="use-spring-data-jpa-with-azure-database-for-mysql"></a>Azure Database for MySQL で Spring Data JPA を使用する

このトピックでは、[Spring Data JPA](https://spring.io/projects/spring-data-jpa) を使って [Azure Database for MySQL](/azure/mysql/) で情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[Java Persistence API (JPA)](https://en.wikipedia.org/wiki/Java_Persistence_API) は、オブジェクト リレーショナル マッピング用の標準的な Java API です。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>サンプル アプリケーション

この記事では、サンプル アプリケーションをコーディングします。 より早く進めたい場合は、このアプリケーションは既にコーディングされており、[https://github.com/Azure-Samples/quickstart-spring-data-jpa-mysql](https://github.com/Azure-Samples/quickstart-spring-data-jpa-mysql) で入手できます。

[!INCLUDE [spring-data-mysql-setup.md](includes/spring-data-mysql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

次のように入力して、コマン ドラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jpa,mysql -d baseDir=azure-database-workshop -d bootVersion=2.3.4.RELEASE -d javaVersion=8 | tar -xzvf -
```

> [!NOTE]
> Spring Initializr では、既定のバージョンとして Java 11 が使用されます。 このトピックで説明されている Spring Boot Starter を使用するには、代わりに Java 8 を選択する必要があります。

### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>Azure Database for MySQL を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開き、以下を追加します。 必ず、2 つの `$AZ_DATABASE_NAME` 変数と、`$AZ_MYSQL_PASSWORD` 変数を、この記事の冒頭で構成した値に置き換えてください。

```properties
logging.level.org.hibernate.SQL=DEBUG

spring.datasource.url=jdbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo?serverTimezone=UTC
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_MYSQL_PASSWORD

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop
```

> [!WARNING]
> 構成プロパティ `spring.jpa.hibernate.ddl-auto=create-drop` は、アプリケーションの起動時に Spring Boot によってデータベース スキーマが自動的に作成され、シャットダウン時にその削除が試行されることを意味します。 これはテストには最適ですが、運用環境では使用できません。

> [!NOTE]
> 構成プロパティ `spring.datasource.url` に `?serverTimezone=UTC` を追加して、データベースへの接続時に UTC 日付形式 (協定世界時) を使用するように JDBC ドライバーに指示します。 そうしないと、Java サーバーではデータベースと同じ日付形式が使用されず、エラーが発生します。

これで、提供されている Maven Wrapper を使用してアプリケーションを起動できるようになりました。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png)](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png#lightbox)

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、JPA を使用して MySQL サーバーでデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-jpa-create-application.md](includes/spring-data-jpa-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png)](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png#lightbox)

お疲れさまでした。 JPA を使用して Azure Database for MySQL でデータを格納および取得する Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data JPA の詳細については、Spring の[リファレンス ドキュメント](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference)を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](../index.yml)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
