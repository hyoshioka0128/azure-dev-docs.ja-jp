---
title: Azure Cosmos DB SQL API で Spring Boot Starter を使用する方法
description: Spring Boot Initializer で作成されたアプリケーションを Azure Cosmos DB SQL API で構成する方法について説明します。
services: cosmos-db
documentationcenter: java
author: KarlErickson
manager: barbkess
ms.author: karler
ms.date: 10/02/2019
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 192550b74a35eb24620c58e86e6a55e86e5e90ab
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118174"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API で Spring Boot Starter を使用する方法

Azure Cosmos DB は、開発者が SQL、MongoDB、Graph、Table API などのさまざまな標準 API を使用してデータを操作できるようにするグローバル分散型データベース サービスです。 Microsoft の Spring Boot Starter を使用すると、開発者は、SQL API を使用して Azure Cosmos DB と簡単に統合できる Spring Boot アプリケーションを使用できます。

この記事では、Azure portal を使用して Azure Cosmos DB を作成する方法、 **[Spring Initializr]** を使用してカスタム Spring Boot アプリケーションを作成する方法、カスタム アプリケーションに [Azure の Spring Boot Cosmos DB Starter] を追加し、Spring Data と Cosmos DB SQL API を使用して Azure Cosmos DB にデータを格納またはデータを取得する方法を示します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Azure Portal を使用して Azure Cosmos DB を作成する

1. <https://portal.azure.com/> で Azure portal を参照し、 **[リソースの作成]** をクリックします。

1. **[データベース]** 、 **[Azure Cosmos DB]** の順にクリックします。

    ![Azure ポータル][AZ02]

1. **[Azure Cosmos DB]** ページで、次の情報を入力します。

    * データベースに使用する**サブスクリプション**を選択します。
    * データベースに対して新しい**リソース グループ**を作成するか、既存のリソース グループを選択するかを指定します。
    * データベースの URI として使用する一意の**アカウント名**を入力します。 例: *wingtiptoysdata*
    * API の**コア (SQL)** を選択します。
    * データベースの**場所**を指定します。

    これらのオプションを指定したら、 **[Review + create]\(確認と作成\)** をクリックして指定した内容を確認し、 **[作成]** をクリックします。

    ![Azure ポータル][AZ03]

1. データベースが作成されると、それが Azure **ダッシュボード**に表示され、 **[すべてのリソース]** ページと **[Azure Cosmos DB]** ページにも表示されます。 これらのいずれかの場所でデータベースをクリックすると、キャッシュのプロパティ ページを開くことができます。

1. データベースのプロパティ ページが表示されたら、 **[キー]** をクリックし、データベースの URI とアクセス キーをコピーします。これらの値は Spring Boot アプリケーションで使用します。

    ![Azure ポータル][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Spring Initializr でシンプルな Spring Boot アプリケーションを作成する

次の手順を使用して、Azure サポートで新しい Spring Boot アプリケーション プロジェクトを作成します。 別の方法として、[azure-spring-boot](https://github.com/microsoft/azure-spring-boot)リポジトリの [azure-cosmosdb-spring-boot-sample](https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-cosmosdb-spring-boot-sample) サンプルを使用することもできます。 その後、そのまま「[アプリのビルドとテスト](#build-and-test-your-app)」に進むことができます。

1. [https://www.microsoft.com](<https://start.spring.io/>) を参照します。

1. **Java** で **Maven プロジェクト**を生成することを指定し、**Spring Boot** のバージョンを指定して、アプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力します。依存関係に **Azure サポート**を追加してから、 **[プロジェクトの生成]** のボタンをクリックします。

    ![基本的な Spring Initializr オプション][SI01]

    > [!NOTE]
    >
    > Spring Initializr では、 **[グループ]** と **[アーティファクト]** の名前を使用してパッケージ名を作成します (例: *com.example.wintiptoysdata*)。

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードして、ファイルを抽出します。

これで、シンプルな Spring Boot アプリケーションを編集できるようになりました。

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>Azure Spring Boot Starter を使用するように Spring Boot アプリケーションを構成する

1. アプリのディレクトリで *pom.xml* ファイルを探します。次に例を示します。

    `C:\SpringBoot\wingtiptoysdata\pom.xml`

    または

    `/users/example/home/wingtiptoysdata/pom.xml`

1. テキスト エディターで *pom.xml* ファイルを開き、`<dependencies>` 要素に以下を追加します。

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
    </dependency>

    <dependency>
        <groupId>io.projectreactor.netty</groupId>
        <artifactId>reactor-netty</artifactId>
        <version>0.8.3.RELEASE</version>
    </dependency>
    ```

1. *properties* 要素に Java と Azure の必要なバージョンが示されていることを確認します。

    ```xml
    <properties>
       <java.version>1.8</java.version>
       <azure.version>2.2.0.M1</azure.version>
    </properties>
    ```

1. *pom.xml* ファイルを保存して閉じます。

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>Azure Cosmos DB を使用するように Spring Boot アプリケーションを構成する

1. アプリの *resources* ディレクトリ内で *application.properties* ファイルを探します。次に例を示します。

    `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

    または

    `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

1. テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値をデータベースの適切なプロパティに置き換えます。

    ```text
    # Specify the DNS URI of your Azure Cosmos DB.
    azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

    # Specify the access key for your database.
    azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

    # Specify the name of your database.
    azure.cosmosdb.database=wingtiptoysdata
    ```

1. *application.properties* ファイルを保存して閉じます。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>基本的なデータベース機能を実装するサンプル コードを追加する

このセクションでは、ユーザー データを格納するための 2 つの Java クラスを作成します。その後、アプリケーションのメイン クラスを変更して "*ユーザー*" クラスのインスタンスを作成し、それをデータベースに保存します。

### <a name="define-a-base-class-for-storing-user-data"></a>ユーザー データを格納するための基本クラスを定義する

1. *User.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。

1. テキスト エディターで *User.java* ファイルを開き、次の行をファイルに追加して、データベースの値を格納および取得する汎用ユーザー クラスを定義します。

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document;
    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.PartitionKey;
    import org.springframework.data.annotation.Id;

    @Document(collection = "mycollection")
    public class User {

        @Id
        private String id;
        private String firstName;

        @PartitionKey
        private String lastName;
        private String address;

        public User(String id, String firstName, String lastName, String address) {
            this.id = id;
            this.firstName = firstName;
            this.lastName = lastName;
            this.address = address;
        }

        public User() {
        }

        public String getId() {
            return id;
        }

        public void setId(String id) {
            this.id = id;
        }

        public String getFirstName() {
            return firstName;
        }

        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }

        public String getLastName() {
            return lastName;
        }

        public void setLastName(String lastName) {
            this.lastName = lastName;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        @Override
        public String toString() {
            return String.format("%s %s, %s", firstName, lastName, address);
        }
    }
    ```

1. *User.java* ファイルを保存して閉じます。

### <a name="define-a-data-repository-interface"></a>データ リポジトリ インターフェイスを定義する

1. *UserRepository.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。

1. テキスト エディターで *UserRepository.java* ファイルを開き、次の行をファイルに追加して、既定の `ReactiveCosmosRepository` インターフェイスを拡張するユーザー リポジトリ インターフェイスを定義します。

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.repository.ReactiveCosmosRepository;
    import org.springframework.stereotype.Repository;
    import reactor.core.publisher.Flux;

    @Repository
    public interface UserRepository extends ReactiveCosmosRepository<User, String> {
        Flux<User> findByFirstName(String firstName);
    }
    ```

    `ReactiveCosmosRepository`インターフェイスは、スターターの前のバージョンの `DocumentDbRepository` インターフェイスに代わるものです。 この新しいインターフェイスには、基本的な保存、削除、検索の各操作用の同期 API とリアクティブ API が用意されています。

1. *UserRepository.java* ファイルを保存して閉じます。

### <a name="modify-the-main-application-class"></a>アプリケーションのメイン クラスを変更する

1. アプリケーションのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。

    `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

    または

    `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

1. テキスト エディターでメイン アプリケーションの Java ファイルを開き、ファイルに次の行を追加します。

    ```java
    package com.example.wingtiptoysdata;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.util.Assert;
    import reactor.core.publisher.Flux;
    import reactor.core.publisher.Mono;

    import javax.annotation.PostConstruct;
    import javax.annotation.PreDestroy;
    import java.util.Optional;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private static final Logger LOGGER = LoggerFactory.getLogger(WingtiptoysdataApplication.class);

        @Autowired
        private UserRepository repository;

        public static void main(String[] args) {
            SpringApplication.run(WingtiptoysdataApplication.class, args);
        }

        public void run(String... var1) throws Exception {
            final User testUser = new User("1", "Tasha", "Calderon", "4567 Main St Buffalo, NY 98052");

            LOGGER.info("Saving user: {}", testUser);

            // Save the User class to Azure CosmosDB database.
            final Mono<User> saveUserMono = repository.save(testUser);

            final Flux<User> firstNameUserFlux = repository.findByFirstName("testFirstName");

            //  Nothing happens until we subscribe to these Monos.
            //  findById will not return the user as user is not present.
            final Mono<User> findByIdMono = repository.findById(testUser.getId());
            final User findByIdUser = findByIdMono.block();
            Assert.isNull(findByIdUser, "User must be null");

            final User savedUser = saveUserMono.block();
            Assert.state(savedUser != null, "Saved user must not be null");
            Assert.state(savedUser.getFirstName().equals(testUser.getFirstName()), "Saved user first name doesn't match");

            LOGGER.info("Saved user");

            firstNameUserFlux.collectList().block();

            final Optional<User> optionalUserResult = repository.findById(testUser.getId()).blockOptional();
            Assert.isTrue(optionalUserResult.isPresent(), "Cannot find user.");

            final User result = optionalUserResult.get();
            Assert.state(result.getFirstName().equals(testUser.getFirstName()), "query result firstName doesn't match!");
            Assert.state(result.getLastName().equals(testUser.getLastName()), "query result lastName doesn't match!");

            LOGGER.info("Found user by findById : {}", result);
        }

        @PostConstruct
        public void setup() {
            LOGGER.info("Clear the database");
            this.repository.deleteAll().block();
        }

        @PreDestroy
        public void cleanup() {
            LOGGER.info("Cleaning up users");
            this.repository.deleteAll().block();
        }
    }
    ```

1. メイン アプリケーションの Java ファイルを保存して閉じます。

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、*pom.xml* ファイルが置かれているフォルダーに移動します。次に例を示します。

    `cd C:\SpringBoot\wingtiptoysdata`

    または

    `cd /users/example/home/wingtiptoysdata`

1. 次のコマンドを使用して、アプリケーションをビルドして実行します。

    ```console
    mvnw clean test
    ```

    このコマンドは、テスト フェーズの一部として、アプリケーションを自動的に実行します。 以下を使用することもできます。

    ```console
    mvnw clean spring-boot:run
    ```

    ビルドとテストの出力が完了すると、コンソール ウィンドウに次のようなメッセージが表示されます。

    ```console
      .   ____          _            __ _ _
     /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
     \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
     =========|_|==============|___/=/_/_/_/
     :: Spring Boot ::            (v2.2.0.RC1)
    >
    > 2019-10-04 15:19:06.817  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Starting WingtiptoysdataApplicationTests on devmachine03 with PID 30013 (started by <user> in /d/source/repos/wingtiptoysdata)
    > 2019-10-04 15:19:06.818  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : No active profile set, falling back to default profiles: default
    > 2019-10-04 15:19:08.329  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.720  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 1369ms. Found 1 repository interfaces.
    > 2019-10-04 15:19:09.734  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.748  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 13ms. Found 0 repository interfaces.

    ... (omitting Cosmos DB connection output) ...

    > 2019-10-04 15:19:46.584  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Started WingtiptoysdataApplicationTests in 40.702 seconds (JVM running for 44.647)
    > 2019-10-04 15:19:46.587  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saving user: Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > 2019-10-04 15:19:47.122  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saved user
    > 2019-10-04 15:19:47.289  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Found user by findById : Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 44.003 s - in com.example.wingtiptoysdata.WingtiptoysdataApplicationTests
    > 2019-10-04 15:19:48.124  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.194  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.200  INFO 30013 --- [extShutdownHook] c.e.w.WingtiptoysdataApplication         : Cleaning up users
    > [INFO]
    > [INFO] Results:
    > [INFO]
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    > [INFO]
    > [INFO]
    > [INFO] --- maven-jar-plugin:3.1.2:jar (default-jar) @ wingtiptoysdata ---
    > [INFO] Building jar: /d/source/repos/wingtiptoysdata/target/wingtiptoysdata-0.0.1-SNAPSHOT.jar
    > [INFO]
    > [INFO] --- spring-boot-maven-plugin:2.2.0.RC1:repackage (repackage) @ wingtiptoysdata ---
    > [INFO] Replacing main artifact with repackaged archive
    > [INFO] ------------------------------------------------------------------------
    > [INFO] BUILD SUCCESS
    > [INFO] ------------------------------------------------------------------------
    > [INFO] Total time:  02:18 min
    > [INFO] Finished at: 2019-10-04T15:20:05-07:00
    > [INFO] ------------------------------------------------------------------------
    ```

    ![アプリケーションからの正常な出力][JV02]

    `Saved user` と `Found user` のメッセージは、データが正常に Cosmos DB に格納され、再度取得されたことを示します。

## <a name="clean-up-resources"></a>リソースのクリーンアップ

このアプリケーションの使用を継続しない場合は、前に作成した Cosmos DB を含むリソース グループを必ず削除してください。 これは Azure Portal から行うことができます。

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。

* [Azure Cosmos DB のドキュメント]。

* [Azure Cosmos DB:Java と Azure portal を使用してドキュメント データベースを作成する][Build a SQL API app with Java]

* [Azure Cosmos DB SQL API の Spring Data]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Azure の Spring Boot Cosmos DB Starter]

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、 **[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Java 開発者向けの Azure]: /azure/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Azure Cosmos DB SQL API の Spring Data]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Azure の Spring Boot Cosmos DB Starter]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: https://azure.microsoft.com/services/devops/java/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png

[JV02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
