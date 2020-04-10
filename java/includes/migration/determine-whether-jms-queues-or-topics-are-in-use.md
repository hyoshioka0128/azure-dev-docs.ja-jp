---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a521396292214fd4e68b4625a0e5f5bcfca8be92
ms.sourcegitcommit: 951fc116a9519577b5d35b6fb584abee6ae72b0f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2020
ms.locfileid: "80612129"
---
### <a name="determine-whether-java-message-service-jms-queues-or-topics-are-in-use"></a>Java Message Service (JMS) キューまたはトピックが使用中かどうか確認する

アプリケーションで JMS キューまたはトピックを使用している場合は、外部でホストされている JMS サーバーにそれらを移行する必要があります。 Azure Service Bus と Advanced Message Queuing Protocol (AMQP) は、JMS を使用している場合の優れた移行方法となります。 詳細については、「[Azure Service Bus と AMQP 1.0 で Java Message Service (JMS) を使用する](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)」を参照してください。

JMS 永続ストアが構成されている場合は、それらの構成を把握して、移行後に適用する必要があります。
