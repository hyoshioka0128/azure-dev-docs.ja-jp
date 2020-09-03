---
title: Spring Boot アプリケーションを Azure Spring Cloud に移行する
description: このガイドでは、既存の Spring Boot アプリケーションを移行して Azure Spring Cloud で実行する場合に注意する必要がある点について説明します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 5/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: 4d2f84c6c77294c9a2d25028608e2feb712599f8
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062059"
---
# <a name="migrate-spring-boot-applications-to-azure-spring-cloud"></a>Spring Boot アプリケーションを Azure Spring Cloud に移行する

このガイドでは、既存の Spring Boot アプリケーションを移行して Azure Spring Cloud で実行する場合に注意する必要がある点について説明します。

## <a name="pre-migration"></a>移行前

移行を確実に成功させるには、開始する前に、次のセクションで説明する評価およびインベントリの手順を完了します。

以下の移行前の要件のいずれかを満たすことができない場合は、次の関連する移行ガイドを参照してください。

* Azure Kubernetes Service のコンテナーに実行可能 JAR アプリケーションを移行する (ガイド計画済)
* 実行可能 JAR アプリケーションを Azure Virtual Machines に移行する (ガイド計画済)

### <a name="inspect-application-components"></a>アプリケーション コンポーネントを検査する

[!INCLUDE [identify-local-state](includes/identify-local-state-azure-spring-cloud.md)]

[!INCLUDE [static-content-azure-spring-cloud](includes/determine-whether-and-how-the-file-system-is-used-azure-spring-cloud.md)]

#### <a name="determine-whether-any-of-the-services-contain-os-specific-code"></a>いずれかのサービスに OS 固有のコードが含まれているかどうかを判断する

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [switch-to-a-supported-platform-azure-spring-cloud](includes/switch-to-a-supported-platform-azure-spring-cloud.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

Spring Boot 1.x を使用しているアプリケーションの場合は、「[Spring Boot 2.0 の移行ガイド](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)」に従って、サポートされている Spring Boot バージョンに更新します。 サポートされているバージョンについては、「[Java Spring アプリをデプロイ用に準備する](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions)」を参照してください。

[!INCLUDE [identify-logs-metrics-apm-azure-spring-cloud.md](includes/identify-logs-metrics-apm-azure-spring-cloud.md)]

### <a name="inventory-external-resources"></a>外部リソースをインベントリする

データ ソース、JMS メッセージ ブローカー、その他のサービスの URL などの外部リソースを特定します。 Spring Boot アプリケーションでは、一般にこのようなリソースの構成は、*src/main/directory* フォルダー内にある通常 *application.properties* または *application.yml* と呼ばれるファイル内にあります。

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

使用中のブローカーを特定したら、対応する設定を見つけます。 Spring Boot アプリケーションでは、これらは通常、アプリケーション ディレクトリの *application.properties* および *application.yml* ファイル内にあります。

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

[!INCLUDE [inventory-identity-providers-spring-boot.md](includes/inventory-identity-providers-spring-boot.md)]

#### <a name="identify-any-clients-relying-on-a-non-standard-port"></a>非標準ポートに依存しているすべてのクライアントを特定する

Azure Spring Cloud によって、デプロイされたアプリケーションの `server.port` 設定が上書きされます。 443 以外のポートで利用できるアプリケーションにいずれかのクライアントが依存している場合、それらを変更する必要があります。

#### <a name="all-other-external-resources"></a>その他のすべての外部リソース

このガイドに、考えられるすべての外部依存関係を記載することはできません。 移行後に、アプリケーションの外部依存関係がすべて満たされることを確認するのは、お客様の責任です。

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-boot](includes/inventory-configuration-sources-and-secrets-spring-boot.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-boot](includes/inspect-the-deployment-architecture-spring-boot.md)]

## <a name="migration"></a>移行

[!INCLUDE [migrate-steps-spring-boot-azure-spring-cloud](includes/migrate-steps-spring-boot-azure-spring-cloud.md)]

## <a name="post-migration"></a>移行後

[!INCLUDE [post-migration-spring-boot-azure-spring-cloud](includes/post-migration-spring-boot-azure-spring-cloud.md)]
