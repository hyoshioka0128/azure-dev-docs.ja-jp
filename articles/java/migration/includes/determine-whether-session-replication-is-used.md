---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 26d0422b9ea569b5657a5b7841319e77ce0f1684
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673538"
---
### <a name="determine-whether-session-replication-is-used"></a>セッション レプリケーションが使用されているかどうか確認する

Oracle の Coherence*Web の有無にかかわらず、アプリケーションがセッション レプリケーションに依存している場合は、次の 3 つの選択肢があります。

* Coherence*Web は、Azure 仮想マシンで WebLogic Server と共に実行できますが、オファーをプロビジョニングした後に、このオプションを手動で構成する必要があります。 スタンドアロン Coherence を使用している場合は、Azure の仮想マシンでそれを実行することもできますが、オファーをプロビジョニングした後に、このオプションを手動で構成する必要があります。
* セッション管理にデータベースを使用するようにアプリケーションをリファクターします。
* Azure Redis サービスへのセッションを外部化するようにアプリケーションをリファクターします。 詳細については、「[Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview)」を参照してください。

これらのすべての選択肢において、WebLogic が HTTP セッション状態のレプリケーションを行う方法を習得することをお勧めします。 詳細については、Oracle のドキュメントの「[HTTP セッション状態のレプリケーション](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/failover.html#GUID-E13D8142-66BA-46A1-854F-4FC6F82992DD)」を参照してください。
