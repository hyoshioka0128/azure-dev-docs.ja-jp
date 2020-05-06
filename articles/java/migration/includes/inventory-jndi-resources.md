---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 171dac6ad7e2761d58f63d173cc78464cedb6dc5
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673338"
---
### <a name="inventory-jndi-resources"></a>JNDI リソースをインベントリする

すべての JNDI リソースをインベントリします。 たとえば、データベースなどのデータソースには、関連付けられている JNDI 名が存在することがあります。これによって、JPA が特定のデータベースに `EntityManager` のインスタンスを正しくバインドできます。 JNDI リソースとデータベースの詳細については、Oracle ドキュメントの「[WebLogic Server データ・ソース](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html)」を参照してください。 JMS メッセージ ブローカーなど、その他の JNDI 関連リソースには、移行または再構成が必要になる場合があります。 JMS 構成の詳細については、「[JMS リソース構成の理解](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/jmsad/overview.html)」を参照してください。
