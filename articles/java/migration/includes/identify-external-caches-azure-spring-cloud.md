---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: f2bc0442aafcfffc963d4e5a6da18085d9205a22
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990164"
---
#### <a name="identify-external-caches"></a>外部キャッシュを特定する

使用中の外部キャッシュを特定します。 一般的に、Redis が Spring Data Redis 経由で使用されています。 構成情報については、[Spring Data Redis](https://spring.io/projects/spring-data-redis) のドキュメントを参照してください。

各構成 ([Java](https://docs.spring.io/spring-session/docs/current/reference/html5/#httpsession-redis-jc) または [XML](https://docs.spring.io/spring-session/docs/current/reference/html5/#httpsession-redis-xml)) を検索して、[Spring Session](https://spring.io/projects/spring-session) 経由でセッション データをキャッシュするかどうかを決定します。
