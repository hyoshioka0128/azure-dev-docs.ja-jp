---
ms.openlocfilehash: f8efc740f9641b72e8aaf1af29bf0b2620457fdd
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81674228"
---
|サンプル   |説明  |
|---------|---------|
| [JDBC を使って Azure SQL Database に接続し、そのデータに対するクエリを実行する][4] | サンプル データベースを構成した後、select、insert、update、delete の各コマンドを実行します。 |
| [SQL データベースの作成と管理][1] | SQL データベースを作成してパフォーマンス レベルを設定し、ファイアウォールを構成します。|
| [複数のリージョンにまたがる SQL データベースを管理する][2] | マスター SQL データベースを作成し、複数のリージョンにマスターから読み取り専用のデータベースを作成します。 仮想ネットワークとファイアウォール規則を使用して、VM を最も近い SQL データベース インスタンスに接続します。 | 
| [エラスティック プールで SQL データベースを管理する][3] | エラスティック プールで SQL データベースを作成、削除、移動します。 | 

[1]: https://github.com/Azure-Samples/sql-database-java-manage-db/
[2]: https://azure.microsoft.com/resources/samples/sql-database-java-manage-sql-databases-across-regions/
[3]: ../java-sdk-manage-sql-elastic-pools.md
[4]: /azure/sql-database/sql-database-connect-query-java