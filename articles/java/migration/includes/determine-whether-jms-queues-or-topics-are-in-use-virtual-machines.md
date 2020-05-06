---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 69bddfce67388d3392e6908f3ddf1af378b116cf
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673488"
---
### <a name="determine-whether-java-message-service-jms-queues-or-topics-are-in-use"></a>Java Message Service (JMS) キューまたはトピックが使用中かどうか確認する

アプリケーションで JMS キューまたはトピックを使用している場合は、外部でホストされている JMS サーバーにそれらを移行する必要があります。 Azure Service Bus と Advanced Message Queuing Protocol は、JMS を使用している場合の優れた移行方法となります。 詳細については、「[Azure Service Bus と AMQP 1.0 で Java Message Service (JMS) を使用する](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)」を参照してください。

JMS 永続ストアが構成されている場合は、それらの構成を把握して、移行後に適用する必要があります。

Oracle メッセージ ブローカーを使用している場合は、このソフトウェアを Azure 仮想マシンに移行し、そのまま使用できます。
