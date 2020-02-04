---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: e36fae7adfda8c05e222151872b137fa600cdd97
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830770"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>JMS キューまたはトピックが使用中かどうか確認する

アプリケーションで JMS キューまたはトピックを使用している場合は、外部でホストされている JMS サーバーにそれらを移行する必要があります。 Azure Service Bus と Advanced Message Queuing Protocol は、JMS を使用している場合の優れた移行方法となります。 詳細については、「[Azure Service Bus と AMQP 1.0 で Java Message Service (JMS) を使用する](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)」を参照してください。

JMS 永続ストアが構成されている場合は、それらの構成を把握して、移行後に適用する必要があります。

Oracle メッセージ ブローカーを使用している場合は、このソフトウェアを Azure 仮想マシンに移行し、そのまま使用できます。
