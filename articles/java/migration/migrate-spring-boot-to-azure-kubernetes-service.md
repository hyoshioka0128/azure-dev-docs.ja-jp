---
title: Azure Kubernetes Service 上で実行するために Spring Boot アプリケーションを移行する
description: このガイドでは、既存の Spring Boot アプリケーションを移行して Azure Kubernetes Service コンテナーで実行する場合に知っておくべきことについて説明します。
author: mnriem
ms.author: manriem
ms.topic: conceptual
ms.date: 4/10/2020
ms.custom: devx-track-java
ms.openlocfilehash: 4d3da50042074b724f614b718ceb0edc7fb83077
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831708"
---
# <a name="migrate-spring-boot-applications-to-azure-kubernetes-service"></a>Azure Kubernetes Service に Spring Boot アプリケーションを移行する

このガイドでは、既存の Spring Boot アプリケーションを移行して Azure Kubernetes Service (AKS) で実行する場合に知っておくべきことについて説明します。

## <a name="pre-migration"></a>移行前

移行を確実に成功させるには、開始する前に、次のセクションで説明する評価とインベントリの手順を完了します。

### <a name="validate-that-the-supported-java-version-works-correctly"></a>サポートされている Java バージョンが正しく動作することを検証する

AKS で Spring Boot アプリケーションを実行する場合は、サポートされているバージョンの Java を使用することをお勧めします。 サポートされているバージョンを使用して、アプリケーションが正しく動作することを確認します。

[!INCLUDE [note-obtain-your-current-java-version](includes/note-obtain-your-current-java-version.md)]

### <a name="determine-whether-and-how-the-file-system-is-used"></a>ファイル システムが使用されているかどうかとその使用方法を判断する

Spring Boot アプリケーションでファイル システムを使用する場合は、再構成や、まれにアーキテクチャの変更が必要になります。 次のセクションに記載された一部または全部のシナリオを確認できます。

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

Spring Boot 1.x を使用しているアプリケーションの場合は、「[Spring Boot 2.0 の移行ガイド](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)」に従って、サポートされている Spring Boot バージョンに更新します。

### <a name="review-your-database-properties"></a>データベース プロパティを確認する

アプリケーションでデータベースを使用する場合は、*application.properties* ファイルのデータベース プロパティを確認して、AKS に移行した後も Spring Boot アプリケーションがデータベースにアクセスできることを確認してください。 データベースがオンプレミスに設置されている場合は、クラウドに移行するか、オンプレミス データベースへの接続を確立する必要があります。

### <a name="identify-log-aggregation-solutions"></a>ログ集計ソリューションを特定する

移行するアプリケーションによって使用されているログ集計ソリューションを特定します。

### <a name="identify-application-performance-management-apm-agents"></a>アプリケーション パフォーマンス管理 (APM) エージェントを特定する

アプリケーションで使用されているアプリケーション パフォーマンス監視エージェント (Dynatrace、Datadog など) を特定します。 これらの APM エージェントを再構成して、Dockerfile または Jib 構成に含まれるようにするか、または Application Insights インプロセス Java エージェントを使用するようにする必要があります。

### <a name="identify-zipkin-dependencies"></a>Zipkin の依存関係を特定する

アプリケーションに Zipkin への明示的な依存関係があるかどうかを確認します。 Maven または Gradle の依存関係内で、`io.zipkin.java` グループの依存関係を探します。

### <a name="inventory-external-resources"></a>外部リソースをインベントリする

データ ソース、JMS メッセージ ブローカー、その他のサービスの URL などの外部リソースを特定します。 Spring Boot アプリケーションでは、一般にこのようなリソースの構成は、*src/main/directory* フォルダー内にある通常 *application.properties* または *application.yml* と呼ばれるファイル内にあります。 さらに、実稼働環境デプロイの環境変数で、関連する構成設定を確認します。

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

使用中のブローカーを特定したら、対応する設定を見つけます。 Spring Boot アプリケーションでは、これらは通常、アプリケーション ディレクトリの *application.properties* および *application.yml* ファイル内にあります。

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-boot](includes/inventory-configuration-sources-and-secrets-spring-boot.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-boot](includes/inspect-the-deployment-architecture-spring-boot.md)]

#### <a name="identity-providers"></a>ID プロバイダー

認証や認可を必要とするすべての ID プロバイダーおよびすべての Spring Boot アプリケーションを特定します。 ID プロバイダーの構成方法については、次を参照してください。

* OAuth または OAuth2 Spring Security の構成については、「[Spring Security](https://spring.io/projects/spring-security)」を参照してください。
* Auth0 の Spring Security の構成については、[Auth0 の Spring Security のドキュメント](https://auth0.com/docs/quickstart/backend/java-spring-security5/01-authorization)を参照してください。
* PingFederate の Spring Security の構成については、[Auth0 の PingFederate の手順](https://auth0.com/authenticate/java-spring-security/ping-federate/)を参照してください。

#### <a name="resources-configured-through-vmware-tanzu-application-service-tas-formerly-pivotal-cloud-foundry"></a>VMware Tanzu Application Service (TAS) (以前の Pivotal Cloud Foundry) によって構成されたリソース

TAS を使用して管理されているアプリケーションでは、多くの場合、前に説明したリソースを含む外部リソースが TAS サービス バインドを使用して構成されています。 このようなリソースの構成を確認するには、[TAS (Cloud Foundry) CLI](https://docs.cloudfoundry.org/cf-cli/) を使用して、アプリケーションの `VCAP_SERVICES` 変数を表示します。

```bash
# Log into TAS, if needed (enter credentials when prompted)
cf login -a <API endpoint>

# Set the organization and space containing the application, if not already selected during login.
cf target org <Organization Name>
cf target space <Space Name>

# Display variables for the application
cf env <Application Name>
```

`VCAP_SERVICES` 変数を調べて、アプリケーションにバインドされている外部サービスの構成設定を確認します。 詳細については、 [TAS (Cloud Foundry) のドキュメント](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html#VCAP-SERVICES)を参照してください。

### <a name="in-place-testing"></a>インプレース テスト

コンテナー イメージを作成する前に、AKS で使用する予定のアプリケーションを JDK と Spring Boot バージョンに移行します。 アプリケーションを十分にテストして、互換性とパフォーマンスを確認します。

## <a name="migration"></a>移行

[!INCLUDE [provision-azure-container-registry-and-azure-kubernetes-service](includes/provision-azure-container-registry-and-azure-kubernetes-service.md)]

### <a name="create-a-docker-image-for-spring-boot"></a>Spring Boot 用の Docker イメージを作成する

Dockerfile を作成するには、次の前提条件が必要です。

* サポートされている JDK。
* JVM ランタイム オプション。
* 環境変数を渡す方法 (該当する場合)。

その後、以下のセクションで説明されている手順を適宜実行できます。 Dockerfile と Spring Boot アプリケーションの開始点として [Spring Boot コンテナーのクイック スタート リポジトリ](https://github.com/Azure/spring-boot-container-quickstart)を使用できます。

#### <a name="configure-azure-key-vault-provider-for-secrets-store-csi-driver"></a>シークレット ストア CSI ドライバーの Azure Key Vault プロバイダーを構成する

Azure KeyVault を作成し、必要なすべてのシークレットを設定します。 詳細については、「[クイック スタート: Azure CLI を使用して Azure Key Vault との間でシークレットの設定と取得を行う](/azure/key-vault/quick-create-cli)」を参照してください。 次に、[シークレット ストア CSI ドライバーの Azure Key Vault プロバイダー](https://github.com/Azure/secrets-store-csi-driver-provider-azure) を構成して、これらのシークレットがポッドからアクセス可能になるようにします。

Spring Boot アプリケーションのブートストラップに使用されるスタートアップ スクリプトを更新する必要もあります。 このスクリプトでは、アプリケーションを起動する前に、Spring Boot によって使用されるキーストアに証明書をインポートする必要があります。

### <a name="build-and-push-the-docker-image-to-azure-container-registry"></a>Docker イメージをビルドし、Azure Container Registry にプッシュする

Dockerfile を作成したら、Docker イメージをビルドし、自身の Azure コンテナー レジストリに発行する必要があります。

[Spring Boot コンテナーのクイック スタート GitHub リポジトリ](https://github.com/Azure/spring-boot-container-quickstart)を使用した場合、イメージをビルドして自身の Azure コンテナー レジストリにプッシュするプロセスは、次の 3 つのコマンドを呼び出すことと同等です。

これらの例では、`MY_ACR` 環境変数は自身の Azure コンテナー レジストリの名前を保持し、`MY_APP_NAME` 変数は、Azure コンテナー レジストリで使用する Web アプリケーションの名前を保持します。

デプロイ ファイルをビルドします。

```bash
mvn package
```

自身の Azure コンテナー レジストリにログインします。

```azurecli
az acr login -n ${MY_ACR}
```

イメージをビルドしてプッシュします。

```azurecli
az acr build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME} .
```

また、次のコマンドに示すように、Docker CLI を使用して最初にイメージをビルドしてローカルでテストすることもできます。 この方法を使用すると、ACR への初期デプロイの前に、イメージのテストと調整を簡単に行えます。 ただし、Docker CLI をインストールし、Docker デーモンが実行されていることを確認する必要があります。

イメージをビルドします。

```bash
docker build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

イメージをローカルで実行します。

```bash
docker run -it -p 8080:8080 ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

`http://localhost:8080` でアプリケーショにアクセスできるようになりました。

自身の Azure コンテナー レジストリにログインします。

```azurecli
az acr login -n ${MY_ACR}
```

自身の Azure コンテナー レジストリにイメージをプッシュします。

```bash
docker push ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

Azure 内でコンテナー イメージをビルドして保存する方法の詳細については、「[Azure Container Registry を使用してコンテナー イメージをビルドして保存する](/learn/modules/build-and-store-container-images/)」を参照してください。

[Spring Boot コンテナーのクイックスタート GitHub リポジトリ](https://github.com/Azure/spring-boot-container-quickstart)を使用した場合は、スタートアップ時に JVM に追加されるカスタム キーストアを含めることもできます。 この追加は、キーストア ファイルを */opt/spring-boot/mycert.crt* に配置した場合に発生します。 これを行うには、前に説明したように、ファイルを Dockerfile に直接追加するか、シークレット ストア CSI ドライバーの Azure Key Vault Provider を使用します。

[Spring Boot コンテナーのクイックスタート GitHub リポジトリ](https://github.com/Azure/spring-boot-container-quickstart)を使用した場合は、Kubernetes デプロイ ファイルで `APPLICATIONINSIGHTS_CONNECTION_STRING` 環境変数を設定することによって Application Insights を有効にすることもできます (この環境変数の値は `InstrumentationKey=00000000-0000-0000-0000-000000000000` のようになります)。 詳細については、[Azure Monitor Application Insights を監視する Java のコード不要のアプリケーション](/azure/azure-monitor/app/java-in-process-agent)に関するページを参照してください。

Docker イメージをカスタマイズする必要がない場合は、代わりに [Maven Jib プラグイン](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin)の使用について調べるか、または AKS にデプロイすることもできます。 詳細については、「[Spring Boot アプリケーションを Azure Kubernetes Service にデプロイする](../spring-framework/deploy-spring-boot-java-app-on-kubernetes.md)」を参照してください。

[!INCLUDE [provision-a-public-ip-address](includes/provision-a-public-ip-address.md)]

### <a name="deploy-to-aks"></a>AKS にデプロイする

Kubernetes YAML ファイルを作成して適用します。 詳細については、「[クイック スタート: Azure CLI を使用して Azure Kubernetes Service クラスターをデプロイする](/azure/aks/kubernetes-walkthrough#run-the-application)」を参照してください。 外部ロード バランサーを (アプリケーションまたはイングレス コントローラー用に) 作成する場合は、前のセクションで `LoadBalancerIP` としてプロビジョニングした IP アドレスを必ず指定してください。

外部化したパラメーターを環境変数として含めます。 詳細については、「[コンテナーの環境変数の定義](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)」を参照してください。

デプロイ YAML を作成するときは、必ずメモリと CPU の設定を含めて、コンテナーのサイズが適切に設定されるようにしてください。

### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>コンソール ログを確認して診断設定を構成する

すべてのアプリケーションが、ファイルではなくコンソールに記録されるようにログ記録を構成します。

アプリケーションを Azure Kubernetes Service にデプロイすると、`kubectl` を使用してログを確認できます。

#### <a name="logstashelk-stack"></a>LogStash/ELK スタック

ログの集計に LogStash/ELK スタックを使用する場合は、コンソールの出力を [Azure Event Hub](/azure/event-hubs/) にストリーミングするように診断設定を構成します。 次に、[LogStash EventHub プラグイン](https://github.com/logstash-plugins/logstash-input-azure_event_hubs)を使用して、ログに記録されたイベントを LogStash に取り込みます。

#### <a name="splunk"></a>Splunk

ログの集計に Splunk を使用する場合は、コンソールの出力を [Azure Blob Storage](/azure/storage/blobs/) にストリーミングするように診断設定を構成します。 次に、[Microsoft Cloud Services 用の Splunk アドオン](https://splunkbase.splunk.com/app/3757/)を使用して、ログに記録されたイベントを Splunk に取り込みます。

### <a name="migrate-and-enable-the-identity-provider"></a>ID プロバイダーを移行して有効にする

Spring Boot アプリケーションのいずれかで認証または認可が必要な場合は、それらが ID プロバイダーにアクセスするように構成されていることを確認します。

* ID プロバイダーが Azure Active Directory である場合は、変更は必要ありません。
* ID プロバイダーがオンプレミスの Active Directory フォレストである場合は、Azure Active Directory を使用したハイブリッド ID ソリューションの実装を検討します。 ガイダンスについては、「[ハイブリッド ID のドキュメント](/azure/active-directory/hybrid/)」を参照してください。
* ID プロバイダーが別のオンプレミス ソリューション (PingFederate など) である場合は、「[Azure AD Connect のカスタム インストール](/azure/active-directory/hybrid/how-to-connect-install-custom)」トピックを参照して、Azure Active Directory とのフェデレーションを構成します。 または、Spring Security を使用して [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) または [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 経由で ID プロバイダーを使用することを検討します。

### <a name="configure-persistent-storage"></a>永続ストレージを構成する

アプリケーションで非揮発性ストレージが必要な場合は、[永続ボリューム](/azure/aks/azure-disks-dynamic-pv) を 1 つ以上構成します。

[!INCLUDE [migrate-scheduled-jobs-aks](includes/migrate-scheduled-jobs-aks.md)]

## <a name="post-migration"></a>移行後

アプリケーションを AKS に移行したので、期待どおりに動作することを確認する必要があります。 これを完了したら、アプリケーションをよりクラウドネイティブにするための推奨事項がいくつかあります。

### <a name="recommendations"></a>推奨事項

* イングレス コントローラーまたはアプリケーション ロード バランサーに割り当てられた IP アドレスに DNS 名を追加することを検討してください。 詳細については、「[AKS で静的パブリック IP アドレスを使用してイングレス コントローラーを作成する](/azure/aks/ingress-static-ip)」を参照してください。

* アプリケーションに [HELM チャート](https://helm.sh/docs/topics/charts/)を追加することを検討してください。 Helm チャートを使用すると、より多様な顧客によって使用およびカスタマイズされるように、アプリケーションのデプロイをパラメーター化できます。

* DevOps の戦略を設計し、実装します。 信頼性を維持しながら開発速度を向上させるには、Azure Pipelines を使用してデプロイとテストを自動化することを検討してください。 詳細については、[AKS へのビルドとデプロイ](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)に関するページをご覧ください。

* [クラスターに対して Azure 監視](/azure/azure-monitor/insights/container-insights-enable-existing-clusters)を有効にし、コンテナー ログの収集や使用状況の追跡などを許可します。

* Prometheus を使用して、アプリケーション固有のメトリックを公開することを検討してください。 Prometheus は、Kubernetes コミュニティで広く採用されているオープンソースのメトリック フレームワークです。 独自の Prometheus サーバーをホストするのではなく、[Azure Monitor で Prometheus メトリックのスクレーピング](/azure/azure-monitor/insights/container-insights-prometheus-integration)を構成して、アプリケーションからのメトリックの集計と、異常な状態に対する自動応答またはエスカレーションを有効にできます。

* 事業継続とディザスター リカバリー戦略を設計し、実装します。 ミッション クリティカルなアプリケーションの場合は、[複数リージョン デプロイのアーキテクチャ](/azure/aks/operator-best-practices-multi-region)を検討してください。

* [Kubernetes バージョン サポート ポリシー](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy)を確認します。 常にサポートされているバージョンが実行されるように、継続的に [AKS クラスターを更新する](/azure/aks/upgrade-cluster)のは開発者の責任です。

* クラスター管理とアプリケーション開発を担当するすべてのチーム メンバーに、関連する [AKS のベスト プラクティス](/azure/aks/best-practices)を確認してもらいます。

* デプロイ ファイルでローリング アップデートの実行方法が指定されていることを確認します。 詳細については、Kubernetes のドキュメントの「[ローリング アップデートのデプロイ](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)」を参照してください。

* ピーク時の負荷に対処するように自動スケーリングを設定します。 詳細については、「[AKS でのアプリケーションの需要を満たすようにクラスターを自動的にスケーリング](/azure/aks/cluster-autoscaler)」を参照してください。

* さらにパフォーマンスを最適化するために、[コードのキャッシュ サイズを監視](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm)し、パラメーター `-XX:InitialCodeCacheSize` および `-XX:ReservedCodeCacheSize` を Dockerfile の `JAVA_OPTS` 変数に追加することを検討します。