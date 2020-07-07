---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: fa66c4e9db481e31853c8e67816a14b6ee753fd2
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507511"
---
`DemoApplication` メイン クラス内で、次のコードを使用し、データベース スキーマを作成する新しい Spring Bean を構成します。

```java
    @Bean
    public ConnectionFactoryInitializer initializer(ConnectionFactory connectionFactory) {
        ConnectionFactoryInitializer initializer = new ConnectionFactoryInitializer();
        initializer.setConnectionFactory(connectionFactory);
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator(new ClassPathResource("schema.sql"));
        initializer.setDatabasePopulator(populator);
        return initializer;
    }
```

この Spring Bean では *schema.sql* というファイルが使用されるので、*src/main/resources* フォルダーにそのファイルを作成し、次のテキストを追加します。
