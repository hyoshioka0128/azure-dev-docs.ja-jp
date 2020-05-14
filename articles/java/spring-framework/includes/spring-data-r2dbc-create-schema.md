---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: 1f73c46d5bc259c94e58a48a8a6bdb883a4454fd
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369809"
---
`DemoApplication` メイン クラス内で、データベース スキーマを作成する新しい Spring Bean を構成します。

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

この Spring Bean では *schema.sql* というファイルが使用されるので、*src/main/resources* フォルダーにそのファイルを作成します。
