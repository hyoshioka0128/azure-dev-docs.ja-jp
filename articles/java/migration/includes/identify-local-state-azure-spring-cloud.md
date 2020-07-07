---
author: yevster
ms.author: yebronsh
ms.date: 5/28/2020
ms.openlocfilehash: 70f54f911d97febf30b78888e6922e2d4ac3eb54
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84512621"
---
#### <a name="identify-local-state"></a>ローカルの状態を特定する

PaaS 環境では、任意のタイミングでアプリケーションが厳密に 1 回実行されるという保証はありません。 1 つのインスタンスで実行するようにアプリケーションを構成するときでも、次のようなケースでは、重複するインスタンスが作成されることがあります。

* 故障またはシステム更新に起因して、物理ホストにアプリケーションを再配置する必要がある。
* アプリケーションが更新中である。

いずれの場合でも、新しいインスタンスが起動を完了するまで、元のインスタンスは実行されたままになります。 これにはアプリケーションにとって次のような大きな意味が含まれている可能性があります。

* [シングルトン](https://en.wikipedia.org/wiki/Singleton_pattern)が実際に 1 つであるとは保証されない。
* 外部ストレージに保存されていないデータは、単一の物理サーバーまたは VM の場合よりずっと早く失われる可能性がある。

Azure Spring Cloud に移行する前に、失われたり、重複したりしてはならないローカルの状態がコードに確実に含まれていないようにしてしてください。 ローカルの状態が存在する場合、アプリケーションの外部にその状態を格納するようにコードを変更します。 クラウド対応アプリケーションでは通常、次のような場所にアプリケーションの状態が格納されます。

* [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-java-get-started)
* [Azure CosmosDB](/azure/cosmos-db/create-sql-api-java)
* [Azure SQL](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview)、[Azure DB for MySQL](/azure/mysql/overview)、[Azure DB for PostgreSQL](/azure/postgresql/overview) など、別の外部データベース。
