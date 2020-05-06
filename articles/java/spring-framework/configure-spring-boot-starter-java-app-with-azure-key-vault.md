---
title: Azure Key Vault 用の Spring Boot Starter の使用方法
description: Azure Key Vault スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: key-vault
documentationcenter: java
ms.date: 10/29/2019
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 2df574104376ec1900c7dc5cbd4f0a49ef1f4732
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82138738"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>Azure Key Vault 用の Spring Boot Starter の使用方法

この記事では、Azure Key Vault 用 Spring Boot Starter を使用した **[Spring Initializr]** を用いて、キー コンテナーにシークレットとして格納されている接続文字列を取得するアプリを作成する方法について説明します。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* Azure サブスクリプション。Azure サブスクリプションをまだお持ちでない場合は、[MSDN サブスクライバーの特典]を有効にするか、または[無料の Azure アカウント]にサインアップできます。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-an-app-using-spring-initializr"></a>Spring Initializr を使用したアプリの作成

次の手順では、Spring Initializr を使用してアプリケーションを作成します。

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定します。  

1. アプリケーションの **[Group]\(グループ\)** と **[Artifact]\(成果物\)** に名前を入力します。

1. **[Dependencies]\(依存関係\)** セクションで、「**Azure Key Vault**」と入力します。

1. ページの下部までスクロールし、 **[Generate]\(生成\)** をクリックします。

   ![Spring Boot プロジェクトを生成する][secrets-01]

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

## <a name="sign-into-azure"></a>Azure へのサインイン

次の手順では、Azure CLI でユーザーを認証します。

1. コマンド プロンプトを開きます。

1. Azure CLI を使って、Azure アカウントにサインインします。

   ```azurecli
   az login
   ```

指示に従って、サインインを完了します。

1. サブスクリプションを一覧表示します。

   ```azurecli
   az account list
   ```

   Azure からサブスクリプションの一覧が返されます。使用するサブスクリプションの GUID をコピーする必要があります。次に例を示します。

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
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

1. Azure で使用するアカウントの GUID を指定します。例を次に示します。

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-a-new-azure-key-vault"></a>新しい Azure Key Vault の作成

次の手順では、キー コンテナーを作成して初期化します。

1. キー コンテナーに使用する Azure リソースのリソース グループを作成します。次に例を示します。

   ```azurecli
   az group create --name vged-rg2 --location westus
   ```

   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | リソース グループの一意の名前を指定します。 |
   | `location` | リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |

   次のように、Azure CLI に、リソース グループ作成の結果が表示されます。  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/vged-rg2",
     "location": "westus",
     "managedBy": null,
     "name": "vged-rg2",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. リソース グループに新しいキー コンテナーを作成します。次に例を示します。

   ```azurecli
   az keyvault create --resource-group vged-rg2 \
       --name vgedkeyvault \
       --enabled-for-deployment true \
       --enabled-for-disk-encryption true \
       --enabled-for-template-deployment true \
       --sku standard --query properties.vaultUri \
       --location westus
   ```

   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | キー コンテナーの一意の名前を指定します。 |
   | `enabled-for-deployment` | [キー コンテナーのデプロイ オプション](/cli/azure/keyvault)を指定します。 |
   | `enabled-for-disk-encryption` | [キー コンテナーの暗号化オプション](/cli/azure/keyvault)を指定します。 |
   | `enabled-for-template-deployment` | [キー コンテナーの暗号化オプション](/cli/azure/keyvault)を指定します。 |
   | `sku` | [キー コンテナーの SKU オプション](/cli/azure/keyvault)を指定します。 |
   | `query` | 応答から取得する値を指定します。これは、このチュートリアルを完了するために必要なキー コンテナーの URI です。 |
   | `location` | リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |

   Azure CLI にキー コンテナーの URI が表示されます。この URI は後で使用します。次に例を示します。  

   ```output
   "https://vgedkeyvault.vault.azure.net"
   ```

3. 新しいキー コンテナーにシークレットを格納します。次に例を示します。

   ```azurecli
   az keyvault secret set --vault-name "vgedkeyvault" \
       --name "connectionString" \
       --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `vault-name` | 前の手順で作成したキー コンテナーの名前を指定します。 |
   | `name` | シークレットの名前を指定します。 |
   | `value` | シークレットの値を指定します。 |

   Azure CLI にシークレット作成の結果が表示されます。次に例を示します。  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://vgedkeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-app"></a>アプリの構成およびコンパイル

次の手順に従って、アプリケーションを構成およびコンパイルします。

1. 以前にディレクトリにダウンロードした Spring Boot プロジェクトのパッケージ ファイルからファイルを抽出します。

2. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。

3. このチュートリアルで既に完了した手順で取得した値を使用して、キー コンテナーの値を追加します。次に例を示します。

   ```yaml
   azure.keyvault.uri=https://vgedkeyvault.vault.azure.net/
   azure.keyvault.enabled=true
   ```

   各値の説明:

   |          パラメーター          |                                 説明                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           キー コンテナーの作成時に取得した URI を指定します。           |
    
    
4. プロジェクトのメイン ソース コード ファイルに移動します (例: */src/main/java/com/vged/secrets*)。

5. テキスト エディターでアプリケーションのメイン Java ファイルを開き(例: *SecretsApplication.java*)、ファイルに次の行を追加します。

   ```java
   package com.vged.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   @SpringBootApplication
   @RestController
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }
   
      @GetMapping("get")
      public String get() {
         return connectionString;
      }
   
      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   このコード例では、キー コンテナーから接続文字列を取得し、URL `https://{your-appservice-name}.azurewebsites.net/get` に表示します。

6. Java ファイルを保存して閉じます。

7. テストを無効にし、Maven を使用して JAR ファイルをビルドします。
    
   ```bash
   mvn clean package -Dmaven.test.skip=true
   ```

## <a name="configure-maven-plugin-for-azure-app-service"></a>Azure App Service 用の Maven プラグインを構成する

このセクションでは、Spring Boot プロジェクトを構成して、Azure App Service にアプリをデプロイできるようにする方法について説明します。

1.  リンクに従って [Azure App Service 用の Maven プラグインを構成する]します。
    
    このリンクにより、新しい Azure App Service が作成されます。 既存のものにアプリをデプロイする場合は、コマンド `mvn azure-webapp:config` でデプロイを再構成し、構成するアプリケーション パーツを選択することができます。
    
    ```bash
    [INFO] Scanning for projects...                                                     
    [INFO]                                                                              
    [INFO] ----------------------< com.wingtiptoys:secrets >-----------------------     
    [INFO] Building secrets 0.0.1-SNAPSHOT                                              
    [INFO] --------------------------------[ jar ]---------------------------------     
    [INFO]                                                                              
    [INFO] --- azure-webapp-maven-plugin:1.9.0:config (default-cli) @ secrets ---       
    Please choose which part to config                                                  
    1. Application                                                                      
    2. Runtime                                                                          
    3. DeploymentSlot                                                                   
    Enter index to use: 1                                                              
    Define value for appName(Default: ********):                                      
    Define value for resourceGroup(Default: ********):                                 
    Define value for region(Default: ********):                                           
    Define value for pricingTier(Default: P1v2):                                        
    1. b1                                                                               
    2. b2                                                                               
    3. b3                                                                               
    4. d1                                                                               
    5. f1                                                                               
    6. p1v2 [*]                                                                         
    7. p2v2                                                                             
    8. p3v2                                                                             
    9. s1                                                                               
    10. s2                                                                              
    11. s3                                                                              
    Enter index to use:                                                                 
    Please confirm webapp properties                                                                                                          
    ```
    
    また、*pom.xml*で `<azure-webapp-maven-plugin>` の `<configuration>` セクションを直接編集することもできます。 `<resourceGroup>`、`<appName>`、および `<region>` の値を特定の App Service に変更します。

2. App Service に ID を割り当て、次の手順のために `principalId` を記録しておきます。

   ```bash
   az webapp identity assign --name your-appservice-name \
      --resource-group vged-rg2
   ```
   
3. MSI にアクセス許可を付与します。

   ```bash
   az keyvault set-policy --name vgedkeyvault \
       --object-id your-managed-identity-objectId \
       --secret-permissions get list
   ```

## <a name="deploy-the-app-to-azure-and-run-app-service"></a>アプリを Azure にデプロイして App Service を実行する

これで、Web アプリを Azure にデプロイする準備ができました。 そのためには、次の手順を実行してください。

1. *pom.xml* ファイルに変更を加えた場合は、Maven を使用して JAR ファイルをリビルドします。

   ```bash
   mvn clean package
   ```
   
2. Maven を使用して Azure にアプリをデプロイします。

   ```bash
   mvn azure-webapp:deploy
   ```
   
3. App Service を再起動します。

4. ブラウザーで URL `https://{your-appservice-name}.azurewebsites.net/get` を確認して `connectionString` を入手します。
   

## <a name="summary"></a>まとめ

このチュートリアルでは、 **[Spring Initializr]** を使用して新しい Java Web アプリケーションを作成しました。次に、機密情報を格納する Azure Key Vault を作成し、キー コンテナーから情報を取得するようにアプリケーションを構成しました。

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure Key Vault の使用方法の詳細については、次の記事をご覧ください。

* [Key Vault のドキュメント]

* [Azure Key Vault の概要]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

マネージド ID を使用した App Service の詳細については、[App Service のマネージド ID を使用する]に関する記事を参照してください。

<!-- URL List -->

[Key Vault のドキュメント]: /azure/key-vault/
[Azure Key Vault の概要]: /azure/key-vault/key-vault-get-started
[Java 開発者向けの Azure]: /azure/developer/java/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[App Service のマネージド ID を使用する]: /azure/app-service/overview-managed-identity
[Azure App Service 用の Maven プラグインを構成する]: /azure/developer/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
