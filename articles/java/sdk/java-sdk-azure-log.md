---
title: Azure SDK for Java でのログ記録の構成
description: Azure SDK for Java クライアント ライブラリのログ記録フレームワークを構成する方法について説明します
keywords: Azure, Java, SDK, ログ記録
author: bmitchell287
ms.author: brendm
ms.date: 03/25/2020
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 9f7ad8d772b7de1335ebc3ba77cdea8aa1e8b683
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81671948"
---
# <a name="configure-logging-with-the-azure-sdk-for-java"></a>Azure SDK for Java でのログ記録の構成

この記事では、[Azure SDK](https://azure.microsoft.com/downloads/) for Java のログ記録の構成例を示します。 クラス別のログ レベルやカスタム ログ記録の設定など、構成オプションの詳細については、選択したログ記録フレームワークのドキュメントを参照してください。

Azure SDK for Java クライアント ライブラリでは、[Simple Logging Facade for Java](https://www.slf4j.org/) (SLF4J) が使用されます。 SLF4J を使用すると、任意のログ記録フレームワークを使用できます。これは、アプリケーションのデプロイ時に呼び出されます。

> [!NOTE]
> この記事は、Azure SDK クライアント ライブラリの最新バージョンに適用されます。 ライブラリがサポートされているかどうかを確認するには、「[Azure SDK の最新リリース](https://azure.github.io/azure-sdk/releases/latest/java.html)」の一覧を参照してください。 Azure SDK クライアント ライブラリの古いバージョンをアプリケーションで使用している場合は、該当するサービスのドキュメントで具体的な手順を参照してください。

## <a name="declare-a-logging-framework"></a>ログ記録フレームワークを宣言する

これらのロガーを実装する前に、関連するフレームワークをプロジェクトの依存関係として宣言する必要があります。 詳細については、[SLF4J のユーザー マニュアル](http://www.slf4j.org/manual.html#projectDep)を参照してください。

## <a name="configure-log4j-or-log4j-2"></a>Log4j または Log4j 2 を構成する

プロパティ ファイルまたは XML ファイルで Log4j およびLog4j 2 ログ記録を構成できます。 Log4j およびLog4j 2 ログ記録の詳細情報については、[Apache Log4j 2 のマニュアル](https://logging.apache.org/log4j/2.x/manual/configuration.html)を参照してください。

### <a name="use-a-properties-file"></a>プロパティ ファイルを使用する

プロジェクトの *./src/main/resource* ディレクトリに、*log4j.properties* または *log4j2.properties* という名前の新しいファイルを作成します (後者が Logj4 2 用)。 以下の例を使用して作業を開始します。

Log4j の例:

```properties
log4j.rootLogger=INFO, A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%m%n
log4j.logger.com.azure.core=ERROR
```

Log4j2 の例:

```properties
appender.console.type = Console
appender.console.name = LogToConsole
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = %msg%n
logger.app.name=com.azure.core
logger.app.level=ERROR
```

### <a name="use-an-xml-file"></a>XML ファイルを使用する

あるいは、XML ファイルを使用して Log4j と Log4j2 を構成することもできます。 プロジェクトの *./src/main/resource* ディレクトリに、*log4j.xml* または *log4j2.xml* という名前の新しいファイルを作成します (後者が Logj4 2 用)。 以下の例を使用して作業を開始します。

Log4j の例:

```xml
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true" xmlns:log4j='http://jakarta.apache.org/log4j/'>

  <appender name="console" class="org.apache.log4j.ConsoleAppender">
    <param name="Target" value="System.out"/>
    <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" value="%m%n" />
    </layout>
  </appender>
  <logger name="com.azure.core" additivity="true">
    <level value="ERROR" />
    <appender-ref ref="console" />
  </logger>

  <root>
    <priority value ="info"></priority>
    <appender-ref ref="console"></appender>
  </root>

</log4j:configuration>
```

Log4j2 の例:

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

## <a name="configure-logback"></a>Logback を構成する

[Logback](https://logback.qos.ch/manual/introduction.html) は、よく使用されるログ記録フレームワークの 1 つであり、SLF4J のネイティブ実装です。 Logback を構成するには、プロジェクトの *./src/main/resources* ディレクトリに *logback.xml* という名前の新しい XML ファイルを作成します。 構成オプションの詳細情報については、[Logback プロジェクトの Web サイト](https://logback.qos.ch/manual/configuration.html)で確認できます。

Logback の構成例を次に示します。

```xml
<?xml version="1.0" encoding="UTF-8"?>
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

コンソールへのログ記録用の単純な Logback 構成を次に示します。

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

1 時間ごとにロールオーバーされ、GZIP ファイル形式でアーカイブされるファイルへのログ記録の構成を次に示します。

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

### <a name="configure-logback-for-a-spring-boot-application"></a>Spring Boot アプリケーション用に Logback を構成する

Spring は、 *./src/main/resources* ディレクトリにある *application.properties*ファイルで、ログ記録などのプロジェクトの構成を検索します。 *application.properties* ファイルに次の行を追加して、*logback.xml*を Spring Boot アプリケーションにリンクします。

```properties
logging.config=classpath:logback.xml
```

## <a name="configure-fallback-logging-for-temporary-debugging"></a>一時的なデバッグ用のファールバック ログ記録を構成する

SLF4J でアプリケーションを再デプロイできない場合は、Azure Core 1.3.0 以降の Java 用 Azure クライアント ライブラリに組み込まれているフォールバック ロガーを使用できます。 このロガーを有効にするには、まず SLF4J がないことを確認してから (これが優先されるため)、`AZURE_LOG_LEVEL` 環境変数を設定します。 この環境変数を設定した後に、アプリケーションを再起動してログの生成を開始します。

次の表は、この環境変数に使用できる値を示しています。

|ログ レベル   |使用可能な環境変数の値   |
|----------|-----------|
|詳細   |"verbose"、"debug"     |
|情報|"info"、"information"、"informational"  |
|WARNING     |"warn"、"warning"       |
|ERROR    |"err"、"error"  |

## <a name="next-steps"></a>次のステップ

- [Azure App Service でのアプリの診断ログの有効化](/azure/app-service/troubleshoot-diagnostic-logs) 
- [Azure のセキュリティ ログと監査のオプションを確認する](/azure/security/fundamentals/log-audit)
- [Azure プラットフォーム ログを操作する方法を確認する](/azure/azure-monitor/platform/platform-logs-overview)
