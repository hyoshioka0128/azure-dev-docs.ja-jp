---
title: Spring Data Azure Cosmos DB 開発者ガイド
description: このガイドでは、Spring Data Azure Cosmos DB SDK を使用する際に知っておくべきことについて説明します。
author: kushagraThapar
ms.author: kuthapar
ms.topic: conceptual
ms.date: 1/9/2019
ms.custom: devx-track-java
ms.openlocfilehash: f2fb9a1af0634e0cecb3b6d990ae423b31da6210
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90829988"
---
# <a name="spring-data-azure-cosmos-db-developers-guide"></a>Spring Data Azure Cosmos DB 開発者ガイド

このトピックでは、SQL API を使用した [Spring Data Cosmos DB](https://github.com/microsoft/spring-data-cosmosdb) の機能について説明します。 このトピックには、一般的な問題、回避策、および診断手順に関するガイダンスも含まれています。

[Azure Cosmos DB](/azure/cosmos-db/introduction) は、開発者がさまざまな標準 API を使用してデータを操作できるようにするグローバル分散型データベース サービスです。 Spring Data Cosmos DB SDK は [Spring Data](https://spring.io/projects/spring-data) フレームワークに基づいており、SQL API を使用した Azure Cosmos DB との統合を提供します。 その他の API のサポートについては、次のトピックを参照してください。

- [Azure Cosmos DB で Spring Data MongoDB API を使用する方法](./configure-spring-data-mongodb-with-cosmos-db.md)
- [Azure Cosmos DB で Spring Data Apache Cassandra API を使用する方法](./configure-spring-data-apache-cassandra-with-cosmos-db.md)
- [Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法](./configure-spring-data-gremlin-java-app-with-cosmos-db.md)

Spring Data Cosmos DB SDK は、GitHub の [spring-data-cosmosdb](https://github.com/microsoft/spring-data-cosmosdb) リポジトリでオープンソースとして提供されています。 このリポジトリには、アクティブな[問題](https://github.com/microsoft/spring-data-cosmosdb/issues)の一覧があります。ここでは、バグを報告したり、既に登録されている問題に対する回避策を確認したりできます。 また、[リリース](https://github.com/microsoft/spring-data-cosmosdb/releases)の一覧で、新しいバージョンで問題が修正されたかどうかを確認することもできます。 Spring Data Cosmos DB SDK バージョン 2.2.x リリース トレインでは spring-data-commons バージョン 2.2.0 リリースがサポートされていますが、バージョン 2.1. x リリース トレインの SDK では、spring-data-common バージョン 2.1.0 リリースがサポートされています。

## <a name="available-features"></a>利用可能な機能

以降のセクションでは、現在使用できる機能について説明します。

### <a name="crudrepository-and-reactivecrudrepository-support"></a>CrudRepository と ReactiveCrudRepository のサポート

Spring Data Cosmos DB SDK には、Spring Data の `CrudRepository` および `ReactiveCrudRepository` インターフェイスを拡張する `CosmosRepository` および `ReactiveCosmosRepository` インターフェイスが用意されています。

次の例は、これらのインターフェイスの拡張方法を示しています。

```java
@Repository
public interface SampleRepository extends CosmosRepository<SampleEntity, String> {
    List<SampleEntity> findByName(String name);
}

@Repository
public interface ReactiveSampleRepository extends ReactiveCosmosRepository<SampleEntity, String> {
    Flux<SampleEntity> findByName(String name);
}
```

使用方法に応じて、`Configuration` クラスで両方のリポジトリを個別に有効にする必要があります。 次に例を示します。

```java
@Configuration
@PropertySource(value = {"classpath:application.properties"})
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

### <a name="define-a-simple-entity"></a>シンプル エンティティを定義する

エンティティを定義するには、`@Document` 注釈を追加し、コレクション名、要求ユニット (RU)、有効期限、自動作成コレクション フラグなどのコレクションに関連するプロパティを指定します。

既定では、コレクション名はユーザードメイン クラスのクラス名になります。 これをカスタマイズするには、`@Document(collection="myCustomCollectionName")` 注釈をドメイン クラスに追加します。 コレクション フィールドは [Spring Expression Language](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) (SpEL) の式もサポートしているため、構成プロパティを使用してプログラムでコレクション名を指定できます。 たとえば、`collection = "${dynamic.collection.name}"` や `collection = "#{@someBean.getCollectionName()}"` などの式を使用できます。

ドメイン クラスのフィールドを Azure Cosmos DB ドキュメントの `id` フィールドにマップするには、次の 2 つの方法があります。

- `@Id` でフィールドに注釈を付ける。
- フィールドの名前を `id` に設定する。

次の例は、`@Document` と `@Id` の注釈の使用方法を示しています。

```java
@Document(collection = "myCollection")
class MyDocument {

    @Id
    private String myId;

    @PartitionKey
    private String data;

    @Version
    private String _etag;
}
```

既定では、`IndexingPolicy` が Azure サービスによって設定されます。 これをカスタマイズするには、注釈 `@DocumentIndexingPolicy` をドメイン クラスに追加します。 この注釈には、次の 4 つの属性があります。

```java
boolean automatic;     // Indicates whether the indexing policy is automatic.
IndexingMode mode;     // The indexing policy mode; the options are Consistent, Lazy, or None.
String[] includePaths; // Included paths for indexing.
String[] excludePaths; // Excluded paths for indexing.
```

SDK ではパーティション分割もサポートされます。 詳細については、「[Azure Cosmos DB でのパーティション分割と水平スケーリング](/azure/cosmos-db/partition-data)」をご覧ください。 ドメイン クラスのフィールドがパーティション キーフィールドになるように指定するには、`@PartitionKey` で注釈を付けます。 次に、CRUD 操作を実行するときに、パーティションの値を指定します。

次の例は、CRUD 操作の実行時に `@PartitionKey` 注釈を使用する方法を示しています。

```java
@Document(ru = "400")
public class Address {
    @Id
    String postalCode;

    @PartitionKey
    String city;

    String street;
    String country;
    String phoneNumber;

    ...
}

class AddressService {

    @Autowired
    AddressRepository repository;

    final Address newAddress = new Address("12345", "city");

    // There's no need to specify a partition key in the save operation.
    repository.save(updatedAddress);

    // Provide a partition key when performing a find-by-id operation.
    final Optional<Address> addressById = repository.findById("12345", new PartitionKey("city"));

    final Address foundAddress = addressById.get();

    // Provide a partition key when performing a delete-by-id operation.
    repository.deleteById(foundAddress.getPostalCode(), new PartitionKey(foundAddress.getCity())); 
}
```

この SDK では、`findByAFieldAndBField` などの Spring Data カスタム クエリ検索操作もサポートされています。 詳細については、Spring ドキュメントの[クエリ メソッドの定義](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.query-methods.details)に関する記事を参照してください。

## <a name="best-practices"></a>ベスト プラクティス

次のセクションでは、SDK を使用する際のベスト プラクティスについて説明します。

### <a name="configuring-the-application"></a>アプリケーションの構成

以下の手順に従ってアプリケーションを構成します。

1. `AbstractCosmosConfiguration` クラスを拡張して、アプリケーションの構成 (Cosmos DB キー、URL、データベース名など) を設定します。
1. `@Configuration` 注釈を追加します。
1. リポジトリの使用状況に応じて、`@EnableCosmosRepositories` と `@EnableReactiveCosmosRepositories` のいずれかまたは両方の注釈を追加します。

`CosmosKeyCredential` 機能を使用すると、すぐにキーを循環させることができます。 `switchToSecondaryKey` メソッドを使用して、キーを切り替えることができます。

次のコード例は、アプリケーション構成を示し、`switchToSecondaryKey` の使用方法を示しています。

```java
@Configuration
@EnableCosmosRepositories
public class AppConfiguration extends AbstractCosmosConfiguration {

    @Value("${azure.cosmosdb.uri}")
    private String uri;

    @Value("${azure.cosmosdb.key}")
    private String key;

    @Value("${azure.cosmosdb.secondaryKey}")
    private String secondaryKey;

    @Value("${azure.cosmosdb.database}")
    private String dbName;

    @Value("${azure.cosmosdb.populateQueryMetrics}")
    private boolean populateQueryMetrics;

    private CosmosKeyCredential cosmosKeyCredential;

    @Bean
    public CosmosDBConfig getConfig() {
        this.cosmosKeyCredential = new CosmosKeyCredential(key);
        CosmosDbConfig cosmosdbConfig = CosmosDBConfig.builder(uri,
            this.cosmosKeyCredential, dbName).build();
        cosmosdbConfig.setPopulateQueryMetrics(populateQueryMetrics);
        cosmosdbConfig.setResponseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation());
        return cosmosdbConfig;
    }

    public void switchToSecondaryKey() {
        this.cosmosKeyCredential.key(secondaryKey);
    }
}
```

次の例に示すように、構成をカスタマイズして、接続モード、最大接続プール サイズ、要求タイムアウトなどを変更することもできます。

```java
public CosmosDBConfig getConfig() {

    this.cosmosKeyCredential = new CosmosKeyCredential(key);
    ConnectionPolicy customizedConnectionPolicy = new ConnectionPolicy();

    // Set the connection mode to Direct (TCP).
    customizedConnectionPolicy.setConnectionMode(ConnectionMode.DIRECT);

    // Set the maximum number of HTTP/TCP connections to 1000 per application.
    customizedConnectionPolicy.setMaxPoolSize(1000);

    // Set the request timeout to 10 seconds.
    customizedConnectionPolicy.requestTimeoutInMillis(10000);

    // Set the idle connection timeout to two minutes.
    customizedConnectionPolicy.idleConnectionTimeoutInMillis(120000);
    CosmosDBConfig cosmosDbConfig = CosmosDBConfig.builder(uri,   this.cosmosKeyCredential, dbName)
                                                  .connectionPolicy  (customizedConnectionPolic  y)
                                                  .build();
    return cosmosDbConfig;
}
```

### <a name="response-diagnostics-and-query-metrics"></a>応答診断とクエリ メトリック

Spring Data Cosmos DB SDK のバージョン 2.2.x では、応答診断文字列とクエリ メトリックがサポートされています。

クエリ メトリックを有効にするには、`application.properties` ファイルで `populateQueryMetrics` フラグを **true** に設定します。 次に、`ResponseDiagnosticsProcessor` インターフェイスを拡張し、`processResponseDiagnostics` メソッドを実装して診断情報をログに記録します。 最後に、実装のインスタンスを `CosmosDbConfig.setResponseDiagnosticsProcessor` メソッドに渡します。 次のコードでは、実装例を示します。

```java
@Configuration
@EnableCosmosRepositories
public class AppConfiguration extends AbstractCosmosConfiguration {

    ...

    @Value("${azure.cosmosdb.populateQueryMetrics}")
    private boolean populateQueryMetrics;

    private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

        @Override
        public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {
            log.info("Response Diagnostics {}", responseDiagnostics);
        }
    }

    @Bean
    public CosmosDBConfig getConfig() {
        this.cosmosKeyCredential = new CosmosKeyCredential(key);
        CosmosDbConfig cosmosdbConfig = CosmosDBConfig.builder(uri, this.cosmosKeyCredential, dbName).build();
        cosmosdbConfig.setPopulateQueryMetrics(populateQueryMetrics);
        cosmosdbConfig.setResponseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation());
        return cosmosdbConfig;
    }
}
```

### <a name="pagination-and-sorting"></a>改ページと並べ替え

Spring Data Cosmos DB SDK では、Spring Data の改ページと並べ替えがサポートされています。 詳細については、Spring ドキュメントの[特別なパラメーター処理](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.special-parameters)に関する記事を参照してください。

Cosmos DB では、データベース アカウントで使用可能な要求ユニット (RU) に基づいて、要求されたサイズ以下のドキュメントを返すことができます。 詳細については、「[Azure Cosmos DB の要求ユニット](/azure/cosmos-db/request-units)」をご覧ください。

各反復で返されるドキュメントの数は可変であるため、`totalPageSize` 値に依存しないでください。 代わりに、次の例に示すように、`Pageable` オブジェクトを反復処理する必要があります。

```java
final Sort sort = Sort.by(Sort.Direction.DESC, "name");
final CosmosPageRequest pageRequest = new CosmosPageRequest(0, pageSize,   null, sort);
Page<T> page = tRepository.findAll(pageRequest);
List<T> pageContent = page.getContent();
while(page.hasNext()) {
    Pageable nextPageable = page.nextPageable();
    page = repository.findAll(nextPageable);
    pageContent = page.getContent();
}
```

## <a name="common-issues-and-workarounds"></a>一般的な問題と対処法

次のセクションでは、Spring Data Cosmos DB SDK を使用する場合に注意する必要がある問題について説明します。

### <a name="getting-the-correct-cosmos-db-configuration"></a>正しい Cosmos DB 構成を取得する

クラスにはさまざまな注釈と構成があるため、`AbstractCosmosConfiguration` インターフェイスの拡張には注意が必要です。 最も一般的な問題は、`Enable Repositories` 注釈に関するものです。

リポジトリで `CosmosRepository` を拡張する場合は、注釈 `@EnableCosmosRepositories` を必ず追加してください。 リポジトリで `ReactiveCosmosRepository` を拡張する場合は、注釈 `@EnableReactiveCosmosRepositories` を必ず追加してください。 以下の例は、これらの注釈の使用方法を示しています。

```java
@Configuration
@PropertySource(value = {"classpath:application.properties"})
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

`CosmosDBConfig` Bean を作成またはカスタマイズするときは、キーを直接使用するのではなく、`CosmosKeyCredential` オブジェクトを使用してください。

`CosmosKeyCredential` 機能を使用すると、すぐにキーを循環させることができます。 `switchToSecondaryKey` メソッドを使用して、キーを切り替えることができます。

`CosmosKeyCredential` はシングルトン オブジェクトである必要があります。これは Cosmos DB SDK が同じオブジェクトを内部的に使用して、このオブジェクト内のキー値の変更を検出するためです。

### <a name="custom-query-execution"></a>カスタム クエリの実行

クエリ注釈機能は、Spring Data Cosmos DB SDK ではまだサポートされていません。 それまでは、Spring アプリケーション コンテキストによって公開されている `cosmosClient` Bean で、カスタムおよび複雑なクエリを直接実行できます。

次のコードは、`cosmosClient` Bean を使用してオフセットを実行し、クエリを制限する方法の簡単な例を示しています。

```java
final FeedOptions feedOptions = new FeedOptions();

// Enable cross-partition queries.
feedOptions.enableCrossPartitionQuery(true);

// Set the page size.
feedOptions.maxItemCount(20);

// Set the number of parallel operations on the client-side SDK when executing parallel queries.
feedOptions.maxDegreeOfParallelism(2);

// Populate query metrics from Cosmos DB.
feedOptions.populateQueryMetrics(true);

final String query = "SELECT * from c OFFSET " + skipCount + " LIMIT " + takeCount;

final CosmosClient cosmosClient = applicationContext.getBean(CosmosClient.class);

Flux<FeedResponse<CosmosItemProperties>> feedResponseFlux =
    cosmosClient.getDatabase(databaseId)
                .getContainer(collectionId)
                .queryItems(query, feedOptions);
    feedResponseFlux.subscribeOn(Schedulers.parallel())
                    .flatMap(feedResponse -> {
                        List<CosmosItemProperties> results =
                        feedResponseFlux.results();
                        log.info("Results are {}", results);
                        return feedResponse;
                    })
                    .subscribe();
```

### <a name="enable-diagnostics-and-query-metrics"></a>診断とクエリ メトリックを有効にする

デバッグ時には、Cosmos DB SDK から応答診断文字列とクエリ メトリックを取得すると便利です。 Cosmos DB SDK は、応答診断文字列をクライアント側に記録します。 バック エンドでクエリ メトリックがログに記録され、Cosmos DB SDK に提供されます。

`ResponseDiagnosticsProcessor.processResponseDiagnostics` メソッドは、Spring Data Cosmos DB SDK のすべての API 呼び出しの後に呼び出されます。 そのため、実装では、バグをなくし、複雑さを回避することで、ハイ パフォーマンスを保証することが重要です。 たとえば、このメソッドで診断情報の完全なセットをログに記録することはお勧めできません。関連する情報の量によってパフォーマンスが大幅に低下するためです。 また、`Debug` ログ記録レベルを使用して、アプリケーションのパフォーマンスに影響を与えないようにする必要があります。

次のコードは、`ResponseDiagnosticsProcessor` インターフェイスを実装する方法の例を示しています。

```java
private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

    @Override
    public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {

        // To log everything:
        if (log.isDebugEnabled()) {
            log.debug("Response diagnostics {}", responseDiagnostics);
        }

        // To log Cosmos DB response diagnostics:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            CosmosResponseDiagnostics cosmosResponseDiagnostics = responseDiagnostics.getCosmosResponseDiagnostics();
            log.debug("Cosmos DB response diagnostics {}", cosmosResponseDiagnostics);
        }

        // To log just the request latency:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            CosmosResponseDiagnostics cosmosResponseDiagnostics = responseDiagnostics.getCosmosResponseDiagnostics();
            log.debug("Request latency {}", cosmosResponseDiagnostics.requestLatency());
        }

        // To log query metrics:
        if (responseDiagnostics != null && log.isDebugEnabled()) {
            FeedResponseDiagnostics feedResponseDiagnostics =
                responseDiagnostics.getFeedResponseDiagnostics();
            log.debug("Query metrics {}", feedResponseDiagnostics);
        }
    }
}
```

## <a name="how-to-troubleshoot"></a>トラブルシューティング方法

以下のセクションでは、一般的な問題のトラブルシューティング方法について説明します。

### <a name="connection-issues"></a>接続に関する問題

接続の問題が発生した場合は、構成クラスに必要なすべての注釈が存在し、正しいことを確認してください。詳細については、「[正しい Cosmos DB 構成を取得する](#getting-the-correct-cosmos-db-configuration)」セクションを参照してください。

### <a name="api-exceptions"></a>API の例外

Spring Data Cosmos DB SDK のバージョン 2.2.1 では、例外処理が次のように強化されています。

- すべての API は `CosmosDBAccessException` をスローします。これにより、getter を通じて `cosmosClientException` フィールドが公開されます。
- Cosmos DB SDK は `CosmosClientException` をスローします。これを使用すると、クライアント側で任意の再試行ロジックを実装できます。
- 再試行する一般的な例外としては、`Resource already exists`、`Request rate too large`、`Request timeout exception` などのメッセージがあります。

### <a name="api-or-query-slowness"></a>API またはクエリのパフォーマンスの低下

API またはクエリの実行で待機時間が長い場合は、「[診断とクエリ メトリックを有効にする](#enable-diagnostics-and-query-metrics)」セクションの説明に従って、診断文字列とクエリ メトリックをログに記録してみてください。 CPU 使用率、ネットワーク帯域幅、および I/O ディスク領域を確認します。これらは、クライアント側のパフォーマンス低下の根本原因になる可能性があります。