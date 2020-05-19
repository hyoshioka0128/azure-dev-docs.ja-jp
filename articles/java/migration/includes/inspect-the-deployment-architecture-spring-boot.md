---
author: mriem
ms.author: manriem
ms.date: 5/8/2020
ms.openlocfilehash: f99653f3ce30c629c2c11127089445a4c3072a69
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990244"
---
### <a name="inspect-the-deployment-architecture"></a>デプロイ アーキテクチャを検査する

#### <a name="document-hardware-requirements-for-each-service"></a>各サービスのハードウェア要件を文書化する

Spring Boot アプリケーションの次の情報を文書化します。

* 実行中のインスタンスの数。
* 各インスタンスに割り当てられた CPU の数。
* 各インスタンスに割り当てられた RAM の量。

#### <a name="document-geo-replicationdistribution"></a>geo レプリケーション/分散を文書化する

Spring Boot アプリケーション インスタンスが現在複数のリージョンまたはデータセンターに分散しているかどうかを判断します。 移行するアプリケーションの稼働時間要件/SLA を文書化します。
