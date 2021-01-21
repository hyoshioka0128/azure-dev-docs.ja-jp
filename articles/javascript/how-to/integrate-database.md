---
title: Azure 上の Node.js アプリを使用したデータベース
description: Azure には、Web アプリや他の Node.js アプリと組み合わせて使用されるさまざまなデータベースが用意されています。
ms.topic: how-to
ms.date: 12/08/2020
ms.custom: devx-track-js
ms.openlocfilehash: b39a7d3e39600081148893a68d3dbc064c1db380
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561688"
---
# <a name="integrate-databases-in-nodejs-apps"></a>データベースを Node.js アプリに統合する

Azure データベースは、データベースに接続するためのネイティブ API または Azure SDK と共に、優れたマネージド クラウド データ ソリューションを提供します。 

## <a name="database-and-data-storage-solutions-on-azure"></a>Azure でのデータベースとデータ ストレージのソリューション

次の表は、Node.js で Azure のデータベースに接続して使用する方法を説明した各種記事へのリンク一覧です。 各種データベース オプションを比較した一覧については、「[データベース - フル マネージドのインテリジェント データベース サービス](https://azure.microsoft.com/product-categories/databases/)」を参照してください。

| サービス | クイックスタート | npm パッケージ |
| --- | --- | --- |
| Cosmos DB での **SQL Server**| [SQL Server を使用して Node.js Azure Cosmos DB Web アプリを作成する](/azure/cosmos-db/create-sql-api-nodejs) | [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos) |
| Cosmos DB での **MongoDB**| [Node.js の MongoDB Web アプリの作成](/azure/app-service-web/app-service-web-tutorial-nodejs-mongodb-app) | 任意の MongoDB クライアント |
| Cosmos DB での **Cassandra**|[Node.js SDK と Azure Cosmos DB を使用して Cassandra アプリを構築する](/azure/cosmos-db/create-cassandra-nodejs)|[npm cassandra-driver](https://www.npmjs.com/package/cassandra-driver)|
| Cosmos DB での **Gremlin**|[Azure Cosmos DB Gremlin API アカウントを使用して Node.js アプリケーションをビルドする](/azure/cosmos-db/create-graph-nodejs)|[npm gremlin](https://www.npmjs.com/package/gremlin)|
| Cosmos DB での **Redis Cache**| [Redis Cache の作成と使用](/azure/redis-cache/cache-nodejs-get-started) | [npm redis](https://www.npmjs.com/package/redis)|
| **Azure SQL データベース** | [Node.js を使用して Azure SQL データベースに照会する](/azure/sql-database/sql-database-connect-query-nodejs) |[npm tedious](https://www.npmjs.com/package/tedious) |
| **MySQL** | [Node.js を使った接続とデータの照会](/azure/mysql/connect-nodejs) | [npm mysql](https://www.npmjs.com/package/mysql)|
| **PostgreSQL** | [Node.js を使った接続とデータの照会](/azure/postgresql/connect-nodejs) |[npm pg](https://www.npmjs.com/package/pg) |

## <a name="cosmos-db-connection-strings-with-azure-cli"></a>Azure CLI での Cosmos DB 接続文字列

次のコマンド [az cosmosdb keys list](/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings) を使用します。

```azurecli-interactive
az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type connection-strings
```

## <a name="sql-connection-strings-with-azure-cli"></a>Azure CLI での SQL 接続文字列

次のコマンド [az sql db show-connection-string](/cli/azure/sql/db#az_sql_db_show_connection_string) を使用します。

```azurecli-interactive
az sql db show-connection-string \
    --client {ado.net, jdbc, odbc, php, php_pdo, sqlcmd} \
     [--auth-type {ADIntegrated, ADPassword, SqlPassword}] \
     [--ids] \
     [--name] \
     [--server] \
     [--subscription]
```

## <a name="mysql-username-and-password-with-azure-cli"></a>Azure CLI での MySQL のユーザー名とパスワード

これらは、[リソースの作成時に](/cli/azure/mysql/server#az_mysql_server_create)設定されます。 

## <a name="postgresql-username-and-password-with-azure-cli"></a>Azure CLI での PostgreSQL ユーザー名とパスワード

これらは、[リソースの作成時に](/cli/azure/postgres/server#az_postgres_server_create)設定されます。 

## <a name="azure-storage-solutions-for-files-and-data"></a>ファイルとデータの Azure Storage ソリューション

ファイル (BLOB)、テーブル、キュー (メッセージ) のストレージには、Azure Storage を使用することもできます。

| サービス | クイックスタート |推奨される SDK |
| --- | --- |--- |
| **BLOB** | [Azure Storage v10 SDK for JavaScript を使用して BLOB のアップロード、ダウンロード、一覧表示、削除を行う](/azure/storage/blobs/storage-quickstart-blobs-nodejs-v10) |[@azure/storage-blob](https://www.npmjs.com/package/@azure/storage-blob)|
| **キュー** | [Node.js から Queue ストレージを使用する方法](/azure/storage/queues/storage-nodejs-how-to-use-queues) |[npm @azure/storage-queue](https://www.npmjs.com/package/@azure/storage-queue)|
| **テーブル** | [Node.js から Table ストレージを使用する方法](/azure/cosmos-db/table-storage-how-to-use-nodejs) |[npm azure-storage](https://www.npmjs.com/package/azure-storage)|

## <a name="next-steps"></a>次のステップ

* Azure Functions を使用して[サーバーレス コードを作成する](develop-serverless-apps.md)