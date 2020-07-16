---
title: Spring Cloud アプリケーションを Azure Spring Cloud に移行する
description: このガイドでは、既存の Spring Cloud アプリケーションを移行して Azure Spring Cloud で実行する場合に知っておくべきことについて説明します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 2/12/2020
ms.custom: devx-track-java
ms.openlocfilehash: fb6a7c108f99081eeeb2bd41f1366f4c38865fe6
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379736"
---
# <a name="migrate-spring-cloud-applications-to-azure-spring-cloud"></a>Spring Cloud アプリケーションを Azure Spring Cloud に移行する

このガイドでは、既存の Spring Cloud アプリケーションを移行して Azure Spring Cloud で実行する場合に知っておくべきことについて説明します。

## <a name="pre-migration"></a>移行前

移行を確実に成功させるには、開始する前に、次のセクションで説明する評価およびインベントリの手順を完了します。

以下の移行前の要件のいずれかを満たすことができない場合は、次の関連する移行ガイドを参照してください。

* Azure Kubernetes Service のコンテナーに実行可能 JAR アプリケーションを移行する (ガイド計画済)
* 実行可能 JAR アプリケーションを Azure Virtual Machines に移行する (ガイド計画済)

### <a name="inspect-application-components"></a>アプリケーション コンポーネントを検査する

[!INCLUDE [determine-whether-and-how-the-file-system-is-used-azure-spring-cloud](includes/determine-whether-and-how-the-file-system-is-used-azure-spring-cloud.md)]

#### <a name="determine-whether-any-of-the-services-contain-os-specific-code"></a>いずれかのサービスに OS 固有のコードが含まれているかどうかを判断する

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [switch-to-a-supported-platform-azure-spring-cloud](includes/switch-to-a-supported-platform-azure-spring-cloud.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

Spring Boot 1.x を使用しているアプリケーションの場合は、「[Spring Boot 2.0 の移行ガイド](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)」に従って、サポートされている Spring Boot バージョンに更新します。 サポートされているバージョンについては、「[Java Spring アプリをデプロイ用に準備する](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions)」を参照してください。

#### <a name="identify-spring-cloud-versions"></a>Spring Cloud のバージョンを特定する

移行する各アプリケーションの依存関係を調べて、使用されている Spring Cloud コンポーネントのバージョンを確認します。

##### <a name="maven"></a>Maven

Maven プロジェクトでは、通常、Spring Cloud バージョンは `spring-cloud.version` プロパティで設定されています。

```xml
  <properties>
    <java.version>1.8</java.version>
    <spring-cloud.version>Hoxton.SR3</spring-cloud.version>
  </properties>
```

##### <a name="gradle"></a>Gradle

Gradle プロジェクトでは、Spring Cloud バージョンは通常、"追加のプロパティ" ブロックで設定されます。

```gradle
ext {
  set('springCloudVersion', "Hoxton.SR3")
}
```

サポートされているバージョンの Spring Cloud を使用するには、すべてのアプリケーションを更新する必要があります。 サポートされているバージョンの一覧については、「[Java Spring アプリをデプロイ用に準備する](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions)」を参照してください。

[!INCLUDE [identify-logs-metrics-apm-azure-spring-cloud.md](includes/identify-logs-metrics-apm-azure-spring-cloud.md)]

#### <a name="identify-zipkin-dependencies"></a>Zipkin の依存関係を特定する

アプリケーションに Zipkin への明示的な依存関係があるかどうかを確認します。 Maven または Gradle の依存関係内で、`io.zipkin.java` グループの依存関係を探します。

### <a name="inventory-external-resources"></a>外部リソースをインベントリする

データ ソース、JMS メッセージ ブローカー、その他のサービスの URL などの外部リソースを特定します。 Spring Cloud アプリケーションでは、通常、このようなリソースの構成は次のいずれかの場所にあります。

* *src/main/directory* フォルダー内にある、通常 *application.properties* または *application.yml* という名前のファイル内。
* 前の手順で確認した Spring Cloud Config リポジトリ。

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

使用中のブローカーを特定したら、対応する設定を見つけます。 Spring Cloud アプリケーションでは、通常、アプリケーション ディレクトリ内の *application.properties* および *application.yml* ファイル内、または Spring Cloud Config サーバー リポジトリ内にあります。

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

#### <a name="identity-providers"></a>ID プロバイダー

認証や認可を必要とするすべての ID プロバイダーおよびすべての Spring Cloud アプリケーションを特定します。 ID プロバイダーの構成方法については、次を参照してください。

* OAuth2 の構成については、[Spring Cloud Security のクイックスタート](https://cloud.spring.io/spring-cloud-static/spring-cloud-security/current/reference/html/#_quickstart)を参照してください。
* Auth0 の Spring Security の構成については、[Auth0 の Spring Security のドキュメント](https://auth0.com/docs/quickstart/backend/java-spring-security5/01-authorization)を参照してください。
* PingFederate の Spring Security の構成については、[Auth0 の PingFederate の手順](https://auth0.com/authenticate/java-spring-security/ping-federate/)を参照してください。

#### <a name="resources-configured-through-pivotal-cloud-foundry-pcf"></a>Pivotal Cloud Foundry (PCF) を使用して構成されたリソース

Pivotal Cloud Foundry を使用して管理されているアプリケーションでは、多くの場合、前に説明したリソースを含む外部リソースが PCF サービス バインドを使用して構成されています。 このようなリソースの構成を確認するには、[Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/) を使用して、アプリケーションの `VCAP_SERVICES` 変数を表示します。

```bash
# Log into PCF, if needed (enter credentials when prompted)
cf login -a <API endpoint>

# Set the organization and space containing the application, if not already selected during login.
cf target org <organization name>
cf target space <space name>

# Display variables for the application
cf env <Application Name>
```

`VCAP_SERVICES` を調べて、アプリケーションにバインドされている外部サービスの構成設定を確認します。 詳細については、[PCF のドキュメント](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html#VCAP-SERVICES)を参照してください。

#### <a name="all-other-external-resources"></a>その他のすべての外部リソース

このガイドに、考えられるすべての外部依存関係を記載することはできません。 移行後に、アプリケーションの外部依存関係がすべて満たされることを確認するのは、お客様の責任です。

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-cloud](includes/inventory-configuration-sources-and-secrets-spring-cloud.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-cloud](includes/inspect-the-deployment-architecture-spring-cloud.md)]

## <a name="migration"></a>移行

### <a name="remove-explicit-configuration-server-settings"></a>明示的な構成サーバー設定を削除する

移行するサービスで、Eureka 設定の明示的な割り当てを見つけて削除します。 こうした設定は、通常 *application.properties* または *application.yml* ファイル内にあります。

**application.yml**

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://myusername:mysecretpassword@localhost:8761/eureka/
```

このような設定がアプリケーション構成にある場合は、それを削除してください。 Azure Spring Cloud により、その構成サーバーの接続情報が自動的に挿入されます。

### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>Azure Spring Cloud のインスタンスとアプリを作成する

Azure サブスクリプションで Azure Spring Cloud インスタンスをプロビジョニングします。 次に、移行するすべてのサービスに対してアプリをプロビジョニングします。 Spring Cloud のレジストリおよび構成のサーバーを含めないでください。 Spring Cloud Gateway サービスは含めてください。 手順については、「[クイック スタート:Azure portal を使用して既存の Azure Spring Cloud アプリケーションを起動する](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)」の手順に従ってください。

### <a name="prepare-the-spring-cloud-config-server"></a>Spring Cloud Config サーバーを準備する

Azure Spring Cloud インスタンスで構成サーバーを構成します。 詳細については、「[チュートリアル:自分のサービス向けに Spring Cloud Config Server インスタンスを設定する](/azure/spring-cloud/spring-cloud-tutorial-config-server)」を参照してください。

> [!NOTE]
> 現在の Spring Cloud Config リポジトリがローカル ファイル システムまたはオンプレミスにある場合は、まず、GitHub、Azure Repos、BitBucket などのプライベート クラウド ベースのリポジトリに構成ファイルを移行またはレプリケートする必要があります。

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](includes/ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](includes/configure-persistent-storage-azure-spring-cloud.md)]

### <a name="migrate-spring-cloud-vault-secrets-to-azure-keyvault"></a>Spring Cloud Vault シークレットを Azure KeyVault に移行する

Azure KeyVault Spring Boot Starter を使用して、Spring 経由でアプリケーションに直接シークレットを挿入できます。 詳細については、「[Azure Key Vault 用の Spring Boot Starter の使用方法](/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-azure-key-vault)」を参照してください。

> [!NOTE]
> 移行において、一部のシークレットの名前変更が必要な場合があります。 必要に応じて、アプリケーション コードを更新してください。

### <a name="migrate-all-certificates-to-keyvault"></a>すべての証明書を KeyVault に移行する

Azure Spring Cloud は JRE キーストアへのアクセスを提供しないため、Azure KeyVault に証明書を移行し、KeyVault 内の証明書にアクセスするようにアプリケーション コードを変更する必要があります。 詳細については、「[Key Vault 証明書の概要](/azure/key-vault/certificates/certificate-scenarios)」と [Java 用の Azure Key Vault の証明書クライアント ライブラリ](/java/api/overview/azure/security-keyvault-certificates-readme)に関するページを参照してください。

### <a name="remove-application-performance-management-apm-integrations"></a>アプリケーション パフォーマンス管理 (APM) 統合を削除する

APM ツール/エージェントとの統合をすべて削除します。 Azure Monitor を使用したパフォーマンス管理の構成については、「[移行後](#post-migration)」セクションを参照してください。

### <a name="replace-explicit-zipkin-dependencies-with-spring-cloud-starters"></a>明示的な Zipkin の依存関係を Spring Cloud のスターターに置き換える

移行したアプリケーションのいずれかに明示的な Zipkin の依存関係がある場合は、「[Azure Spring Cloud で Java Spring アプリケーションをデプロイ用に準備する](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)」の「[分散トレースの依存関係](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#distributed-tracing-dependency)」セクションの説明に従って、それらを削除して Spring Cloud のスターターに置き換えます。 Azure App Insights を使用した分散トレースについては、「[移行後](#post-migration)」セクションを参照してください。

### <a name="disable-metrics-clients-and-endpoints-in-your-applications"></a>アプリケーションのメトリック クライアントおよびエンドポイントを無効にする

使用しているメトリック クライアント、またはアプリケーションで公開されているメトリック エンドポイントを削除します。

### <a name="deploy-the-services"></a>サービスをデプロイする

移行した各マイクロサービス (Spring Cloud Config および Registry のサーバーは含みません) をデプロイします。[クイック スタート:Azure portal を使用して既存の Azure Spring Cloud アプリケーションを起動する](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)」の手順に従ってください。

### <a name="configure-per-service-secrets-and-externalized-settings"></a>サービスごとのシークレットと外部化された設定を構成する

サービスごとの構成設定を各サービスに環境変数として挿入できます。 Azure portal で次の手順を使用します。

1. Azure Spring Cloud インスタンスに移動し、 **[アプリ]** を選択します。
1. 構成するサービスを選択します。
1. **[構成]** を選択します。
1. 構成する変数を入力します。
1. **[保存]** を選択します。

![Spring Cloud アプリの構成設定](media/migrate-spring-cloud-to-azure-spring-cloud/spring-cloud-app-configuration-settings.png)

### <a name="migrate-and-enable-the-identity-provider"></a>ID プロバイダーを移行して有効にする

Spring Cloud アプリケーションのいずれかで認証または認可が必要な場合は、それらが ID プロバイダーにアクセスするように構成されていることを確認します。

* ID プロバイダーが Azure Active Directory である場合は、変更は必要ありません。
* ID プロバイダーがオンプレミスの Active Directory フォレストである場合は、Azure Active Directory を使用したハイブリッド ID ソリューションの実装を検討します。 ガイダンスについては、「[ハイブリッド ID のドキュメント](/azure/active-directory/hybrid/)」を参照してください。
* ID プロバイダーが別のオンプレミス ソリューション (PingFederate など) である場合は、「[Azure AD Connect のカスタム インストール](/azure/active-directory/hybrid/how-to-connect-install-custom)」トピックを参照して、Azure Active Directory とのフェデレーションを構成します。 または、Spring Security を使用して [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) または [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 経由で ID プロバイダーを使用することを検討します。

### <a name="update-client-applications"></a>クライアント アプリケーションを更新する

移行したアプリケーションに対して公開されている Azure Spring Cloud エンドポイントを使用するように、すべてのクライアント アプリケーションの構成を更新します。

## <a name="post-migration"></a>移行後

* 一貫性のある自動デプロイのためにデプロイ パイプラインを追加することを検討します。 [Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd)、[GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions)、 [Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service) についての手順が用意されています。

* ステージング環境のデプロイを使用してコードの変更を運用環境でテストした後に、一部またはすべてのエンド ユーザーがそれらを使用できるようにすることを検討します。 詳細については、「[Azure Spring Cloud でステージング環境を設定する](/azure/spring-cloud/spring-cloud-howto-staging-environment)」を参照してください。

* サポートされている Azure データベースにアプリケーションを接続するために、サービス バインドを追加することを検討します。 これらのサービス バインドにより、資格情報などの接続情報を Spring Cloud アプリケーションに提供する必要がなくなります。

* アプリケーションのパフォーマンスおよび相互作用を監視するために、[分散トレースと Azure App Insights の使用](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)を検討します。

* 異常な状態を迅速に検出して対処するために、Azure Monitor のアラート ルールおよびアクション グループを追加することを検討します。 詳細については、[アラートとアクション グループを使用して Spring Cloud のリソースを監視する](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)」を参照してください。

* 待機時間を短くし、信頼性とフォールト トレランスを高めるために、別のリージョンに Azure Spring Cloud デプロイをレプリケートすることを検討します。 [Azure Traffic Manager](/azure/traffic-manager) を使用してデプロイ間で負荷を分散するか、[Azure Front Door](/azure/frontdoor) を使用して SSL オフロードと DDoS 保護付きの Web アプリケーション ファイアウォールを追加します。

* geo レプリケーションが不要な場合は、[Azure Application Gateway](/azure/application-gateway) を追加して、SSL オフロードと DDoS 保護付きの Web アプリケーション ファイアウォールを追加することを検討します。

* アプリケーションでレガシ Spring Cloud Netflix コンポーネントを使用する場合は、それらを現在の代替候補に置き換えることを検討します。

   | 従来                        | Current                                                |
   |-------------------------------|--------------------------------------------------------|
   | Spring Cloud Eureka           | Spring Cloud Service Registry                          |
   | Spring Cloud Netflix Zuul     | Spring Cloud Gateway                                   |
   | Spring Cloud Netflix Archaius | Spring Cloud Config Server                             |
   | Spring Cloud Netflix Ribbon   | Spring Cloud Load Balancer (クライアント側のロード バランサー) |
   | Spring Cloud Hystrix          | Spring Cloud Circuit Breaker と Resilience4J            |
   | Spring Cloud Netflix Turbine  | Micrometer と Prometheus                                |
