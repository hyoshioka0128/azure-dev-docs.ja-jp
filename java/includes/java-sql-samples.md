---
ms.openlocfilehash: 2dcb0d79f484927bece4d64b397aeae6283232c8
ms.sourcegitcommit: 6fa28ea675ae17ffb9ac825415e2e26a3dfe7107
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "77013805"
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
[4]: https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-java