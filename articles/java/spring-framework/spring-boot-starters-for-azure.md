---
title: Azure 向けの Spring Boot Starter
description: この記事では、Azure で利用できる各種 Spring Boot Starter について説明します。
documentationcenter: java
ms.date: 12/19/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 48e1381761311a4ed9b5bd6f764af6f19f008fc1
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81670018"
---
# <a name="spring-boot-starters-for-azure"></a>Azure 向けの Spring Boot Starter

この記事では、Java 開発者に Microsoft Azure で作業するための統合機能を提供する、[Spring Initializr] の各種 Spring Boot Starter について説明します。

![Azure の Spring Boot Starter][spring-boot-starters]

Azure では、次の Spring Boot Starter を現在利用できます。

* **[Azure Support](#azure-support)**

   Azure サービス (Service Bus、Storage、Active Directory など) の自動構成サポートを提供します。

* **[Azure Active Directory](#azure-active-directory)**

   認証用に Azure Active Directory と Spring Security の統合サポートを提供します。

* **[Azure Key Vault](#azure-key-vault)**

   Azure Key Vault のシークレットと統合するために、Spring の値アノテーション サポートを提供します。

* **[Azure Storage](#azure-storage)**

   Azure Storage サービスの Spring Boot サポートを提供します。

<a name="azure-support"></a>
## <a name="azure-support"></a>Azure Support

この Spring Boot Starter は、Azure サービス (Service Bus、Storage、Active Directory、Cosmos DB、Key Vault など) の自動構成サポートを提供します。

このスターターが提供するさまざまな Azure 機能の使用方法の例については、次のページを参照してください。

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples>

このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。

* `<properties>` 要素に次のプロパティが追加されます。

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* `spring-boot-starter` の既定の依存関係が次のように置き換えられます。

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-spring-boot</artifactId>
   </dependency>
   ```

* ファイルに次のセクションが追加されます。

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-active-directory"></a>
## <a name="azure-active-directory"></a>Azure Active Directory

この Spring Boot Starter は、認証用に Azure Active Directory と統合するために、Spring Security の自動構成サポートを提供します。

このスターターが提供する Azure Active Directory 機能の使用方法の例については、次のページを参照してください。

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-sample>

このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。

* `<properties>` 要素に次のプロパティが追加されます。

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* `spring-boot-starter` の既定の依存関係が次のように置き換えられます。

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-active-directory-spring-boot-starter</artifactId>
   </dependency>
   ```

* ファイルに次のセクションが追加されます。

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-key-vault"></a>
## <a name="azure-key-vault"></a>Azure Key Vault

この Spring Boot Starter は、Azure Key Vault のシークレットと統合するために、Spring の値アノテーション サポートを提供します。

このスターターが提供する Azure Key Vault 機能の使用方法の例については、次のページを参照してください。

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-keyvault-secrets-spring-boot-sample>

このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。

* `<properties>` 要素に次のプロパティが追加されます。

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* `spring-boot-starter` の既定の依存関係が次のように置き換えられます。

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-keyvault-secrets-spring-boot-starter</artifactId>
   </dependency>
   ```

* ファイルに次のセクションが追加されます。

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

<a name="azure-storage"></a>
## <a name="azure-storage"></a>Azure Storage

この Spring Boot Starter は、Azure Storage サービスの Spring Boot 統合サポートを提供します。

このスターターが提供する Azure Storage 機能の使用方法の例については、次のページをご覧ください。

* [Azure Storage 用の Spring Boot Starter の使用方法](configure-spring-boot-starter-java-app-with-azure-storage.md)

* <https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-storage-spring-boot-sample>

このスターターを Spring Boot プロジェクトに追加すると、*pom.xml* ファイルが次のように変更されます。

* `<properties>` 要素に次のプロパティが追加されます。

   ```xml
   <properties>
      <!-- Other properties will be listed here -->
      <azure.version>0.2.0</azure.version>
   </properties>
   ```

* `spring-boot-starter` の既定の依存関係が次のように置き換えられます。

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-storage-spring-boot-starter</artifactId>
   </dependency>
   ```

* ファイルに次のセクションが追加されます。

   ```xml
   <dependencyManagement>
      <dependencies>
         <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-spring-boot-bom</artifactId>
            <version>${azure.version}</version>
            <type>pom</type>
            <scope>import</scope>
         </dependency>
      </dependencies>
   </dependencyManagement>
   ```

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>その他のリソース

Azure での [Spring Boot] アプリケーションの使用の詳細については、「[Azure の Spring]」をご覧ください。

Java での Azure の使用の詳細については、「[Java 開発者向けの Azure]」および「[Azure DevOps と Java の操作]」を参照してください。

独自の Spring Boot アプリケーションの使用開始に関するヘルプについては、「**Spring Initializr**」(https://start.spring.io/ ) を参照してください。

<!-- URL List -->

[Java 開発者向けの Azure]: /azure/developer/java/
[Azure DevOps と Java の操作]: /azure/devops/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Azure の Spring]: /azure/developer/java/spring-framework/
[Spring Framework]: https://spring.io/
[Spring Initializr]: https://start.spring.io/

<!-- IMG List -->

[spring-boot-starters]: media/spring-boot-starters-for-azure/spring-boot-starters-cropped.png
