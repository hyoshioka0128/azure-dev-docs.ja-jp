---
title: Azure で実行中の Java アプリ用の Logz.io の概要
description: このチュートリアルでは、Azure で実行される Java アプリ用に Logz.io を統合して構成する方法について説明します。
author: jdubois
manager: bborges
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: judubois
ms.openlocfilehash: d7f90939701bfbcdcd895b8baf3cabcae4d5efa9
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812452"
---
# <a name="tutorial-getting-started-with-monitoring-and-logging-using-logzio-for-java-apps-running-on-azure"></a>チュートリアル:Azure で実行される Java アプリ用の Logz.io を使用した監視とログ記録の概要

このチュートリアルでは、インジェストと分析のために [Logz.io](https://logz.io/) サービスにログを送信するように、クラシック Java アプリケーションを構成する方法について説明します。 Logz.io により、Elasticsearch/Logstash/Kibana (ELK) および Grafana に基づく完全な監視ソリューションが提供されます。

このチュートリアルは、Log4J または Logback の使用を前提としています。 これらのライブラリは、Java でのログ記録に最も広く使用されています。そのため、このチュートリアルは Azure で実行されるほとんどのアプリケーションに適用できます。 既に Elastic スタックを使用して Java アプリケーションを監視している場合、このチュートリアルでは、Logz.io エンドポイントをターゲットにするように再構成する方法を示します。

このチュートリアルで学習する内容は次のとおりです。

> [!div class="checklist"]
> * 既存の Java アプリケーションから Logz.io にログを送信する。
> * 診断ログとメトリックを Azure サービスから Logz.io に送信する。

## <a name="prerequisites"></a>前提条件

* [Java Developer Kit](https://aka.ms/azure-jdks) バージョン 8 以降
* [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/logz.logzio-elk-as-a-service-pro) からの Logz.io アカウント
* Log4J または Logback を使用する既存の Java アプリケーション

## <a name="send-java-application-logs-to-logzio"></a>Java アプリケーションログを Logz.io に送信する

まず、Logz.io アカウントへのアクセス権を付与するトークンを使用して Java アプリケーションを構成する方法について説明します。

### <a name="get-your-logzio-access-token"></a>Logz.io アクセス トークンを取得する

トークンを取得するには、Logz.io アカウントにログインし、右上隅の歯車アイコンを選択します。次に、 **[Settings]\(設定\) > [General]\(全般\)** を選択します。 アカウント設定に表示されているアクセス トークンをコピーして、後で使用できるようにします。

### <a name="install-and-configure-the-logzio-library-for-log4j-or-logback"></a>Log4J または Logback 用の Logz.io ライブラリをインストールして構成する

Logz.io Java ライブラリは Maven Central で入手できるため、依存関係としてアプリ構成に追加できます。 Maven Central でバージョン番号を確認し、次の構成設定で最新バージョンを使用します。

Maven を使用している場合は、`pom.xml` ファイルに次の依存関係を追加します。

**Log4J:**

```xml
<dependency>
    <groupId>io.logz.log4j2</groupId>
    <artifactId>logzio-log4j2-appender</artifactId>
    <version>1.0.11</version>
</dependency>
```

**Logback:**

```xml
<dependency>
    <groupId>io.logz.logback</groupId>
    <artifactId>logzio-logback-appender</artifactId>
    <version>1.0.22</version>
</dependency>
```

Gradle を使用している場合は、ビルド スクリプトに次の依存関係を追加します。

**Log4J:**

```
implementation 'io.logz.log4j:logzio-log4j-appender:1.0.11'
```

**Logback:**

```
implementation 'io.logz.logback:logzio-logback-appender:1.0.22'
```

次に、Log4J または Logback の構成ファイルを更新します。

**Log4J:**

```xml
<Appenders>
    <LogzioAppender name="Logzio">
        <logzioToken><your-logz-io-token></logzioToken>
        <logzioType>java-application</logzioType>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
    </LogzioAppender>
</Appenders>

<Loggers>
    <Root level="info">
        <AppenderRef ref="Logzio"/>
    </Root>
</Loggers>
```

**Logback:**

```xml
<configuration>
    <!-- Use shutdownHook so that we can close gracefully and finish the log drain -->
    <shutdownHook class="ch.qos.logback.core.hook.DelayingShutdownHook"/>
    <appender name="LogzioLogbackAppender" class="io.logz.logback.LogzioLogbackAppender">
        <token><your-logz-io-token></token>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
        <logzioType>java-application</logzioType>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>INFO</level>
        </filter>
    </appender>

    <root level="debug">
        <appender-ref ref="LogzioLogbackAppender"/>
    </root>
</configuration>
```

`<your-logz-io-token>` プレースホルダーを実際のアクセス トークンに、`<your-logz-io-listener-host>` プレースホルダーを自分の地域のリスナー ホスト (listener.logz.io など) に置き換えます。 アカウントのリージョンを検索する方法の詳細については、「[アカウント リージョン](https://docs.logz.io/user-guide/accounts/account-region.html)」を参照してください。

`logzioType` 要素は、異なるドキュメントを相互に分離するために使用される Elasticsearch の論理フィールドを参照します。 Logz.io を最大限に活用するには、このパラメーターを適切に構成することが不可欠です。

Logz.io の "Type" は、ログの形式 (例: Apache、NGinx、MySQL) であり、ソース (例: server1、server2、server3) ではありません。 このチュートリアルでは、Java アプリケーションを構成しているため `java-application` 型を呼び出しています。これらのアプリケーションはすべて同じ形式であると想定されています。

高度な使用方法として、Java アプリケーションをさまざまな型にグループ化することができます。各グループは独自のログ形式 (Log4J と Logback で構成可能) を持ちます。 たとえば、"spring-boot-monolith" という型と "spring-boot-microservice" という型を使用できます。

### <a name="test-your-configuration-and-log-analysis-on-logzio"></a>Logz.io で構成とログ分析をテストする

Logz.io ライブラリの構成が完了すると、アプリケーションはログを直接送信するようになります。 すべてが正しく動作することをテストするには、Logz.io コンソールにアクセスし、 **[Live tail]\(ライブ テイル\)** タブを選択し、 **[run]\(実行\)** を選択します。 接続が機能していることを示す次のようなメッセージが表示されます。

```output
Requesting Live Tail access...
Access granted. Opening connection...
Connected. Tailing...
````

次に、アプリケーションを起動するか、ログを生成するために使用します。 ログが画面に直接表示されます。 たとえば、Spring Boot アプリケーションの最初の起動メッセージは次のようになります。

```output
2019-09-19 12:54:40.685Z Starting JavaApp on javaapp-default-9-5cfcb8797f-dfp46 with PID 1 (/workspace/BOOT-INF/classes started by cnb in /workspace)
2019-09-19 12:54:40.686Z The following profiles are active: prod
2019-09-19 12:54:42.052Z Bootstrapping Spring Data repositories in DEFAULT mode.
2019-09-19 12:54:42.169Z Finished Spring Data repository scanning in 103ms. Found 6 repository interfaces.
2019-09-19 12:54:43.426Z Bean 'spring.task.execution-org.springframework.boot.autoconfigure.task.TaskExecutionProperties' of type [org.springframework.boot.autoconfigure.task.TaskExecutionProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
```

ログが Logz.io によって処理されたので、プラットフォームのすべてのサービスを活用できます。

## <a name="send-azure-services-data-to-logzio"></a>Azure サービスのデータを Logz.io に送信する

次に、Azure リソースから Logz.io にログとメトリックを送信する方法について説明します。

### <a name="deploy-the-template"></a>テンプレートのデプロイ

最初の手順では、Logz.io と Azure の統合テンプレートをデプロイします。 統合は、既製の Azure デプロイ テンプレートに基づいており、パイプラインのすべての必要な構成要素を設定します。 このテンプレートは、イベント ハブ名前空間、イベント ハブ、2 つのストレージ BLOB、および必要なすべての適切なアクセス許可と接続を作成します。 自動化されたデプロイによって設定されたリソースは、1 つの Azure リージョンのデータを収集し、そのデータを Logz.io に送信できます。

[リポジトリの readme の最初の手順](https://github.com/logzio/logzio-azure-serverless)に表示されている **[Deploy to Azure]** ボタンを見つけます。

**[Deploy to Azure]** を選択すると、Azure portal の **[カスタム デプロイ]** ページに、入力済みのフィールドの一覧が表示されます。

ほとんどのフィールドはそのままにしておくことができますが、次の設定を入力してください。

* **[リソース グループ]** :既存のグループを選択するか、新しいグループを作成します。
* **Logzio Logs/Metrics Host \(Logzio ログ/メトリック ホスト\)** :Logz.io リスナーの URL を入力します。 この URL がわからない場合は、ログイン URL を確認してください。 それが app.logz.io の場合、listener.logz.io (既定の設定) を使用します。 app-eu.logz.io の場合は listener-eu.logz.io を使用します。
* **Logzio Logs/Metrics Token \(Logzio ログ/メトリック トークン\)** :Azure のログまたはメトリックを送付する Logz.io アカウントのトークンを入力します。 このトークンは、Logz.io UI のアカウント ページで見つけることができます。

ページの下部にある条項に同意し、 **[Purchase]\(購入\)** を選択します。 その後、Azure によってテンプレートがデプロイされます。これには 1 - 2 分かかる場合があります。 最終的には、ポータルの上部に "デプロイに成功しました" というメッセージが表示されます。

定義されたリソース グループにアクセスして、デプロイされたリソースを確認できます。

Azure Blob Storage にデータをバックアップするように logzio-azure-serverless を構成する方法については、「[Ship Azure activity logs](https://docs.logz.io/shipping/log-sources/azure-activity-logs.html)」を参照してください。

### <a name="stream-azure-logs-and-metrics-to-logzio"></a>Azure のログとメトリックを Logz.io にストリーム配信する

統合テンプレートをデプロイしたので、デプロイしたイベント ハブに診断データをストリーム配信するように Azure を構成する必要があります。 データがイベント ハブに送られると、関数アプリがそのデータを Logz.io に転送します。

1. 検索バーに「診断」と入力し、 **[診断設定]** を選択します。

2. リソースの一覧からリソースを選択し、 **[診断設定を追加する]** を選択して、 **[診断設定]** パネルを開きます。

    ![[診断設定] パネル](media/java-get-started-with-logzio/diagnostics-settings.png)

3. 診断設定の**名前**を付けます。

4. **[イベントハブへのストリーム]** を選択し、 **[構成]** を選択して **[イベント ハブの選択]** パネルを開きます。

5. イベント ハブを選択します。

    * **イベント ハブの名前空間の選択**:先頭が **Logzio** の名前空間 (`LogzioNS6nvkqdcci10p`, など) を選択します。
    * **イベント ハブ名を選択する**:ログ用に **insights-operational-logs** を選択し、メトリック用に **insights-operational-metrics** を選択します。
    * **イベント ハブ ポリシー名の選択**:**LogzioSharedAccessKey** を選択します。

6. **[OK]** を選択して、 **[診断設定]** パネルに戻ります。

7. [ログ] セクションで、ストリーム配信するデータを選択し、 **[保存]** を選択します。

これで、選択したデータがイベント ハブにストリーム配信されるようになります。

### <a name="visualize-your-data"></a>データを視覚化する

次に、少し時間をおいてデータをシステムから Logz.io に送信してから、Kibana を開きます。 データ (種類 _eventhub_) がダッシュボードに表示されます。 ダッシュボードを作成する方法の詳細については、「[Creating the Perfect Kibana Dashboard](https://logz.io/blog/perfect-kibana-dashboard/)」を参照してください。

そこから、 **[検出]** タブで特定のデータを照会したり、Kibana オブジェクトを作成して **[視覚化]** タブでデータを視覚化したりできます。

## <a name="clean-up-resources"></a>リソースのクリーンアップ

このチュートリアルで作成した Azure リソースの使用が完了したら、次のコマンドを使用して削除できます。

```azurecli-interactive
az group delete --name <resource group>
```

## <a name="next-steps"></a>次の手順

このチュートリアルでは、Logz.io にログとメトリックを送信するように Java アプリケーションと Azure サービスを構成する方法について学習しました。

次に、イベント ハブを使用してアプリケーションを監視する方法について詳しく説明します。

> [!div class="nextstepaction"]
> [外部ツールで使用する Azure 監視データのイベント ハブへのストリーム配信](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)
