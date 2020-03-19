---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: b2458a80eccfcae24a3364208334cce3aedea861
ms.sourcegitcommit: 21ddeb9bd9abd419d143dc2ca8a7c821a1758cf9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128902"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>JMS キューまたはトピックが使用中かどうか確認する

アプリケーションで JMS キューまたはトピックを使用している場合は、外部でホストされている JMS サーバーにそれらを移行する必要があります (Azure Service Bus など。[メッセージ ブローカーとしての Service Bus の使用](/azure/service-bus-messaging/message-transfers-locks-settlement)に関するページをご覧ください)。

JMS 永続ストアが構成されている場合は、それらの構成を把握して、移行後に適用する必要があります。
