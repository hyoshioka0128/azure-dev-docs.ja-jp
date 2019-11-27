---
title: Azure Key Vault 用の Spring Boot Starter の使用方法
description: Azure Key Vault スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: key-vault
documentationcenter: java
author: bmitchell287
ms.author: brendm
ms.date: 10/29/2019
ms.devlang: java
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 7841386ba89f2f14e4ef6e5c279d62293940f4af
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118023"
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

1. [https://www.microsoft.com](<https://start.spring.io/>) を参照します。

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

2. アプリケーション登録から Azure サービス プリンシパルを作成します。次に例を示します。
   ```shell
   az ad sp create-for-rbac --name "vgeduser"
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | Azure サービス プリンシパルの名前を指定します。 |

   Azure CLI から JSON の状態メッセージが返されます。このメッセージに含まれている *appId* と *password* は、後でクライアント ID およびクライアント パスワードとして使用します。次に例を示します。

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "vgeduser",
     "name": "http://vgeduser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. リソース グループに新しいキー コンテナーを作成します。次に例を示します。

   ```azurecli
   az keyvault create --name vgedkeyvault --resource-group vged-rg2 --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```

   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | キー コンテナーの一意の名前を指定します。 |
   | `location` | リソース グループをホストする [Azure リージョン](https://azure.microsoft.com/regions/)を指定します。 |
   | `enabled-for-deployment` | [キー コンテナーのデプロイ オプション](/cli/azure/keyvault)を指定します。 |
   | `enabled-for-disk-encryption` | [キー コンテナーの暗号化オプション](/cli/azure/keyvault)を指定します。 |
   | `enabled-for-template-deployment` | [キー コンテナーの暗号化オプション](/cli/azure/keyvault)を指定します。 |
   | `sku` | [キー コンテナーの SKU オプション](/cli/azure/keyvault)を指定します。 |
   | `query` | 応答から取得する値を指定します。これは、このチュートリアルを完了するために必要なキー コンテナーの URI です。 |

   Azure CLI にキー コンテナーの URI が表示されます。この URI は後で使用します。次に例を示します。  

   ```azurecli
   "https://vgedkeyvault.vault.azure.net"

   ```

4. 前の手順で作成した Azure サービス プリンシパルのアクセス ポリシーを設定します。次に例を示します。

   ```azurecli
   az keyvault set-policy --name vgedkeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```

   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `name` | 前の手順で作成したキー コンテナーの名前を指定します。 |
   | `secret-permission` | キー コンテナーの[セキュリティ ポリシー](/cli/azure/keyvault)を指定します。 |
   | `spn` | アプリケーションの登録時に取得した GUID を指定します。 |

   Azure CLI にセキュリティ ポリシー作成の結果が表示されます。次に例を示します。  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "vgedkeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "vged-rg2",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

5. 新しいキー コンテナーにシークレットを格納します。次に例を示します。

   ```azurecli
   az keyvault secret set --vault-name "vgedkeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
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
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```

   各値の説明:

   |          パラメーター          |                                 説明                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           キー コンテナーの作成時に取得した URI を指定します。           |
   | `azure.keyvault.client-id`  |  サービス プリンシパルの作成時に取得した *appId* GUID を指定します。   |
   | `azure.keyvault.client-key` | サービス プリンシパルの作成時に取得した *password* GUID を指定します。 |


4. プロジェクトのメイン ソース コード ファイルに移動します (例: */src/main/java/com/vged/secrets*)。

5. テキスト エディターでアプリケーションのメイン Java ファイルを開き(例: *SecretsApplication.java*)、ファイルに次の行を追加します。

   ```java
   package com.vged.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   このコード例では、キー コンテナーから接続文字列を取得し、コマンド ラインに表示します。

6. Java ファイルを保存して閉じます。

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

次の手順に従って、アプリケーションをテストします。

1. Spring Boot アプリの *pom.xml* ファイルがあるディレクトリに移動します。

1. Spring Boot アプリケーションを Maven でビルドします。次に例を示します。

   ```bash
   mvn clean package
   ```

   Maven にビルドの結果が表示されます。

   ![Spring Boot アプリケーションのビルドの状態][build-application-01]

1. Maven で Spring Boot アプリケーションを実行します。アプリケーションによって、キー コンテナーから取得した接続文字列が表示されます。 For example:

   ```bash
   mvn spring-boot:run
   ```

   ![Spring Boot の実行時メッセージ][build-application-02]

## <a name="summary"></a>まとめ

このチュートリアルでは、 **[Spring Initializr]** を使用して新しい Java Web アプリケーションを作成しました。次に、機密情報を格納する Azure Key Vault を作成し、キー コンテナーから情報を取得するようにアプリケーションを構成しました。

## <a name="next-steps"></a>次の手順

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure Key Vault の使用方法の詳細については、次の記事をご覧ください。

* [Key Vault のドキュメント]

* [Azure Key Vault の概要]

Azure での Spring Boot アプリケーションの使用の詳細については、次の記事を参照してください。

* [Spring Boot アプリケーションを Azure App Service にデプロイする](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service (Azure Container Service での Kubernetes クラスター上の Spring Boot アプリケーションの実行)](deploy-spring-boot-java-app-on-kubernetes.md)

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

<!-- URL List -->

[Key Vault のドキュメント]: /azure/key-vault/
[Azure Key Vault の概要]: /azure/key-vault/key-vault-get-started
[Java 開発者向けの Azure]: /azure/java/
[無料の Azure アカウント]: https://azure.microsoft.com/pricing/free-trial/
[Azure DevOps と Java の操作]: /azure/devops/
[MSDN サブスクライバーの特典]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
