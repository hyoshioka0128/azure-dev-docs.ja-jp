---
title: コンテナー用 Azure App Service で Spring Boot Web アプリをデプロイする
description: このチュートリアルでは、Microsoft Azure の Linux Web アプリとして Spring Boot アプリケーションをデプロイする方法について説明します。
services: azure app service
documentationcenter: java
ms.date: 12/31/2019
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 9288f680f5eb92046ada7ce420d64de2e5301fcc
ms.sourcegitcommit: aceed8548ad4529a81d83eb15a095edc8607cac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/18/2020
ms.locfileid: "77440975"
---
# <a name="deploy-a-spring-boot-application-on-azure-app-service-for-container"></a>コンテナー用 Azure App Service で Spring Boot アプリケーションをデプロイする

このチュートリアルでは、[Docker] を使用してお使いの [Spring Boot] アプリケーションをコンテナー化し、ご自身の Docker イメージを [Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) の Linux ホストにデプロイする手順について説明します。

## <a name="prerequisites"></a>前提条件

このチュートリアルの手順を完了するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* [Azure コマンド ライン インターフェイス (CLI)]。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* Apache の [Maven] 構築ツール (バージョン 3)。
* [Git] クライアント。
* [Docker] クライアント。

> [!NOTE]
>
> このチュートリアルには仮想化要件があるため、仮想マシンでこの記事の手順を実行することはできません。仮想化機能を有効にした物理コンピューターを使用する必要があります。
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Spring Boot on Docker Getting Started Web アプリを作成する

次の手順では、単純な Spring Boot Web アプリケーションを作成し、それをローカルにテストするために必要な手順について説明します。

1. コマンド プロンプトを開き、アプリケーションを保持するためのローカル ディレクトリを作成して、そのディレクトリに変更します。次に例を示します。
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- または --
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. [Docker での Spring Boot の使用開始]のサンプル プロジェクトを今作成したディレクトリに複製します。次に例を示します。
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. 完成したプロジェクトにディレクトリを変更します。次に例を示します。
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Maven を使用して JAR ファイルを構築します。次に例を示します。
   ```
   mvn package
   ```

1. Web アプリが作成されたら、JAR ファイルがある `target` ディレクトリにディレクトリを変更し、Web アプリを起動します。次に例を示します。
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar --server.port=80
   ```

1. Web アプリのテストは、Web ブラウザーを使用してアプリをローカルで参照して行います。 たとえば curl が使用でき、Tomcat サーバーをポート 80 で実行されるように構成した場合は、次のようになります。
   ```
   curl http://localhost
   ```

1. 次のメッセージが表示されます。**Hello Docker World**

   ![サンプル アプリをローカルに参照する][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Azure Container Registry をプライベート Docker レジストリとして使用するために作成する

次の手順では、Azure portal を使用して Azure Container Registry を作成する方法を説明します。

> [!NOTE]
>
> Azure Portal ではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成](/azure/container-registry/container-registry-get-started-azure-cli)」の手順に従います。
>

1. [Azure Portal]を参照して、サインインします。

   Azure portal のアカウントにサインインしたら、[Azure portal を使用したプライベート Docker コンテナー レジストリの作成]に関する記事の手順に従います。便宜上、この手順を改めて以下で説明します。

1. **[+ 新規]** のメニュー アイコン、 **[コンテナー]** 、 **[Azure Container Registry]** の順にクリックします。
   
   ![Azure Container Registry を新しく作成する][AR01]

1. **[コンテナー レジストリの作成]** ページが表示されたら、 **[レジストリ名]** 、 **[サブスクリプション]** 、 **[リソース グループ]** 、 **[場所]** を入力します。 **[管理者ユーザー]** で **[有効]** を選択します。 **[Create]** をクリックします。

   ![Azure Container Registry 設定を構成する][AR03]

1. コンテナー レジストリが作成されたら、Azure portal でコンテナー レジストリに移動して、 **[アクセス キー]** をクリックします。 次の手順で使用するため、ユーザー名とパスワードをメモします。

   ![Azure Container Registry のアクセス キー][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Azure Container Registry のアクセス キーを使用するために Maven を構成する

1. Spring Boot アプリケーションの完了プロジェクト ディレクトリ ("*C:\SpringBoot\gs-spring-boot-docker\complete*" や " */users/robert/SpringBoot/gs-spring-boot-docker/complete*" など) に移動し、*pom.xml* ファイルをテキスト エディターで開きます。

1. *pom.xml* ファイル内の `<properties>` コレクションを、最新バージョンの [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) と、このチュートリアルの前のセクションにある Azure Container Registry のログイン サーバー値とアクセス設定で更新します。 次に例を示します。

   ```xml
   <properties>
      <jib-maven-plugin.version>1.7.0</jib-maven-plugin.version>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <username>wingtiptoysregistry</username>
      <password>{put your Azure Container Registry access key here}</password>
   </properties>
   ```

1. *pom.xml* ファイルの `<plugins>` コレクションに [jib-maven-plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) を追加します。  この例では、バージョン1.8.0 を使用します。 

`<from>/<image>` に基本イメージを指定します。ここでは `openjdk:8-jre-alpine` です。 基本イメージからビルドする最終イメージの名前を `<to>/<image>` に指定します。  

認証 `{docker.image.prefix}` は、前に示したレジストリ ページの**ログイン サーバー**です。 `{project.artifactId}` は、プロジェクトの最初の Mavin ビルドに由来する JAR ファイルの名前とバージョン番号です。

レジストリ ペインのユーザー名とパスワードを `<to>/<auth>` ノードに指定します。 次に例を示します。

   ```xml
   <plugin>
     <artifactId>jib-maven-plugin</artifactId>
     <groupId>com.google.cloud.tools</groupId>
     <version>1.8.0</version>
     <configuration>
        <from>
            <image>openjdk:8-jre-alpine</image>
        </from>
        <to>
            <image>${docker.image.prefix}/${project.artifactId}</image>
            <auth>
               <username>${username}</username>
               <password>${password}</password>
            </auth>
        </to>
     </configuration>
   </plugin>
   ```

1. Spring Boot アプリケーション用の完了プロジェクト ディレクトリに移動し、次のコマンドを実行してアプリケーションをリビルドし、コンテナーを Azure Container Registry にプッシュします。

   ```cmd
   mvn compile jib:build
   ```

> [!NOTE]
>
> Jib を使ってイメージを Azure Container Registry にプッシュする場合、イメージは *Dockerfile* を使用しません。詳細については、[こちら](https://cloudplatform.googleblog.com/2018/07/introducing-jib-build-java-docker-images-better.html)のドキュメントをご覧ください。
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>コンテナー イメージを使用して Azure App Service で Linux に Web アプリを作成する

1. [Azure Portal]を参照して、サインインします。

2. **[+ リソースの作成]** のメニュー アイコンをクリックし、 **[Web]** 、 **[Web App for Containers]** の順にクリックします。
   
   ![Azure ポータルで Web アプリを新しく作成する][LX01]

3. **[Web App on Linux]** ページが表示されたら、次の情報を入力します。

   * **[サブスクリプション]** ボックスの一覧で、サブスクリプションを選択します。

   * **[リソース グループ]** ボックスの一覧で、既存のリソース グループを選択するか、新しいリソース グループの名前を指定して作成します。

   * **[App name]\(アプリ名\)** に一意の名前 ("*wingtiptoyslinux*" など) を入力します。

   * **[発行]** に `Docker Container` を指定します。

   * **[オペレーティング システム]** として *[Linux]* を選択します。

   * **リージョン**を選択します。

   * **[Linux プラン]** を受け入れて既存の **App Service プラン**を選択するか、 **[新規作成]** をクリックして新しい App Service プランを作成します。

   * **[次へ: Docker]** をクリックします。

   ![Web アプリの設定を構成する][LX02]

      **[Web アプリ]** ページで、 **\[Docker\]** を選択し、次の情報を入力します。

   * **[単一コンテナー]** を選択します。

   * **レジストリ**:コンテナー ("*wingtiptoysregistry*" など) を選択します

   * **イメージ**:前に作成したイメージ ("*gs-spring-boot-docker*" など) を選択します

   * **タグ**: イメージのタグ ("*latest*" など) を選択します。
   
   * **スタートアップ コマンド**:イメージには既にスタートアップ コマンドがあるため、これは空白のままにしておきます
   
   上記の情報をすべて入力したら、 **[レビュー + 作成]** をクリックします。

   ![Web アプリの設定を構成する][LX02-A]

   * **[Review + create]\(レビュー + 作成\)** をクリックします。
   
情報を確認し、 **[作成]** をクリックします。

デプロイが完了したら、 **[リソースに移動]** をクリックします。  デプロイ ページには、アプリケーションにアクセスするための URL が表示されます。

   ![デプロイの URL を取得する][LX02-B]

> [!NOTE]
>
> Azure は、80 または 8080 の標準のポートで実行されている埋め込みの Tomcat サーバーにインターネットの要求を自動的にマップします。 ただし、埋め込みの Tomcat サーバーをカスタム ポートで実行するように構成している場合は、埋め込みの Tomcat サーバーのポートを定義する環境変数を Web アプリに追加する必要があります。 そのためには、次の手順を実行してください。
>
> 1. [Azure Portal]を参照して、サインインします。
> 
> 2. **[Web Apps]** のアイコンをクリックし、 **[App Services]** ページからアプリを選択します。
>
> 4. 左側のナビゲーション ウィンドウで、 **[構成]** をクリックします。
>
> 5. **[アプリケーション設定]** セクションで、**WEBSITES_PORT** という名前の新しい設定を追加して、この値にカスタム ポート番号を入力します。
>
> 6. **[OK]** をクリックします。 **[保存]** をクリックします。
>
> ![Azure Portal でのカスタム ポート番号の保存][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)
* [Azure Container Service で Spring Boot アプリケーションを Kubernetes クラスターにデプロイする](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

Docker サンプル プロジェクトでの Spring Boot の詳細については、[Docker での Spring Boot の使用開始]に関するページを参照してください。

独自の Spring Boot アプリケーションの使用開始に関するヘルプについては、「**Spring Initializr**」(https://start.spring.io/ ) を参照してください。

単純な Spring Boot アプリケーションの作成開始の詳細については、「Spring Initializr」(https://start.spring.io/ ) を参照してください。

Azure でカスタム Docker イメージを使用する方法に関するその他の例については、「[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]」を参照してください。

<!-- URL List -->

[Azure コマンド ライン インターフェイス (CLI)]: /cli/azure/overview
[Azure Container Service (AKS)]: https://azure.microsoft.com/services/container-service/
[Java 開発者向けの Azure]: /azure/java/
[Azure Portal]: https://portal.azure.com/
[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
[Azure Web App on Linux 向けのカスタム Docker イメージを使用する]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Azure DevOps と Java の操作]: /azure/devops/java/
[Maven]: http://maven.apache.org/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Docker での Spring Boot の使用開始]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->

[SB01]: ./media/deploy-spring-boot-java-app-on-linux/SB01.png
[SB02]: ./media/deploy-spring-boot-java-app-on-linux/SB02.png
[AR01]: ./media/deploy-spring-boot-java-app-on-linux/AR01.png
[AR03]: ./media/deploy-spring-boot-java-app-on-linux/AR03.png
[AR04]: ./media/deploy-spring-boot-java-app-on-linux/AR04.png
[LX01]: ./media/deploy-spring-boot-java-app-on-linux/LX01.png
[LX02]: ./media/deploy-spring-boot-java-app-on-linux/LX02.png
[LX02-A]: ./media/deploy-spring-boot-java-app-on-linux/LX02-A.png
[LX02-B]: ./media/deploy-spring-boot-java-app-on-linux/LX02-B.png
[LX03]: ./media/deploy-spring-boot-java-app-on-linux/LX03.png
