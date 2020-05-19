---
title: Azure SQL Database で Spring Data R2DBC を使用する
description: Azure SQL Database で Spring Data R2DBC を使用する方法を説明します。
documentationcenter: java
ms.date: 04/28/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 057f261de707adac2ab3ef9ac52834a9848362b6
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369998"
---
# <a name="use-spring-data-r2dbc-with-azure-sql-database"></a>Azure SQL Database で Spring Data R2DBC を使用する

このトピックでは、[GitHub の r2dbc-mssql リポジトリ](https://github.com/r2dbc/r2dbc-mssql)の Microsoft SQL Server 用の R2DBC 実装を使用して、[Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) を使って [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) に情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[R2DBC](https://r2dbc.io/) は、従来のリレーショナル データベースにリアクティブ API を提供します。 これを Spring WebFlux と共に使用すると、非ブロッキング API を使用する完全にリアクティブな Spring Boot アプリケーションを作成できます。 "接続ごとに 1 つのスレッド" という従来の手法よりも優れたスケーラビリティが実現されます。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="prepare-the-working-environment"></a>作業環境を準備する

まず、次のコマンドを使用して、いくつかの環境変数を設定します。

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_SQL_SERVER_USERNAME=spring
AZ_SQL_SERVER_PASSWORD=<YOUR_AZURE_SQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

プレースホルダーは、この記事全体で使用される次の値に置き換えてください。

- `<YOUR_DATABASE_NAME>`:Azure SQL Database サーバーの名前。 Azure 全体で一意である必要があります。
- `<YOUR_AZURE_REGION>`:使用する Azure リージョン。 既定で `eastus` を使用できますが、居住地に近いリージョンを構成することをお勧めします。 「`az account list-locations`」を入力すると、使用可能なリージョンの完全な一覧を表示できます。
- `<AZ_SQL_SERVER_PASSWORD>`:Azure SQL Database サーバーのパスワード。 パスワードは 8 文字以上にする必要があります。 これには、英大文字、英小文字、数字 (0 から 9)、英数字以外の文字 (!、$、#、% など) のうち、3 つのカテゴリの文字が含まれている必要があります。
- `<YOUR_LOCAL_IP_ADDRESS>`:ローカル コンピューターの IP アドレス。そこから、Spring Boot アプリケーションを実行します。 これを確認する簡単な方法は、ブラウザーで [whatismyip.akamai.com](http://whatismyip.akamai.com/)にアクセスすることです。

次に、リソース グループを作成します。

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
    | jq
```

> [!NOTE]
> JSON データを表示して読みやすくするために [Azure Cloud Shell](https://shell.azure.com/) に既定でインストールされている `jq` ユーティリティを使用します。
> このツールを使用しない場合は、使用するすべてのコマンドの `| jq` の部分を削除しても問題ありません。

## <a name="create-an-azure-sql-database-instance"></a>Azure SQL Database インスタンスを作成する

最初に作成するのは、マネージド Azure SQL Database サーバーです。

> [!NOTE]
> Azure SQL Database サーバーの作成の詳細については、「[クイックスタート: Azure SQL Database の単一データベースを作成する](/azure/sql-database/sql-database-single-database-get-started)」を参照してください。

[Azure Cloud Shell](https://shell.azure.com/) で次のスクリプトを実行します。

```azurecli
az sql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --admin-user $AZ_SQL_SERVER_USERNAME \
    --admin-password $AZ_SQL_SERVER_PASSWORD \
    | jq
```

このコマンドにより、Azure SQL Database サーバーが作成されます。

### <a name="configure-a-firewall-rule-for-your-azure-sql-database-server"></a>Azure SQL Database サーバーのファイアウォール規則を構成する

Azure SQL Database インスタンスは、既定でセキュリティ保護されています。 受信接続を一切許可しないファイアウォールがあります。 データベースを使用できるようにするには、データベース サーバーにアクセスするためのローカル IP アドレスを許可するファイアウォール規則を追加する必要があります。

この記事の冒頭でローカル IP アドレスを構成したので、次を実行してサーバーのファイアウォールを開くことができます。

```azurecli
az sql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-azure-sql-database"></a>Azure SQL データベースを構成する

先ほど作成した Azure SQL Database サーバーは空です。 Spring Boot アプリケーションで使用できるデータベースはありません。 `demo` という新しいデータベースを作成します。

```azurecli
az sql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server $AZ_DATABASE_NAME \
    | jq
```

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

次のように入力して、コマン ドラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.0.RC1 -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-azure-sql-database-driver-implementation"></a>リアクティブ Azure SQL Database ドライバーの実装を追加する

生成されたプロジェクトの *pom.xml* ファイルを開き、[GitHub の r2dbc-mssql リポジトリ](https://github.com/r2dbc/r2dbc-mssql)からリアクティブ Azure SQL Database ドライバーを追加します。

`spring-boot-starter-webflux` 依存関係の後に、次のスニペットを追加します。

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-mssql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>Azure SQL Database を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開いて、以下を追加します。

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:mssql://$AZ_DATABASE_NAME.database.windows.net:1433/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_SQL_SERVER_PASSWORD
```

- 2 つの `$AZ_DATABASE_NAME` 変数を、この記事の冒頭で構成した値に置き換えます。
- `$AZ_SQL_SERVER_PASSWORD` 変数を、この記事の冒頭で構成した値で置き換えます。

> [!NOTE]
> パフォーマンスを向上させるために、`spring.r2dbc.url` プロパティは [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool) を使用して接続プールを使用するように構成されています。

これで、提供されている Maven Wrapper を使用してアプリケーションを起動できるようになりました。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

[![実行中のアプリケーション](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png#lightbox)

### <a name="create-the-database-schema"></a>データベース スキーマを作成する

[!INCLUDE [spring-data-r2dbc-create-schema.md](includes/spring-data-r2dbc-create-schema.md)]

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

実行中のアプリケーションを停止して再起動します。 これで、アプリケーションは、先ほど作成した `demo` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

データベース テーブルの作成中のスクリーンショットを次に示します。

[![データベース テーブルの作成](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png#lightbox)

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、R2DBC を使用して Azure SQL Database サーバーにデータを格納および取得する Java コードを追加します。

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png#lightbox)

お疲れさまでした。 R2DBC を使用して Azure SQL Database にデータを格納および取得する、完全なリアクティブ Spring Boot アプリケーションを作成しました。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>その他のリソース

Spring Data R2DBC の詳細については、Spring の「[リファレンス ドキュメント](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference)」を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
