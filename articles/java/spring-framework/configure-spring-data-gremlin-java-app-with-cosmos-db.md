---
title: Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法
description: Spring Boot Initializer で作成されたアプリケーションを Azure Cosmos DB SQL API で構成する方法について説明します。
services: cosmos-db
documentationcenter: java
ms.date: 01/10/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 81a80d14e4cf371801cf75af1618048dda8775d7
ms.sourcegitcommit: b224b276a950b1d173812f16c0577f90ca2fbff4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810625"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a>Azure Cosmos DB SQL API で Spring Data Gremlin Starter を使用する方法

## <a name="overview"></a>概要

Spring Data Gremlin Starter は、Apache の Gremlin クエリ言語に Spring Data のサポートを提供します。これは、開発者が任意の Gremlin 互換データ ストアで使用できます。

この記事では、Azure portal を使用して Gremlin API で使うための Azure Cosmos DB を作成する方法、 **[Spring Initializr]** を使用してカスタム Java アプリケーションを作成する方法、カスタム アプリケーションに Spring Data Gremlin Starter 機能を追加し、Gremlin を使用して Azure Cosmos DB にデータを格納する方法、または Azure Cosmos DB からデータを取得する方法を示します。

## <a name="prerequisites"></a>前提条件

この記事の手順に従うには、次の前提条件が必要です。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。


## <a name="create-resource"></a>リソースを作成する

### <a name="create-azure-cosmos-db"></a>Azure Cosmos DB の作成

1. <https://portal.azure.com/> で Azure portal を参照し、 `+Create a resource` をクリックします。

   >[!div class="mx-imgBorder"]
   >![create-a-resource][create-a-resource-01]

1. [`Databases`] をクリックし、[`Azure Cosmos DB`] をクリックします。

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db][create-a-resource-02]

1. [`Azure Cosmos DB`] ページで、次の情報を入力します。

   * データベースに使用する `Subscription` を選択します。
   * データベースに対して新しい`Resource Group`を作成するか、既存のリソース グループを選択するかを指定します。
   * データベースの Gremlin URI の一部として使用する一意の `Account Name` を入力します。 たとえば、`Account Name` に対して `account-sample` を入力した場合、Gremlin URI は `account-samplewingtiptoysdata.gremlin.cosmosdb.azure.com` になります。
   * API の`Gremlin (Graph)` を選択します。
   * データベースの`Location`を指定します。
   
1. これらのオプションを指定したら、[`Review + create`] をクリックします。

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db-account][create-a-resource-03]

1. 指定した内容を確認し、[`Create`] をクリックしてデータベースを作成します。

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>ご自身の Azure Cosmos Database にグラフを追加する

1. Cosmos DB のページで、[`Data Explorer`] をクリックし、次に [`New Graph`] をクリックします。

   >[!div class="mx-imgBorder"]
   >![new-graph][create-a-graph-01]

1. [`Add Graph`] が表示されたら、次の情報を入力します。

   * お使いのデータベースの一意の `Database id` を指定します。
   * `Storage capacity` は、指定することも、既定値をそのまま使用することもできます。
   * お使いのグラフの一意の `Graph id` を指定します。
   * `Partition key` を指定します。 詳細については、[グラフのパーティション]に関する記事を参照してください。
[`OK`] をクリックします。
   
   これらのオプションの指定後、[`OK`] をクリックして、グラフを作成します。

   >[!div class="mx-imgBorder"]
   >![add-graph][create-a-graph-02]

1. 目的のグラフが作成されたら、`Data Explorer` を使用してそれを表示できます。

   >[!div class="mx-imgBorder"]
   >![graph-detail][create-a-graph-03]
   
   

## <a name="create-simple-spring-boot-application-with-the-spring-initializr"></a>Spring Initializr でシンプルな Spring Boot アプリケーションを作成する

1. <https://start.spring.io/> を参照します。

1. プロジェクトのメタデータを入力し、[`GENERATE`] をクリックします。

   >[!div class="mx-imgBorder"]
   >![spring-initializr][spring-initializr-01]

1. ファイルを解凍し、お使いの IDE にインポートします。


## <a name="update-code-according-to-the-sample-project"></a>サンプル プロジェクトに従ってコードを更新する

サンプル プロジェクト ([azure-spring-data-sample-gremlin]) のようにプロジェクトを変更します。

1. `azure-spring-data-gremlin` の依存関係を追加します。

1. `src/test/` のすべての内容を削除します。

1. このサンプルと同様に、`src/main/java` にすべての java ファイルを追加します。

1. `src/main/resorces/application.properties` で構成を更新します。この場合、次のようになります。

   | フィールド              | 説明                                                                                                                                                                                                             |
   |--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `endpoint`         | ご自身のデータベースの Gremlin URI を指定します。これは、このチュートリアルで先ほど Azure Cosmos DB を作成したときに指定した一意の **ID** から派生します。                                                 |
   | `port`             | TCP/IP ポートを指定します。HTTPS 用の **443** を使用する必要があります。                                                                                                                                                           |
   | `username`         | このチュートリアルで先ほどグラフを追加したときに使用した一意の**データベース ID** と**グラフ ID** を指定します。"/dbs/**{Database id}**/colls/**{Graph id}**" という構文を使用して入力する必要があります。 |
   | `password`         | このチュートリアルで先ほどコピーしたプライマリまたはセカンダリの**アクセス キー**を指定します。                                                                                                                      |
   | `sslEnabled`       | SSL を有効にするかどうかを指定します。                                                                                                                                                                                           |
   | `telemetryAllowed` | 利用統計情報を有効にする場合は **true**、それ以外の場合は **false** を指定します。
   | `maxContentLength` | 内容の最大長を指定します。                                                                                                                                                                                           |

1. パスワードの取得方法について:

   >[!div class="mx-imgBorder"]
   >![get-password][get-password-01]

## <a name="build-and-run-the-project"></a>プロジェクトをビルドして実行する

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. アプリケーションが正常に起動した場合は、Azure portal でグラフを確認できます。

   >[!div class="mx-imgBorder"]
   >![execute-result][execute-result-01]


## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Gremlin および Graph API に対する Azure サポートの詳細については、次の記事をご覧ください。

* [Azure Cosmos DB の概要: Graph API](/azure/cosmos-db/graph-introduction)

* [Azure Cosmos DB での Gremlin グラフのサポート](/azure/cosmos-db/gremlin-support)

* [Azure Cosmos DB: グラフ データベースを Java と Azure Portal で作成する](/azure/cosmos-db/create-graph-java)

* [チュートリアル: Gremlin を使って Azure Cosmos DB Graph API を照会する](/azure/cosmos-db/tutorial-query-graph)

* [Spring Data Gremlin Starter]

Azure Cosmos DB と Java の使用について詳しくは、次の記事をご覧ください。

* [Azure Cosmos DB のドキュメント]。

* [Azure Cosmos DB:Java と Azure portal を使用してドキュメント データベースを作成する][Build a SQL API app with Java]

* [Azure Cosmos DB SQL API の Spring Data]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

**[Spring Framework]** は Java 開発者のエンタープライズ レベルのアプリケーション作成を支援するオープンソース ソリューションです。 このプラットフォームで構築される特に知られたプロジェクトの 1 つが [Spring Boot] です。これによって、スタンドアロンの Java アプリケーションの作成方法が簡略化されます。 Spring Boot を使い始めた開発者を支援するために、<https://github.com/spring-guides/> では、サンプルの Spring Boot パッケージがいくつか用意されています。 基本的な Spring Boot プロジェクトの一覧から選択するだけでなく、 **[Spring Initializr]** は、開発者がカスタム Spring Boot アプリケーションの作成を開始できるように支援します。

<!-- URL List -->

[Azure Cosmos DB のドキュメント]: /azure/cosmos-db/
[Java 開発者向けの Azure]: /azure/developer/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Azure Cosmos DB SQL API の Spring Data]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[グラフのパーティション]: https://docs.microsoft.com/azure/cosmos-db/graph-partitioning
[azure-spring-data-sample-gremlin]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-data-sample-gremlin

<!-- IMG List -->

[create-a-resource-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-01.png
[create-a-resource-02]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-02.png
[create-a-resource-03]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-03.png

[create-a-graph-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-01.png
[create-a-graph-02]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-02.png
[create-a-graph-03]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-03.png

[spring-initializr-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/spring-initializr-01.png

[get-password-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/get-password-01.png

[execute-result-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/execute-result-01.png
