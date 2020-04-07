---
title: Azure Database for MySQL で Spring Data R2DBC を使用する方法
description: Azure Database for MySQL データベースで Spring Data R2DBC を使用する方法を説明します。
documentationcenter: java
ms.date: 03/18/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: article
ms.author: judubois
ms.openlocfilehash: f5c9f3d0cccad7f6e1a8b5ab199f841925f416cc
ms.sourcegitcommit: a65fa8dbb168bd39e225a293d9ee73d18ece1864
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366753"
---
# <a name="how-to-use-spring-data-r2dbc-with-azure-mysql"></a>Azure MySQL で Spring Data R2DBC を使用する方法

このトピックでは、[https://github.com/mirromutth/r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql) の MySQL 用の R2DBC 実装を使用して、[Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc)を使って [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/)から情報を格納および取得するサンプル アプリケーションを作成する方法を説明します。

[R2DBC](https://r2dbc.io/) は、従来のリレーショナル データベースにリアクティブ API を提供します。 これを Spring WebFlux と共に使用すると、非ブロッキング API を使用し、"接続ごとに 1 つのスレッド" という従来の手法よりも優れたスケーラビリティを実現して、完全にリアクティブな Spring Boot アプリケーションを作成できます。

## <a name="prerequisites"></a>前提条件

- Azure アカウント。 所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) または [Azure CLI](/cli/azure/install-azure-cli)。 Azure Cloud Shell をお勧めします。これにより、自動的にログインし、必要なすべてのツールにアクセスできるようになります。
- [Java 8](https://www.azul.com/downloads/zulu/) (Azure Cloud Shell に付属)。
- [cURL](https://curl.haxx.se) または機能をテストするための類似の HTTP ユーティリティ。

## <a name="prepare-the-working-environment"></a>作業環境を準備する

まず、次のコマンドを使用して、いくつかの環境変数を設定します。

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_MYSQL_USERNAME=r2dbc
AZ_MYSQL_PASSWORD=<YOUR_MYSQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

プレースホルダーは、このトピック全体で使用される次の値に置き換えてください。

- `<YOUR_DATABASE_NAME>`:MySQL サーバー インスタンスの名前。 Azure 全体で一意である必要があります。
- `<YOUR_AZURE_REGION>`: 使用する Azure リージョン。 既定で `eastus` を使用できますが、居住地に近いリージョンを構成することをお勧めします。 「`az account list-locations`」を入力すると、使用可能なリージョンの完全な一覧を表示できます。
- `<YOUR_MYSQL_PASSWORD>`: MySQL データベース サーバーのパスワード。 このパスワードは少なくとも 8 文字で、英大文字、英小文字、数字 (0 から 9)、英数字以外の文字 (!、$、#、% など) のうち、3 つのカテゴリの文字が含まれている必要があります。
- `<YOUR_LOCAL_IP_ADDRESS>`: ローカル コンピューターの IP アドレス。そこから、Spring Boot アプリケーションを実行します。 これを確認する簡単な方法は、ブラウザーで [http://ipv4.icanhazip.com](http://ipv4.icanhazip.com) にアクセスすることです。

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

## <a name="create-an-azure-database-for-mysql"></a>Azure Database for MySQL の作成

最初に作成するのは、マネージド MySQL サーバー インスタンスです。

> [!NOTE]
> 
> MySQL データベースの作成に関する詳細については、「[Azure portal を使用した Azure Database for MySQL サーバーの作成](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)」を参照してください。

引き続き [Azure Shell](https://shell.azure.com/) インスタンスで、次のスクリプトを実行します。

```azurecli
az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \
    | jq
```

このコマンドにより、小規模な MySQL サーバー インスタンスが作成されます。

### <a name="configure-a-firewall-rule-for-your-mysql-server-instance"></a>MySQL サーバー インスタンスのファイアウォール規則を構成する

Azure Database for MySQL インスタンスは既定でセキュリティ保護されており、受信接続を一切許可しないファイアウォールがあります。 データベースを使用できるようにするには、データベース サーバーにアクセスするためのローカル IP アドレスを許可するファイアウォール規則を追加する必要があります。

この記事の冒頭でローカル IP アドレスを構成したので、次を実行してサーバーのファイアウォールを開くことができます。

```azurecli
az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-mysql-database"></a>MySQL データベースを構成する

先ほど作成した MySQL サーバーは空です。Spring Boot アプリケーションで使用できるデータベースはありません。 `r2dbc` という新しいデータベースを作成します。

```azurecli
az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name r2dbc \
    --server-name $AZ_DATABASE_NAME \
    | jq
```

## <a name="create-a-reactive-spring-boot-application"></a>リアクティブ Spring Boot アプリケーションを作成する

リアクティブ Spring Boot アプリケーションを作成するために、[Spring Initializr](https://start.spring.io/)を使用します。 作成するアプリケーションでは、以下が使用されます。

- Spring Boot 2.3.0 M3
- Java 8 (ただし、Java 11 などの新しいバージョンでも機能します)
- 次の依存関係: Spring Reactive Web ("Spring WebFlux" とも呼ばれる) およびSpring Data R2DBC。

### <a name="generate-the-application-using-spring-initializr"></a>Spring Initializr を使用してアプリケーションを生成する

次のように入力して、コマンドラインを使用してこのアプリケーションを生成します。

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-r2dbc-workshop -d bootVersion=2.3.0.M3 -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-mysql-driver-implementation"></a>リアクティブ MySQL ドライバー実装を追加する

生成されたプロジェクトの *pom.xml* ファイルを開き、[https://github.com/mirromutth/r2dbc-mysql](https://github.com/mirromutth/r2dbc-mysql)から リアクティブ MySQL ドライバーを追加します。

`spring-boot-starter-webflux` 依存関係の後に、次のスニペットを追加します。

```xml
<dependency>
   <groupId>dev.miku</groupId>
   <artifactId>r2dbc-mysql</artifactId>
   <version>0.8.1.RELEASE</version>
   <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-the-azure-database-for-mysql"></a>Azure Database for MySQL を使用するように Spring Boot を構成する

*src/main/resources/application.properties* ファイルを開いて、以下を追加します。

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/r2dbc
spring.r2dbc.username=r2dbc@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_MYSQL_USERNAME
```

- 2 つの `$AZ_DATABASE_NAME` 変数を、この記事の冒頭で構成した値に置き換えます。
- `$AZ_MYSQL_USERNAME` 変数を、この記事の冒頭で構成した値で置き換えます。

これで、提供されている Maven Wrapper を使用してアプリケーションを起動できるようになりました。

```bash
./mvnw spring-boot:run
```

アプリケーションを初めて実行したときのスクリーンショットを次に示します。

![アプリケーションの実行][R2DBC-MYSQL01]

### <a name="create-the-database-schema"></a>データベース スキーマを作成する

`DemoApplication` メイン クラス内で、使用するデータベース スキーマを作成する新しい Spring Bean を構成します。

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
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

次のコマンドを使用して、アプリケーションを停止してから再実行します。 これで、アプリケーションは、先ほど作成した `r2dbc` データベースを使用し、その中に `todo` テーブルを作成します。

```bash
./mvnw spring-boot:run
```

作成されるデータベース テーブルのスクリーンショットを次に示します。

   ![データベース テーブルを作成する][R2DBC-MYSQL02]

## <a name="code-the-application"></a>アプリケーションをコーディングする

次に、R2DBC を使用して MySQL サーバー インスタンスからデータを格納および取得する Java コードを追加します。

ここで、`DemoApplication` クラスの横に新しい `Todo` Java クラスを作成します。

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

次に、データを格納および取得できるコントローラーを作成して、アプリケーションを完成させます。 同じパッケージに `TodoController` クラスを実装し、次のコードを追加します。

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

このコマンドを実行すると、作成した項目を含む "todo" の一覧が返されます。

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```

これらの cURL 要求のスクリーンショットを次に示します。

   ![cURL を使用してテストする][R2DBC-MYSQL03]

お疲れさまでした。 R2DBC を使用して Azure Database for MySQL からデータを格納および取得する、完全なリアクティブ Spring Boot アプリケーションを作成しました。

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
> [Azure の Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Spring Data R2DBC の詳細については、Spring の「[リファレンス ドキュメント](https://docs.spring.io/spring-data/r2dbc/docs/1.0.x/reference/html/#reference)」を参照してください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure](/azure/java/)」および [Azure DevOps と Java の操作](/azure/devops/)に関するページを参照してください。

<!-- IMG List -->

[R2DBC-MYSQL01]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-01.png
[R2DBC-MYSQL02]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-02.png
[R2DBC-MYSQL03]: media/configure-spring-data-r2dbc-with-azure-mysql/create-mysql-03.png
