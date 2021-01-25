---
title: Azure Cosmos DB SQL API で Spring Boot Starter を使用する方法
description: Spring Boot Initializer で作成されたアプリケーションを Azure Cosmos DB SQL API で構成する方法について説明します。
services: cosmos-db
documentationcenter: java
author: KarlErickson
ms.author: karler
ms.date: 10/13/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 10f85c7d1208ff77f2c85ec14e77ced450d5fcc8
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561307"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API で Spring Boot Starter を使用する方法

Azure Cosmos DB は、開発者が SQL、MongoDB、Graph、Table API などのさまざまな標準 API を使用してデータを操作できるようにするグローバル分散型データベース サービスです。 Microsoft の Spring Boot Starter を使用すると、開発者は、SQL API を使用して Azure Cosmos DB と簡単に統合できる Spring Boot アプリケーションを使用できます。

この記事では、Azure portal を使用して Azure Cosmos DB を作成する方法、 **[Spring Initializr]** を使用してカスタム Spring Boot アプリケーションを作成する方法、カスタム アプリケーションに [Azure Cosmos DB 用の Azure Spring Boot Starter] を追加し、Spring Data と Cosmos DB SQL API を使用して Azure Cosmos DB にデータを格納またはそこからデータを取得する方法を示します。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Azure Portal を使用して Azure Cosmos DB を作成する

1. <https://portal.azure.com/> で Azure portal を参照し、 **[リソースの作成]** を選択します。

1. **[データベース]**、**[Azure Cosmos DB]** の順に選択します。

    ![Azure portal で [Azure Cosmos DB] を選択します。][AZ02]

1. **[Azure Cosmos DB]** ページで、次の情報を入力します。

    * データベースに使用する **サブスクリプション** を選択します。
    * データベースに対して新しい **リソース グループ** を作成するか、既存のリソース グループを選択するかを指定します。
    * データベースの URI として使用する一意の **アカウント名** を入力します。 たとえば、*contosoaccount* です。
    * API の **コア (SQL)** を選択します。
    * データベースの **場所** を指定します。

    これらのオプションを指定したら、 **[Review + create]\(確認と作成\)** を選択して、指定した内容を確認し、 **[作成]** を選択します。

    ![[Review + Create]\(確認と作成\) を選択して続行します。][AZ03]

1. データベースが作成されると、それが Azure **ダッシュボード** に表示され、 **[すべてのリソース]** と **[Azure Cosmos DB]** の各ページに表示されます。 これらのいずれかの場所でデータベースを選択すると、キャッシュのプロパティ ページを開くことができます。

1. データベースのプロパティ ページが表示されたら、 **[キー]** を選択し、データベースの URI とアクセス キーをコピーします。これらの値は Spring Boot アプリケーションで使用します。

    ![[キー] セクションで、URI とアクセス キーをコピーします。][AZ05]

## <a name="create-a-spring-boot-application-with-the-spring-initializr"></a>Spring Initializr で Spring Boot アプリケーションを作成する

次の手順を使用して、Azure サポートで新しい Spring Boot アプリケーション プロジェクトを作成します。 別の方法として、[azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java)リポジトリの [azure-spring-boot-sample-cosmosdb](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-cosmos) サンプルを使用することもできます。 その後、そのまま「[アプリのビルドとテスト](#build-and-test-your-app)」に進むことができます。

1. <https://start.spring.io/> を参照します。

1. 次のオプションを指定します。

   * **Java** で **Maven** プロジェクトを生成します。
   * **Spring Boot** のバージョンを指定します。
   * アプリケーションの **グループ (Group)** と **成果物 (Artifact)** の名前を指定します。
   * Java バージョンとして **11** を選択します。
   * **Azure サポート** を依存関係に追加します。

   >[!div class="mx-imgBorder"]
   >![基本的な Spring Initializr オプション][SI01]

   > [!NOTE]
   > 1. Spring Initializr では、 **[グループ]** と **[アーティファクト]** の名前を使用してパッケージ名を作成します (例: *com.example.wingtiptoysdata*)。
   > 1. Spring Boot のバージョンは、Azure サポートでサポートされているバージョンよりも高い場合があります。 プロジェクトが自動的に生成されたら、Spring Boot のバージョンを、Azure でサポートされている最も高いバージョン ([こちら][azure-spring-boot-version-matrix]で確認できます) に手動で変更することができます。

1. 上記のオプションを指定したら、 **[生成]** を選択します。

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
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-boot-starter-cosmos</artifactId>
        <version>3.0.0</version>
    </dependency>
    ```

1. *properties* 要素に Java と Azure の必要なバージョンが示されていることを確認します。

    ```xml
    <properties>
        <java.version>11</java.version>
        <azure.version>3.0.0</azure.version>
    </properties>
    ```

1. *pom.xml* ファイルを保存して閉じます。

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>Azure Cosmos DB を使用するように Spring Boot アプリケーションを構成する

1. アプリの *resources* ディレクトリ内で *application.properties* ファイルを探します。次に例を示します。

    `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

    または

    `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

1. テキスト エディターで *application.properties* ファイルを開き、そのファイルに次の行を追加し、サンプルの値をデータベースの適切なプロパティに置き換えます。

    ```properties
    # Specify the DNS URI of your Azure Cosmos DB.
    azure.cosmos.uri=https://contosoaccount.documents.azure.com:443/
    
    # Specify the access key for your database.
    azure.cosmos.key=replace-your-access-key-here
    
    # Specify the name of your database. 
    azure.cosmos.database=contosoaccount
    azure.cosmos.populateQueryMetrics=true
    ```

1. *application.properties* ファイルを保存して閉じます。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>基本的なデータベース機能を実装するサンプル コードを追加する

このセクションでは、ユーザー データを格納するための 2 つの Java クラスを作成します。その後、アプリケーションのメイン クラスを変更して "*ユーザー*" クラスのインスタンスを作成し、それをデータベースに保存します。

### <a name="define-a-base-class-for-storing-user-data"></a>ユーザー データを格納するための基本クラスを定義する

1. *User.java* という名前の新しいファイルをメイン アプリケーションの Java ファイルと同じディレクトリに作成します。

1. テキスト エディターで *User.java* ファイルを開き、次の行をファイルに追加して、データベースの値を格納および取得する汎用ユーザー クラスを定義します。

    ```java
    package com.example.wingtiptoysdata;
    
    import com.azure.spring.data.cosmos.core.mapping.Container;
    import com.azure.spring.data.cosmos.core.mapping.PartitionKey;
    import org.springframework.data.annotation.Id;
    
    @Container(containerName = "mycollection")
    public class User {
        @Id
        private String id;
        private String firstName;
        @PartitionKey
        private String lastName;
        private String address;
    
        public User() {
    
        }
    
        public User(String id, String firstName, String lastName, String address) {
            this.id = id;
            this.firstName = firstName;
            this.lastName = lastName;
            this.address = address;
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
    
    import com.azure.spring.data.cosmos.repository.ReactiveCosmosRepository;
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
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.util.Assert;
    import reactor.core.publisher.Flux;
    import reactor.core.publisher.Mono;
    
    import java.util.Optional;
    
    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {
    
        private static final Logger LOGGER = LoggerFactory.getLogger(WingtiptoysdataApplication.class);
    
        @Autowired
        private UserRepository repository;
    
        public static void main(String[] args) {
            SpringApplication.run(WingtiptoysdataApplication.class, args);
        }
    
        public void run(String... var1) {
            this.repository.deleteAll().block();
            LOGGER.info("Deleted all data in container.");
    
            final User testUser = new User("testId", "testFirstName", "testLastName", "test address line one");
    
            // Save the User class to Azure Cosmos DB database.
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
    
            firstNameUserFlux.collectList().block();
    
            final Optional<User> optionalUserResult = repository.findById(testUser.getId()).blockOptional();
            Assert.isTrue(optionalUserResult.isPresent(), "Cannot find user.");
    
            final User result = optionalUserResult.get();
            Assert.state(result.getFirstName().equals(testUser.getFirstName()), "query result firstName doesn't match!");
            Assert.state(result.getLastName().equals(testUser.getLastName()), "query result lastName doesn't match!");
    
            LOGGER.info("findOne in User collection get result: {}", result.toString());
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
    mvnw clean
    ```

    このコマンドは、テスト フェーズの一部として、アプリケーションを自動的に実行します。 以下を使用することもできます。

    ```console
    mvnw spring-boot:run
    ```

    ビルドとテストの出力が完了すると、コンソール ウィンドウに次のようなメッセージが表示されます。

    ```console
    INFO 1365 --- [           main] c.e.w.WingtiptoysdataApplication         : Deleted all data in container.
    
    ... (omitting connection and diagnostics output) ...
    
    INFO 1365 --- [           main] c.e.w.WingtiptoysdataApplication         : findOne in User collection get result: testFirstName testLastName, test address line one
    ```

    上記の出力メッセージは、データが正常に Cosmos DB に保存され、再度取得されたことを示します。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

このアプリケーションの使用を継続しない場合は、前に作成した Cosmos DB を含むリソース グループを必ず削除してください。 これは Azure Portal から行うことができます。

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](./index.yml)

### <a name="more-resources"></a>その他のリソース

Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。

* [Azure Cosmos DB のドキュメント]。

* [Azure Cosmos DB:Java と Azure portal を使用してドキュメント データベースを作成する][Build a SQL API app with Java]

* [Azure Cosmos DB SQL API の Spring Data]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Azure Cosmos DB 用の Azure Spring Boot Starter]

* [Spring Boot アプリケーションを Azure App Service の Linux にデプロイする](deploy-spring-boot-java-app-on-linux.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、 **[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Java 開発者向けの Azure]: ../index.yml
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java
[Azure Cosmos DB SQL API の Spring Data]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Azure Cosmos DB 用の Azure Spring Boot Starter]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmos
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: https://azure.microsoft.com/services/devops/java/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[azure-spring-boot-version-matrix]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring#azure-spring-boot

<!-- IMG List -->
[AZ02]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ05]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png
