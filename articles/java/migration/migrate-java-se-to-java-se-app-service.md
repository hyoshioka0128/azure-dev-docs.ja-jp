---
title: Java SE アプリケーションを Azure App Service 上の Java SE に移行する
description: このガイドでは、Java SE を使用して、既存の Spring Boot またはその他のスタンドアロン Web アプリケーションを Azure App Service に移行する場合に知っておくべきことについて説明します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 9576a3831afb00b10b9bec3531c2dbc1dc60f29f
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672188"
---
# <a name="migrate-executable-jar-web-applications-to-java-se-on-azure-app-service"></a>実行可能 JAR Web アプリケーションを Azure App Service 上の Java SE に移行する

このガイドでは、Java SE を使用して、既存の Spring Boot またはその他の埋め込みサーバー Web アプリケーションを Azure App Service に移行する場合に知っておくべきことについて説明します。

## <a name="before-you-start"></a>開始する前に

移行前の要件を満たすことができない場合は、以下の関連する移行ガイドを参照してください。

* Azure Kubernetes Service のコンテナーに実行可能 JAR アプリケーションを移行する (ガイド計画済)
* 実行可能 JAR アプリケーションを Azure Virtual Machines に移行する (ガイド計画済)

## <a name="pre-migration"></a>移行前

### <a name="switch-to-a-supported-platform"></a>サポートされているプラットフォームに切り替える

App Service では、特定のバージョンの Java SE が提供されています。 互換性を確保するために、残りの手順を続行する前に、現在の環境でサポートされているいずれかのバージョンにアプリケーションを移行してください。 結果として得られた構成は、十分にテストしてください。 このようなテストでは、Linux ディストリビューションの安定した最新リリースを使用します。

[!INCLUDE [note-obtain-your-current-java-version](includes/note-obtain-your-current-java-version.md)]

### <a name="inventory-external-resources"></a>外部リソースをインベントリする

データ ソース、JMS メッセージ ブローカー、その他のサービスの URL などの外部リソースを特定します。 Spring Boot アプリケーションでは、一般に、このようなリソースの構成は、*src/main/ ディレクトリ*内にある通常 *application.properties* または *application.yml* と呼ばれるファイル内にあります。 さらに、実稼働環境デプロイの環境変数で、関連する構成設定を確認します。

#### <a name="databases"></a>データベース

SQL データベースが存在する場合は、接続文字列を特定します。

Spring Boot アプリケーションの場合、接続文字列は通常、構成ファイルに記載があります。

*application.properties* ファイルの例を次に示します。

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

*application.yaml* ファイルの例を次に示します。

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

詳細については、Spring のドキュメントの「[JPA Repositories](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories)」と「[JDBC Repositories](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#jdbc.repositories)」を参照してください。

#### <a name="jms-message-brokers"></a>JMS メッセージ ブローカー

使用されているブローカーを特定します。 これは、関連する依存関係についてビルド マニフェスト (通常は、*pom.xml* または *build.gradle*) を調べることによって確認できます。

たとえば、ActiveMQ を使用する Spring Boot アプリケーションには、通常、この依存関係が *pom.xml* に含まれています。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

独自のブローカーを使用する Spring Boot アプリケーションには、通常、ブローカーの JMS ドライバー ライブラリへの直接的な依存関係が含まれています。 *build.gradle* ファイルの例を次に示します。

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```

使用されているブローカーを特定したら、対応する設定を見つけます。これは、通常、Spring Boot の *application.properties* や *application.yml* ファイル内にあります。

*application.properties* ファイルの例を次に示します。

```properties
spring.activemq.brokerurl=broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
spring.activemq.user=admin
spring.activemq.password=tryandguess
```

*application.yaml* ファイルの例を次に示します。

```yaml
ibm:
  mq:
    queueManager: qm1
    channel: dev.ORDERS
    connName: localhost(14)
    user: admin
    password: big$ecr3t
```

#### <a name="all-other-external-resources"></a>その他のすべての外部リソース

このガイドでは、考えられるすべての外部依存関係を記載することはできません。 App Service の移行後に、アプリケーションの外部依存関係がすべて満たされることを確認するのは、お客様のチームの責任です。

### <a name="inventory-secrets"></a>シークレットをインベントリする

#### <a name="passwords-and-secure-strings"></a>パスワードとセキュリティで保護された文字列

すべてのシークレット文字列とパスワードについて、実稼働デプロイ上のすべてのプロパティおよび構成ファイルとすべての環境変数を確認します。 Spring Boot アプリケーションでは、多くの場合、このような文字列は *application.properties* または *application.yml* で見つかります。

[!INCLUDE [inventory-certificates](includes/inventory-certificates.md)]

[!INCLUDE [inventory-persistence-usage](includes/inventory-persistence-usage.md)]

### <a name="special-cases"></a>特殊なケース

運用環境のシナリオによっては、追加の変更が必要な場合や、追加の制限が課される場合があります。 そのようなシナリオはめったに発生しませんが、それらがアプリケーションに適用されないこと、または正しく解決されることを確認することが重要です。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>スケジュールされたジョブにアプリケーションが依存しているかどうかを判断する

スケジュールされたジョブ (Quartz Scheduler タスクや cron ジョブなど) は、App Service では使用できません。 App Service では、スケジュールされたタスクを含むアプリケーションの内部でのデプロイが妨げられることはありません。 ただし、アプリケーションをスケールアウトすると、同じスケジュールされたジョブが、スケジュールされた期間に複数回実行されることがあります。 このような場合、意図しない結果になることがあります。

アプリケーション プロセスの内部または外部でスケジュールされたすべてのジョブをインベントリします。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>アプリケーションに OS 固有のコードが含まれているかどうかを判断する

アプリケーションに、そのアプリケーションが実行されている OS に対応するコードが含まれている場合は、基盤の OS に依存しないようにアプリケーションをリファクターする必要があります。 たとえば、ファイル システム パスで `/` や `\` が使用されている場合は、[`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) または [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-) に置き換える必要があります。

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>実稼働サーバーで実行されているすべての外部プロセス/デーモンを識別する

デーモンの監視など、アプリケーション サーバーの外部で実行されているプロセスは、他の場所に移行するか削除する必要があります。

#### <a name="identify-handling-of-non-http-requests-or-multiple-ports"></a>HTTP 以外の要求または複数のポートの処理を特定する

App Service では、1 つのポートで 1 つの HTTP エンドポイントのみがサポートされます。 アプリケーションが複数のポートでリッスンしているか、HTTP 以外のプロトコルを使用して要求を受け入れる場合は、Azure App Service を使用しないでください。

## <a name="migration"></a>移行

### <a name="parameterize-the-configuration"></a>構成のパラメーター化

すべての外部リソース座標 (データベース接続文字列など) およびその他のカスタマイズ可能な設定が環境変数から読み取れることを確認します。 Spring Boot アプリケーションを移行する場合は、すべての構成設定が既に外部化可能である必要があります。 詳細については、Spring Boot のドキュメントの「[Externalized Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)」を参照してください。

`SERVICEBUS_CONNECTION_STRING`application.properties*ファイルから* 環境変数を参照する例を次に示します。

```properties
spring.jms.servicebus.connection-string=${SERVICEBUS_CONNECTION_STRING}
spring.jms.servicebus.topic-client-id=contoso1
spring.jms.servicebus.idle-timeout=10000
```

### <a name="provision-an-app-service-plan"></a>App Service プランをプロビジョニングする

[利用可能なサービス プランの一覧](https://azure.microsoft.com/pricing/details/app-service/linux/)から、仕様が現在の実稼働ハードウェアの仕様を満たしているか超えているプランを選択します。

> [!NOTE]
> ステージング/カナリア デプロイを実行する場合、または[デプロイ スロット](/azure/app-service/deploy-staging-slots)を使用する場合は、App Service プランにその追加容量が含まれている必要があります。 Java アプリケーションでは、Premium 以上のプランを使用することをお勧めします。

[App Service プランを作成します](/azure/app-service/app-service-plan-manage#create-an-app-service-plan)。

### <a name="create-and-deploy-web-apps"></a>Web アプリを作成してデプロイする

実行予定のすべての実行可能な JAR ファイルごとに、App Service プランで (ランタイム スタックとして "Java SE" を選択して) Web アプリを作成する必要があります。

#### <a name="maven-applications"></a>Maven アプリケーション

アプリケーションが Maven POM ファイルから作成されている場合は、[Maven 用の Webapp プラグインを使用](/azure/developer/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service)して Web アプリを作成し、アプリケーションをデプロイします。

#### <a name="non-maven-applications"></a>Maven 以外のアプリケーション

Maven プラグインを使用できない場合は、次のような他のメカニズムを使用して Web アプリをプロビジョニングする必要があります。

* [Azure Portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [Azure CLI](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Web アプリが作成されたら、[利用可能なデプロイ メカニズム](/azure/app-service/deploy-ftp)のいずれかを使用してアプリケーションをデプロイします。 可能であれば、アプリケーションを */home/site/wwwroot/app.jar* にアップロードします。 JAR の名前を app.jar に変更したくない場合は、JAR を実行するコマンドが含まれるシェル スクリプトをアップロードできます。 その後、ポータルの構成セクションで [[スタートアップ ファイル]](/azure/app-service/containers/app-service-linux-faq#built-in-images) テキストボックスにこのスクリプトの完全なパスを貼り付けます。 スタートアップ スクリプトは、配置先のディレクトリからは実行されません。 そのため、スタートアップ スクリプトでファイルを参照するには、常に絶対パスを使用します (例: `java -jar /home/myapp/myapp.jar`)。

### <a name="migrate-jvm-runtime-options"></a>JVM ランタイムオプションを移行する

アプリケーションで特定のランタイム オプションが必要な場合は、[最適なメカニズムを使用してそれらを指定します](/azure/app-service/containers/configure-language-java#set-java-runtime-options)。

[!INCLUDE [configure-custom-domain-and-ssl](includes/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/import-backend-certificates.md)]

### <a name="migrate-external-resource-coordinates-and-other-settings"></a>外部リソース座標とその他の設定を移行する

接続文字列とその他の設定を移行するには、[こちらの手順に](/azure/app-service/containers/configure-language-java#spring-boot-1)従います。

> [!NOTE]
> 「[構成のパラメーター化](#parameterize-the-configuration)」セクションの変数を使用してパラメーター化された Spring Boot アプリケーション設定の場合、それらの環境変数は、アプリケーション構成で定義する必要があります。 環境変数を使用して明示的にパラメーター化されていない Spring Boot アプリケーション設定は、アプリケーション構成によって依然としてオーバーライドできます。 次に例を示します。

  ```properties
  spring.jms.servicebus.connection-string=${CUSTOMCONNSTR_SERVICE_BUS}
  spring.jms.servicebus.topic-client-id=contoso1
  spring.jms.servicebus.idle-timeout=1800000
  ```

![App Service アプリケーションの構成](media/migrate-java-se-to-java-se-app-service/app-service-parameterized-spring-boot-app-settings.png)

[!INCLUDE [migrate-scheduled-jobs](includes/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>再起動とスモーク テスト

最後に、Web アプリを再起動してすべての構成の変更を適用する必要があります。 再起動が完了したら、アプリケーションが正しく実行されていることを確認します。

## <a name="post-migration"></a>移行後

アプリケーションを Azure App Service に移行したので、期待どおりに動作することを確認する必要があります。 これを完了したら、アプリケーションをよりクラウドネイティブにするための推奨事項がいくつかあります。

### <a name="recommendations"></a>Recommendations

* ファイル ストレージ用に */home* ディレクトリを使用することを選択した場合は、[それを Azure Storage に置き換える](/azure/app-service/containers/how-to-serve-content-from-azure-storage)ことを検討してください。

* 接続文字列、SSL キー、およびその他の機密情報が含まれている */home* ディレクトリ内に構成がある場合、可能であれば、[Azure Key Vault](/azure/app-service/app-service-key-vault-references) または[アプリケーション設定を使用するパラメーター インジェクション](/azure/app-service/configure-common#configure-app-settings)、あるいはその両方を使用することを検討してください。

* ダウンタイムなしで信頼性の高いデプロイを行うには、[デプロイ スロットの使用](/azure/app-service/deploy-staging-slots)を検討してください。

* DevOps の戦略を設計し、実装します。 信頼性を維持しながら開発速度を向上させるには、[Azure Pipelines を使用してデプロイとテストを自動化する](/azure/devops/pipelines/ecosystems/java-webapp)ことを検討してください。 デプロイ スロットを使用する場合は、[スロットへのデプロイと後続のスロット スワップを自動化](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot)できます。

* 事業継続とディザスター リカバリー戦略を設計し、実装します。 ミッション クリティカルなアプリケーションの場合は、[複数リージョン デプロイのアーキテクチャ](/azure/architecture/reference-architectures/app-service-web-app/multi-region)を検討してください。
