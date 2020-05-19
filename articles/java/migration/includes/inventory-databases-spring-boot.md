---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: 6bf4c54c8f428e3ffce79a7ba9ac49aae028e2fd
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990234"
---
#### <a name="databases"></a>データベース

SQL データベースが存在する場合は、接続文字列を特定します。

Spring Boot アプリケーションの場合、接続文字列は通常、構成ファイルに記載があります。 

*application.properties* ファイルの例を次に示します。

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

*application.yaml* ファイルの例を次に示します。

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

使用可能な構成シナリオについては、Spring Data のドキュメントを参照してください。

* [JPA リポジトリ](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories)
* [JDBC リポジトリ](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#jdbc.repositories)
* [Cassandra リポジトリ](https://docs.spring.io/spring-data/cassandra/docs/current/reference/html/#cassandra.repositories)
* [MongoDB リポジトリ](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/#mongodb.repositories)
