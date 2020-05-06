---
title: WebLogic アプリケーションを Azure Kubernetes Service 上の WildFly に移行する
description: このガイドでは、既存の WebLogic アプリケーションを移行して Azure Kubernetes Service コンテナーの WildFly 上で実行する場合に知っておくべきことについて説明します。
author: mriem
ms.author: manriem
ms.topic: conceptual
ms.date: 2/28/2020
ms.openlocfilehash: d17551aeb1041415e2c5b6d5fd8a43d3b7b670aa
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673388"
---
# <a name="migrate-weblogic-applications-to-wildfly-on-azure-kubernetes-service"></a>WebLogic アプリケーションを Azure Kubernetes Service 上の WildFly に移行する

このガイドでは、既存の WebLogic アプリケーションを移行して Azure Kubernetes Service コンテナーの WildFly 上で実行する場合に知っておくべきことについて説明します。

## <a name="before-you-start"></a>開始する前に

移行前の要件を満たすことができない場合は、関連する移行ガイドを参照してください。

* [WebLogic アプリケーションを Azure Virtual Machines に移行する](migrate-weblogic-to-virtual-machines.md)

## <a name="pre-migration"></a>移行前

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

[!INCLUDE [inventory-all-secrets](includes/inventory-all-secrets.md)]

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [inventory-jndi-resources](includes/inventory-jndi-resources.md)]

### <a name="determine-whether-session-replication-is-used"></a>セッション レプリケーションが使用されているかどうか確認する

Oracle の Coherence*Web の有無にかかわらず、アプリケーションがセッション レプリケーションに依存している場合は、次の 2 つの選択肢があります。

* セッション管理にデータベースを使用するようにアプリケーションをリファクターします。
* Azure Redis サービスへのセッションを外部化するようにアプリケーションをリファクターします。 詳細については、「[Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview)」を参照してください。

[!INCLUDE [document-datasources](includes/document-datasources.md)]

[!INCLUDE [determine-whether-weblogic-has-been-customized](includes/determine-whether-weblogic-has-been-customized.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

[!INCLUDE [determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries](includes/determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries.md)]

[!INCLUDE [determine-whether-osgi-bundles-are-used](includes/determine-whether-osgi-bundles-are-used.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-oracle-service-bus-is-in-use](includes/determine-whether-oracle-service-bus-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

[!INCLUDE [determine-whether-your-application-is-packaged-as-an-ear](includes/determine-whether-your-application-is-packaged-as-an-ear.md)]

<!-- AKS-specific extension of the last INCLUDE. -->
> [!NOTE]
> AKS リソースの使用を改善するために各 Web アプリケーションを個別に拡張できるようにする場合は、個々の Web アプリケーションに EAR を分割する必要があります。
<!-- end extension -->

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

### <a name="validate-that-the-supported-java-version-works-correctly"></a>サポートされている Java バージョンが正しく動作することを検証する

Azure Kubernetes Service で WildFly を使用するには、特定のバージョンの Java が必要です。 したがって、そのサポートされているバージョンを使用してアプリケーションを正常に実行できることを検証する必要があります。 現在のサーバーでサポートされていない JDK (Oracle JDK や IBM OpenJ9 など) を使用している場合、この検証が特に重要です。

現在のバージョンを取得するには、実稼働サーバーにサインインし、次のコマンドを実行します。

```bash
java -version
```

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

### <a name="determine-whether-weblogic-scripting-tool-wlst-is-used"></a>WebLogic Scripting Tool (WLST) を使用しているかどうか確認する

現在、WLST を使用してデプロイを実行している場合は、実行内容を評価する必要があります。 WLST によってデプロイの一部としてアプリケーションの (ランタイム) パラメーターが変更される場合は、これらのパラメーターが次のいずれかのオプションに準拠していることを確認する必要があります。

* アプリ設定として外部化される。
* アプリケーションに埋め込まれている。
* デプロイ中に JBoss CLI を使用する。

WLST により上記の説明よりも多くの作業が行われている場合は、移行中に追加の作業が必要になります。

### <a name="determine-whether-your-application-uses-weblogic-specific-apis"></a>アプリケーションで WebLogic 固有の API が使用されているかどうかを判断する

アプリケーションで WebLogic 固有の API が使用される場合、それらの依存関係を削除するためにそれをリファクタリングする必要があります。 たとえば、[Oracle WebLogic Server 向け Java API リファレンス](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlapi/index.html?overview-summary.html)に記載されているクラスを使用している場合は、アプリケーションで WebLogic 固有の API を使用しています。

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

### <a name="determine-whether-a-deployment-plan-was-used"></a>デプロイ計画が使用されたかどうかを判断する

アプリがデプロイ計画を使用してデプロイされていた場合は、デプロイ計画で何が行われるかを評価する必要があります。 デプロイ計画が単純なデプロイである場合は、変更を加えなくても Web アプリケーションをデプロイできます。 デプロイ計画がより複雑な場合は、JBoss CLI を使用してアプリケーションをデプロイの一部として適切に構成できるかどうかを判断する必要があります。 JBoss CLI を使用できない場合は、デプロイ計画が不要になったような方法でアプリケーションをリファクタリングする必要があります。

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="validate-whether-and-how-the-file-system-is-used"></a>ファイル システムが使用されているかどうかとその使用方法を検証する

アプリケーション サーバーでファイル システムを使用する場合は、再構成や、まれにアーキテクチャの変更が必要になります。 ファイル システムは、WebLogic 共有モジュールまたはアプリケーション コードによって使用される場合があります。 次のセクションに記載された一部または全部のシナリオを確認できます。

#### <a name="read-only-static-content"></a>読み取り専用の静的コンテンツ

現在、アプリケーションで静的コンテンツを提供している場合は、そのための別の場所が必要になります。 静的コンテンツを Azure Blob Storage に移動し、グローバルな高速ダウンロードのために Azure CDN を追加することを検討できます。 詳細については、「[Azure Storage での静的 Web サイト ホスティング](/azure/storage/blobs/storage-blob-static-website)」と[「クイック スタート:Azure ストレージ アカウントと Azure CDN との統合](/azure/cdn/cdn-create-a-storage-account-with-cdn)」を参照してください。

#### <a name="dynamically-published-static-content"></a>動的に公開される静的コンテンツ

アプリケーションによってアップロードまたは生成されるが、作成後に変更できない静的コンテンツをアプリケーションで許可する場合は、前述のように Azure Blob Storage と Azure CDN を使用し、Azure Function でアップロードと CDN の更新を処理します。 「[Azure Functions を使用した静的コンテンツのアップロードと CDN の事前読み込み](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)」で、ご利用いただけるサンプルの実装を提供しています。

#### <a name="dynamic-or-internal-content"></a>動的または内部のコンテンツ

アプリケーションで頻繁に書き込みおよび読み取りされるファイル (一時データ ファイルなど) や、アプリケーションでのみ表示できる静的ファイルには、Azure Storage 共有を永続ボリュームとしてマウントできます。 詳細については、「[Azure Kubernetes Service で Azure Files を含む永続ボリュームを動的に作成して使用する](/azure/aks/azure-files-dynamic-pv)」を参照してください。

### <a name="determine-whether-jca-connectors-are-used"></a>JCA コネクタが使用されているかどうかを判断する

アプリケーションで JCA コネクタを使用する場合は、WildFly 上で JCA コネクタを使用できることを確認する必要があります。 JCA の実装が WebLogic に関連付けられている場合は、その依存関係を削除するようにアプリケーションをリファクタリングする必要があります。 コネクタを使用できる場合は、JAR をサーバーのクラスパスに追加し、必要な構成ファイルを WildFly サーバー ディレクトリ内の適切な場所に配置して使用できるようにする必要があります。

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

### <a name="determine-whether-weblogic-clustering-is-used"></a>WebLogic クラスタリングが使用されているかどうか確認する

多くの場合、高可用性を実現するために、アプリケーションは複数の WebLogic サーバーにデプロイされています。 Azure Kubernetes Service はスケーリングが可能ですが、WebLogic Cluster API を使用している場合は、コードをリファクタリングしてその API を使用しないようにする必要があります。

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

アプリケーションを Azure Kubernetes Service に移行したので、期待どおりに動作することを確認する必要があります。 これを完了したら、アプリケーションをよりクラウドネイティブにするための次の推奨事項をご覧ください。

[!INCLUDE [recommendations-wildfly-on-aks](includes/recommendations-wildfly-on-aks.md)]
