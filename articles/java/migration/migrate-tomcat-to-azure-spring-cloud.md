---
title: Tomcat アプリケーションを Azure Spring Cloud に移行する
description: このガイドでは、既存の Tomcat アプリケーションを Azure Spring Cloud に移行する場合に知っておくべきことについて説明します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 6/16/2020
ms.openlocfilehash: a718c7b8b481b99f5a4b1303acac7147c88d1550
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831668"
---
# <a name="migrate-a-tomcat-application-to-azure-spring-cloud"></a>Tomcat アプリケーションを Azure Spring Cloud に移行する

このガイドでは、既存の Tomcat アプリケーションを Azure Spring Cloud に移行して実行する場合に知っておくべきことについて説明します。

## <a name="pre-migration"></a>移行前

移行を確実に成功させるには、開始する前に、次のセクションで説明する評価およびインベントリの手順を完了します。

### <a name="switch-to-a-supported-platform"></a>サポートされているプラットフォームに切り替える

Azure Spring Cloud では、特定のバージョンの Java SE が提供されています。 互換性を確保するために、残りの手順を続行する前に、現在の環境でサポートされているいずれかのバージョンにアプリケーションを移行してください。 結果として得られた構成は、十分にテストしてください。 このようなテストでは、Linux ディストリビューションの安定した最新リリースを使用します。

[!INCLUDE [note-obtain-your-current-java-version](includes/note-obtain-your-current-java-version.md)]

[!INCLUDE [determine-whether-and-how-the-file-system-is-used](includes/determine-whether-and-how-the-file-system-is-used.md)]

### <a name="identify-the-application-builddependency-system"></a>アプリケーション ビルド/依存関係システムを識別する

すべての依存関係をダウンロードするなど、アプリケーションのビルド/パッケージ化に使用するツールを特定します。

[!INCLUDE [inventory-external-resources](includes/inventory-external-resources.md)]

### <a name="inventory-secrets"></a>シークレットをインベントリする

#### <a name="passwords-and-secure-strings"></a>パスワードとセキュリティで保護された文字列

すべてのシークレット文字列とパスワードについて、運用サーバー上のすべてのプロパティと構成ファイルを確認します。 *$CATALINA_BASE/conf* にある *server.xml* と *context.xml* を必ず確認してください。 また、アプリケーション内にパスワードや資格情報を含むファイルが *META-INF/context.xml*に存在する場合もあります。

[!INCLUDE [inventory-certificates](includes/inventory-certificates.md)]

### <a name="determine-whether-your-application-contains-os-specific-code"></a>アプリケーションに OS 固有のコードが含まれているかどうかを判断する

[!INCLUDE [determine-whether-your-application-contains-os-specific-code-no-title](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

### <a name="determine-whether-tomcat-is-connected-to-a-web-server"></a>Tomcat が Web サーバーに接続されているかどうかを確認する

Tomcat は、`mod_jk` などの tomcat コネクタを介して、Apache などの静的な Web サーバーに接続できます。 `conf` ディレクトリ内の `workers.properties` ファイルを調べて、そのような接続が存在するかどうかを確認します。

### <a name="special-cases"></a>特殊なケース

運用環境のシナリオによっては、追加の変更が必要な場合や、追加の制限が課される場合があります。 そのようなシナリオはめったに発生しませんが、それらがアプリケーションに適用されないこと、または正しく解決されることを確認することが重要です。

#### <a name="determine-if-the-application-uses-filters"></a>アプリケーションでフィルターを使用しているかを判断する

アプリケーションの *web.xml* ファイルを調べて、[構成されているフィルター](https://tomcat.apache.org/tomcat-9.0-doc/config/filter.html#Expires_Filter/Basic_configuration_sample)がないかを確認します。

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs-azure-spring-cloud](includes/determine-whether-your-application-relies-on-scheduled-jobs-azure-spring-cloud.md)]

#### <a name="determine-whether-non-http-connectors-are-used"></a>非 HTTP コネクタが使用されているかどうかを判断する

Azure Spring Cloud では、カスタマイズできない単一の HTTP ポートでの HTTP 接続のみがサポートされます。 アプリケーションに追加のポートや追加のプロトコルが必要な場合は、Azure Spring Cloud を使用しないでください。

アプリケーションによって使用されている HTTP コネクタを特定するには、Tomcat 構成の *server.xml* ファイル内で `<Connector>` 要素を探します。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>SSL セッションの追跡が使用されているかどうかを判断する

Azure Spring Cloud では、アプリケーション コードに到達する前に SSL セッションが終了します。そのため、[SSL セッション追跡](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)は使用できません。 代わりに [Spring Session](https://docs.spring.io/spring-session/docs/current/reference/html5/index.html) を使用するように切り替える必要があります。

#### <a name="determine-whether-tomcat-realms-are-used"></a>Tomcat の領域が使用されているかどうかを判断する

Azure Spring Cloud では、Tomcat の領域の代わりに Spring Security を使用する必要があります。 *server.xml* ファイルを調べて、[構成されている領域](https://tomcat.apache.org/tomcat-9.0-doc/realm-howto.html#Configuring_a_Realm)をすべてインベントリします。

#### <a name="determine-whether-servlet-filters-are-used"></a>サーブレット フィルターが使用されているかどうかを判断する

アプリケーションの *web.xml* ファイルを調べて、`<filter>` 要素がないかどうか確認します。 使用可能なフィルターの一覧については、[Tomcat のフィルターのドキュメント](https://tomcat.apache.org/tomcat-9.0-doc/config/filter.html)を参照してください。

## <a name="migration"></a>移行

### <a name="remove-connection-to-web-server-if-present"></a>Web サーバーへの接続を削除する (存在する場合)

Tomcat が静的な Web サーバー (通常は `mod_jk` 経由で Apache) に接続されている場合は、その接続を無効にして Tomcat がスタンドアロン サーバーとして実行されるようにし、必要に応じて標準サーバーからの Web リダイレクトを作成します。 Azure Content Delivery Network (CDN) を使用して Azure Storage に静的 Web コンテンツを移行することを検討してください。 詳細については、「[Azure Storage での静的 Web サイト ホスティング](/azure/storage/blobs/storage-blob-static-website)」と[「クイック スタート:Azure Storage アカウントと Azure CDN を統合する](/azure/cdn/cdn-create-a-storage-account-with-cdn)」を参照してください。

### <a name="update-to-tomcat-9"></a>Tomcat 9 に更新する

現在のアプリケーションが、9 より前のバージョンの Tomcat で実行されている場合は、Tomcat 9 に移行し、アプリケーションが完全に機能することを確認します。 詳細については、[Tomcat 9 の移行ガイド](http://tomcat.apache.org/migration-9.html)を参照してください。

### <a name="switch-to-maven-or-gradle"></a>Maven または Gradle への切り替え

Spring Boot と Spring Cloud では、ビルドと依存関係の管理に Maven または Gradle が必要です。 アプリケーションで別のビルドまたは依存関係管理システムを使用している場合は、続行する前に、[Maven](https://maven.apache.org/download.cgi) の現在のバージョンに切り替えてください。 Gradle もサポートされていますが、このガイドの手順全体では Maven を使用します。 Gradle を使用する場合は、必要に応じて手順を調整してください。

アプリケーションの [POM ファイル](https://maven.apache.org/pom.html)を作成し、アプリケーションが完全な機能でビルドされ実行されていることを確認してから続行してください。

### <a name="migrate-to-spring-boot"></a>Spring Boot への移行

次の表は、Tomcat アプリケーションを Spring Boot に移行し、その後、Azure Spring Cloud に移行するために必要な移行とコード変更の概要を示しています。 「従来」列のいずれかの要素がアプリケーションで使用されている場合は、対応する「最小限」、理想的には「推奨」列の要素で置き換える必要があります。

|従来 | 確認する場所 |最小限の移行 |推奨される移行|
|---|---|---|---|
|[DataSource 経由の JDBC](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html)|*server.xml*|[JDBC テンプレート](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-using-jdbc-template)を使用した [Spring Data Datasource](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-sql)|必要に応じて、Spring Data と JPA を検討してください。|
|Servlets |*web.xml*|[サーブレット コンテキストの初期化](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container-context-initializer)を有効にし、`@WebServlet` で注釈を付ける|[Spring MVC コントローラーとして書き換える (`@RestController` を使用](https://spring.io/guides/gs/rest-service/#_create_a_resource_controller))
|フィルター |*web.xml*| [サーブレット コンテキストの初期化](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container-context-initializer)を有効にし、[`@WebFilter` で注釈を付ける](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/WebFilter.html) |最小限と同じ|
|JavaServer Pages (JSP)|*web.xml* と WAR ファイルの内容|[Spring MVC の JSP ビュー](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-view-jsp)|ビュー レイヤーを個別にホストする|
|Java Message Service (JMS)|*server.xml*|接続ファクトリを [Spring Bean](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-spring-beans-and-dependency-injection) としてインスタンス化する|[Spring JMS](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/integration.html#jms-using) を使用する
|WAR ファイル内の静的なコンテンツ (画像、JavaScript ファイルなど)|静的コンテンツ ディレクトリ (通常は */static*、 */public*、または */resources*)|*/src/main/resources* にコンテンツを移動する|[移行前の静的コンテンツに関する推奨事項](#read-only-static-content)に関する記事を参照してください。
|WAR ファイルの外部にある静的コンテンツ (画像、JavaScript ファイルなど)|ローカル ファイル システムのパス|コンテンツを *src/main/resources* に移動します。 ソース コードでハードコーディングされたパスを検索し、[ClassPathResource](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/io/ClassPathResource.html) で置き換えます |[移行前の静的コンテンツに関する推奨事項](#read-only-static-content)に関する記事を参照してください。

1. アプリケーションが JNDI リソース (JDBC ドライバーなど) によって挿入されたライブラリに依存している場合は、これらのライブラリを依存関係として POM ファイルに追加します。 Tomcat サーバー (通常は *tomcat/lib* ディレクトリ) からライブラリを削除し、続行する前に、アプリケーションが完全な機能で実行されていることを確認します。

1. Spring Boot の親 POM を POM ファイルに追加します。 詳細については、Spring Boot のドキュメントの「[Creating the POM](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-first-application-pom)」を参照してください。

1. Spring Boot Tomcat starter を依存関係として POM ファイルに追加します。

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ```

    これは以前の Tomcat アプリケーションですが、`war` をターゲット パッケージとして追加しないでください。

1. Tomcat データソースを Spring データ ソースに置き換えます。 アプリケーションによって使用されるすべてのデータベースに対して、[Spring DataSource を構成](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-configure-a-datasource)します。 直接 SQL クエリを実行するコードがある場合は、[JdbcTemplate を使用](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-using-jdbc-template)するように変更します。 トランザクション管理や CRUD ツールなどの追加のデータ アクセス機能については、[Spring Framework のドキュメント](https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc)と [Spring Data のドキュメント](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference)を参照してください。

1. [埋め込みサーブレット コンテナー](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container)内にサーブレットを実装することはできますが、これは推奨されません。 代わりに、[サーブレットの実装](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequest.html)を Spring [Rest コントローラー](https://spring.io/guides/gs/rest-service/#_create_a_resource_controller)に置き換えます。 アプリケーションで Spring MVC 以外のフレームワークを使用している場合は、Spring MVC に置き換えます。 詳細については、[Spring MVC 注釈付きコントローラーのリファレンス](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/web.html#mvc-controller)を参照してください。

1. [Spring beans](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-spring-beans-and-dependency-injection) を使用して、他のすべての JNDI 依存関係を再作成します。 メッセージングに [Spring JMS](https://spring.io/guides/gs/messaging-jms/) を使用するなど、Spring のメカニズムの使用を優先します。

1. Tomcat の領域を [Spring Security](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-filters-review) に置き換えます。 認可管理には、[Active Directory 用の Spring Boot Starter](../spring-framework/spring-boot-starters-for-azure.md#azure-active-directory)経由で Azure Active Directory を使用することを検討してください。

1. *web.xml* に構成されているサーブレット フィルターを [Spring beans](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-add-a-servlet-filter-or-listener-as-spring-bean) または [クラスパス スキャン](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-add-a-servlet-filter-or-listener-using-scanning)で再作成します。

1. イメージや JavaScript ファイルなどの静的コンテンツがアプリケーションに含まれている場合、または参照されている場合、これらのファイルは、プロジェクト ソース コード内の *src/main/resources* に移動する必要があります。 ファイルを移動した後、ソース コードを更新して、ローカル ファイル システムの参照を削除します。 Spring の `ClassPathResource` クラスを使用して、このようなファイルにアクセスします。

`mvn spring-boot:run` を実行してアプリケーションをテストします。 続行する前に、アプリケーションが完全な機能で実行されていることを確認してください。

[!INCLUDE [migrate-steps-spring-boot-azure-spring-cloud](includes/migrate-steps-spring-boot-azure-spring-cloud.md)]

## <a name="post-migration"></a>移行後

[!INCLUDE [post-migration-spring-boot-azure-spring-cloud](includes/post-migration-spring-boot-azure-spring-cloud.md)]