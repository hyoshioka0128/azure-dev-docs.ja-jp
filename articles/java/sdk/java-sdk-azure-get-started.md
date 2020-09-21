---
title: Azure SDK for Java を使用する作業の開始
description: Azure クラウド リソースを作成し、それらのリソースに接続して Java アプリケーションで使用する方法について説明します。
keywords: Azure, Java, SDK, API, 認証, 概要
author: rloutlaw
ms.date: 04/16/2017
ms.topic: article
ms.service: multiple
ms.assetid: b1e10b79-f75e-4605-aecd-eed64873e2d3
ms.custom: seo-java-august2019, devx-track-java
ms.openlocfilehash: d8f173fca56af289fb83012a101bc5cb99e4ed12
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379516"
---
# <a name="get-started-with-cloud-development-using-java-on-azure"></a>Azure での Java を使用したクラウド開発の開始

このガイドでは、Java での Azure 開発用の開発環境を設定する手順について説明します。 Azure リソースをいくつか作成し、それらに接続して基本的なタスク (ファイルのアップロードや Web アプリケーションのデプロイなど) を実行します。 作業が完了すると、独自の Java アプリケーションで Azure サービスの使用を開始できるようになります。

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="prerequisites"></a>前提条件

- Azure アカウント。 所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) または [Azure CLI 2.0](/cli/azure/install-az-cli2)。
- [Java 8](https://www.azul.com/downloads/zulu/) (Azure Cloud Shell に付属)
- [Maven 3](https://maven.apache.org/download.cgi) (Azure Cloud Shell に付属)

## <a name="set-up-authentication"></a>認証の設定

このチュートリアルのサンプル コードを実行する Java アプリケーションには、Azure サブスクリプションの読み取りと作成のアクセス許可が必要です。 サービス プリンシパルを作成し、その資格情報で動作するようにアプリケーションを構成してください。 サービス プリンシパルによって、自分の ID に関連付けられた非対話型のアカウントを作成し、アプリの実行に必要な権限だけを付与することができます。

[Azure CLI 2.0 を使ってサービス プリンシパルを作成](/cli/azure/create-an-azure-service-principal-azure-cli)し、その出力をキャプチャしてください。 password 引数には、`MY_SECURE_PASSWORD` ではなく、[セキュリティで保護されたパスワード](/azure/active-directory/active-directory-passwords-policy)を指定します。 パスワードは 8 ～ 16 文字にし、次の 4 つの条件のうち少なくとも 3 つの条件に一致する必要があります。

* 小文字が含まれている
* 大文字が含まれている
* 数字が含まれている
* 次の記号のいずれかが含まれている: @ # $ % ^ & * - _ ! + = [ ] { } | \ : ' , . ? / ` ~ " ( ) ;

```azurecli-interactive
az ad sp create-for-rbac --name AzureJavaTest --password "MY_SECURE_PASSWORD"
```

次の形式で応答が返されます。

```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "AzureJavaTest",
  "name": "http://AzureJavaTest",
  "password": password,
  "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
}
```

次に、ご利用のシステム上のテキスト ファイルに次のコードをコピーします。

```text
# sample management library properties file
subscription=ssssssss-ssss-ssss-ssss-ssssssssssss
client=cccccccc-cccc-cccc-cccc-cccccccccccc
key=kkkkkkkkkkkkkkkk
tenant=tttttttt-tttt-tttt-tttt-tttttttttttt
managementURI=https\://management.core.windows.net/
baseURL=https\://management.azure.com/
authURL=https\://login.windows.net/
graphURL=https\://graph.windows.net/
```

上から 4 つの値は、次の内容に置き換えてください。

- subscription: Azure CLI 2.0 の `az account show` から得られる *id* 値を使用します。
- client: サービス プリンシパルの出力から得られる *appId* 値を使用します。
- key: サービス プリンシパルの出力から得られる *password* 値を使用します。
- tenant: サービス プリンシパルの出力から得られる *tenant* 値を使用します。

このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。 このファイルは今後のコードに使用できるため、この記事のアプリケーションの外部の場所に保存することをお勧めします。

ご利用のシェルから、認証ファイルの完全なパスを保持する環境変数 `AZURE_AUTH_LOCATION` を設定します。

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azureauth.properties
```

Windows 環境で作業している場合は、システムのプロパティに変数を追加します。 管理者特権で PowerShell ウィンドウを開き、2 番目の変数をファイルのパスで置き換えた後、次のコマンドを入力します。

```powershell
setx AZURE_AUTH_LOCATION "C:\<fullpath>\azureauth.properties" /m
```

## <a name="tooling"></a>ツール

### <a name="create-a-new-maven-project"></a>新しい Maven プロジェクトを作成する

> [!NOTE]
> このガイドでは、Maven ビルド ツールを使って、サンプル コードをビルドして実行していますが、Java 用 Azure ライブラリは他のビルド ツール (Gradle など) で使用することもできます。

コマンド ラインを使って、ご利用のシステム上の新しいディレクトリに Maven プロジェクトを作成します。

```shell
mkdir java-azure-test
cd java-azure-test
mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=AzureApp \
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

これにより、基本的な Maven プロジェクトが `testAzureApp` フォルダーに作成されます。 プロジェクトの `pom.xml` に次のエントリを追加して、このチュートリアルのサンプル コードで使用するライブラリをインポートします。

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-storage</artifactId>
    <version>5.0.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.1.jre8</version>
</dependency>
```

[maven-exec-plugin](https://www.mojohaus.org/exec-maven-plugin/) を使ってサンプルを実行するために、最上位の `project` 要素に `build` エントリを追加します。

```XML
<build>
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.AzureApp</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
 ```

### <a name="install-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ をインストールする

Web アプリや API をプログラムでデプロイする予定でも、その他の開発に現在使用していない場合は、[Azure Toolkit](/azure/developer/java/toolkit-for-intellij/installation) が必要です。 インストール プロセスの概要を次に示します。 クイック スタートについては、[Azure Toolkit for IntelliJ のクイック スタート](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app)に関するページをご覧ください。

- **[File]\(ファイル\)** メニューを選択し、 **[Settings...]\(設定...\)** を選択します。

- **[Browse repositories...]\(リポジトリを参照...\)** を選択し、"Azure" を検索して、**Azure Toolkit for Intellij** をインストールします。

- Intellij を再起動します。

### <a name="install-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse をインストールする

Web アプリや API をプログラムでデプロイする予定でも、その他の開発に現在使用していない場合は、[Azure Toolkit](/azure/developer/java/toolkit-for-eclipse) が必要です。 インストール プロセスの概要を次に示します。 クイック スタートについては、[Azure Toolkit for Eclipse のクイック スタート](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app)に関するページをご覧ください。

- **[Help]\(ヘルプ\)** メニューを選択し、 **[Install New software]\(新しいソフトウェアのインストール\)** を選択します。

- **[Work with:]\(処理:\)** フィールドに「`http://dl.microsoft.com/eclipse/`」と入力し、Enter キーを押します。

- 次に、 **[Azure Toolkit for Java]** の横のチェック ボックスをオンにし、 **[Contact all update sites during install to find required software]\(インストール中にすべての更新サイトに接続して必要なソフトウェアを見つける\)** のチェック ボックスをオフにします。 [Next]\(次へ\) をクリックします。

## <a name="create-a-linux-virtual-machine"></a>Linux 仮想マシンの作成

プロジェクトの `src/main/java/com/fabirkam` ディレクトリに `AzureApp.java` という名前の新しいファイルを作成し、次のコード ブロックを貼り付けます。 `userName` 変数と `sshKey` 変数は、ご利用のマシンの実際の値に置き換えてください。 このコードによって、米国東部 Azure リージョンで実行されるリソース グループ `sampleResourceGroup` に、`testLinuxVM` という名前の新しい Linux VM が作成されます。

```java
package com.fabrikam;

import com.microsoft.azure.management.Azure;
import com.microsoft.azure.management.compute.VirtualMachine;
import com.microsoft.azure.management.compute.KnownLinuxVirtualMachineImage;
import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
import com.microsoft.azure.management.appservice.WebApp;
import com.microsoft.azure.management.storage.StorageAccount;
import com.microsoft.azure.management.storage.SkuName;
import com.microsoft.azure.management.storage.StorageAccountKey;
import com.microsoft.azure.management.sql.SqlDatabase;
import com.microsoft.azure.management.sql.SqlServer;
import com.microsoft.azure.management.resources.fluentcore.arm.Region;
import com.microsoft.azure.management.resources.fluentcore.utils.SdkContext;

import com.microsoft.rest.LogLevel;

import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

import java.io.File;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class AzureApp {

    public static void main(String[] args) {

        final String userName = "YOUR_VM_USERNAME";
        final String sshKey = "YOUR_PUBLIC_SSH_KEY";

        try {

            // use the properties file with the service principal information to authenticate
            // change the name of the environment variable if you used a different name in the previous step
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            // create a Ubuntu virtual machine in a new resource group
            VirtualMachine linuxVM = azure.virtualMachines().define("testLinuxVM")
                    .withRegion(Region.US_EAST)
                    .withNewResourceGroup("sampleVmResourceGroup")
                    .withNewPrimaryNetwork("10.0.0.0/24")
                    .withPrimaryPrivateIPAddressDynamic()
                    .withoutPrimaryPublicIPAddress()
                    .withPopularLinuxImage(KnownLinuxVirtualMachineImage.UBUNTU_SERVER_16_04_LTS)
                    .withRootUsername(userName)
                    .withSsh(sshKey)
                    .withUnmanagedDisks()
                    .withSize(VirtualMachineSizeTypes.STANDARD_D3_V2)
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
}
```

コマンド ラインでサンプルを実行します。

```shell
mvn compile exec:java
```

コンソールには、REST の要求と応答がいくつか表示されます。これは、SDK が内部的に Azure REST API を呼び出して、仮想マシンとそのリソースを構成しているためです。 プログラムの実行が完了したら、サブスクリプション内の仮想マシンを Azure CLI 2.0 で確認します。

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

コードが正しく動作したことを確認したら、CLI で VM とそのリソースを削除します。

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>GitHub リポジトリからの Web アプリのデプロイ

`AzureApp.java` の main メソッドを以下のメソッドに差し替えます。`appName` 変数には、コードを実行する前に一意の値を指定してください。 このコードは、無料の価格レベルで稼働する新しい [Azure App Service Web App](/azure/app-service-web/app-service-web-overview) に、パブリック GitHub リポジトリの `master` ブランチから Web アプリケーションをデプロイするものです。

```java
    public static void main(String[] args) {
        try {

            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            final String appName = "YOUR_APP_NAME";

            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            WebApp app = azure.webApps().define(appName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleWebResourceGroup")
                    .withNewWindowsPlan(PricingTier.FREE_F1)
                    .defineSourceControl()
                    .withPublicGitRepository(
                            "https://github.com/Azure-Samples/app-service-web-java-get-started")
                    .withBranch("master")
                    .attach()
                    .create();

        } catch (Exception e) {
            System.out.println(e.getMessage());
            e.printStackTrace();
        }
    }
```

先ほどと同様、このコードも Maven を使用して実行します。

```shell
mvn clean compile exec:java
```

ブラウザーを開いてアプリケーションにアクセスします。CLI から次のコマンドを入力してください。

```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

デプロイを検証したら、Web アプリとプランをサブスクリプションから削除します。

```azurecli-interactive
az group delete --name sampleWebResourceGroup
```

## <a name="connect-to-an-azure-sql-database"></a>Azure SQL データベースに接続する

`AzureApp.java` にある現行の main メソッドを以下のコードに差し替えます。`dbPassword` 変数には、実際の値を設定してください。
このコードは、リモート アクセスを許可するファイアウォール規則で新しい SQL データベースを作成し、SQL Database JDBC ドライバーを使ってそのデータベースに接続するものです。

```java
    public static void main(String args[])
    {
        // create the db using the management libraries
        try {
            final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
            Azure azure = Azure.configure()
                    .withLogLevel(LogLevel.BASIC)
                    .authenticate(credFile)
                    .withDefaultSubscription();

            final String adminUser = SdkContext.randomResourceName("db",8);
            final String sqlServerName = SdkContext.randomResourceName("sql",10);
            final String sqlDbName = SdkContext.randomResourceName("dbname",8);
            final String dbPassword = "YOUR_PASSWORD_HERE";


            SqlServer sampleSQLServer = azure.sqlServers().define(sqlServerName)
                            .withRegion(Region.US_EAST)
                            .withNewResourceGroup("sampleSqlResourceGroup")
                            .withAdministratorLogin(adminUser)
                            .withAdministratorPassword(dbPassword)
                            .withNewFirewallRule("0.0.0.0","255.255.255.255")
                            .create();

            SqlDatabase sampleSQLDb = sampleSQLServer.databases().define(sqlDbName).create();

            // assemble the connection string to the database
            final String domain = sampleSQLServer.fullyQualifiedDomainName();
            String url = "jdbc:sqlserver://"+ domain + ":1433;" +
                    "database=" + sqlDbName +";" +
                    "user=" + adminUser+ "@" + sqlServerName + ";" +
                    "password=" + dbPassword + ";" +
                    "encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;";

            // connect to the database, create a table and insert a entry into it
            Connection conn = DriverManager.getConnection(url);

            String createTable = "CREATE TABLE CLOUD ( name varchar(255), code int);";
            String insertValues = "INSERT INTO CLOUD (name, code ) VALUES ('Azure', 1);";
            String selectValues = "SELECT * FROM CLOUD";
            Statement createStatement = conn.createStatement();
            createStatement.execute(createTable);
            Statement insertStatement = conn.createStatement();
            insertStatement.execute(insertValues);
            Statement selectStatement = conn.createStatement();
            ResultSet rst = selectStatement.executeQuery(selectValues);

            while (rst.next()) {
                System.out.println(rst.getString(1) + " "
                        + rst.getString(2));
            }


        } catch (Exception e) {
            System.out.println(e.getMessage());
            System.out.println(e.getStackTrace().toString());
        }
    }
```

コマンド ラインでサンプルを実行します。

```shell
mvn clean compile exec:java
```

その後、CLI で次のコマンドを入力して、リソースをクリーンアップします。

```azurecli-interactive
az group delete --name sampleSqlResourceGroup
```

## <a name="write-a-blob-into-a-new-storage-account"></a>新しいストレージ アカウントへの BLOB の書き込み

`AzureApp.java` にある現行の main メソッドを以下のコードに差し替えます。 このコードは、[Azure ストレージ アカウント](/azure/storage/common/storage-introduction)を作成し、Azure Storage Libraries for Java を使って新しいテキスト ファイルをクラウドに作成するものです。

```java
public static void main(String[] args) {

    try {

        // use the properties file with the service principal information to authenticate
        // change the name of the environment variable if you used a different name in the previous step
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
                .withLogLevel(LogLevel.BASIC)
                .authenticate(credFile)
                .withDefaultSubscription();

        // create a new storage account
        String storageAccountName = SdkContext.randomResourceName("st",8);
        StorageAccount storage = azure.storageAccounts().define(storageAccountName)
                    .withRegion(Region.US_WEST2)
                    .withNewResourceGroup("sampleStorageResourceGroup")
                    .create();

        // create a storage container to hold the file
        List<StorageAccountKey> keys = storage.getKeys();
        final String storageConnection = "DefaultEndpointsProtocol=https;"
                + "AccountName=" + storage.name()
                + ";AccountKey=" + keys.get(0).value()
                + ";EndpointSuffix=core.windows.net";

        CloudStorageAccount account = CloudStorageAccount.parse(storageConnection);
        CloudBlobClient serviceClient = account.createCloudBlobClient();

        // Container name must be lower case.
        CloudBlobContainer container = serviceClient.getContainerReference("helloazure");
        container.createIfNotExists();

        // Make the container public
        BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
        containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
        container.uploadPermissions(containerPermissions);

        // write a blob to the container
        CloudBlockBlob blob = container.getBlockBlobReference("helloazure.txt");
        blob.uploadText("hello Azure");

    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }
    }
}
```

コマンド ラインでサンプルを実行します。

```shell
mvn clean compile exec:java
```

ストレージ アカウント内の `helloazure.txt` ファイルは、Azure Portal または [Azure Storage Explorer](/azure/vs-azure-tools-storage-explorer-blobs) から参照することができます。

CLI で次のコマンドを入力して、ストレージ アカウントをクリーンアップします。

```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>その他のサンプルを探す

Azure Management Libraries for Java を使ってリソースを管理したりタスクを自動化したりする方法をさらに詳しく知るには、[仮想マシン](java-sdk-azure-virtual-machine-samples.md)、[Web アプリ](java-sdk-azure-web-apps-samples.md)、[SQL データベース](java-sdk-azure-sql-database-samples.md)に関するサンプル コードを参照してください。

## <a name="reference-and-release-notes"></a>リファレンスとリリース ノート

すべてのパッケージには、[リファレンス](/java/api)が提供されています。

## <a name="get-help-and-give-feedback"></a>質問とフィードバック

ご質問は、[Stack Overflow](https://stackoverflow.com/questions/tagged/azure+java) のコミュニティに投稿してください。 Java 用 Azure ライブラリに関する未解決の問題やバグは、[プロジェクト GitHub](https://github.com/Azure/azure-sdk-for-java) にご報告ください。
