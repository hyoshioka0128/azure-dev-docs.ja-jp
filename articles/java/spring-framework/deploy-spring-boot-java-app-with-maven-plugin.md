---
title: Spring Boot JAR ファイルの Web アプリを Azure App Service on Linux にデプロイする
description: Azure Web アプリの Maven プラグインを使用して、Spring Boot アプリの Jar ファイルを App Service on Linux にデプロイする方法について説明します。
services: app-service
documentationcenter: java
ms.date: 12/19/2018
ms.service: app-service
ms.topic: article
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: f4c0a870ce61508a846d62ae956fb13653e051ea
ms.sourcegitcommit: 2760d3ca0ff0b939181d976a652f2b35ea5b3fb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2020
ms.locfileid: "83426082"
---
# <a name="deploy-a-spring-boot-jar-file-app-to-azure-app-service-with-maven-and-azure-on-linux"></a>Spring Boot JAR ファイルのアプリを Maven を使用して Linux 上の Azure App Service にデプロイする

このクイックスタートでは、[Azure App Service Web Apps 用の Maven プラグイン](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)を使用して、Java SE JAR としてパッケージ化された Spring Boot アプリケーションを [Azure App Service on Linux](/azure/app-service/containers/) にデプロイします。 アプリの依存関係、ランタイム、構成を 1 つのデプロイ可能な成果物に統合する必要がある場合は、[Tomcat および WAR ファイル](/azure/app-service/containers/quickstart-java)での Java SE デプロイを選択します。

Azure サブスクリプションがない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルの手順を完了するには、次をインストールして構成する必要があります。

* [Azure CLI](/cli/azure/)。ローカルで、または [Azure Cloud Shell](https://shell.azure.com) を介して使用。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* Apache の [Maven](https://maven.apache.org/) バージョン 3。
* [Git](https://git-scm.com/downloads) クライアント。

## <a name="install-and-sign-in-to-azure-cli"></a>Azure CLI のインストールとサインイン

Maven プラグインで最もシンプルかつ容易に Spring Boot アプリケーションをデプロイするには、[Azure CLI](/cli/azure/) を使用します。

Azure CLI を使って、Azure アカウントにサインインします。
   
   ```shell
   az login
   ```
   
指示に従って、サインインを完了します。

## <a name="clone-the-sample-app"></a>サンプル アプリの複製

このセクションでは、完成した Spring Boot アプリケーションを複製し、ローカルでテストします。

1. コマンド プロンプトまたはターミナル ウィンドウを開き、Spring Boot アプリケーションを保持するためのローカル ディレクトリを作成して、次の例のようにそのディレクトリに移動します。
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- または --
   ```shell
   mkdir ~/SpringBoot
   cd ~/SpringBoot
   ```

1. [Spring Boot Getting Started] サンプル プロジェクトを作成したディレクトリに複製します。次に例を示します。
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. 完成したプロジェクトにディレクトリを変更します。次に例を示します。
   ```shell
   cd gs-spring-boot/complete
   ```

1. Maven を使用して JAR ファイルを構築します。次に例を示します。
   ```shell
   mvn clean package
   ```

1. Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。
   ```shell
   mvn spring-boot:run
   ```

1. Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。 たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。
   ```shell
   curl http://localhost:8080
   ```

1. 次のメッセージが表示されます。**Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>Azure App Service 用の Maven プラグインを構成する

このセクションでは、Maven でアプリを Linux 上の Azure App Service にデプロイできるように、Spring Boot プロジェクト `pom.xml` を構成します。

1. コード エディターで `pom.xml` を開きます。

2. pom.xml の `<build>` セクションで、`<plugins>` タグ内に次の `<plugin>` エントリを追加します。

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.9.1</version>
   </plugin>
   ```

3. 次にデプロイを構成し、コマンド プロンプトで次の maven コマンドを実行して、**数字**を使用してプロンプトで以下のオプションを選択できます。
    * **OS**: linux  
    * **javaVersion**:Java 8    
    
   ```cmd
   mvn azure-webapp:config
   ```

   **Confirm (Y/N)** のプロンプトが表示されたら、 **'y'** を押すと構成が終了します。

   ```cmd
   ~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
   [INFO] Scanning for projects...
   [INFO]
   [INFO] -----------------< org.springframework:gs-spring-boot >-----------------
   [INFO] Building gs-spring-boot 0.1.0
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO]
   [INFO] --- azure-webapp-maven-plugin:1.9.1:config (default-cli) @ gs-spring-boot ---
   [WARNING] The plugin may not work if you change the os of an existing webapp.
   Define value for OS(Default: Linux):
   1. linux [*]
   2. windows
   3. docker
   Enter index to use:
   Define value for javaVersion(Default: Java 8):
   1. Java 11
   2. Java 8 [*]
   Enter index to use:
   Please confirm webapp properties
   AppName : gs-spring-boot-1559091271202
   ResourceGroup : gs-spring-boot-1559091271202-rg
   Region : westeurope
   PricingTier : Premium_P1V2
   OS : Linux
   RuntimeStack : JAVA 8-jre8
   Deploy to slot : false
   Confirm (Y/N)? : Y
   ```

4. `<appSettings>` セクションを `<azure-webapp-maven-plugin>`の `<configuration>` セクションに追加して、*80* のポートでリッスンするようにします。

   ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.9.1</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>
          <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>
            <webContainer>jre8</webContainer>
          </runtime>
          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          <deployment>
            <resources>
              <resource>
                <directory>${project.basedir}/target</directory>
                <includes>
                  <include>*.jar</include>
                <includes>
              </resource>
            </resources>
          </deployment>
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a>Azure にアプリケーションをデプロイする

この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。 そのためには、次の手順を実行してください。

1. *pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。
   ```shell
   mvn clean package
   ```

1. Maven を使って次の例のように Azure に Web アプリをデプロイします。
   ```shell
   mvn azure-webapp:deploy
   ```

Maven によって、ご自身の Web アプリが Azure にデプロイされます。Web アプリまたは Web アプリ プランが存在しない場合は、Maven によって新たに作成されます。 出力に示されている URL に Web アプリが表示されるまで数分かかることがあります。 Web ブラウザーで URL にアクセスします。  次のメッセージが表示されます。Greetings from Spring Boot!

Web アプリのデプロイが完了すると、[Azure portal] で Web アプリを管理できるようになります。

* Web アプリは **App Services** に一覧表示されます。

   ![Azure Portal の App Services に一覧表示される Web アプリ][AP01]

* Web アプリの URL は、Web アプリの **[概要]** に一覧表示されます。

   ![Azure portal の App Services で Web アプリの URL を検索][AP02]

前と同じ cURL コマンドを使用して、`localhost` ではなくポータルからご自身の Web アプリ URL を使って、デプロイが適切に行われたことを確認します。 次のメッセージが表示されます。**Greetings from Spring Boot!** 

## <a name="clean-up-resources"></a>リソースをクリーンアップする
Azure リソースが不要になったら、リソース グループを削除して、デプロイしたリソースをクリーンアップします。

- Azure portal で、左側のメニューから [リソース グループ] を選択します。
- **[名前でフィルター]** フィールドに **gs-spring-boot-** と入力します。このチュートリアルで作成したリソース グループにはこのプレフィックスが付いています。
- このチュートリアルで作成したリソース グループを選択します。
- トップ メニューから [リソース グループの削除] を選択します。

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/developer/java/spring-framework)

### <a name="additional-esources"></a>その他のリソース

この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。

* [Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]

* [Azure Web Apps 用の Maven プラグインを使用して、コンテナー化された Spring Boot アプリを Azure にデプロイする方法](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [Azure CLI 2.0 で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven の設定リファレンス](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /azure/developer/java/
[Azure Portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->


[AP01]: media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png
