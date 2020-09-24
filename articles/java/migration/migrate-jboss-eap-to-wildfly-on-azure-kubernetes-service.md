---
title: JBoss EAP アプリケーションを Azure Kubernetes Service 上の WildFly に移行する
description: このガイドでは、既存の JBoss EAP アプリケーションを移行して Azure Kubernetes Service コンテナーの WildFly 上で実行する場合に知っておくべきことについて説明します。
author: mnriem
ms.author: manriem
ms.topic: conceptual
ms.date: 3/16/2020
ms.custom: devx-track-java
ms.openlocfilehash: 5de4032f4413f4c72bf51990723c47874bc39ab2
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738570"
---
# <a name="migrate-jboss-eap-applications-to-wildfly-on-azure-kubernetes-service"></a>JBoss EAP アプリケーションを Azure Kubernetes Service 上の WildFly に移行する

このガイドでは、既存の JBoss EAP アプリケーションを移行して Azure Kubernetes Service コンテナーの WildFly 上で実行する場合に知っておくべきことについて説明します。

## <a name="pre-migration"></a>移行前

移行を確実に成功させるには、開始する前に、次のセクションで説明する評価とインベントリの手順を完了します。

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

### <a name="inventory-all-secrets"></a>すべてのシークレットをインベントリする

すべてのシークレットとパスワードについて、実稼働サーバー上のすべてのプロパティと構成ファイルを確認します。 必ず、WAR 内の *jboss-web.xml* を確認してください。 また、パスワードや資格情報を含む構成ファイルがアプリケーション内に見つかる場合もあります。

これらのシークレットを Azure Key Vault に格納することを検討してください。 詳細については、「[Azure Key Vault の基本的な概念](/azure/key-vault/basic-concepts)」を参照してください。

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly-wildfly](includes/validate-that-the-supported-java-version-works-correctly-wildfly.md)]

### <a name="inventory-jndi-resources"></a>JNDI リソースをインベントリする

すべての JNDI リソースをインベントリします。 JMS メッセージ ブローカーなどでは、移行または再構成が必要になる場合があります。

### <a name="determine-whether-session-replication-is-used"></a>セッション レプリケーションが使用されているかどうか確認する

アプリケーションがセッション レプリケーションに依存している場合は、アプリケーションを変更してこの依存関係を削除する必要があります。

#### <a name="inside-your-application"></a>アプリケーション内

*WEB-INF/jboss-web.xml* または *WEB-INF/web.xml*、あるいはその両方のファイルを調べます。

### <a name="document-datasources"></a>データソースの文書化

アプリケーションでデータベースを使用する場合は、次の情報を把握する必要があります。

* データソース名
* 接続プールの構成
* JDBC ドライバーの JAR ファイルの場所

詳細については、JBoss EAP のドキュメントの「[JBoss EAP データソース](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.3/html/configuration_guide/datasource_management)」を参照してください。

### <a name="determine-whether-and-how-the-file-system-is-used"></a>ファイル システムが使用されているかどうかとその使用方法を判断する

アプリケーション サーバーでファイル システムを使用する場合は、再構成や、まれにアーキテクチャの変更が必要になります。 ファイル システムは、JBoss EAP モジュールまたはアプリケーション コードによって使用される場合があります。 次のセクションに記載された一部または全部のシナリオを確認できます。

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

### <a name="determine-whether-your-application-uses-jboss-eap-specific-apis"></a>アプリケーションで JBoss-EAP 固有の API が使用されているかどうかを判断する

アプリケーションで JBoss-EAP 固有の API が使用される場合、それらの依存関係を削除するためにそれをリファクタリングする必要があります。

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="determine-whether-jca-connectors-are-in-use"></a>JCA コネクタが使用されているかどうかを判断する

アプリケーションで JCA コネクタを使用する場合は、WildFly で JCA コネクタを使用できることを確認する必要があります。 JCA の実装が JBoss EAP に関連付けられている場合は、その依存関係を削除するようにアプリケーションをリファクタリングする必要があります。 WildFly で JCA コネクタを使用できる場合は、JAR をサーバーのクラスパスに追加し、必要な構成ファイルを WildFly サーバー ディレクトリ内の適切な場所に配置して使用できるようにする必要があります。

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

### <a name="determine-whether-your-application-is-packaged-as-an-ear"></a>アプリケーションが EAR としてパッケージ化されているかどうか確認する

アプリケーションが EAR ファイルとしてパッケージ化されている場合は、必ず *application.xml* ファイルを調べて、構成を把握してください。

> [!NOTE]
> AKS リソースの使用を改善するために各 Web アプリケーションを個別に拡張できるようにする場合は、個々の Web アプリケーションに EAR を分割する必要があります。

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [perform-in-place-testing](includes/perform-in-place-testing.md)]

## <a name="migration"></a>移行

[!INCLUDE [provision-azure-container-registry-and-azure-kubernetes-service](includes/provision-azure-container-registry-and-azure-kubernetes-service.md)]

[!INCLUDE [create-a-docker-image-for-wildfly](includes/create-a-docker-image-for-wildfly.md)]

[!INCLUDE [build-and-push-the-docker-image-to-azure-container-registry](includes/build-and-push-the-docker-image-to-azure-container-registry.md)]

[!INCLUDE [provision-a-public-ip-address](includes/provision-a-public-ip-address.md)]

[!INCLUDE [deploy-to-aks](includes/deploy-to-aks.md)]

### <a name="configure-persistent-storage"></a>永続ストレージを構成する

アプリケーションで非揮発性ストレージが必要な場合は、[永続ボリューム](/azure/aks/azure-disks-dynamic-pv) を 1 つ以上構成します。

[!INCLUDE [migrate-scheduled-jobs-aks](includes/migrate-scheduled-jobs-aks.md)]

## <a name="post-migration"></a>移行後

アプリケーションを Azure Kubernetes Service に移行したので、期待どおりに動作することを確認する必要があります。 これを完了したら、アプリケーションをよりクラウドネイティブにするための推奨事項がいくつかあります。

[!INCLUDE [recommendations-wildfly-on-aks](includes/recommendations-wildfly-on-aks.md)]
