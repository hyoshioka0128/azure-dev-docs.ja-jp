---
title: Maven を使用して Helidon Web アプリを Azure App Service にデプロイする
description: Azure Web アプリの Maven プラグインを使用して、Helidon アプリを App Service on Linux にデプロイする方法について説明します。
services: app-service
documentationcenter: java
ms.date: 06/10/2020
ms.service: app-service
ms.topic: article
ms.custom: ''
ms.openlocfilehash: eaf9d71f469f1b0008a0ea085b13dbebf28f320a
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846683"
---
# <a name="deploy-a-helidon-web-app-to-azure-app-service-with-maven"></a>Maven を使用して Helidon Web アプリを Azure App Service にデプロイする

このクイックスタートでは、[Azure App Service Web Apps 用の Maven プラグイン](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)を使用して、Helidon アプリケーションを [Azure App Service on Linux](/azure/app-service/containers/) にデプロイします。 アプリの依存関係、ランタイム、構成を 1 つのデプロイ可能な成果物に統合する必要がある場合は、[Tomcat および WAR ファイル](/azure/app-service/containers/quickstart-java)での Java SE デプロイを選択します。

Azure サブスクリプションがない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。

## <a name="prerequisites"></a>前提条件

* [Azure CLI](/cli/azure/)。ローカルで、または [Azure Cloud Shell](https://shell.azure.com) を介して使用。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* Apache [Maven](https://maven.apache.org/) バージョン 3。

## <a name="install-and-sign-in-to-azure-cli"></a>Azure CLI のインストールとサインイン

Maven プラグインで最もシンプルかつ容易に Helidon アプリケーションをデプロイするには、[Azure CLI](/cli/azure/) を使用します。

Azure CLI を使って、Azure アカウントにサインインします。

```azurecli
az login
```

指示に従って、サインインを完了します。

## <a name="create-sample-app-from-microprofile-starter"></a>MicroProfile Starter からサンプル アプリを作成する

このセクションでは、Helidon アプリケーションを作成し、ローカルでテストします。

1. Web ブラウザーを開き、[MicroProfile Starter](https://start.microprofile.io/) サイトにアクセスします。

   ![MicroProfile Starter](./media/helidon/microprofile-starter-helidon.png)

1. 次のようにフィールドを入力または選択します。  

   |  フィールド  | 値  |
   | ---- | ---- |
   |  groupId  |  com.microsoft.azure.samples.helidon  |
   |  artifactId  |  helidon-hello-azure  |
   |  MicroProfile Version  |  MP 3.2  |
   |  Java SE Version  |  Java 11  |
   |  MicroProfile Runtime  |  Helidon  |
   |  Examples for Specifications  |  Metrics、OpenAPI  |

1. **[DOWNLOAD]** ボタンをクリックしてファイルをダウンロードします。

1. アーカイブ ファイルを解凍します。次に例を示します。

   ```bash
   unzip helidon-hello-azure.zip
   ```

1. または、次のコマンドを使用してプロジェクトを作成することもできます。

   ```bash
   mvn -U archetype:generate -DinteractiveMode=false \
       -DarchetypeGroupId=io.helidon.archetypes \
       -DarchetypeArtifactId=helidon-quickstart-se \
       -DarchetypeVersion=2.0.0 \
       -DgroupId=com.microsoft.azure.samples.helidon \
       -DartifactId=helidon-hello-azure \
       -Dpackage=com.microsoft.azure.samples.helidon
   ```

1. 完成したプロジェクトにディレクトリを変更します。次に例を示します。

   ```bash
   cd helidon-hello-azure/
   ```

1. Maven を使用して JAR ファイルを構築します。次に例を示します。

   ```bash
   mvn clean package
   ```

1. Web アプリを作成したら、次の例のように Maven を使って Web アプリを起動します。

   ```bash
   java -jar target/helidon-hello-azure.jar
   ```

1. Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。 たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。

   ```bash
   curl http://localhost:8080/greet
   ```

1. 次のメッセージが表示されます。**Hello World**

## <a name="configure-maven-plugin-for-azure-app-service"></a>Azure App Service 用の Maven プラグインを構成する

このセクションでは、Maven でアプリを Azure App Service on Linux にデプロイできるように、Helidon プロジェクトの *pom.xml* ファイルを構成します。

1. コード エディターで *pom.xml* ファイルを開きます。

2. *pom.xml* ファイルの `<build>` セクションで、`<plugins>` タグ内に次の `<plugin>` エントリを追加します。

   ```xml
   <build>
     <finalName>helidon-hello-azure</finalName>
     <plugins>
       <plugin>
         <groupId>com.microsoft.azure</groupId>
         <artifactId>azure-webapp-maven-plugin</artifactId>
         <version>1.10.0</version>
       </plugin>
     </plugins>
   </build>
   ```

3. 次にデプロイを構成し、コマンド プロンプトで次の maven コマンドを実行して、**数字**を使用してプロンプトで以下のオプションを選択できます。

   ```bash
   mvn azure-webapp:config
   ```

   オプション パラメーター:  

   |  入力フィールド  |  値の入力または選択  |
   | ---- | ---- |
   |  Define value for OS(Default: Linux):  | 1. linux  |
   |  Define value for javaVersion(Default: Java 8):   | 1.Java 11  |
   |  確認 (Y/N) | ○ |

   構成するには、次のコマンドを使います。

   ```bash
   mvn azure-webapp:config
   [INFO] Scanning for projects...
   [INFO]
   [INFO] ------< com.microsoft.azure.samples.helidon:helidon-hello-azure >-------
   [INFO] Building myproject 1.0-SNAPSHOT
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO] 
   [INFO] --- azure-webapp-maven-plugin:1.10.0:config (default-cli) @ helidon-hello-azure ---
   Define value for OS(Default: Linux): 
   1. linux [*]
   2. windows
   3. docker
   Enter index to use: 1
   Define value for javaVersion(Default: Java 8): 
   1. Java 11
   2. Java 8 [*]
   Enter index to use: 1
   Please confirm webapp properties
   AppName : helidon-hello-azure-1600998900939
   ResourceGroup : helidon-hello-azure-1600998900939-rg
   Region : westeurope
   PricingTier : PremiumV2_P1v2
   OS : Linux
   RuntimeStack : JAVA 11-java11
   Deploy to slot : false
   Confirm (Y/N)? : y
   [INFO] Saving configuration to pom.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:44 min
   [INFO] Finished at: 2020-09-25T10:57:35+09:00
   [INFO] ------------------------------------------------------------------------
   ```

4. `PORT`、`WEBSITES_PORT`、`WEBSITES_CONTAINER_START_TIME_LIMIT` の `<configuration>`セクションに `<appSettings>` セクションを追加します。 `<include>/libs/*.jar</include>` をデプロイのリソースに追加します。

   ```xml
   <plugin>
     <groupId>com.microsoft.azure</groupId>  
     <artifactId>azure-webapp-maven-plugin</artifactId>  
     <version>1.10.0</version>  
     <configuration>
       <schemaVersion>V2</schemaVersion>
       <resourceGroup>microprofile</resourceGroup>
       <appName>helidon-hello-azure-1591663020899</appName>
       <pricingTier>P1v2</pricingTier>
       <region>japaneast</region>
       <runtime>
         <os>linux</os>
         <javaVersion>java11</javaVersion>
         <webContainer>java11</webContainer>
       </runtime>
       <appSettings>
         <property>
           <name>PORT</name>
           <value>8080</value>
         </property>
         <property>
           <name>WEBSITES_PORT</name>
           <value>8080</value>
         </property>
         <property>
           <name>WEBSITES_CONTAINER_START_TIME_LIMIT</name>
           <value>600</value>
         </property>
       </appSettings>
       <deployment>
         <resources>
           <resource>
             <directory>${project.basedir}/target</directory>
             <includes>
               <include>*.jar</include>
               <include>/libs/*.jar</include>
             </includes>
           </resource>
         </resources>
       </deployment>
     </configuration>
   ```

## <a name="deploy-the-app-to-azure"></a>Azure にアプリケーションをデプロイする

この記事の前のセクションで説明した設定をすべて構成した後、Azure に Web アプリをデプロイします。 そのためには、次の手順を実行してください。

1. *pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。

   ```bash
   mvn clean package
   ```

1. Maven を使って次の例のように Azure に Web アプリをデプロイします。

   ```bash
   mvn azure-webapp:deploy
   ```

Maven によって、ご自身の Web アプリが Azure にデプロイされます。Web アプリまたは Web アプリ プランが存在しない場合は、Maven によって新たに作成されます。 出力に示されている URL に Web アプリが表示されるまで数分かかることがあります。 Web ブラウザーで URL にアクセスします。  次のメッセージが表示されます。**Hello World**

Web アプリのデプロイが完了すると、[Azure portal] でそれを管理できるようになります。

* Web アプリは **App Services** に一覧表示されます。

   ![Azure Portal の App Services に一覧表示される Web アプリ](./media/helidon/azure-portal-app-service-screen.png)

* Web アプリにアクセスするには、Web アプリの **[概要]** にある [`Browse`] ボタンをクリックします。

   ![Azure portal の App Services で Web アプリの URL を検索](./media/helidon/azure-portal-app-service-overview.png)

前と同じ cURL コマンド (`/data/hello`) を使用して、`localhost` ではなくポータルからご自身の Web アプリ URL を使って、デプロイが適切に行われたことを確認します。 次のメッセージが表示されます。**Hello World**

## <a name="confirm-the-log-stream-from-running-app-service"></a>実行中の App Service からログ ストリームを確認する

実行中の App Service からログを表示 ("tail") できます。 サイト コードでの `console.log` への呼び出しはすべて、ターミナルに表示されます。

```azurecli
az webapp log tail -g microprofile -n helidon-hello-azure-1600998900939
```

![ログ出力を示すターミナル ウィンドウ](./media/helidon/azure-cli-app-service-log-stream.png)

## <a name="clean-up-resources"></a>リソースをクリーンアップする

Azure リソースが不要になったら、リソース グループを削除して、デプロイしたリソースをクリーンアップします。

* Azure portal で、左側のメニューから [リソース グループ] を選択します。
* **[名前でフィルター]** フィールドに「**microprofile**」と入力します。このチュートリアルで作成したリソース グループにはこのプレフィックスが付いています。
* このチュートリアルで作成したリソース グループを選択します。
* トップ メニューから [リソース グループの削除] を選択します。

## <a name="next-steps"></a>次のステップ

MicroProfile および Azure の詳細については、Azure ドキュメント センターで引き続き MicroProfile に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の MicroProfile](./index.yml)

### <a name="additional-resources"></a>その他のリソース

この記事で説明しているさまざまなテクノロジの詳細については、次の記事をご覧ください。

* [Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]

* [Azure CLI 2.0 で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven の設定リファレンス](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: ../index.yml
[Azure Portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: ../fundamentals/java-jdk-long-term-support.md
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png