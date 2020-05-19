---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: 7b2ad87dfe2e2c7358737ff02ec52b97b1332ae7
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990134"
---
### <a name="inspect-the-deployment-architecture"></a>デプロイ アーキテクチャを検査する

#### <a name="document-hardware-requirements-for-each-service"></a>各サービスのハードウェア要件を文書化する

個々の Spring Cloud サービスについて (構成サーバー、レジストリ、ゲートウェイを除く)、次の情報を文書化します。

* 実行中のインスタンスの数。
* 各インスタンスに割り当てられた CPU の数。
* 各インスタンスに割り当てられた RAM の量。

#### <a name="document-geo-replicationdistribution"></a>geo レプリケーション/分散を文書化する

Spring Cloud アプリケーションが現在複数のリージョンまたはデータセンターに分散しているかどうかを判断します。 移行するアプリケーションの稼働時間要件/SLA を文書化します。

#### <a name="identify-clients-that-bypass-the-service-registry"></a>サービス レジストリを使用しないクライアントを特定する

Spring Cloud サービス レジストリを使用せずに、移行する任意のサービスを呼び出すクライアント アプリケーションを特定します。 移行後は、このような呼び出しはできなくなります。 移行前に、[Spring Cloud OpenFeign](https://spring.io/projects/spring-cloud-openfeign) を使用するように、このようなクライアントを更新します。
