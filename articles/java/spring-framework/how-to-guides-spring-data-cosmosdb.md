---
title: Spring Data Azure Cosmos DB 開発者ガイド
description: このガイドでは、Spring Data Azure Cosmos DB SDK を使用する際に知っておくべきことについて説明します。
author: anfeldma-ms
ms.author: anfeldma
ms.topic: conceptual
ms.date: 11/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 45872eba2fd6929cf406df4a551559fd5ad78c40
ms.sourcegitcommit: 63732132cb88206b99876f0bcd035b52c301f315
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523137"
---
# <a name="spring-data-azure-cosmos-db-developers-guide"></a>Spring Data Azure Cosmos DB 開発者ガイド

このトピックでは、SQL API を使用した [Spring Data Azure Cosmos DB](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos) の機能について説明します。 このトピックには、一般的な問題、回避策、および診断手順に関するガイダンスも含まれています。

[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) は、開発者がさまざまな標準 API を使用してデータを操作できるようにするグローバル分散型データベース サービスです。 Spring Data Azure Cosmos DB SDK は [Spring Data](https://spring.io/projects/spring-data) フレームワークに基づいており、SQL API を使用した Azure Cosmos DB との統合を提供します。 その他の API のサポートについては、次のトピックを参照してください。

- [Azure Cosmos DB で Spring Data MongoDB API を使用する方法](./configure-spring-data-mongodb-with-cosmos-db.md)
- [Azure Cosmos DB で Spring Data Apache Cassandra API を使用する方法](./configure-spring-data-apache-cassandra-with-cosmos-db.md)
- [Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法](./configure-spring-data-gremlin-java-app-with-cosmos-db.md)

Spring Data Azure Cosmos DB SDK は、GitHub の [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-cosmos) リポジトリでオープンソースとして提供されています。 このリポジトリには、アクティブな[問題](https://github.com/Azure/azure-sdk-for-java/issues)の一覧があります。ここでは、バグを報告したり、既に登録されている問題に対する回避策を確認したりできます。 また、[リリース](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/cosmos/azure-spring-data-cosmos/CHANGELOG.md)の一覧で、新しいバージョンで問題が修正されたかどうかを確認することもできます。 

## <a name="available-features"></a>利用可能な機能

以降のセクションでは、現在使用できる機能について説明します。

### <a name="crudrepository-and-reactivecrudrepository-support"></a>CrudRepository と ReactiveCrudRepository のサポート

Spring Data Azure Cosmos DB SDK には、Spring Data の `CrudRepository` および `ReactiveCrudRepository` インターフェイスを拡張する `CosmosRepository` および `ReactiveCosmosRepository` インターフェイスが用意されています。

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
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

### <a name="define-a-simple-entity"></a>シンプル エンティティを定義する

エンティティを定義するには、`@Container` 注釈を追加し、コレクション名、要求ユニット (RU)、有効期限、自動作成コレクション フラグなどのコレクションに関連するプロパティを指定します。

既定では、コレクション名はユーザードメイン クラスのクラス名になります。 これをカスタマイズするには、`@Container(containerName="myCustomCollectionName")` 注釈をドメイン クラスに追加します。 `containerName` フィールドは [Spring Expression Language](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) (SpEL) の式もサポートしているため、構成プロパティを使用してプログラムでコレクション名を指定できます。 たとえば、`containerName = "${dynamic.container.name}"` や `containerName = "#{@someBean.getContainerName()}"` などの式を使用できます。

ドメイン クラスのフィールドを Azure Cosmos DB ドキュメントの `id` フィールドにマップするには、次の 2 つの方法があります。

- `@Id` でフィールドに注釈を付ける。
- フィールドの名前を `id` に設定する。

次の例は、`@Container` と `@Id` の注釈の使用方法を示しています。

```java
@Container(containerName = "myContainer")
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

SDK ではパーティション分割もサポートされます。 詳細については、「[Azure Cosmos DB でのパーティション分割と水平スケーリング](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#:~:text=%20Partitioning%20and%20horizontal%20scaling%20in%20Azure%20Cosmos,partitions.%20Azure%20Cosmos%20DB%20transparently%20and...%20More%20)」をご覧ください。 ドメイン クラスのフィールドがパーティション キーフィールドになるように指定するには、`@PartitionKey` で注釈を付けます。 次に、CRUD 操作を実行するときに、パーティションの値を指定します。

次の例は、CRUD 操作の実行時に `@PartitionKey` 注釈を使用する方法を示しています。

```java
@Container(ru = "400")
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

    final Address newAddress = new Address("12345", "Seattle");

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

### <a name="pulling-configuration-properties-into-the-application"></a>アプリケーションへの構成プロパティのプル

**application.properties** の設定を Java アクセス メソッドとして公開する properties クラスを作成できます。 **application.properties** の構造は次のようになります

```xml
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

# Populate query metrics
cosmos.queryMetricsEnabled=true
```

この構造をミラーリングし、次のように構造化された Java クラス `CosmosProperties` 作成します。

```java
@ConfigurationProperties(prefix = "cosmos")
public class CosmosProperties {

    private String uri;

    private String key;

    private String secondaryKey;

    private boolean queryMetricsEnabled;

    public String getUri() {
        return uri;
    }

    public void setUri(String uri) {
        this.uri = uri;
    }

    public String getKey() {
        return key;
    }

    public void setKey(String key) {
        this.key = key;
    }

    public String getSecondaryKey() {
        return secondaryKey;
    }

    public void setSecondaryKey(String secondaryKey) {
        this.secondaryKey = secondaryKey;
    }

    public boolean isQueryMetricsEnabled() {
        return queryMetricsEnabled;
    }

    public void setQueryMetricsEnabled(boolean enableQueryMetrics) {
        this.queryMetricsEnabled = enableQueryMetrics;
    }
}
```

このクラスには各 **application.properties** 構成プロパティに対応するメンバーがあり、各メンバーに対して `CosmosProperties` が *get* と *set* の各メソッドを公開していることに注目してください。 `@ConfigurationProperties` 注釈は、クラスを構成プロパティを表すものとして識別し、`prefix = "cosmos"` 引数は、`CosmosProperties` の特定の "*メンバー*" が **application.properties** の `cosmos.member` プロパティにマップされることを示します。

次のセクションでは、 `CosmosProperties` クラスを自動構成フローに組み込む方法について説明します。 構成時に、 `CosmosProperties` インスタンスが作成され、そのインスタンス メソッドに **application.properties** の構成設定が入力されます。 この properties インスタンスにより、アプリケーションは実行時に構成プロパティを読み取り、変更することができます。

### <a name="configuring-the-application-based-on-properties"></a>プロパティに基づくアプリケーションの構成

次の手順では、アプリケーションの構成を自動化する構成クラスを作成します。 次に例を示し、その作成方法について説明します

```java
@Configuration
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class AppConfiguration extends AbstractCosmosConfiguration {

    private static final Logger logger = LoggerFactory.getLogger(QuickstartSampleConfiguration.class);

    @Autowired
    private CosmosProperties properties;

    private AzureKeyCredential azureKeyCredential;

    @Bean
    public CosmosClientBuilder cosmosClientBuilder() {
        this.azureKeyCredential = new AzureKeyCredential(properties.getKey());
        return new CosmosClientBuilder()
            .endpoint(properties.getUri())
            .key(this.azureKeyCredential)
    }

    @Bean
    public CosmosConfig cosmosConfig() {
        DirectConnectionConfig directConnectionConfig = DirectConnectionConfig.getDefaultConfig();        
        return CosmosConfig.builder()
                           .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
                           .enableQueryMetrics(properties.isQueryMetricsEnabled())
                           .directMode(directConnectionConfig);                           
                           .build();
    }

    @Override
    protected String getDatabaseName() {
        return "testdb";
    }

    private static class ResponseDiagnosticsProcessorImplementation implements ResponseDiagnosticsProcessor {

        @Override
        public void processResponseDiagnostics(@Nullable ResponseDiagnostics responseDiagnostics) {
            logger.info("Response Diagnostics {}", responseDiagnostics);
        }
    }

    public void switchToSecondaryKey() {
        this.cosmosKeyCredential.key(secondaryKey);
    }
}
```

構成クラスの構造を作成するには、次のようにします。

1. `AbstractCosmosConfiguration` クラスを拡張して、アプリケーションの構成 (Cosmos DB キー、URL、データベース名など) を設定します。
1. `@Configuration` 注釈を追加します。
1. リポジトリの使用状況に応じて、`@EnableCosmosRepositories` と `@EnableReactiveCosmosRepositories` のいずれかまたは両方の注釈を追加します。
1. `@PropertySource("classpath:application.properties")` 注釈を追加します。これは、**application.properties** からキーと値のペアを抽出するようにシグナルを送信します
1. `@EnableConfigurationProperties` 注釈を追加します。これは、**application.properties** からキーと値のペアを格納できるクラスを Spring Data に指定します。 この注釈は、クラス定義を引数として取ります。 `CosmosProperties.class` を渡す必要があります。

構成クラスでは、次のメンバーが使用されます。

1. Spring Data がすべてのログ出力に使用する log4j2 `logger` メンバーを宣言して定義します。
1. `@Autowired` `CosmosProperties` メンバーを宣言します。**ここに application.properties 設定が格納されます**

`AzureKeyCredential` 機能を使用すると、すぐにキーを循環させることができます。 これを有効にするには、`AzureKeyCredential` メンバーを定義します。 上の例に示すように、`switchToSecondaryKey` メソッドを追加することで、キーを切り替えることができます。

次に、自動構成をどのように行うかを定義する必要があります。
1. `CosmosClientBuilder` を使用してクライアントの初期化を処理する `@Bean` `cosmosClientBuilder` メソッドを定義します。 このメソッドの目的は、基本的なクライアントの設定を実行することです。つまり、アカウント エンドポイントの URI とアクセス キーを指定します。 通常、アカウント エンドポイントの URI とアクセス キーは **application.properties** で定義され、`properties` に入力されます。 `azureKeyCredential` メンバーを `properties.getKey()` で初期化してから、`properties.getUri()` と `this.azureKeyCredential` をそれぞれ `endpoint` と `key` の各ビルダー メソッドにフィードすることができます。 

上の例では、`cosmosClientBuilder` がクライアント ビルダーで `build()` を呼び出さないことに注目してください。これにより、ファイナライズされていないビルダー構造が返されます。 Spring Data を使用すると、構成を 2 段階で実行できます。最初に、`cosmosClientBuilder` で基本構成を適用して構成構造を返すことができます。次に、Spring Data は `cosmosConfig` メソッドを呼び出します。これにより、メトリックや診断などのより高度な構成を定義できます。 次に、`cosmosConfig` メソッドでのこの高度な構成について説明します。
1. 上に示すように、 `@Bean` `cosmosConfig` メソッドを作成します。
1. Azure Cosmos DB は、各要求に関連付けられているサーバー側診断を返すことができます。 Spring Data を使用すると、顧客診断プロセッサを定義することで、未加工の診断出力をログに記録する前に変換することができます。 上に示すように、 `ResponseDiagnosticsProcessor` を実装して `processResponseDiagnostics` メソッドをオーバーライドするクラスを定義します。 診断出力の処理方法を制御するために `processResponseDiagnostics` を定義できます。 上の例では、単純に未加工の診断をログに記録します。
1. 診断を有効にし、診断プロセッサを初期化するには、 `responseDiagnosticsProcessor` ビルダー メソッドを呼び出して、顧客プロセッサ クラスの新しいインスタンスを渡します。

    ```java
    return CosmosConfig.builder()
                       .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
    ```
1. Azure Cosmos DB には、クエリ メトリックと呼ばれる、クエリ用のより具体的なパフォーマンス メトリック機能もあります。 前のセクションで示したように、ベスト プラクティスは、クエリ メトリックを有効または無効にする **application.properties** 設定を使用することです。 `cosmosConfig` に `.enableQueryMetrics(properties.isQueryMetricsEnabled())` ビルダー メソッドを追加して、この構成設定を適用します。
1. 最小待機時間と最大スループットを実現するためには、直接モード接続をお勧めします。これにより、クライアント ビルダーでもそれを構成できます。

`cosmosConfig` の高度な構成が完了したら、構成構造で `build()` を呼び出してクライアントの作成をトリガーする必要があります。これにより、構成設定に基づいてAzure Cosmos DB クライアントインスタンスが生成されます。

構成プロセスを定義する最後の手順は、Azure Cosmos DB データベースの名前を文字列として返す `@Override` メソッド `getDatabaseName()` を追加することです。

### <a name="customizing-the-configuration"></a>構成のカスタマイズ

次の例に示すように、構成をカスタマイズして、接続モード、最大接続プール サイズ、要求タイムアウトなどを変更することもできます。

```java
    @Bean
    public CosmosConfig cosmosConfig() {

        // Set the connection mode to Direct (TCP) which applies to data plane operations
        DirectConnectionConfig directConnectionConfig = DirectConnectionConfig.getDefaultConfig(); 

        // Even in Direct mode, some control plane operations always pass through the gateway as HTTP requests (i.e. container/database CRUD.)
        // Optionally, you can customize connection properties for these specific operations which are
        // always Gateway mode
        GatewayConnectionConfig gatewayConnectionConfig = GatewayConnectionConfig.getDefaultConfig(); 

        // Set the maximum number of HTTP connections to 1000 per application.
        gatewayConnectionConfig.setMaxConnectionPoolSize(1000);

        // Set the request timeout to 10 seconds.
        gatewayConnectionConfig.setIdleConnectionTimeout(Duration.ofMillis(10000));

        return CosmosConfig.builder()
                           .responseDiagnosticsProcessor(new ResponseDiagnosticsProcessorImplementation())
                           .enableQueryMetrics(properties.isQueryMetricsEnabled())
                           .directMode(directConnectionConfig, gatewayConnectionConfig); // directMode() has an override which accepts Gateway config                        
                           .build();
    }
```

### <a name="response-diagnostics-and-query-metrics"></a>応答診断とクエリ メトリック

Spring Data Azure Cosmos DB SDK では、バージョン 2 から応答診断文字列とクエリ メトリックがサポートされています。

クエリ メトリックを有効にするには、`application.properties` ファイルで `queryMetricsEnabled` フラグを **true** に設定します。 次に、前のセクションで説明したプロセスに従って `ResponseDiagnosticsProcessor` インターフェイスを拡張し、`processResponseDiagnostics` メソッドを実装して診断情報をログに記録します。 最後に、実装のインスタンスを `CosmosDbConfig.setResponseDiagnosticsProcessor` メソッドに渡します。

### <a name="pagination-and-sorting"></a>改ページと並べ替え

Spring Data Azure Cosmos DB SDK では、Spring Data の改ページと並べ替えがサポートされています。 詳細については、Spring ドキュメントの[特別なパラメーター処理](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#repositories.special-parameters)に関する記事を参照してください。

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

次のセクションでは、Spring Data Azure Cosmos DB SDK を使用する場合に注意する必要がある問題について説明します。

### <a name="getting-the-correct-cosmos-db-configuration"></a>正しい Cosmos DB 構成を取得する

クラスにはさまざまな注釈と構成があるため、`AbstractCosmosConfiguration` インターフェイスの拡張には注意が必要です。 最も一般的な問題は、`Enable Repositories` 注釈に関するものです。

リポジトリで `CosmosRepository` を拡張する場合は、注釈 `@EnableCosmosRepositories` を必ず追加してください。 リポジトリで `ReactiveCosmosRepository` を拡張する場合は、注釈 `@EnableReactiveCosmosRepositories` を必ず追加してください。 以下の例は、これらの注釈の使用方法を示しています。

```java
@Configuration
@EnableConfigurationProperties(CosmosProperties.class)
@EnableCosmosRepositories
@EnableReactiveCosmosRepositories
@PropertySource("classpath:application.properties")
public class TestRepositoryConfig extends AbstractCosmosConfiguration {
    ...
}
```

`CosmosDBConfig` Bean を作成またはカスタマイズするときは、キーを直接使用するのではなく、`AzureKeyCredential` オブジェクトを使用してください。

`AzureKeyCredential` 機能を使用すると、すぐにキーを循環させることができます。 `switchToSecondaryKey` メソッドを使用して、キーを切り替えることができます。

`AzureKeyCredential` はシングルトン オブジェクトである必要があります。これは Cosmos DB SDK が同じオブジェクトを内部的に使用して、このオブジェクト内のキー値の変更を検出するためです。

### <a name="custom-query-execution"></a>カスタム クエリの実行

Spring Data Azure Cosmos DB SDK 3.x.x では、カスタム クエリを定義するための `@query` 注釈がサポートされています。

次のコードは、`@query` 注釈を使用してオフセットを定義し、クエリを制限する方法の簡単な例を示しています。

```java
@Repository
public interface SampleRepository extends CosmosRepository<SampleEntity, String> {

    ...

    @Query(value = "SELECT * from c OFFSET @skipCount LIMIT @takeCount")
    List<SampleEntity> findByName(@Param("skipCount") int skipCount, @Param("takeCount") int takeCount);
}
```

### <a name="enable-diagnostics-and-query-metrics"></a>診断とクエリ メトリックを有効にする

デバッグ時には、Cosmos DB SDK から応答診断文字列とクエリ メトリックを取得すると便利です。 Cosmos DB SDK は、応答診断文字列をクライアント側に記録します。 バック エンドでクエリ メトリックがログに記録され、Cosmos DB SDK に提供されます。

`ResponseDiagnosticsProcessor.processResponseDiagnostics` メソッドは、Spring Data Azure Cosmos DB SDK のすべての API 呼び出しの後に呼び出されます。 そのため、実装では、バグをなくし、複雑さを回避することで、ハイ パフォーマンスを保証することが重要です。 たとえば、このメソッドで診断情報の完全なセットをログに記録することはお勧めできません。関連する情報の量によってパフォーマンスが大幅に低下するためです。 また、`Debug` ログ記録レベルを使用して、アプリケーションのパフォーマンスに影響を与えないようにする必要があります。

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

### <a name="naming-changes"></a>名前の変更

Spring Data Azure Cosmos DB SDK のバージョン 3.1.0 以降には、クラス、メソッド、注釈、Maven 成果物の名前またはインターフェイスに関して次のような重要な変更があります。
* グループ ID が `com.azure` に更新されました。
* 成果物 ID が `azure-spring-data-cosmos` に更新されました。
* 同期 API の戻り値の型が、`List` ではなく `Iterable` に更新されました。
* `CosmosDbFactory` から `CosmosFactory`。
* `CosmosDBConfig` から `CosmosConfig`。
* `CosmosDBAccessException` から `CosmosAccessException`。
* `Document` 注釈から `Container` 注釈。
* `DocumentIndexingPolicy` 注釈から `CosmosIndexingPolicy` 注釈。
* `DocumentQuery` から `CosmosQuery`。
* **application.properties** フラグ `populateQueryMetrics` から `queryMetricsEnabled`。

### <a name="key-bug-fixes"></a>主要なバグ修正

Spring Data Azure Cosmos DB SDK のバージョン 3.1.0 以降には、次の主なバグ修正が含まれています
* 注釈付きクエリによって注釈付きコンテナー名が選択されない問題を修正しました。
* Netty I/O スレッドがブロックされないようにするための、並列スレッドへの診断ログ タスクのスケジュール。
* 削除操作でのオプティミスティック ロックを修正しました。
* IN 句に対するクエリのエスケープに関する問題を修正しました。
* @Id に対して long データ型を許可することで、問題を修正しました。
* @PartitionKey 注釈のデータ型としてブール値、long、int、double を許可することで、問題を修正しました。
* 大文字と小文字が区別されないクエリに対する IgnoreCase と AllIgnoreCase キーワードを修正しました。
* コンテナーを自動的に作成するときの、既定の要求ユニットの値 4000 を削除しました。
* @GeneratedValue 注釈と共に使用した場合の入れ子になったパーティション キーのバグが修正されました。

### <a name="api-or-query-slowness"></a>API またはクエリのパフォーマンスの低下

API またはクエリの実行で待機時間が長い場合は、「[診断とクエリ メトリックを有効にする](#enable-diagnostics-and-query-metrics)」セクションの説明に従って、診断文字列とクエリ メトリックをログに記録してみてください。 CPU 使用率、ネットワーク帯域幅、および I/O ディスク領域を確認します。これらは、クライアント側のパフォーマンス低下の根本原因になる可能性があります。