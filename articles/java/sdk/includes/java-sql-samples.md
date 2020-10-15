---
ms.openlocfilehash: 069524ad8c17db20f90114b1c6c630dce6866290
ms.sourcegitcommit: 8fcb6c2d17be63064090f801f46c9c754821f979
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2020
ms.locfileid: "91828892"
---
|サンプル   |説明  |
|---------|---------|
| [JDBC を使って Azure SQL Database に接続し、そのデータに対するクエリを実行する][4] | サンプル データベースを構成した後、select、insert、update、delete の各コマンドを実行します。 |
| [SQL データベースの作成と管理][1] | SQL データベースを作成してパフォーマンス レベルを設定し、ファイアウォールを構成します。|
| [複数のリージョンにまたがる SQL データベースを管理する][2] | マスター SQL データベースを作成し、複数のリージョンにマスターから読み取り専用のデータベースを作成します。 仮想ネットワークとファイアウォール規則を使用して、VM を最も近い SQL データベース インスタンスに接続します。 | 
| [エラスティック プールで SQL データベースを管理する][3] | エラスティック プールで SQL データベースを作成、削除、移動します。 | 

[1]: https://github.com/Azure-Samples/sql-database-java-manage-db/
[2]: https://github.com/Azure-Samples/sql-database-java-manage-sql-databases-across-regions
[3]: ../java-sdk-manage-sql-elastic-pools.md
[4]: /azure/sql-database/sql-database-connect-query-java