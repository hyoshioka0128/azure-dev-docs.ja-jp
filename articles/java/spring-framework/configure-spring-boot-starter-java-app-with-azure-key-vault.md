---
title: Spring Boot アプリケーションでの Azure Key Vault からのシークレットの読み取りに関するチュートリアル
description: Spring Boot アプリケーションでの Azure Key Vault からのシークレットの読み取りに関するチュートリアル
services: key-vault
documentationcenter: java
ms.date: 10/14/2020
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: identity
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 09b1a13dda348329eee454574eed999f6d58a059
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561828"
---
# <a name="tutorial-reading-a-secret-from-azure-key-vault-in-a-spring-boot-application"></a>チュートリアル:Spring Boot アプリケーションでの Azure Key Vault からのシークレットの読み取り

Spring Boot アプリケーションは、ユーザー名やパスワードなどの機密情報を外部化します。  機密情報を外部化することで、保守容易性、テストの容易性、セキュリティを向上させることができます。  シークレットをコードの外部に格納することは、情報をハードコーディングしたり、ビルド時にインライン展開したりするよりも優れています。

このチュートリアルでは、Azure Key Vault から値を読み取り、アプリを Azure App Service と Azure Spring Cloud にデプロイする Spring Boot アプリを作成する方法について説明します。

> [!div class="checklist"]
> * Azure Key Vault を作成してシークレットを格納する
> * Spring Initializr を使用してアプリを作成する
> * アプリに Key Vault 統合を追加する
> * Azure App Service に配置する
> * Azure リソースのマネージド ID を使用して Azure App Service に再デプロイする
> * Azure Spring Cloud にデプロイする

## <a name="prerequisites"></a>前提条件

* 有効な Azure サブスクリプション
  * Azure サブスクリプションをお持ちでない場合は、[無料アカウントを作成](https://azure.microsoft.com/free/)してください。
* [Azure CLI バージョン 2.0.67 以降をインストール](/cli/azure/install-azure-cli)し、`az extension add --name spring-cloud` コマンドを使用して Azure Spring Cloud 拡張機能をインストールする
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。
* `curl` コマンド。  ほとんどの UNIX 系オペレーティング システムには、このコマンドがプレインストールされています。  OS 固有のクライアントは、[curl の公式 Web サイト](https://curl.haxx.se/)で入手できます。
* `jq` コマンド。 ほとんどの UNIX 系オペレーティング システムには、このコマンドがプレインストールされています。  OS 固有のクライアントは、[jq の公式 Web サイト](https://stedolan.github.io/jq/)で入手できます。

## <a name="create-a-new-azure-key-vault"></a>新しい Azure キー コンテナーの作成

以下のセクションでは、Azure にログインし、Azure Key Vault を作成する方法について説明します。

### <a name="sign-into-azure-and-set-your-subscription"></a>Azure にサインインしてサブスクリプションを設定する

まず、次の手順に従って、Azure CLI を使用して認証します。

1. 必要に応じて、ログアウトして一部の認証ファイルを削除し、残留する資格情報を削除します。

   ```azurecli
   az logout
   rm ~/.azure/accessTokens.json
   rm ~/.azure/azureProfile.json
   ```

1. Azure CLI を使って、Azure アカウントにサインインします。

   ```azurecli
   az login
   ```

   指示に従って、サインインを完了します。

1. サブスクリプションを一覧表示します。

   ```azurecli
   az account list
   ```

   Azure はサブスクリプションの一覧を返します。 使用するサブスクリプションの `id` をコピーします。次に例を示します。

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. Azure で使用するサブスクリプションの GUID を指定します。次に例を示します。

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

### <a name="create-a-service-principal-for-use-in-by-your-app"></a>アプリで使用するサービス プリンシパルを作成する

Azure AD の *サービス プリンシパル* は、サブスクリプション内の Azure リソースへのアクセスを提供します。 サービス プリンシパルは、サービスのユーザー ID と考えることができます。  "サービス" とは、このチュートリアルでビルドするサンプル アプリを含む、Azure リソースにアクセスする必要があるアプリケーション、サービス、またはプラットフォームのことです。 指定したそれらのリソースのみをスコープとするアクセス権を持つサービス プリンシパルを構成できます。 その後、サービス プリンシパルの資格情報を使用してそれらのリソースにアクセスするようにアプリケーションまたはサービスを構成します。

次のコマンドを使用して、サービス プリンシパルを作成します。

```azurecli
az ad sp create-for-rbac --name contososp
```

`name` オプションの値は、お使いのサブスクリプション内で一意にする必要があります。  このチュートリアルで後ほど使用するために、コマンドから返された値を保存します。  戻される JSON は次のようになります。

```json
{
  "appId": "sample-app-id",
  "displayName": "ejbcontososp",
  "name": "http://ejbcontososp",
  "password": "sample-password",
  "tenant": "sample-tenant"
}
```

### <a name="create-the-key-vault-instance"></a>Key Vault インスタンスを作成する

次の手順では、Key Vault を作成して初期化します。

1. どの Azure リージョンにリソースを保持するかを決定します。
   1. [リージョンとその場所の一覧](https://azure.microsoft.com/regions/)をご確認ください。
   1. `az account list-locations` コマンドを使用すると、選択したリージョンの正しい `Name` を見つけることができます。

      ```azurecli
      az account list-locations -o table
      ```

      このチュートリアルでは、`eastus` を使用します。
1. Key Vault と App Service アプリを保持するリソース グループを作成します。  値は Azure サブスクリプション内で一意である必要があります。  このチュートリアルでは、`contosorg` を使用します。

   ```azurecli
   az group create --name contosorg --location eastus
   ```

1. リソース グループに新しい Key Vault を作成します。

   ```azurecli
   az keyvault create \
       --resource-group contosorg \
       --name contosokv \
       --enabled-for-deployment true \
       --enabled-for-disk-encryption true \
       --enabled-for-template-deployment true \
       --location eastus
       --query properties.vaultUri \
       --sku standard 
   ```

    > [!NOTE]
    > `--name` オプションの値は、Azure サブスクリプション内で一意にする必要があります。

   次の表では、上記のオプションについて説明します。

   | パラメーター | 説明 |
   |---|---|
   | `enabled-for-deployment` | [Key Vault のデプロイ オプション](/cli/azure/keyvault)を指定します。 |
   | `enabled-for-disk-encryption` | [Key Vault の暗号化オプション](/cli/azure/keyvault)を指定します。 |
   | `enabled-for-template-deployment` | [Key Vault の暗号化オプション](/cli/azure/keyvault)を指定します。 |
   | `location` | リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |
   | `name` | Key Vault の一意の名前を指定します。 |
   | `query` | 応答から Key Vault URI を取得します。  このチュートリアルを完了するには URI が必要です。 |
   | `sku` | [Key Vault SKU オプション](/cli/azure/keyvault)を指定します。 |

   Azure CLI に Key Vault の URI が表示されます。この URI は後で使用します。次に例を示します。

   ```output
   "https://contosokv.vault.azure.net/"
   ```

1. Key Vault を構成して、そのマネージド ID からの `get` と `list` 操作を許可します。  `object-id` の値は、上記の `az ad sp create-for-rbac` コマンドの `appId` です。

   ```azurecli
   az keyvault set-policy --name contosokv --spn http://ejbcontososp --secret-permissions get list
   ```

   出力は、Key Vault に関する完全な情報を含む JSON オブジェクトになります。  `Microsoft.KeyVault/vaults` の値を持つ `type` エントリがあります。

   次の表では、上記のプロパティについて説明します。

   | パラメーター | 説明 |
   |---|---|
   | name | Key Vault の名前。 |
   | spn | 上記の `az ad sp create-for-rbac` コマンドの出力からの `name`。 |
   | secret-permissions | 名前付きプリンシパルからの許可する操作の一覧。 |

    > [!NOTE]
    > 最小限の特権の原則ではリソースに対して最小限の特権セットを許可することが推奨されますが、Key Vault 統合の設計には少なくとも `get` と `list` が必要です。

1. 新しい Key Vault にシークレットを格納します。  一般的なユース ケースでは、JDBC 接続文字列を格納します。  次に例を示します。

   ```azurecli
   az keyvault secret set --name "connectionString" \
       --vault-name "contosokv" \
       --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   次の表では、上記のオプションについて説明します。

   | パラメーター | 説明 |
   |---|---|
   | `name` | シークレットの名前を指定します。 |
   | `value` | シークレットの値を指定します。 |
   | `vault-name` | 前の手順で作成した Key Vault の名前を指定します。 |

   Azure CLI にシークレット作成の結果が表示されます。次に例を示します。

   ```json
   {
     "attributes": {
       "created": "2020-08-24T21:48:09+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2020-08-24T21:48:09+00:00"
     },
     "contentType": null,
     "id": "https://contosokv.vault.azure.net/secrets/connectionString/sample-id",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

Key Vault を作成し、シークレットを保存したので、次のセクションでは Spring Initializr を使用してアプリを作成する方法について説明します。

## <a name="create-the-app-with-spring-initializr"></a>Spring Initializr を使用してアプリを作成する

このセクションでは、Spring Initializr および `RestController` を使用して Spring Boot アプリケーションをローカルで作成して実行する方法について説明します。

1. <https://start.spring.io/> を参照します。
1. この一覧に続く図に示すように項目を選択します。
   1. **Project**: `Maven Project`
   1. **Language**: `Java`
   1. **Spring Boot**: `2.3.3`
   1. **グループ**: `com.contoso` (任意の有効な Java パッケージ名をここに入力できます)。
   1. **Artifact**: *keyvault* (任意の有効な Java クラス名をここに入力できます。)
   1. **Packaging**: `Jar`
   1. **Java**: `11` (8 を選択できますが、このチュートリアルは 11 で検証されています。)
1. **[Add Dependencies...]** を選択します。
1. テキスト フィールドに「`Spring Web`」と入力し、Ctrl + Enter キーを押します。
1. テキスト フィールドに「`Azure Key Vault`」と入力し、Enter キーを押します。  画面は次のようになります。
   :::image type="content" source="media/configure-spring-boot-starter-java-app-with-azure-key-vault/spring-initializr-choices.png" alt-text="項目が正しく選択された Spring Initializr。":::
1. ページ下部にある **[Generate]** を選択します。
1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。  この説明では、現在のユーザーのホーム ディレクトリ内の *keyvault* ディレクトリを使用します。  上記の値を指定すると、そのディレクトリ内の *keyvault .zip* ファイルが表示されます。

アプリケーションを確認してローカルで実行するには、次の手順に従います。

1. *keyvault.zip* ファイルを解凍します。  ファイル レイアウトは次のようになります。  このチュートリアルでは、*test* ディレクトリとその内容は無視します。

   ```bash
   ├── HELP.md
   ├── mvnw
   ├── mvnw.cmd
   ├── pom.xml
   └── src
       ├── main
       │   ├── java
       │   │   └── com
       │   │       └── contoso
       │   │           └── keyvault
       │   │               └── KeyvaultApplication.java
       │   └── resources
       │       ├── application.properties
       │       ├── static
       │       └── templates
   ```

1. テキスト エディターで *KeyvaultApplication.java* ファイルを開きます。  ファイルを以下のように変更します。

   * クラスには `@RestController` の注釈が付いています。  `@RestController` は、クラスが RESTful HTTP 要求に応答できることを Spring Boot に示します。
   * クラスには `@GetMapping(get)` の注釈が付いたメソッドがあります。  `@GetMapping` は Spring Boot に対し、`/get` パスを含む HTTP 要求をそのメソッドに送信し、そのメソッドからの応答を HTTP クライアントに返すことを許可するように指示します。
   * クラスにはプライベート インスタンス変数 `connectionString` があります。  このインスタンス変数の値は `get()` メソッドから返されます。

   ```java
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;

   @SpringBootApplication
   @RestController
   public class KeyvaultApplication {

      public static void main(String[] args) {
        SpringApplication.run(KeyvaultApplication.class, args);
      }

     @GetMapping("get")
     public String get() {
       return connectionString;
     }

     private String connectionString = "defaultValue\n";  

     public void run(String... varl) throws Exception {
       System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
     }  

   }
   ```

1. *pom.xml* ファイルがある最上位の *keyvault* ディレクトリ で、「`mvn spring-boot:run`」と入力します。  
1. コマンド出力の「**Completed initialization**」(初期化が完了しました) というメッセージは、サーバーの準備ができていることを意味します。  別のシェル ウィンドウに、次のコマンドを入力します。

   ```bash
   $ curl http://localhost:8080/get
   ```

   出力には `defaultValue` が表示されます。

1. `mvn spring-boot:run` から実行されているプロセスを強制終了します。  Ctrl + C キーを押すか、`jps` コマンドを使用して `Launcher` プロセスの PID を取得し、強制終了することができます。

次のセクションでは、ローカルで実行しているアプリケーションに Key Vault 統合を追加する方法について説明します。

## <a name="add-key-vault-integration-to-the-app"></a>アプリに Key Vault 統合を追加する

次の手順では、Spring Boot アプリケーション `KeyvaultApplication` に必要な変更について説明します。

Key Vault がシークレットをアプリケーション コードから外部化できるのと同様に、Spring 構成では構成をコードから外部化できます。  Spring 構成の最も単純な形式は、*application.properties* ファイルです。  Maven プロジェクトでは、このファイルは *src/main/resources/application.properties* にあります。  Spring Initializr ではこの場所に長さゼロのファイルが含まれています。

このファイルに必要な構成を追加するには、次の手順に従います。

1. *src/main/resources/application.properties* をエディターで開き、次の内容を追加し、ご使用の Azure サブスクリプションについての値を調整します。

   ```txt
   azure.keyvault.client-id=685on005-ns8q-4o04-8s16-n7os38o2ro5n
   azure.keyvault.client-key=4bt.lCKJKlbYLn_3XF~wWtUwyHU0jKggu2
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=72s988os-86s1-41ns-91no-2q7pq011qo47
   azure.keyvault.uri=https://contosokv.vault.azure.net/
   ```

   次の表では、上記のプロパティについて説明します。

   | パラメーター | 説明 |
   |---|---|
   | azure.keyvault.client-id | `az ad sp create-for-rbac` から返される JSON の `appId` です。|
   | azure.keyvault.client-key | `az ad sp create-for-rbac` から返される JSON の `password` です。|
   | azure.keyvault.enabled | この構成は、デプロイ時に `enabled` または `disabled` を設定する必要がある場合に便利です。  Spring 構成の詳細については、[Spring のドキュメント](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/#boot-features-external-config)を参照してください。
   | azure.keyvault.tenant-id | `az ad sp create-for-rbac` から返される JSON の `tenant` です。|
   | azure.keyvault.uri | 上記の `az keyvault create` コマンドからの出力値です。 |

   使用できるプロパティの完全な一覧については[プロパティのリファレンス](https://aka.ms/azure-spring-boot-starter-keyvault-secrets)を参照してください。

1. ファイルを保存して閉じます。

*KeyvaultApplication.java* ファイル (または該当するクラス名) に単純な変更を加えます。

1. テキスト エディターで *src/main/java/com/contoso/keyvault/KeyvaultApplication.java* を開きます。
1. 次のインポートを追加します。

   ```java
   import org.springframework.beans.factory.annotation.Value;
   ```

1. `connectionString` インスタンス変数に注釈を追加します。

   ```java
   @Value("${connectionString}")
   private String connectionString;  
   ```

   Key Vault 統合により、Key Vault の値から取り込まれる Spring `PropertySource` が提供されます。  その他の実装の詳細については、[リファレンス ドキュメント](https://aka.ms/azure-spring-boot-starter-keyvault-secrets)を参照してください。

1. *pom.xml* ファイルがある最上位の *keyvault* ディレクトリ で、「`mvn clean package spring-boot:run`」と入力します。  
1. コマンド出力の「**initialization completed**」(初期化が完了しました) というメッセージは、サーバーの準備ができていることを意味します。  別のシェル ウィンドウに、次のコマンドを入力します。

   ```bash
   $ curl http://localhost:8080/get
   ```

   出力には `defaultValue` でなく `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE` が表示されます。

1. `mvn spring-boot:run` から実行されているプロセスを強制終了します。  Ctrl + C キーを押すか、`jps` コマンドを使用して `Launcher` プロセスの PID を取得し、強制終了することができます。


次のセクションでは、このアプリを Azure App Service にデプロイする方法について説明します。

## <a name="deploy-to-azure-app-service"></a>Azure App Service に配置する

このセクションの手順に従って、Azure App Service に `KeyvaultApplication` をデプロイします。

### <a name="use-the-azure-maven-web-app-plugin-to-deploy-the-application-to-azure-app-service"></a>Azure Maven Web アプリ プラグインを使用してアプリケーションを Azure App Service にデプロイする

次の手順に従って、`KeyvaultApplication` を Azure App Service にデプロイできるようにします。

1. 最上位の *keyvault* ディレクトリで、*pom.xml* ファイルを開きます。
1. `<build><plugins>` セクションで、次の XML を挿入して `azure-webapp-maven-plugin` を追加します。

   ```xml
    <plugin>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-webapp-maven-plugin</artifactId>
     <version>1.12.0</version>
    </plugin>
   ```

   > [!NOTE]
   > 書式設定について心配しないでください。  `azure-webapp-maven-plugin` は、このプロセス中に POM 全体を再フォーマットします。

1. *pom.xml* を保存して閉じます。
1. コマンド ラインで、新しく追加したプラグインの `config` ゴールを呼び出します。  Maven プラグインによっていくつかの質問が表示され、回答に基づいて *pom.xml* ファイルが編集されます。  POM をさらに編集します。

   ```bash
   mvn azure-webapp:config
   ```

1. `Subscription` では、作成した Key Vault と同じサブスクリプション ID が選択されていることを確認します。
1. `Web App` では、既存の Web アプリを選択するか、`<create>` を選択して新規作成できます。既存の Web アプリを選択した場合は、最後の **確認** の手順に直接移動します。
1. `OS` では、`linux` が選択されていることを確認します。
1. `javaVersion` には、Spring Initializr で選択した Java のバージョンが選択されていることを確認します。  上記で `11` を選択したので、ここでは 11 を選択します。
1. 残りの質問については既定値を受け入れます。
1. 確認を求められたら、Y と答えて続行するか、N で質問への回答を再び開始します。  プラグインの実行が完了すると、POM を編集できるようになります。

次の手順に従って、POM にさらに必要な編集を行います。

1. 最上位の *keyvault* ディレクトリで、*pom.xml* ファイルを開きます。
1. `<plugins> セクションで `azure-webapp-maven-plugin` エントリを探します。
1. `<resourceGroup>`、`<appName>`、`<region>` の値を変更します。  
   1. `<resourceGroup>` の値を、Key Vault の作成時に指定した値に設定します。
   1. サブスクリプション内で一意の実用的な値を `<appName>` に選択します。
   1. `<region>` の値を、Key Vault の作成時に指定した値に設定します。
1. サーバーが TCP ポート80でリッスンする `<appSettings>` 要素を含めます。
1. `azure-webapp-maven-plugin` の変更された完全な `<plugin>` を次に示します。  変更する必要がある値は `*` で示されています。

   ```xml
   <plugins> 
     <plugin> 
       <groupId>org.springframework.boot</groupId>  
       <artifactId>spring-boot-maven-plugin</artifactId> 
     </plugin>  
     <plugin> 
       <groupId>com.microsoft.azure</groupId>  
       <artifactId>azure-webapp-maven-plugin</artifactId>  
       <version>1.12.0</version>  
       <configuration>
         <schemaVersion>V2</schemaVersion>
         *<subscriptionId>********-****-****-****-************</subscriptionId>
         *<resourceGroup>contosorg</resourceGroup>
         *<appName>contosokeyvault</appName>
         <pricingTier>P1v2</pricingTier>
         *<region>eastus</region>
         <runtime>
           <os>linux</os>
           <javaVersion>java 11</javaVersion>
           <webContainer>Java SE</webContainer>
         </runtime>
         *<!-- Begin of App Settings  -->
         *<appSettings>
         *  <property>
         *    <name>JAVA_OPTS</name>
         *    <value>-Dserver.port=80</value>
         *  </property>
         *</appSettings>
         *<!-- End of App Settings  -->          
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
   </plugins>
   ```

1. POM を保存して閉じます。
1. アプリを Azure App Service にデプロイします。

   ```bash
   mvn -DskipTests clean package azure-webapp:deploy
   ```

1. ユーザーが制御できない多くの要因により、コマンドの実行に数分かかることがあります。  次のような出力が表示されると、アプリが正常にデプロイされたことがわかります。

   ```bash
   [INFO] Deploying the zip package contosokeyvault-22b7c1a3-b41b-4082-a9f0-9339723fa36a11893059035499017844.zip...
   [INFO] Successfully deployed the artifact to https://contosokeyvault.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  01:45 min
   [INFO] Finished at: 2020-08-16T22:47:48-04:00
   [INFO] ------------------------------------------------------------------------
   ```

1. デプロイが完了するまで 3 から 5 分待ちます。  その後、上記のような `curl` コマンドを使用してデプロイにアクセスできますが、今回は `BUILD SUCCESS` 出力に示されているホスト名を使用します。  たとえば、次の `curl` コマンドの出力は成功を示します。

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

これで、Azure App Service にアプリがデプロイされました。

## <a name="redeploy-to-azure-app-service-and-use-managed-identities-for-azure-resources"></a>Azure App Service に再デプロイして Azure リソースのマネージド ID を使用する

このセクションでは、アプリの Azure リソースに ID を関連付ける方法について説明します。  これは、Azure がセキュリティを適用し、アクセスを追跡できるようにするために必要です。  使用するリソースに対してのみ料金を支払うことは、クラウド コンピューティングの基本原則です。  このような細かいリソースの追跡は、すべてのリソースが ID に関連付けられている場合にのみ可能です。  Azure App Service と Azure Key Vault は、Azure リソースのマネージド ID を利用する多くの Azure サービスのうちの 2 つです。  この重要なテクノロジの詳細については、「[Azure リソースのマネージド ID とは](/azure/active-directory/managed-identities-azure-resources/overview)」を参照してください。

> [!NOTE]
> Azure リソースのマネージド ID は、以前のマネージドサービス ID (MSI) の新しい名前です。

次の手順に従って、Azure App Service アプリのマネージド ID を作成し、その ID で Key Vault にアクセスできるようにします。

1. App Service アプリのマネージド ID を作成します。  オプションを POM の `<resourceGroup>` と `<appName>` の要素の値に置き換えます。

   ```azurecli
   az webapp identity assign --resource-group contosorg --name contosokeyvault
   ```

   出力は、次のようになります。  次の手順のために `principalId` の値をメモしておきます。

   ```json
   {
     "principalId": "2r7s6r00-92o9-4sq7-n10r-popq294ssr8s",
     "tenantId": "72s988os-86s1-41ns-91no-2q7pq011qo47",
     "type": "SystemAssigned",
     "userAssignedIdentities": null
   }
   ```

1. *application.properties* を編集することによって、前の手順で作成した Azure リソースのマネージド ID に名前を付けます。

   1. `azure.keyvault.client-key` を削除します。
   1. 前の手順の `principalId` の値を設定して `azure.keyvault.client-id` を更新します。  完成したファイルは次のようになります。

   ```bash
   azure.keyvault.client-id=56rqs994-0o66-43o3-9roo-8e3534d0cb23
   azure.keyvault.enabled=true
   azure.keyvault.tenant-id=72s988os-86s1-41ns-91ab-2q7pq011qo47
   azure.keyvault.uri=https://contosokv.vault.azure.net/    
   ```

1. Key Vault がそのマネージド ID からの `get` と `list` 操作を許可するように構成します。  `object-id` の値は、前の出力からの `principalId` です。

   ```azurecli
   az keyvault set-policy --name contosokv \
     --object-id 2r7s6r00-92o9-4sq7-n10r-popq294ssr8s --secret-permissions get list
   ```

   出力は、Key Vault に関する完全な情報を含む JSON オブジェクトになります。  `Microsoft.KeyVault/vaults` の値を持つ `type` エントリがあります。

   次の表では、上記のプロパティについて説明します。

   | パラメーター | 説明 |
   |---|---|
   | name | Key Vault の名前。 | 
   | object-id | 前のコマンドからの `principalId`。 |
   | secret-permissions | 名前付きプリンシパルからの許可する操作の一覧。 |

1. アプリケーションをパッケージ化して再度デプロイします。

   ```bash
   mvn -DskipTests clean package azure-webapp:deploy
   ```

1. さらに、デプロイが完了するまで数分待ちます。  その後、上記のような `curl` コマンドを使用してデプロイにアクセスできますが、今回は `BUILD SUCCESS` 出力に示されているホスト名を使用します。  たとえば、次の `curl` コマンドの出力は成功を示します。

   ```bash
   curl https://contosokeyvault.azurewebsites.net/get
   jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;
   ```

`defaultValue` を返す代わりに、`connectionString` の値が検索され、Key Vault から返されます。  次のセクションでは、同じアプリを Azure Spring Cloud にデプロイします。

## <a name="deploy-to-azure-spring-cloud"></a>Azure Spring Cloud にデプロイする

Azure Spring Cloud は、Azure で Spring Boot アプリケーションをデプロイして実行するためのフル マネージド プラットフォームです。  Azure Spring Cloud の概要については、「[Azure Spring Cloud とは](/azure/spring-cloud/spring-cloud-overview)」を参照してください。

このセクションでは、既存の Spring Boot アプリと以前に作成された Key Vault を、Azure Spring Cloud の新しいインスタンスとともに使用します。

次の手順では、Azure Spring Cloud リソースを作成し、それにアプリをデプロイする方法を示します。  [前提条件](#prerequisites)に示されているように、Azure Spring Cloud の Azure CLI 拡張機能がインストールされていることを確認します。

1. サービス インスタンスの名前を決定します。  Azure サブスクリプション内で Azure Spring Cloud を使用するには、Azure Spring Cloud の種類の Azure リソースを作成する必要があります。  他のすべての Azure リソースと同様に、サービス インスタンスはリソース グループ内に維持されている必要があります。  作成済みのリソース グループを使用して、サービス インスタンスを保持します。  次のコマンドでサービス インスタンスを作成します。 

   ```bash
   az spring-cloud create --resource-group "contosorg" --name "contososvc" 
   ```

   このコマンドは、完了するまでに数分かかります。

1. サービス内に Spring Cloud アプリを作成します。

   ```bash
   az spring-cloud app create --resource-group "contosorg" --name "contosoascsapp" --assign-identity --is-public true
     --runtime-version Java_11 --service "contososvc"
   ```

   次の表では、上記のオプションについて説明します。

   | パラメーター | 説明 |
   |---|---|
   | assign-identity | サービスによって、Azure リソースのマネージド ID 用の ID が作成されます。 |
   | is-public | パブリック DNS ドメイン名をサービスに割り当てます。 |
   | name | アプリの名前。 |
   | resource-group | 既存のサービス インスタンスを作成したリソース グループの名前。 |
   | runtime-version | Java ランタイム バージョン。  **値は、上記の Spring Initializr で選択した値と一致する必要があります。** |
   | サービス (service) | 既存のサービスの名前です。 |

   "*サービス*" と "*アプリ*" の違いを理解するには、「[Azure Spring Cloud でのアプリとデプロイについて](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment)」を参照してください。

1. Azure リソースのマネージド ID を取得します。  これを使用して、既存の Key Vault がこのアプリからのアクセスを許可するように構成します。

   ```bash
   SERVICE_IDENTITY=$(az spring-cloud app show --resource-group "contosorg" --name "contosoascsapp" --service "contososvc" | jq -r '.identity.principalId')
   az keyvault set-policy --name "contosokv" --object-id <the value of the environment variable SERVICE_IDENTITY> --secret-permissions set get list
   ```

1. 既存の Spring Boot アプリには、必要な構成を行った *application.properties* ファイルが既に存在するため、次のコマンドを使用して、Spring Cloud にこのアプリを直接デプロイできます。  POM が格納されているディレクトリでコマンドを実行します。

   ```bash
   az spring-cloud app deploy --resource-group "contosorg" --name "contosoascsapp" --jar-path target/keyvault-0.0.1-SNAPSHOT.jar \
     --service "contososvc"
   ```

   このコマンドは、サービス内のアプリ内に "*デプロイ*" を作成します。  サービス インスタンス、アプリ、デプロイの概念の詳細については、「[Azure Spring Cloud でのアプリとデプロイについて](/azure/spring-cloud/spring-cloud-concept-understand-app-and-deployment)」を参照してください。

   デプロイが正常に完了しない場合は、「[アプリケーション ログの構成](https://aka.ms/azure-spring-cloud-configure-logs)」の説明に従って、トラブルシューティング用のログを構成します。  ログには、問題の診断と解決に役立つ情報が含まれている可能性があります。

1. アプリが正常にデプロイされたら、`curl` を使用して Key Vault 統合が機能していることを確認できます。  `--is-public` を指定したため、サービスの既定の URL は `https://<service name>-<app name>.azuremicroservices.io/` です。  適切な値を置き換え、`@GetMapping` 注釈の値を追加すると、この `curl` のコマンドになります。

   ```bash
   curl https://contososvc-contosoascsapp.azuremicroservices.io/get
   ```

   出力には `jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE` が表示されます。

## <a name="summary"></a>まとめ

**Spring Initializr** を使用して新しい Java Web アプリケーションを作成しました。  次に、機密情報を格納する Azure Key Vault を作成し、Key Vault から情報を取得するようにアプリケーションを構成しました。  ローカルでテストした後、Azure App Service と Azure Spring Cloud にアプリをデプロイしました。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

予想外の課金を防ぐために、この記事で作成したリソースが不要になったら、[Azure portal](https://portal.azure.com/) を使用して削除してください。

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターの Spring に関する次のトピックに進んでください。

> [!div class="nextstepaction"]
> [Azure Service Bus JMS 用の Spring Boot スターターの使用方法](configure-spring-boot-starter-java-app-with-azure-service-bus.md)
