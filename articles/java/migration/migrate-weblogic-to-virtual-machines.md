---
title: WebLogic アプリケーションを Azure Virtual Machines に移行する
description: このガイドでは、既存の WebLogic アプリケーションを移行して Azure Virtual Machines で実行する場合に知っておくべきことについて説明します。
author: edburns
ms.author: edburns
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 10edb96e4e0781945da85d5a872b14178db3122f
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673518"
---
# <a name="migrate-weblogic-applications-to-azure-virtual-machines"></a>WebLogic アプリケーションを Azure Virtual Machines に移行する

このガイドでは、既存の WebLogic アプリケーションを移行して Azure Virtual Machines で実行する場合に知っておくべきことについて説明します。

## <a name="pre-migration"></a>移行前

### <a name="define-what-you-mean-by-migration-complete"></a>"移行の完了" が意味することを定義する

このガイドおよび対応する Azure Marketplace オファーが、WebLogic Server ワークロードの Azure への移行を促進するための出発点となります。 移行作業の範囲を定義することが重要です。 たとえば、既存のインフラストラクチャから Azure Virtual Machines に厳密な "リフト アンド シフト" を行うのでしょうか。 その場合、移行の際に多少の "リフト アンド改良" を取り入れたくなる可能性があります。

このガイドで詳しく説明されている必要な変更を考慮して、できるだけ純粋な "リフ トアンド シフト" に近づけることをお勧めします。 このマイルストーンに到達したことがわかるように、"移行の完了" が意味することを定義してください。 "移行の完了" に到達したら、「[スナップショットの作成](/azure/virtual-machines/windows/snapshot-copy-managed-disk)」の説明に従って、Virtual Machines のスナップショットを取得できます。 スナップショットから正常に復元できることを確認した後は、これまでに達成した移行の進捗を無駄にする心配なしに、より安心して機能強化を行えます。

### <a name="determine-whether-the-pre-built-marketplace-offers-are-a-good-starting-point"></a>事前構築済みの Marketplace オファーが適切な出発点であるかどうか判断する

Oracle と Microsoft は提携して、Azure ソリューション テンプレートのセットを Azure Marketplace に導入し、Azure への移行の確固たる出発点を提供しています。 「[Oracle Fusion Middleware](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/)」ドキュメントでオファーの一覧を参照し、既存のデプロイに最も適合するものを選択してください。 オファーの一覧は、[Oracle のドキュメント](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/select-required-oracle-weblogic-server-offer-azure-marketplace.html#GUID-187739C5-EE7A-47C6-B3BA-C0A0333DC398)に記載されています。

既存のオファーがいずれも適切な開始点でない場合は、Azure 仮想マシンのリソースを使用して手動でデプロイを再現する必要があります。 詳細については、「[IaaS とは](https://azure.microsoft.com/overview/what-is-iaas/)」を参照してください。

### <a name="determine-whether-the-weblogic-version-is-compatible"></a>WebLogic バージョンに互換性があるかどうか確認する

既存の WebLogic バージョンは、IaaS オファーのバージョンと互換性がある必要があります。 このクエリでは、[WebLogic バージョン 12.2.1.3](https://azuremarketplace.microsoft.com/marketplace/apps?search=oracle%20weblogic%2012.2.1.3&page=1) のオファーが表示されます。 既存の WebLogic バージョンがそのバージョンと互換性がない場合は、Azure IaaS リソースを使用して手動でデプロイを再現する必要があります。 詳細については、[Azure のドキュメント](https://azure.microsoft.com/overview/what-is-iaas/)を参照してください。

[!INCLUDE [inventory-server-capacity-virtual-machines](includes/inventory-server-capacity-virtual-machines.md)]

[!INCLUDE [inventory-all-secrets](includes/inventory-all-secrets.md)]

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly](includes/validate-that-the-supported-java-version-works-correctly.md)]

[!INCLUDE [inventory-jndi-resources](includes/inventory-jndi-resources.md)]

[!INCLUDE [domain-configuration](includes/domain-configuration.md)]

[!INCLUDE [determine-whether-session-replication-is-used](includes/determine-whether-session-replication-is-used.md)]

[!INCLUDE [document-datasources](includes/document-datasources.md)]

[!INCLUDE [determine-whether-weblogic-has-been-customized](includes/determine-whether-weblogic-has-been-customized.md)]

[!INCLUDE [determine-whether-management-over-rest-is-used](includes/determine-whether-management-over-rest-is-used.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use-virtual-machines](includes/determine-whether-jms-queues-or-topics-are-in-use-virtual-machines.md)]

[!INCLUDE [determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries](includes/determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries.md)]

[!INCLUDE [determine-whether-osgi-bundles-are-used](includes/determine-whether-osgi-bundles-are-used.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-oracle-service-bus-is-in-use](includes/determine-whether-oracle-service-bus-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

[!INCLUDE [determine-whether-your-application-is-packaged-as-an-ear](includes/determine-whether-your-application-is-packaged-as-an-ear.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [determine-whether-wlst-is-used](includes/determine-whether-wlst-is-used.md)]

[!INCLUDE [validate-whether-and-how-the-file-system-is-used](includes/validate-whether-and-how-the-file-system-is-used.md)]

[!INCLUDE [determine-the-network-topology](includes/determine-the-network-topology.md)]

[!INCLUDE [account-for-the-use-of-jca-adapters-and-resource-adapters](includes/account-for-the-use-of-jca-adapters-and-resource-adapters.md)]

[!INCLUDE [account-for-the-use-of-custom-security-providers-and-jaas](includes/account-for-the-use-of-custom-security-providers-and-jaas.md)]

[!INCLUDE [determine-whether-weblogic-clustering-is-used](includes/determine-whether-weblogic-clustering-is-used.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-used](includes/determine-whether-the-java-ee-application-client-feature-is-used.md)]

## <a name="migration"></a>移行

### <a name="select-a-weblogic-on-azure-virtual-machines-offer"></a>Azure Virtual Machines オファーで WebLogic を選択する

Azure Virtual Machines 上の WebLogic については、次のオファーを利用できます。

オファーのデプロイ中に、WebLogic Server ノードの仮想マシンのサイズを選択するよう求められます。 VM サイズを選択する際、サイズ設定 (メモリ、プロセッサ、ディスク) のすべての側面を考慮することが重要です。 詳細については、[オファーに関するドキュメント](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlazu/deploy-oracle-weblogic-server-administration-server-single-node.html)と、[仮想マシンのサイズ設定に関する Azure ドキュメント](/azure/cloud-services/cloud-services-sizes-specs)を参照してください。

#### <a name="weblogic-server-single-node-with-no-admin-server"></a>管理サーバーのない WebLogic Server 単一ノード

このオファーでは、単一の VM が作成され、それに WebLogic がインストールされますが、ドメインは構成されません。これは、高度にカスタマイズされたドメイン構成を使用する場合に便利です。

#### <a name="weblogic-server-single-node-with-admin-server"></a>管理サーバーがある WebLogic Server 単一ノード

このオファーでは、単一の VM がプロビジョニングされ、それに WebLogic Server 12.1.2.3 がインストールされます。 ドメインが作成され、管理サーバーが起動されます。

#### <a name="weblogic-server-n-node-cluster"></a>WebLogic Server N ノード クラスター

このオファーでは、WebLogic Server VM の高可用性クラスターが作成されます。

#### <a name="weblogic-server-n-node-dynamic-cluster"></a>WebLogic Server N ノード動的クラスター

このオファーでは、可用性が高くてスケーラブルな、WebLogic Server VM の動的クラスターが作成されます

### <a name="provision-the-offer"></a>オファーをプロビジョニングする

開始するオファーを選択したら、[オファーのドキュメント](https://wls-eng.github.io/arm-oraclelinux-wls/)に記載されている手順に従って、そのオファーをプロビジョニングします。 既存のドメイン名と一致するドメイン名を必ず選択してください。 ドメイン パスワードを既存のドメイン パスワードと一致させることもできます。

### <a name="migrate-the-domains"></a>ドメインの移行

オファーをプロビジョニングしたら、ドメインの構成を確認し、ドメインの移行方法の詳細について[こちらのガイダンス](https://support.oracle.com/knowledge/Middleware/2336356_1.html)に従います。

### <a name="connect-the-databases"></a>データベースへの接続

ドメインを移行したら、[オファーのドキュメント](https://wls-eng.github.io/arm-oraclelinux-wls/#connecting-a-database-to-a-cluster)の手順に従うことで、データベースに接続できます。 これらの手順は、すべてのデータベース シークレットと、関連するアクセス文字列を考慮するのに役立ちます。

### <a name="account-for-keystores"></a>キーストアの考慮

アプリケーションで使用されるすべての SSL キーストアの移行を考慮する必要があります。 詳細については、「[キーストアの構成](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/secmg/identity_trust.html#GUID-7F03EB9C-9755-430B-8B86-17199E0C01DC)」を参照してください。

### <a name="connect-the-jms-sources"></a>JMS ソースの接続

データベースに接続した後、WebLogic のドキュメントの「[Fusion Middleware Oracle WebLogic Server JMS リソースの管理](https://docs.oracle.com/middleware/12213/wls/JMSAD/toc.htm)」の手順に従って JMS を構成できます。

### <a name="account-for-logging"></a>ログの考慮

ドメインを移行するときは、既存のログ構成を引き継ぐ必要があります。 詳細については、「[java.util.logging ロガー・レベルの構成](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlach/taskhelp/logging/ConfigureJavaLoggingLevels.html)」および「[Oracle WebLogic Server ログ・ファイルの構成とログ・メッセージのフィルタリング](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wllog/index.html)」を参照してください。

### <a name="migrating-your-applications"></a>アプリケーションの移行

開発チームからテスト、ステージング、および実稼働の各サーバーにアプリケーションをデプロイするために使用される手法は、状況に応じて大きく異なります。 高度に進化した CI/CD プラットフォームが存在する場合、これにより、アプリケーションを WebLogic Server にデプロイすることになります。 また、プロセスがより手動的になる場合もあります。 Azure Virtual Machines を使用してクラウドに WebLogic アプリケーションを移行する利点の 1 つは、既存のプロセスが引き続き機能することです。

CI/CD パイプラインまたは手動デプロイ システムからのアクセスを許可するように、オファーによってプロビジョニングされたネットワーク セキュリティ グループを構成する必要があります。 詳細については、Azure ドキュメントの「[セキュリティ グループ](/azure/virtual-network/security-overview)」を参照してください。

### <a name="testing"></a>テスト

アプリケーションに対するコンテナー内テストは、Azure 内で実行されている新しいサーバーにアクセスするように構成する必要があります。 CI/CD に関する考慮事項と同様に、ネットワーク セキュリティ規則により、Azure にデプロイされたアプリケーションにテストでアクセスできるようにする必要があります。 詳細については、Azure ドキュメントの「[セキュリティ グループ](/azure/virtual-network/security-overview)」を参照してください。

## <a name="post-migration"></a>移行後

「[移行前](#pre-migration)」ステップで定義した移行の目標に到達したら、エンド ツー エンドの受け入れテストを実施して、すべてが予期したとおりに機能することを確認します。 移行後の機能強化に関するトピックをいくつか記載していますが、もちろん以下に限定されません。

* Azure Storage を使用して、仮想マシンにマウントされた静的コンテンツを提供します。 詳細については、[仮想マシンでのデータ ディスクのアタッチまたはデタッチ](/azure/lab-services/devtest-lab-attach-detach-data-disk)に関する記事を参照してください。

* Azure DevOps を使用して、移行した WebLogic クラスターにアプリケーションをデプロイします。 詳細については、[Azure DevOps の使用開始に関するドキュメント](/azure/devops/get-started/?view=azure-devops)を参照してください。

* 高度な負荷分散サービスを使用してネットワーク トポロジを強化します。 詳細については、「[Azure で負荷分散サービスを使用する](/azure/traffic-manager/traffic-manager-load-balancing-azure)」を参照してください。

* Azure マネージド ID をマネージド シークレットに活用し、Azure リソースへのロール ベースのアクセス権を割り当てます。 詳細については、「[Azure リソースのマネージド ID とは](/azure/active-directory/managed-identities-azure-resources/overview)」を参照してください。

* WebLogic の Java EE 認証と承認を Azure Active Directory と統合します。 詳細については、[Azure Active Directory の統合のファースト ステップ ガイド](/azure/active-directory/manage-apps/plan-an-application-integration)に関する記事を参照してください。

* Azure Key Vault を使用して、"シークレット" として機能するすべての情報を格納します。 詳細については、「[Azure Key Vault の基本的な概念](/azure/key-vault/basic-concepts)」を参照してください。
