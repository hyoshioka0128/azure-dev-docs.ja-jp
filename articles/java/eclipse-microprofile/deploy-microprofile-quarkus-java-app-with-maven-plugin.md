---
title: Maven を使用して Quarkus Web アプリを Azure App Service にデプロイする
description: Azure Web アプリの Maven プラグインを使用して、Quarkus アプリを App Service on Linux にデプロイする方法について説明します。
services: app-service
documentationcenter: java
ms.date: 06/10/2020
ms.service: app-service
ms.topic: article
ms.custom: ''
ms.openlocfilehash: f4566f0cb5a7c1deacd5d33b2180a9305b936446
ms.sourcegitcommit: 0d2ea78f18430c845a32e0d2311427ab81033465
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/23/2020
ms.locfileid: "97754168"
---
# <a name="deploy-a-quarkus-web-app-to-azure-app-service-with-maven"></a>Maven を使用して Quarkus Web アプリを Azure App Service にデプロイする

このクイックスタートでは、[Azure App Service Web Apps 用の Maven プラグイン](https://github.com/microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md)を使用して、Quarkus アプリケーションを [Azure App Service on Linux](/azure/app-service/containers/) にデプロイします。 アプリの依存関係、ランタイム、構成を 1 つのデプロイ可能な成果物に統合する必要がある場合は、[Tomcat および WAR ファイル](/azure/app-service/containers/quickstart-java)での Java SE デプロイを選択します。

Azure サブスクリプションがない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。

## <a name="prerequisites"></a>前提条件

* [Azure CLI](/cli/azure/)。ローカルで、または [Azure Cloud Shell](https://shell.azure.com) を介して使用。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* Apache [Maven](https://maven.apache.org/) バージョン 3。

## <a name="install-and-sign-in-to-azure-cli"></a>Azure CLI のインストールとサインイン

Maven プラグインで最もシンプルかつ容易に Quarkus アプリケーションをデプロイするには、[Azure CLI](/cli/azure/) を使用します。

Azure CLI を使って、Azure アカウントにサインインします。

```azurecli
az login
```

指示に従って、サインインを完了します。

## <a name="create-sample-app-from-microprofile-starter"></a>MicroProfile Starter からサンプル アプリを作成する

このセクションでは、Quarkus アプリケーションを作成し、ローカルでテストします。

### <a name="create-java-se-8-base-project"></a>Java SE 8 基本プロジェクトを作成する

1. Web ブラウザーを開き、[MicroProfile Starter](https://start.microprofile.io/) サイトにアクセスします。

   ![Quarkus 用 MicroProfile Starter](./media/quarkus/microprofile-starter-quarkus.png)

2. 次のようにフィールドを入力または選択します。  

   |  フィールド  |  値  |
   | ---- | ---- |
   |  groupId  |  com.microsoft.azure.samples.quarkus  |
   |  artifactId  |  quarkus-hello-azure  |
   |  MicroProfile Version  |  MP 3.2  |
   |  Java SE Version  |  Java 8  |
   |  MicroProfile Runtime  |  Quarkus  |
   |  Examples for Specifications  |  Metrics、OpenAPI  |

3. **[DOWNLOAD]** ボタンをクリックしてファイルをダウンロードします。

4. アーカイブ ファイルを解凍します。次に例を示します。

   ```bash
   unzip Quarkus-hello-azure.zip
   ```

### <a name="create-java-se-11-base-project"></a>Java SE 11 基本プロジェクトを作成する

Java 11 基本プロジェクトを作成するには、次のコマンドを使用して実行できます。

   ```bash
   $ mvn io.quarkus:quarkus-maven-plugin:1.8.1.Final:create \
     -DprojectGroupId=com.microsoft.azure.samples.quarkus \
     -DprojectArtifactId=quarkus-hello-azure  \
     -DclassName="com.microsoft.azure.samples.quarkus.App" \
     -Dpath="/hello"
   ```

### <a name="run-the-application-in-local-environment"></a>アプリケーションをローカル環境で実行する

1. 完成したプロジェクトにディレクトリを変更します。次に例を示します。

   ```bash
   cd quarkus-hello-azure/
   ```

2. Maven を使用してプロジェクトをビルドおよび実行します。次に例を示します。

   ```bash
   mvn quarkus:dev
   ```

3. Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。 たとえば、curl を使うことができる場合は次のようなコマンドを実行できます。

   Java SE 8 プロジェクトの場合:

   ```bash
   curl http://localhost:8080/data/hello
   ```

   Java SE 11 プロジェクトの場合:

   ```bash
   curl localhost:8080/hello
   ```

4. 次のメッセージが表示されます。**Hello World** または **hello**

## <a name="configure-maven-plugin-for-azure-app-service"></a>Azure App Service 用の Maven プラグインを構成する

このセクションでは、Maven でアプリを Azure App Service on Linux にデプロイできるように、Quarkus プロジェクトの *pom.xml* ファイルを構成します。

1. コード エディターで *pom.xml* ファイルを開きます。

2. *pom.xml* ファイルの `<build>` セクションで、`<plugins>` タグ内の `maven-surefire-plugin` の後に、次の `<plugin>` エントリを追加します。

   ```xml
   <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.10.0</version>
   </plugin>
   ```

3. 次にデプロイを構成し、コマンド プロンプトで次の maven コマンドを実行して、**数字** を使用してプロンプトで以下のオプションを選択できます。

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
   $ mvn azure-webapp:config

   [INFO] Scanning for projects...
   [INFO] 
   [INFO] ------< com.microsoft.azure.samples.quarkus:quarkus-hello-azure >-------
   [INFO] Building quarkus-hello-azure 1.0-SNAPSHOT
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO] 
   [INFO] --- azure-webapp-maven-plugin:1.10.0:config (default-cli) @ quarkus-hello-azure ---

   Define value for OS(Default: Linux): 
   1. linux [*]
   2. windows
   3. docker
   Enter index to use: 
   Define value for javaVersion(Default: Java 8): 
   1. Java 11
   2. Java 8 [*]
   Enter index to use: 1
   Please confirm webapp properties
   AppName : quarkus-hello-azure-1601011883156
   ResourceGroup : quarkus-hello-azure-1601011883156-rg
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
   [INFO] Total time:  16.502 s
   [INFO] Finished at: 2020-09-25T14:31:34+09:00
   [INFO] ------------------------------------------------------------------------
   ```

4. `PORT`、`WEBSITES_PORT`、`WEBSITES_CONTAINER_START_TIME_LIMIT` の `<configuration>`セクションに `<appSettings>` セクションを追加します。  
 これで、`azure-webapp-maven-plugin` についての次の XML エントリを確認できます。

   ```xml
      <plugin>
        <groupId>com.microsoft.azure</groupId>  
        <artifactId>azure-webapp-maven-plugin</artifactId>  
        <version>1.10.0</version>  
        <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>microprofile</resourceGroup>
          <appName>quarkus-hello-azure-1591836715762</appName>
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
                </includes>
              </resource>
            </resources>
          </deployment>
        </configuration>
      </plugin>
   ```

5. Uber (FAT) jar を作成するための次のエントリが、プロパティ ファイル (`src/main/resources/application.properties`) に追加されました。

   `quarkus.package.uber-jar=true`

## <a name="deploy-the-app-to-azure"></a>Azure にアプリケーションをデプロイする

この記事の前のセクションで説明した設定をすべて構成したら、次は Azure に Web アプリをデプロイします。 そのためには、次の手順を実行してください。

1. *pom.xml* ファイルを変更する場合は、以前使っていたコマンド プロンプトまたはターミナル ウィンドウで、次の例のように Maven を使って JAR ファイルをリビルドします。

   ```bash
   mvn clean package
   ```

2. Maven を使って次の例のように Azure に Web アプリをデプロイします。

   ```bash
   mvn azure-webapp:deploy
   ```

   デプロイに成功した場合、コンソールに次のメッセージが表示されます。

   ```bash
   mvn azure-webapp:deploy

   [INFO] Successfully deployed the artifact to https://quarkus-hello-azure-1591836715762.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:20 min
   [INFO] Finished at: 2020-06-11T10:06:51+09:00
   [INFO] ------------------------------------------------------------------------
   ```

   Maven によって、ご自身の Web アプリが Azure にデプロイされます。Web アプリまたは Web アプリ プランが存在しない場合は、Maven によって新たに作成されます。 出力に示されている URL に Web アプリが表示されるまで数分かかることがあります。 Web ブラウザーで URL にアクセスします。  次の画面が表示されます。

   ![Quarkus のフロント ページ](./media/quarkus/quarkus-front-page-11.png)

   Web アプリのデプロイが完了すると、[Azure portal] でそれを管理できるようになります。

   * Web アプリは **microprofile** リソース グループに一覧表示されます。

   ![Azure Portal の App Services に一覧表示される Web アプリ](./media/quarkus/quarkus-azure-portal-rg.png)

   * Web アプリにアクセスするには、Web アプリの **[概要]** にある [`Browse`] ボタンをクリックします。  
   デプロイが成功して実行中であることを確認します。 次の画面が表示されます。

   ![Azure portal の App Services で Web アプリの URL を検索](./media/quarkus/quarkus-azure-portal-manage.png)

## <a name="confirm-the-log-stream-from-running-app-service"></a>実行中の App Service からログ ストリームを確認する

実行中の App Service からログを表示 ("tail") できます。 サイト コードでの `console.log` への呼び出しはすべて、ターミナルに表示されます。

   ```azurecli
   az webapp log tail -g microprofile -n quarkus-hello-azure-1601011883156
   ```

   ![ログ出力を示すターミナル ウィンドウ。](./media/Quarkus/azure-cli-app-service-log-stream.png)


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
[Maven Plugin for Azure Web Apps (Azure Web Apps 用の Maven プラグイン)]: https://github.com/microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md

[Java Development Kit (JDK)]: ../fundamentals/java-jdk-long-term-support.md
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[AP01]: media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png