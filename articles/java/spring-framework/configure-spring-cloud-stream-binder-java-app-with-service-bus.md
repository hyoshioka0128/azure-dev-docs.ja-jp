---
title: Azure Service Bus 用の Spring Cloud Azure Stream Binder を使用する方法
description: この記事では、Spring Cloud Stream Binder を使用して Azure Service Bus との間でメッセージを送受信する方法について説明します。
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 08/21/2019
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 1ecedc4f3b3fb3eb92b66403f00aa14660323ce2
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379046"
---
# <a name="how-to-use-spring-cloud-azure-stream-binder-for-azure-service-bus"></a>Azure Service Bus 用の Spring Cloud Azure Stream Binder を使用する方法

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

Azure には、[Azure Service Bus](/azure/service-bus-messaging/service-bus-messaging-overview) ("Service Bus") という、[Advanced Message Queueing Protocol 1.0](http://www.amqp.org/) ("AMQP 1.0") 標準に基づいた非同期のメッセージング プラットフォームが用意されています。 Service Bus は、サポートされている Azure プラットフォームの範囲全体で使用することができます。

この記事では、Spring Cloud Stream Binder を使用して Service Bus `queues` および `topics` との間でメッセージを送受信する方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事の前提条件は次のとおりです。

1. Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)を有効にするか、[無料アカウント](https://azure.microsoft.com/free/)にサインアップしてください。

1. サポートされている Java Development Kit (JDK) (バージョン 8 以降)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。

1. Apache [Maven](http://maven.apache.org/) バージョン 3.2 以降。

1. 構成済みの Service Bus キューまたはトピックが既にある場合は、Service Bus 名前空間が次の要件を満たすようにしてください。

    1. すべてのネットワークからのアクセスを許可する
    1. Standard (またはそれ以上)
    1. 自分のキューおよびトピックに対する読み取り/書き込みアクセス権限を含んだアクセス ポリシーがある

1. 構成済みの Service Bus キューまたはトピックがない場合は、Azure portal を使用して [Service Bus キュー](/azure/service-bus-messaging/service-bus-quickstart-portal)または [Service Bus トピック](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal)を作成します。 前の手順で指定された要件を名前空間が確実に満たすようにしてください。 また、名前空間に含まれる接続文字列をメモしてください。これは、このチュートリアルのテスト アプリに必要です。

1. Spring Boot アプリケーションがない場合は、[Spring Initializr で **Maven** プロジェクトを作成](https://start.spring.io/)します。 必ず、 **[Maven プロジェクト]** を選択し、 **[依存関係]** に **[Web]** 依存関係を追加してください。

## <a name="use-the-spring-cloud-stream-binder-starter"></a>Spring Cloud Stream Binder スターターを使用する

1. 自分のアプリの親ディレクトリで *pom.xml* ファイルを探します。例:

    `C:\SpringBoot\servicebus\pom.xml`

    または

    `/users/example/home/servicebus/pom.xml`

1. テキスト エディターで *pom.xml* ファイルを開きます。

1. Service Bus キューとトピックのどちらを使用しているかに応じて、 **&lt;dependencies>** 要素に次のコード ブロックを追加します。

    **Service Bus キュー**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-queue-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Service Bus キューの pom.xml ファイルを編集します。](media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-queue.png)

    **Service Bus トピック**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-topic-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Service Bus トピックの pom.xml ファイルを編集します。](media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-topic.png)

1. *pom.xml* ファイルを保存して閉じます。

## <a name="configure-the-app-for-your-service-bus"></a>アプリでサービス バス用の構成を行う

接続文字列または資格情報ファイルに基づいて、自分のアプリを構成できます。 このチュートリアルでは、接続文字列を使用します。 資格情報ファイルの使用の詳細については、「[Service Bus キュー用 Spring Cloud Azure Stream Binder のコード サンプル](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-queue-binder-sample#credential-file-based-usage
)」と「[Service Bus トピック用 Spring Cloud Azure Stream Binder のコード サンプル](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-topic-binder-sample#credential-file-based-usage)」を参照してください。

1. アプリの *resources* ディレクトリ内で *application.properties* ファイルを探します。次に例を示します。

   `C:\SpringBoot\servicebus\src\main\resources\application.properties`

   または

   `/users/example/home/servicebus/src/main/resources/application.properties`

1. テキスト エディターで *application.properties* ファイルを開きます。

1. Service Bus キューとトピックのどちらを使用しているかに応じて、*application.properties* ファイルの最後に適切なコードを追加します。 [「フィールドの説明」の表](#fd)を使用して、サンプルの値を、実際のサービス バスの適切なプロパティに置き換えてください。

    **Service Bus キュー**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=examplequeue
    spring.cloud.stream.bindings.output.destination=examplequeue
    spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **Service Bus トピック**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=exampletopic
    spring.cloud.stream.bindings.input.group=examplesubscription
    spring.cloud.stream.bindings.output.destination=exampletopic
    spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **<a name="fd">フィールドの説明</a>**

    |                                        フィールド                                   |                                                                                   説明                                                                                    |
    |--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |               `spring.cloud.azure.servicebus.connection-string`                |                                        Azure portal の自分の Service Bus 名前空間で取得した接続文字列を指定します。                                   |
    |               `spring.cloud.stream.bindings.input.destination`                 |                            このチュートリアルで自分が使用した Service Bus キューまたは Service Bus トピックを指定します。                         |
    |                  `spring.cloud.stream.bindings.input.group`                    |                                            Service Bus トピックを使用した場合は、トピックのサブスクリプションを指定します。                                |
    |               `spring.cloud.stream.bindings.output.destination`                |                               入力先に使用したものと同じ値を指定します。                        |
    | `spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode` |                                                       `MANUAL` を指定します。                                                   |
    | `spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode` |                                                       `MANUAL` を指定します。                                                   |

1. *application.properties* ファイルを保存して閉じます。

## <a name="implement-basic-service-bus-functionality"></a>基本的な Service Bus 機能を実装する

このセクションでは、自分のサービス バスにメッセージを送信するために必要な Java クラスを作成します。

### <a name="modify-the-main-application-class"></a>アプリケーションのメイン クラスを変更する

1. アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。

    `C:\SpringBoot\servicebus\src\main\java\com\example\ServiceBusBinderApplication.java`

   または

   `/users/example/home/servicebus/src/main/java/com/example/ServiceBusBinderApplication.java`

1. テキスト エディターでメイン アプリケーションの Java ファイルを開きます。

1. 次のコードをファイルに追加します。

    ```java
    package com.example;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusBinderApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusBinderApplication.class, args);
        }
    }
    ```

1. ファイルを保存して閉じます。

### <a name="create-a-new-class-for-the-source-connector"></a>ソース コネクタの新しいクラスを作成する

1. テキスト エディターを使用して、*StreamBinderSource.java* という名前の Java ファイルを自分のアプリのパッケージ ディレクトリに作成します。

1. この新しいファイルに次のコードを追加します。

    ```java
    package com.example;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @EnableBinding(Source.class)
    @RestController
    public class StreamBinderSource {

        @Autowired
        private Source source;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```

1. *StreamBinderSources.java* ファイルを保存して閉じます。

### <a name="create-a-new-class-for-the-sink-connector"></a>シンク コネクタの新しいクラスを作成する

1. テキスト エディターを使用して、*StreamBinderSink.java* という名前の Java ファイルを自分のアプリのパッケージ ディレクトリに作成します。

1. この新しいファイルに次のコード行を追加します。

    ```java
    package com.example;

    import com.microsoft.azure.spring.integration.core.AzureHeaders;
    import com.microsoft.azure.spring.integration.core.api.Checkpointer;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.annotation.StreamListener;
    import org.springframework.cloud.stream.messaging.Sink;
    import org.springframework.messaging.handler.annotation.Header;

    @EnableBinding(Sink.class)
    public class StreamBinderSink {

        @StreamListener(Sink.INPUT)
        public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
            System.out.println(String.format("New message received: '%s'", message));
            checkpointer.success().handle((r, ex) -> {
                if (ex == null) {
                    System.out.println(String.format("Message '%s' successfully checkpointed", message));
                }
                return null;
            });
        }
    }
    ```

1. *StreamBinderSink.java* ファイルを保存して閉じます。

## <a name="build-and-test-your-application"></a>アプリケーションをビルドしてテストする

1. コマンド プロンプトを開きます。

1. ディレクトリを自分の *pom.xml* ファイルの保存先に変更します。例:

    `cd C:\SpringBoot\servicebus`

    または

    `cd /users/example/home/servicebus`

2. 自分の Spring Boot アプリケーションを Maven でビルドし、実行します。

    ```shell
    mvn clean spring-boot:run
    ```

3. 自分のアプリケーションが実行されたら、*curl* を使用してアプリケーションをテストできます。

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    自分のアプリケーションのログに送信された "hello" が表示されます。

    ```shell
    New message received: 'hello'
    Message 'hello' successfully checkpointed
    ```

## <a name="clean-up-resources"></a>リソースをクリーンアップする

予想外の課金を防ぐために、この記事で作成したリソースが不要になったら、[Azure portal](https://portal.azure.com/) を使用して削除してください。

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure の Spring](/java/azure/spring-framework)