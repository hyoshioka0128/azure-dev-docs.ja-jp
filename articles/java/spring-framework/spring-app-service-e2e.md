---
title: Azure Database for MySQL を使用して Spring/Tomcat アプリを App Service に デプロイする
description: MySQL を使用した Java App Service のエンドツーエンド チュートリアル
author: KarlErickson
ms.author: karler
ms.date: 11/12/2019
ms.service: app-service
ms.topic: article
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: a8e0614547fa7bfca3a8d66af8a1c1d4240fe161
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689175"
---
# <a name="deploy-a-spring-app-to-app-service-with-mysql"></a>MySQL を使用して Spring アプリを App Service にデプロイする

このチュートリアルでは、App Service Linux で Java Web アプリをビルド、構成、デプロイ、トラブルシューティング、およびスケールするプロセスについて説明します。

このチュートリアルは、一般的な Spring PetClinic サンプル アプリに基づいています。 このトピックでは、HSQLDB バージョンのアプリをローカルでテストし、[Azure App Service](/azure/app-service/containers) にデプロイします。 その後、[Azure Database for MySQL](/azure/mysql) を使用するバージョンを構成して展開します。 最後に、アプリのログにアクセスし、アプリを実行するワーカーの数を増増やしてスケールアウトする方法について説明します。

## <a name="prerequisites"></a>前提条件

* [Azure CLI](/cli/azure/overview)
* [Java 8](http://java.oracle.com/)
* [Maven 3](http://maven.apache.org/)
* [Git](https://github.com/)
* [Tomcat 9](https://tomcat.apache.org/download-80.cgi)
* [MySQL CLI](http://dev.mysql.com/downloads/mysql/)

## <a name="get-the-sample"></a>サンプルを入手する

サンプル アプリを使い始める場合は、次のコマンドを使用してソース リポジトリを複製し、準備します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
git clone https://github.com/spring-petclinic/spring-framework-petclinic.git
cd spring-framework-petclinic
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
git clone https://github.com/spring-petclinic/spring-framework-petclinic.git
cd spring-framework-petclinic
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
git clone https://github.com/spring-petclinic/spring-framework-petclinic.git
cd spring-framework-petclinic
```
---

## <a name="build-and-run-the-hsqldb-sample-locally"></a>HSQLDB サンプルをローカルでビルドして実行する

まず、HSQLDB をデータベースとして使用して、サンプルをローカルでテストします。

サンプル アプリの HSQLDB バージョンをビルドします。

``` azurecli
mvn package
```

次に、TOMCAT_HOME 環境変数を Tomcat のインストール場所に設定します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
export TOMCAT_HOME=<Tomcat install directory>
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
$env:TOMCAT_HOME="<Tomcat install directory>"
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
set TOMCAT_HOME=<Tomcat install directory>
```
---

次に、WAR ファイルをデプロイするための *pom.xml* ファイルを更新します。 次の XML を既存の `<plugins>` 要素の子として追加します。 必要に応じて、`1.7.11` を [Cargo Maven 2 プラグイン](https://mvnrepository.com/artifact/org.codehaus.cargo/cargo-maven2-plugin)の最新バージョンに変更します。

```xml
<plugin>
    <groupId>org.codehaus.cargo</groupId>
    <artifactId>cargo-maven2-plugin</artifactId>
    <version>1.7.11</version>
    <configuration>
        <container>
            <containerId>tomcat9x</containerId>
            <type>installed</type>
            <home>${TOMCAT_HOME}</home>
        </container>
        <configuration>
            <type>existing</type>
            <home>${TOMCAT_HOME}</home>
        </configuration>
        <deployables>
            <deployable>
                <groupId>${project.groupId}</groupId>
                <artifactId>${project.artifactId}</artifactId>
                <type>war</type>
                <properties>
                    <context>/</context>
                </properties>
            </deployable>
        </deployables>
    </configuration>
</plugin>
```

この構成を使用すると、ローカルでアプリを Tomcat に展開できます。

```azurecli
mvn cargo:deploy
```

次に、Tomcat を起動します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
${TOMCAT_HOME}/bin/catalina.sh run
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
& $env:TOMCAT_HOME/bin/catalina.bat run
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
%TOMCAT_HOME%\bin\catalina.bat run
```
---

これで、ブラウザーで `http://localhost:8080` に移動して実行中のアプリを表示し、動作を確認できるようになりました。 完了したら、Bash プロンプトで Ctrl + C キーを押して Tomcat を停止します。

## <a name="deploy-to-azure-app-service"></a>Azure App Service にデプロイする

ローカルで動作することを確認したので、次はアプリを Azure にデプロイします。

まず、次の環境変数を設定します。 `REGION` については、`West US 2` を使用するか、[こちら](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=app-service)で見つけることができる他のリージョンを使用します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
export RESOURCEGROUP_NAME=<resource group>
export WEBAPP_NAME=<web app>
export WEBAPP_PLAN_NAME=${WEBAPP_NAME}-appservice-plan
export REGION=<region>
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
$env:RESOURCEGROUP_NAME="<resource group>"
$env:WEBAPP_NAME="<web app>"
$env:WEBAPP_PLAN_NAME="$env:WEBAPP_NAME-appservice-plan"
$env:REGION="<region>"
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
set RESOURCEGROUP_NAME=<resource group>
set WEBAPP_NAME=<web app>
set WEBAPP_PLAN_NAME=%WEBAPP_NAME%-appservice-plan
set REGION=<region>
```
---

Maven では、指定した名前で Azure リソースを作成するためにこれらの値が使用されます。 環境変数を使用すると、プロジェクト ファイルのアカウントのシークレットを保持することができます。

次に、 *pom.xml* ファイルを更新して、Azure へのデプロイ用に Maven を構成します。 以前に追加した `<plugin>` 要素の後に、次の XML を追加します。 必要に応じて、`1.11.0` を [Azure App Service 用 Maven プラグイン](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)の最新バージョンに変更します。

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.12.0</version>
    <configuration>
        <schemaVersion>v2</schemaVersion>
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>
        <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>
            <webContainer>TOMCAT 9.0</webContainer>
        </runtime>
        <deployment>
            <resources>
                <resource>
                    <directory>${project.basedir}/target</directory>
                    <includes>
                        <include>*.war</include>
                    </includes>
                </resource>
             </resources>
         </deployment>
    </configuration>
</plugin>
```

次に、Azure にサインインします。

```azurecli
az login
```

次に、アプリを App Service Linux にデプロイします。

```azurecli
mvn azure-webapp:deploy
```

これで、(`https://<app-name>.azurewebsites.net` を置換した後に) `<app-name>` に移動して実行中のアプリを表示できるようになりました。

## <a name="set-up-azure-database-for-mysql"></a>Azure Database for MySQL の設定

次に、HSQLDB ではなく MySQL を使用するように切り替えます。 Azure 上で MySQL サーバー インスタンスを作成し、データベースを追加した後、新しいデータベース接続情報を使用してアプリの構成を更新します。

まず、後の手順で使用するために、次の環境変数を設定します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
export MYSQL_SERVER_NAME=<server>
export MYSQL_SERVER_FULL_NAME=${MYSQL_SERVER_NAME}.mysql.database.azure.com
export MYSQL_SERVER_ADMIN_LOGIN_NAME=<admin>
export MYSQL_SERVER_ADMIN_PASSWORD=<password>
export MYSQL_DATABASE_NAME=<database>
export DOLLAR=\$
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
$env:MYSQL_SERVER_NAME="<server>"
$env:MYSQL_SERVER_FULL_NAME="$env:MYSQL_SERVER_NAME.mysql.database.azure.com"
$env:MYSQL_SERVER_ADMIN_LOGIN_NAME="<admin>"
$env:MYSQL_SERVER_ADMIN_PASSWORD="<password>"
$env:MYSQL_DATABASE_NAME="<database>"
$env:DOLLAR="$"
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
set MYSQL_SERVER_NAME=<server>
set MYSQL_SERVER_FULL_NAME=%MYSQL_SERVER_NAME%.mysql.database.azure.com
set MYSQL_SERVER_ADMIN_LOGIN_NAME=<admin>
set MYSQL_SERVER_ADMIN_PASSWORD=<password>
set MYSQL_DATABASE_NAME=<database>
set DOLLAR=$
```
---

次に、データベース サーバーを作成して初期化します。 初期構成には [az mysql up](/cli/azure/ext/db-up/mysql?view=azure-cli-latest#ext-db-up-az-mysql-up) を使用します。 次に、[az mysql server configuration set](/cli/azure/mysql/server/configuration?view=azure-cli-latest#az-mysql-server-configuration-set) を使用して接続タイムアウトを増やし、サーバーのタイムゾーンを設定します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
az extension add --name db-up

az mysql up \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server-name ${MYSQL_SERVER_NAME} \
    --database-name ${MYSQL_DATABASE_NAME} \
    --admin-user ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
    --admin-password ${MYSQL_SERVER_ADMIN_PASSWORD}

az mysql server configuration set --name wait_timeout \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value 2147483

az mysql server configuration set --name time_zone \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value=-8:00
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
az extension add --name db-up

az mysql up `
    --resource-group $env:RESOURCEGROUP_NAME `
    --server-name $env:MYSQL_SERVER_NAME `
    --database-name $env:MYSQL_DATABASE_NAME `
    --admin-user $env:MYSQL_SERVER_ADMIN_LOGIN_NAME `
    --admin-password $env:MYSQL_SERVER_ADMIN_PASSWORD

az mysql server configuration set --name wait_timeout `
    --resource-group $env:RESOURCEGROUP_NAME `
    --server $env:MYSQL_SERVER_NAME --value 2147483

az mysql server configuration set --name time_zone `
    --resource-group $env:RESOURCEGROUP_NAME `
    --server $env:MYSQL_SERVER_NAME --value=-8:00
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
az extension add --name db-up

az mysql up ^
    --resource-group %RESOURCEGROUP_NAME% ^
    --server-name %MYSQL_SERVER_NAME% ^
    --database-name %MYSQL_DATABASE_NAME% ^
    --admin-user %MYSQL_SERVER_ADMIN_LOGIN_NAME% ^
    --admin-password %MYSQL_SERVER_ADMIN_PASSWORD%

az mysql server configuration set --name wait_timeout ^
    --resource-group %RESOURCEGROUP_NAME% ^
    --server %MYSQL_SERVER_NAME% --value 2147483

az mysql server configuration set --name time_zone ^
    --resource-group %RESOURCEGROUP_NAME% ^
    --server %MYSQL_SERVER_NAME% --value=-8:00
```
---

次に、MySQL CLI を使用して Azure 上のデータベースに接続します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
mysql -u ${MYSQL_SERVER_ADMIN_LOGIN_NAME}@${MYSQL_SERVER_NAME} \
 -h ${MYSQL_SERVER_FULL_NAME} -P 3306 -p
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
mysql -u $env:MYSQL_SERVER_ADMIN_LOGIN_NAME@$env:MYSQL_SERVER_NAME `
 -h $env:MYSQL_SERVER_FULL_NAME -P 3306 -p
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
mysql -u %MYSQL_SERVER_ADMIN_LOGIN_NAME%@%MYSQL_SERVER_NAME% ^
 -h %MYSQL_SERVER_FULL_NAME% -P 3306 -p
```
---

MySQL CLI プロンプトで次のコマンドを実行して、`MYSQL_DATABASE_NAME` 環境変数に以前に指定したのと同じ値で名前が付けられたデータベースを確認します。

```console
show databases;
```

これで MySQL を使用する準備ができました。

## <a name="configure-the-app-for-mysql"></a>MySQL 用にアプリを構成する

次に、MySQL バージョンのアプリに接続情報を追加し、App Service にデプロイします。

*pom.xml* ファイルを更新して、MySQL をアクティブな構成にします。 次に示すように、HSQLDB プロファイルから `<activation>` 要素を削除し、代わりに MySQL プロファイルに配置します。 スニペットの残りの部分は、既存の構成を示しています。 以前に設定した環境変数が、Maven によって MySQL アクセスの構成にどのように使用されているかに注目してください。

```xml
<profile>
    <id>MySQL</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
        <db.script>mysql</db.script>
        <jpa.database>MYSQL</jpa.database>
        <jdbc.driverClassName>com.mysql.jdbc.Driver</jdbc.driverClassName>
        <jdbc.url>jdbc:mysql://${DOLLAR}{MYSQL_SERVER_FULL_NAME}:3306/${DOLLAR}{MYSQL_DATABASE_NAME}?useUnicode=true</jdbc.url>
        <jdbc.username>${DOLLAR}{MYSQL_SERVER_ADMIN_LOGIN_NAME}@${DOLLAR}{MYSQL_SERVER_FULL_NAME}</jdbc.username>
        <jdbc.password>${DOLLAR}{MYSQL_SERVER_ADMIN_PASSWORD}</jdbc.password>
    </properties>
    ...
</profile>
```

次に、 *pom.xml* ファイルを構成して、Azure のデプロイと MySQL の使用のために Maven を構成します。 以前に追加した `<plugin>` 要素の後に、次の XML を追加します。 必要に応じて、`1.11.0` を [Azure App Service 用 Maven プラグイン](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)の最新バージョンに変更します。

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.12.0</version>
    <configuration>
        <schemaVersion>v2</schemaVersion>
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>
        <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>
            <webContainer>TOMCAT 9.0</webContainer>
        </runtime>
        <appSettings>
            <property>
                <name>MYSQL_SERVER_FULL_NAME</name>
                <value>${MYSQL_SERVER_FULL_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_LOGIN_NAME</name>
                <value>${MYSQL_SERVER_ADMIN_LOGIN_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_PASSWORD</name>
                <value>${MYSQL_SERVER_ADMIN_PASSWORD}</value>
            </property>
            <property>
                <name>MYSQL_DATABASE_NAME</name>
                <value>${MYSQL_DATABASE_NAME}</value>
            </property>
        </appSettings>
        <deployment>
            <resources>
                <resource>
                    <directory>${project.basedir}/target</directory>
                    <includes>
                        <include>*.war</include>
                    </includes>
                </resource>
             </resources>
         </deployment>
    </configuration>
</plugin>
```

次に、アプリをビルドし、ローカルで展開し、Tomcat で実行してテストします。

# <a name="bash"></a>[bash](#tab/bash)

```bash
mvn package
mvn cargo:deploy
${TOMCAT_HOME}/bin/catalina.sh run
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
mvn package
mvn cargo:deploy
& $env:TOMCAT_HOME/bin/catalina.bat run
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```bash
mvn package
mvn cargo:deploy
%TOMCAT_HOME%\bin\catalina.bat run
```
---

これで、`http://localhost:8080` のローカルでアプリを表示できるようになりました。 アプリは前と同様に動作しますが、HSQLDB ではなく Azure Database for MySQL を使用します。 完了したら、Bash プロンプトで Ctrl + C キーを押して Tomcat を停止します。

最後に、アプリを App Service にデプロイします。

```bash
mvn azure-webapp:deploy
```

これで、`https://<app-name>.azurewebsites.net` に移動し、App Service と Azure Database for MySQL を使用して実行中のアプリを表示できるようになりました。

## <a name="access-the-app-logs"></a>アプリのログにアクセスする

トラブルシューティングが必要な場合は、アプリのログを確認できます。 ローカル コンピューター上でリモート ログ ストリームを開くには、次のコマンドを使用します。

# <a name="bash"></a>[bash](#tab/bash)

```bash
az webapp log tail --name ${WEBAPP_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
az webapp log tail --name $env:WEBAPP_NAME `
    --resource-group $env:RESOURCEGROUP_NAME
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```bash
az webapp log tail --name %WEBAPP_NAME% ^
    --resource-group %RESOURCEGROUP_NAME%
```
---

ログの表示が終了したら、Ctrl + C キーを押してストリームを停止します。

ログ ストリームは、`https://<app-name>.scm.azurewebsites.net/api/logstream` でも使用できます。

## <a name="scale-out"></a>スケール アウト

アプリへのトラフィックの増加をサポートするために、次のコマンドを使用して複数のインスタンスにスケールアウトできます。

# <a name="bash"></a>[bash](#tab/bash)

```bash
az appservice plan update --number-of-workers 2 \
    --name ${WEBAPP_PLAN_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
az appservice plan update --number-of-workers 2 `
    --name $env:WEBAPP_PLAN_NAME `
    --resource-group $env:RESOURCEGROUP_NAME
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```bash
az appservice plan update --number-of-workers 2 ^
    --name %WEBAPP_PLAN_NAME% ^
    --resource-group %RESOURCEGROUP_NAME%
```
---

お疲れさまでした。 Spring Framework、JSP、Spring Data、Hibernate、JDBC、App Service Linux、および Azure Database for MySQL を使用して Java Web アプリをビルドし、スケールアウトしました。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

前のセクションでは、リソース グループに Azure リソースを作成しました。 これらのリソースを今後使用する予定がない場合は、次のコマンドを実行してリソース グループを削除します。


# <a name="bash"></a>[bash](#tab/bash)

```bash
az group delete --name ${RESOURCEGROUP_NAME}
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```ps
az group delete --name $env:RESOURCEGROUP_NAME
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```bash
az group delete --name %RESOURCEGROUP_NAME%
```
---

## <a name="next-steps"></a>次のステップ

次は、App Service で Java に使用できるその他の構成オプションと CI/CD オプションを確認します。

> [!div class="nextstepaction"]
> [Azure App Service 向けの Linux Java アプリを構成する](/azure/app-service/containers/configure-language-java)
> [!div class="nextstepaction"]
> [Azure Pipelines を使用した Java Web アプリのビルドとデプロイ](/azure/devops/pipelines/ecosystems/java-webapp?view=azure-devops&tabs=java-tomcat)
> [!div class="nextstepaction"]
> [Jenkins プラグインを使用した Azure App Service へのデプロイ](/azure/jenkins/deploy-jenkins-app-service-plugin)
