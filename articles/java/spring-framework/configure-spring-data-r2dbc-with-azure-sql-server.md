---
title: Azure SQL Database で Spring Data R2DBC を使用する
description: Azure SQL Database で Spring Data R2DBC を使用する方法を説明します。
documentationcenter: java
ms.date: 04/28/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 80ccbbc84e4d23ff9083777f38615eb5d676e484
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82766161"
---
# <a name="use-spring-data-r2dbc-with-azure-sql-database"></a>Azure SQL Database で Spring Data R2DBC を使用する

このトピックでは、[GitHub の r2dbc-mssql リポジトリ](https://github.com/r2dbc/r2dbc-mssql)の Microsoft SQL Server 用の R2DBC 実装を使用して、[Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) を使って [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) に情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[R2DBC](https://r2dbc.io/) は、従来のリレーショナル データベースにリアクティブ API を提供します。 これを Spring WebFlux と共に使用すると、非ブロッキング API を使用する完全にリアクティブな Spring Boot アプリケーションを作成できます。 "接続ごとに 1 つのスレッド" という従来の手法よりも優れたスケーラビリティが実現されます。

## <a name="prerequisites"></a>前提条件

- Azure アカウント。 所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) または [Azure CLI](/cli/azure/install-azure-cli)。 Azure Cloud Shell をお勧めします。これにより、自動的にログインし、必要なすべてのツールにアクセスできるようになります。
- [Java 8](https://www.azul.com/downloads/zulu/) (Azure Cloud Shell に付属)。
- [cURL](https://curl.haxx.se) または機能をテストするための類似の HTTP ユーティリティ。

## <a name="prepare-the-working-environment"></a>作業環境を準備する

まず、次のコマンドを使用して、いくつかの環境変数を設定します。

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_SQL_SERVER_USERNAME=r2dbc
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

先ほど作成した Azure SQL Database サーバーは空です。 Spring Boot アプリケーションで使用できるデータベースはありません。 `r2dbc` という新しいデータベースを作成します。

```azurecli
az sql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name r2dbc \
    --server $AZ_DATABASE_NAME \
    | jq
```

## <a name="create-a-reactive-spring-boot-application"></a>リアクティブ Spring Boot アプリケーションを作成する

リアクティブ Spring Boot アプリケーションを作成するために、[Spring Initializr](https://start.spring.io/) を使用します。 作成するアプリケーションでは、以下が使用されます。

- Spring Boot 2.3.0 M4。
- Java 8 (ただし、Java 11 などの新しいバージョンでも機能します)。
- 次の依存関係: Spring Reactive Web (Spring WebFlux とも呼ばれる) およびSpring Data R2DBC。

### <a name="generate-the-application-by-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

次のように入力して、コマン ドラインでアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-r2dbc-workshop -d bootVersion=2.3.0.M4 -d javaVersion=8 | tar -xzvf -
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

spring.r2dbc.url=r2dbc:pool:mssql://$AZ_DATABASE_NAME.database.windows.net:1433/r2dbc
spring.r2dbc.username=r2dbc@$AZ_DATABASE_NAME
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

`DemoApplication` メイン クラス内で、データベース スキーマを作成する新しい Spring Bean を構成します。

```java
    @Bean
    public ConnectionFactoryInitializer initializer(ConnectionFactory connectionFactory) {
        ConnectionFactoryInitializer initializer = new ConnectionFactoryInitializer();
        initializer.setConnectionFactory(connectionFactory);
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator(new ClassPathResource("schema.sql"));
        initializer.setDatabasePopulator(populator);
        return initializer;
    }
```

この Spring Bean では *schema.sql* というファイルが使用されるので、*src/main/resources* フォルダーにそのファイルを作成します。

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

次のコマンドを使用して、アプリケーションを停止してから再実行します。 これで、アプリケーションは、先ほど作成した `r2dbc` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

データベース テーブルの作成中のスクリーンショットを次に示します。

[![データベース テーブルの作成](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png#lightbox)

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、R2DBC を使用して Azure SQL Database サーバーにデータを格納および取得する Java コードを追加します。

`DemoApplication` クラスの横に新しい `Todo` Java クラスを作成します。

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

このクラスは、先ほど作成した `todo` テーブルにマップされるドメイン モデルです。

このクラスを管理するには、リポジトリが必要です。 同じパッケージ内に新しい `TodoRepository` インターフェイスを定義します。

```java
package com.example.demo;

import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface TodoRepository extends ReactiveCrudRepository<Todo, Long> {
}
```

このリポジトリは、Spring Data R2DBC によって管理されるリアクティブ リポジトリです。

データを格納および取得できるコントローラーを作成して、アプリケーションを完成させます。 同じパッケージに `TodoController` クラスを実装し、次のコードを追加します。

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Todo> createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Flux<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

最後に、アプリケーションを停止して再起動します。

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>アプリケーションをテストする

アプリケーションをテストするには、cURL を使用します。

まず、データベースに新しい "todo" 項目を作成します。

```bash
curl  --header "Content-Type: application/json" \
          --request POST \
          --data '{"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done": "true"}' \
          http://127.0.0.1:8080
```

次のコマンドを実行すると、作成した項目が返されます。

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}
```

次に、新しい cURL 要求を使用してデータを取得します。

```bash
curl http://127.0.0.1:8080
```

このコマンドを実行すると、作成した項目を含む "todo" 項目の一覧が返されます。

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```

これらの cURL 要求のスクリーンショットを次に示します。

[![cURL を使用してテストする](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png#lightbox)

お疲れさまでした。 R2DBC を使用して Azure SQL Database にデータを格納および取得する、完全なリアクティブ Spring Boot アプリケーションを作成しました。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

このクイックスタートで使用したすべてのリソースをクリーンアップするには、リソース グループを削除します。

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Spring Data R2DBC の詳細については、Spring の「[リファレンス ドキュメント](https://docs.spring.io/spring-data/r2dbc/docs/1.0.x/reference/html/#reference)」を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/developer/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。
