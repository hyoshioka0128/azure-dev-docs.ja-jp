---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: e9d6993c96fc90c065cda5f8b3177147c35d0242
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990264"
---
*application.properties* ファイルからの ActiveMQ の例を次に示します。

```properties
spring.activemq.brokerurl=broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
spring.activemq.user=admin
spring.activemq.password=tryandguess
```

ActiveMQ 構成の詳細については、[Spring Boot のメッセージングのドキュメント](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/boot-features-messaging.html)を参照してください。

*application.yaml* ファイルからの IBM MQ の例を次に示します。

```yaml
ibm:
  mq:
    queueManager: qm1
    channel: dev.ORDERS
    connName: localhost(14)
    user: admin
    password: big$ecr3t
```

IBM MQ 構成の詳細については、[IBM MQ Spring コンポーネントのドキュメント](https://github.com/ibm-messaging/mq-jms-spring#ibm-mq-jms-spring-components)を参照してください。
