---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: d17b69ea5299c24406a63fc6239c350b6cbcbdd4
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990144"
---
#### <a name="jms-message-brokers"></a>JMS メッセージ ブローカー

関連する依存関係のビルド マニフェスト (通常は *pom.xml* または *build.gradle* ファイル) を調べて、使用中のブローカーを特定します。

たとえば、ActiveMQ を使用する Spring Boot アプリケーションには、通常、この依存関係が *pom.xml* ファイルに含まれています。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

独自のブローカーを使用する Spring Boot アプリケーションには、通常、ブローカーの JMS ドライバー ライブラリへの直接的な依存関係が含まれています。 *build.gradle* ファイルの例を次に示します。

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```
