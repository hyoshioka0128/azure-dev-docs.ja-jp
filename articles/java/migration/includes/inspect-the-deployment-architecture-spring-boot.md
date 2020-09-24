---
author: mnriem
ms.author: manriem
ms.date: 5/8/2020
ms.openlocfilehash: 82cf9553654e1efc884542ecdd52d294688291df
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738152"
---
### <a name="inspect-the-deployment-architecture"></a>デプロイ アーキテクチャを検査する

#### <a name="document-hardware-requirements-for-each-service"></a>各サービスのハードウェア要件を文書化する

Spring Boot アプリケーションの次の情報を文書化します。

* 実行中のインスタンスの数。
* 各インスタンスに割り当てられた CPU の数。
* 各インスタンスに割り当てられた RAM の量。

#### <a name="document-geo-replicationdistribution"></a>geo レプリケーション/分散を文書化する

Spring Boot アプリケーション インスタンスが現在複数のリージョンまたはデータセンターに分散しているかどうかを判断します。 移行するアプリケーションの稼働時間要件/SLA を文書化します。
