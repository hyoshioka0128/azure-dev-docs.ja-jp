---
title: Azure Service Bus JMS 用の Spring Boot スターターの使用方法
description: この記事では、Spring JMS スターターを使用して Azure Service Bus との間でメッセージを送受信する方法について説明します。
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 10/13/2019
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: c1b8baa716fbe24efbf635bc5f8a17833590137a
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442143"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-service-bus-jms"></a>Azure Service Bus JMS 用の Spring Boot スターターの使用方法

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

Azure には、[Azure Service Bus](/azure/service-bus-messaging/service-bus-messaging-overview) ("Service Bus") という、[Advanced Message Queueing Protocol 1.0](http://www.amqp.org/) ("AMQP 1.0") 標準に基づいた非同期のメッセージング プラットフォームが用意されています。 Service Bus は、サポートされている Azure プラットフォームの範囲全体で使用することができます。

Azure Service Bus JMS 用の Spring Boot スターターを使用すると、Spring と Service Bus を統合することができます。

この記事では、Azure Service Bus JMS 用の Spring Boot スターターを使用して、Service Bus の `queues` および `topics` との間でメッセージを送受信する方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事の前提条件は次のとおりです。

1. Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)を有効にするか、または[無料のアカウント](https://azure.microsoft.com/free/)にサインアップできます。

1. サポートされている Java Development Kit (JDK) (バージョン 8 以降)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。

1. [Apache Maven](http://maven.apache.org/) バージョン 3.2 以降。

1. 構成済みの Service Bus キューまたはトピックが既にある場合は、Service Bus 名前空間が次の要件を満たすようにしてください。

    1. すべてのネットワークからのアクセスを許可する
    1. Premium (またはそれ以上) である
    1. 自分のキューおよびトピックに対する読み取り/書き込みアクセス権限を含んだアクセス ポリシーがある

1. 構成済みの Service Bus キューまたはトピックがない場合は、Azure portal を使用して [Service Bus キュー](/azure/service-bus-messaging/service-bus-quickstart-portal)または [Service Bus トピック](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal)を作成します。 前の手順で指定された要件を名前空間が確実に満たすようにしてください。 また、名前空間に含まれる接続文字列をメモしてください。これは、このチュートリアルのテスト アプリに必要です。

1. Spring Boot アプリケーションがない場合は、[Spring Initializr](https://start.spring.io/) で **Maven** プロジェクトを作成します。 必ず、 **[Maven プロジェクト]** を選択し、 **[依存関係]** に **[Web]** 依存関係を追加してください。


## <a name="use-the-azure-service-bus-jms-starter"></a>Azure Service Bus JMS スターターを使用する

1. 自分のアプリの親ディレクトリで *pom.xml* ファイルを探します。例:

    *C:\SpringBoot\servicebus\pom.xml*

    \- または -

    */users/example/home/servicebus/pom.xml*

1. テキスト エディターで *pom.xml* ファイルを開きます。

1. Spring Boot Azure Service Bus JMS スターターを `<dependencies>` のリストに追加します。

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms-spring-boot-starter</artifactId>
        <version>2.3.5</version>
    </dependency>
    ```

    ![pom.xml ファイルに dependency セクションを追加します。](media/configure-spring-boot-starter-java-app-with-azure-service-bus/add-dependency-section-new.png)

1. *pom.xml* ファイルを保存して閉じます。

## <a name="configure-the-app-for-your-service-bus"></a>アプリでサービス バス用の構成を行う 

このセクションでは、Service Bus キューまたはトピックを使用するように自分のアプリを構成する方法について確認します。

### <a name="use-a-service-bus-queue"></a>Service Bus キューを使用する

1. アプリの *resources* ディレクトリで *application.properties* を探します。次に例を示します。

    *C:\SpringBoot\servicebus\application.properties*

    \- または -

    */users/example/home/servicebus/application.properties*

1. テキスト エディターで *application.properties* ファイルを開きます。

1. *application.properties* ファイルの最後に次のコードを追加します。 プレースホルダーの値をサービス バスの適切な値で置き換えます。値は引用符で囲まないでください。

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **フィールドの説明**

    | フィールド                                     | 説明                                                                                     |
    |-------------------------------------------|-------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | Azure portal の自分の Service Bus 名前空間で取得した接続文字列を指定します。 |
    | `spring.jms.servicebus.idle-timeout`      | アイドル タイムアウトをミリ秒単位で指定します。 このチュートリアルで推奨される値は 1800000 です。   |

1. *application.properties* ファイルを保存して閉じます。

### <a name="use-service-bus-topic"></a>Service Bus トピックを使用する

1. アプリの *resources* ディレクトリで *application.properties* を探します。次に例を示します。

    *C:\SpringBoot\servicebus\application.properties*

    \- または -

    */users/example/home/servicebus/application.properties*

1. テキスト エディターで *application.properties* ファイルを開きます。

1. *application.properties* ファイルの最後に次のコードを追加します。 プレースホルダーの値をサービス バスの適切な値で置き換えます。値は引用符で囲まないでください。

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.topic-client-id=<ServiceBusSubscriptionID>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **フィールドの説明**

    | フィールド                                     | 説明                                                                                       |
    |-------------------------------------------|---------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | Azure portal の自分の Service Bus 名前空間で取得した接続文字列を指定します。   |
    | `spring.jms.servicebus.topic-client-id`   | JMS クライアント ID を指定します。これは、Azure portal 内の Service Bus のサブスクリプション ID です。                | 
    | `spring.jms.servicebus.idle-timeout`      | アイドル タイムアウトをミリ秒単位で指定します。 このチュートリアルで推奨される値は 1800000 です。     |

1. *application.properties* ファイルを保存して閉じます。

## <a name="implement-basic-service-bus-functionality"></a>基本的な Service Bus 機能を実装する

このセクションでは、自分の Service Bus キューまたはトピックにメッセージを送信したり、対応するキューまたはトピックのサブスクリプションからメッセージを受信したりするために必要な Java クラスを作成します。

### <a name="modify-the-main-application-class"></a>アプリケーションのメイン クラスを変更する

1. アプリのパッケージ ディレクトリでメイン アプリケーションの Java ファイルを探します。次に例を示します。

    *C:\SpringBoot\servicebus\src\main\java\com\wingtiptoys\servicebus\ServiceBusJmsStarterApplication.java*

    \- または -

    */users/example/home/servicebus/src/main/java/com/wingtiptoys/servicebus/ServiceBusJmsStarterApplication.java*

1. テキスト エディターでメイン アプリケーションの Java ファイルを開きます。

1. 次のコードをファイルに追加します。

   ```java
    package com.wingtiptoys.servicebus;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusJmsStarterApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusJmsStarterApplication.class, args);
        }
    }
    ```

1. ファイルを保存して閉じます。

### <a name="define-a-test-java-class"></a>テスト Java クラスを定義する

1. テキスト エディターを使用して、*User.java* という名前の Java ファイルを自分のアプリのパッケージ ディレクトリに作成します。

1. ユーザーの名前を格納したり取得したりする汎用的なユーザー クラスを定義します。

    ```java
    package com.wingtiptoys.servicebus;

    import java.io.Serializable;

    // Define a generic User class.
    public class User implements Serializable {

        private static final long serialVersionUID = -295422703255886286L;

        private String name;

        public User() {
        }

        public User(String name) {
            setName(name);
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

    }
    ```

    Spring フレームワークで `JmsTemplate` の `send` メソッドを使用するために、`Serializable` が実装されます。 それ以外の場合は、テキスト形式の json に内容をシリアル化するために、カスタマイズした `MessageConverter` bean を定義する必要があります。 `MessageConverter` の詳細については、公式の [Spring JMS スターター プロジェクト](https://spring.io/guides/gs/messaging-jms/)を参照してください。

1. *User.java* ファイルを保存して閉じます。

### <a name="create-a-new-class-for-the-message-send-controller"></a>メッセージ送信コントローラー用の新しいクラスを作成する

1. テキスト エディターを使用して、*SendController.java* という名前の Java ファイルを自分のアプリのパッケージ ディレクトリに作成します。

1. この新しいファイルに次のコードを追加します。

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.jms.core.JmsTemplate;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class SendController {

        private static final String DESTINATION_NAME = "<DestinationName>";

        private static final Logger logger = LoggerFactory.getLogger(SendController.class);

        @Autowired
        private JmsTemplate jmsTemplate;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            logger.info("Sending message");
            jmsTemplate.convertAndSend(DESTINATION_NAME, new User(message));
            return message;
        }
    }
    ```

    > [!NOTE]
    > `<DestinationName>` は、自分の Service Bus 名前空間に構成されている実際のキュー名またはトピック名に置き換えてください。

1. *SendController.java* を保存して閉じます。

### <a name="create-a-class-for-the-message-receive-controller"></a>メッセージ受信コントローラー用のクラスを作成する

#### <a name="receive-messages-from-a-service-bus-queue"></a>Service Bus キューからメッセージを受信する

1. テキスト エディターを使用して、*QueueReceiveController.java* という名前の Java ファイルを自分のアプリのパッケージ ディレクトリに作成します。

1. この新しいファイルに次のコードを追加します。

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class QueueReceiveController {

        private static final String QUEUE_NAME = "<ServiceBusQueueName>";

        private final Logger logger = LoggerFactory.getLogger(QueueReceiveController.class);

        @JmsListener(destination = QUEUE_NAME, containerFactory = "jmsListenerContainerFactory")
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

    > [!NOTE]
    > `<ServiceBusQueueName>` は、自分の Service Bus 名前空間に構成されている実際のキュー名に置き換えてください。

1. *QueueReceiveController.java* ファイルを保存して閉じます。

#### <a name="receive-messages-from-a-service-bus-subscription"></a>Service Bus サブスクリプションからメッセージを受信する

1. テキスト エディターを使用して、*TopicReceiveController.java* という名前の Java ファイルを自分のアプリのパッケージ ディレクトリに作成します。 

1. この新しいファイルに次のコードを追加します。 `<ServiceBusTopicName>` プレースホルダーは、自分の Service Bus 名前空間に構成されている実際のトピック名に置き換えてください。 `<ServiceBusSubscriptionName>` プレースホルダーは、自分の Service Bus トピックの実際のサブスクリプション名に置き換えてください。

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class TopicReceiveController {

        private static final String TOPIC_NAME = "<ServiceBusTopicName>";

        private static final String SUBSCRIPTION_NAME = "<ServiceBusSubscriptionName>";

        private final Logger logger = LoggerFactory.getLogger(TopicReceiveController.class);

        @JmsListener(destination = TOPIC_NAME, containerFactory = "topicJmsListenerContainerFactory",
                subscription = SUBSCRIPTION_NAME)
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

1. *TopicReceiveController.java* ファイルを保存して閉じます。

## <a name="build-and-test-your-application"></a>アプリケーションをビルドしてテストする

1. コマンド プロンプトを開き、ディレクトリを自分の *pom.xml* の保存先に変更します。例:

    ```cmd
    cd C:\SpringBoot\servicebus 
    ```

    または

    ```bash
    cd /users/example/home/servicebus 
    ```

1. 自分の Spring Boot アプリケーションを Maven でビルドし、実行します。

    ```shell
    mvn clean spring-boot:run
    ```

1. 自分のアプリケーションが実行されたら、*curl* を使用してアプリケーションをテストできます。

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    自分のアプリケーションのログに送信された "Sending message" および "hello" が表示されます。

    ```shell
    [nio-8080-exec-1] com.wingtiptoys.servicebus.SendController : Sending message
    [enerContainer-1] com.wingtiptoys.servicebus.ReceiveController : Received message: hello
    ```

## <a name="clean-up-resources"></a>リソースをクリーンアップする

予想外の課金を防ぐために、この記事で作成したリソースが不要になったら、[Azure portal](https://portal.azure.com/) を使用して削除してください。

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Service Bus と AMQP 1.0 で JMS API を使用する方法](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)
