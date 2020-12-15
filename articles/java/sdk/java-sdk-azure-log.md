---
title: Azure SDK for Java でのログ記録の構成
description: Azure SDK for Java クライアント ライブラリのログ記録フレームワークを構成する方法について説明します
keywords: Azure, Java, SDK, ログ記録
author: bmitchell287
ms.author: brendm
ms.date: 03/25/2020
ms.topic: article
ms.service: multiple
ms.custom: devx-track-java
ms.openlocfilehash: 927f20601ded9a0ea6b2793ef1b0c8e1b5e6ac19
ms.sourcegitcommit: ae2fa266a36958c04625bb0ab212e6f2db98e026
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857770"
---
# <a name="configure-logging-with-the-azure-sdk-for-java"></a>Azure SDK for Java でのログ記録の構成

この記事では、[Azure SDK](https://azure.microsoft.com/downloads/) for Java のログ記録の構成例を示します。 クラス別のログ レベルやカスタム ログ記録の設定など、構成オプションの詳細については、選択したログ記録フレームワークのドキュメントを参照してください。

Azure SDK for Java クライアント ライブラリでは、[Simple Logging Facade for Java](https://www.slf4j.org/) (SLF4J) が使用されます。 SLF4J を使用すると、任意のログ記録フレームワークを使用できます。これは、アプリケーションのデプロイ時に呼び出されます。 クライアント ビルダーで [HttpLogOptions](/java/api/com.azure.core.http.policy.httplogoptions?view=azure-java-stable) を設定する機能が提供されている場合は、すべてのログを出力するために、[HttpLogDetailLevel](/java/api/com.azure.core.http.policy.httplogdetaillevel?view=azure-java-stable) および許容されるすべてのヘッダーとクエリ パラメーターも指定する必要があります。

> [!NOTE]
> この記事は、Azure SDK クライアント ライブラリの最新バージョンに適用されます。 ライブラリがサポートされているかどうかを確認するには、「[Azure SDK の最新リリース](https://azure.github.io/azure-sdk/releases/latest/java.html)」の一覧を参照してください。 Azure SDK クライアント ライブラリの古いバージョンをアプリケーションで使用している場合は、該当するサービスのドキュメントで具体的な手順を参照してください。

## <a name="declare-a-logging-framework"></a>ログ記録フレームワークを宣言する

これらのロガーを実装する前に、関連するフレームワークをプロジェクトの依存関係として宣言する必要があります。 詳細については、[SLF4J のユーザー マニュアル](https://www.slf4j.org/manual.html#projectDep)を参照してください。

次のセクションでは、一般的なログ記録フレームワークの構成例を示します。

## <a name="use-log4j"></a>Log4j を使用する

次の例は、Log4j ログ記録フレームワークの構成を示しています。 詳細については、[Log4j のドキュメント](https://logging.apache.org/log4j/1.2/)を参照してください。

**Maven の依存関係を追加して Log4j を有効にする**

自分のプロジェクトの *pom.xml* ファイルに以下を追加します。

```xml
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>[1.0,)</version> <!-- Version number 1.0 and above -->
</dependency>
```

**プロパティ ファイルを使用して Log4j を有効にする**

自分のプロジェクトの *./src/main/resource* ディレクトリに *log4j.properties* ファイルを作成し、次の内容を追加します。

```properties
log4j.rootLogger=INFO, A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%m%n
log4j.logger.com.azure.core=ERROR
```

**XML ファイルを使用して Log4j を有効にする**

自分のプロジェクトの *./src/main/resource* ディレクトリに *log4j.xml* ファイルを作成し、次の内容を追加します。

```xml
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true" xmlns:log4j='http://jakarta.apache.org/log4j/'>

    <appender name="console" class="org.apache.log4j.ConsoleAppender">
        <param name="Target" value="System.out"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%m%n" />
        </layout>
    </appender>
    <logger name="com.azure.core">
        <level value="ERROR" />
        <appender-ref ref="console" />
    </logger>

    <root>
        <level value="info" />
        <appender-ref ref="console" />
    </root>

</log4j:configuration>
```

## <a name="use-log4j-2"></a>Log4j 2 を使用する

次の例は、Log4j 2 ログ記録フレームワークの構成を示しています。 詳細については、[Log4j 2 のドキュメント](https://logging.apache.org/log4j/2.x/manual/configuration.html)を参照してください。

**Maven の依存関係を追加して Log4j 2 を有効にする**

自分のプロジェクトの *pom.xml* ファイルに以下を追加します。

```
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-slf4j-impl -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>[2.0,)</version> <!-- Version number 2.0 and above -->
</dependency>
```

**プロパティ ファイルを使用して Log4j 2 を有効にする**

自分のプロジェクトの *./src/main/resource* ディレクトリに *log4j2.properties* ファイルを作成し、次の内容を追加します。

```properties
appender.console.type = Console
appender.console.name = STDOUT
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = %msg%n
logger.app.name=com.azure.core
logger.app.level=ERROR

rootLogger.level = info
rootLogger.appenderRefs = stdout
rootLogger.appenderRef.stdout.ref = STDOUT
```

**XML ファイルを使用して Log4j 2 を有効にする**

自分のプロジェクトの *./src/main/resource* ディレクトリに *log4j2.xml* ファイルを作成し、次の内容を追加します。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="INFO">
    <Appenders>
        <Console name="console" target="SYSTEM_OUT">
            <PatternLayout
                pattern="%msg%n" />
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.azure.core" level="error" additivity="true">
            <appender-ref ref="console" />
        </Logger>
        <Root level="info" additivity="false">
            <appender-ref ref="console" />
        </Root>
     </Loggers>
</Configuration>
```

## <a name="use-logback"></a>Logback を使用する

次の例は、Logback ログ記録フレームワークの基本構成を示しています。 詳細については、[Logback のドキュメント](https://logback.qos.ch/manual/configuration.html)を参照してください。

**Maven の依存関係を追加して Logback を有効にする**

自分のプロジェクトの *pom.xml* ファイルに以下を追加します。

```
<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>[0.2.5,)</version> <!-- Version number 0.2.5 and above -->
</dependency>
```

**XML ファイルを使用して Logback を有効にする**

自分のプロジェクトの *./src/main/resource* ディレクトリに *logback.xml* ファイルを作成し、次の内容を追加します。

```xml
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%message%n</pattern>
    </encoder>
  </appender>

  <logger name="com.azure.core" level="ERROR" />

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

## <a name="use-logback-in-a-spring-boot-application"></a>Spring Boot アプリケーションで Logback を使用する

次の例では、Spring で Logback を使用するための構成をいくつか示します。 通常、自分のプロジェクトの *./src/main/resources* ディレクトリ内の *logback.xml* ファイルにログ記録の構成を追加します。 Spring では、ログ記録などのさまざまな構成について、このファイルが参照されます。 詳細については、[Logback のドキュメント](https://logback.qos.ch/manual/configuration.html)を参照してください。

任意のファイルから Logback 構成を読み取るようにアプリケーションを構成できます。 *logback.xml* ファイルを Spring アプリケーションにリンクするには、自分のプロジェクトの *./src/main/resources* ディレクトリに *application.properties* ファイルを作成し、次の内容を追加します。

```properties
logging.config=classpath:logback.xml
```

コンソールにログを記録するための Logback 構成を作成するには、次の内容を *logback.xml* ファイルに追加します。

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <appender name="Console"
    class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <Pattern>
        %black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %blue(%logger{100}): %msg%n%throwable
      </Pattern>
    </layout>
  </appender>

  <root level="INFO">
    <appender-ref ref="Console" />
  </root>
</configuration>
```

1 時間ごとにロールオーバーされ、gzip 形式でアーカイブされるファイルへのログ記録を構成するには、次の内容を *logback.xml* ファイルに追加します。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <property name="LOGS" value="./logs" />
  <appender name="RollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>${LOGS}/spring-boot-logger.log</file>
    <encoder
      class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
      <Pattern>%d %p %C{1.} [%t] %m%n</Pattern>
    </encoder>

    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <!-- rollover hourly and gzip logs -->
      <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd-HH}.log.gz</fileNamePattern>
    </rollingPolicy>
  </appender>

  <!-- LOG everything at INFO level -->
  <root level="info">
    <appender-ref ref="RollingFile" />
  </root>
</configuration>
```

## <a name="configure-fallback-logging-for-temporary-debugging"></a>一時的なデバッグ用のファールバック ログ記録を構成する

SLF4J ロガーでアプリケーションを再デプロイできない場合は、Azure Core 1.3.0 以降の Java 用 Azure クライアント ライブラリに組み込まれているフォールバック ロガーを使用できます。 このロガーを有効にするには、まず SLF4J がないことを確認してから (これが優先されるため)、`AZURE_LOG_LEVEL` 環境変数を設定します。 この環境変数を設定した後に、アプリケーションを再起動してログの生成を開始します。

次の表は、この環境変数に使用できる値を示しています。

|ログ レベル   |使用可能な環境変数の値   |
|----------|-----------|
|詳細   |"verbose"、"debug"     |
|情報|"info"、"information"、"informational"  |
|WARNING     |"warn"、"warning"       |
|ERROR    |"err"、"error"  |

## <a name="setting-an-httplogdetaillevel"></a>HttpLogDetailLevel の設定
使用されるログ メカニズムに関係なく、クライアント ビルダーで [HttpLogOptions](/java/api/com.azure.core.http.policy.httplogoptions?view=azure-java-stable) を設定する機能が提供されている場合、ログを出力するにはこれらのオプションを追加で構成する必要があります。 ログに記録する情報を示すには、[HttpLogDetailLevel](/java/api/com.azure.core.http.policy.httplogdetaillevel?view=azure-java-stable) を指定する必要があります。  この値は既定で `NONE` に設定されているため、これが指定されていない場合、ログ記録フレームワークまたはフォールバック ログ記録が適切に構成されていても、ログは出力されません。 セキュリティ上の理由により、ヘッダーとクエリ パラメーターは既定で修正されているため、安全に出力できるヘッダーおよびクエリ パラメーターを示す `Set<String>` でログ オプションも指定する必要があります。 これらの値は下のように構成できます。 ログ記録は、本文の内容とヘッダーの両方の値を出力するように設定されます。すべてのヘッダー値は、キー `"foo"` に対応するユーザー指定のメタデータの値を除いて修正され、すべてのクエリ パラメーターは、存在する可能性があるすべての SAS の署名済みバージョンを示す SAS トークン クエリ パラメーター `"sv"` を除いて修正されます。 

```
new BlobClientBuilder().endpoint(<endpoint>)
            .httpLogOptions(new HttpLogOptions()
                .setLogLevel(HttpLogDetailLevel.BODY_AND_HEADERS)
                .setAllowedHeaderNames(Set.of("x-ms-meta-foo"))
                .setAllowedQueryParamNames(Set.of("sv")))
            .buildClient();
```
> [!NOTE]
> この例ではストレージ クライアント ビルダーを使用していますが、この原則は、`HttpLogOptions` を受け入れるすべてのビルダーに適用されます。 さらに、この例はクライアントの完全な構成を例示するものではなく、ログ記録の構成を示すことだけを目的としています。 クライアントの構成の詳細については、各ビルダーのドキュメントを参照してください。

## <a name="next-steps"></a>次の手順

- [Azure App Service のアプリの診断ログの有効化](/azure/app-service/troubleshoot-diagnostic-logs) 
- [Azure のセキュリティ ログと監査のオプションを確認する](/azure/security/fundamentals/log-audit)
- [Azure プラットフォーム ログを操作する方法を確認する](/azure/azure-monitor/platform/platform-logs-overview)
