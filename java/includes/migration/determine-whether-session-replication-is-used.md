---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 4c20feba0a91d10846963f8697457735e5932c20
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830730"
---
### <a name="determine-whether-session-replication-is-used"></a>セッション レプリケーションが使用されているかどうか確認する

Oracle の Coherence*Web の有無にかかわらず、アプリケーションがセッション レプリケーションに依存している場合は、次の 3 つの選択肢があります。

1. Coherence*Web は、Azure 仮想マシンで WebLogic Server と共に実行できますが、オファーをプロビジョニングした後に、このオプションを手動で構成する必要があります。 スタンドアロン Coherence を使用している場合は、Azure の仮想マシンでそれを実行することもできますが、オファーをプロビジョニングした後に、このオプションを手動で構成する必要があります。
2. セッション管理にデータベースを使用するようにアプリケーションをリファクターします。
3. Azure Redis サービスへのセッションを外部化するようにアプリケーションをリファクターします。 詳細については、「[Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview)」を参照してください。

これらのすべての選択肢において、WebLogic が HTTP セッション状態のレプリケーションを行う方法を習得することをお勧めします。 詳細については、Oracle のドキュメントの「[HTTP セッション状態のレプリケーション](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/failover.html#GUID-E13D8142-66BA-46A1-854F-4FC6F82992DD)」を参照してください。
