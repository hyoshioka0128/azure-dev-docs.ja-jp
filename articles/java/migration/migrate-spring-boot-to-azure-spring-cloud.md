---
title: Spring Boot アプリケーションを Azure Spring Cloud に移行する
description: このガイドでは、既存の Spring Boot アプリケーションを移行して Azure Spring Cloud で実行する場合に注意する必要がある点について説明します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 5/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: c779a85f49ccd7507882fbd123a329addca60a7c
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379746"
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

### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>Azure Spring Cloud のインスタンスとアプリを作成する

存在していない場合、Azure サブスクリプションで Azure Spring Cloud インスタンスをプロビジョニングします。 その後、そこでアプリケーションを作成します。 詳細については、「[クイック スタート: Azure portal を使用して既存の Azure Spring Cloud アプリケーションを起動する](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)」の手順に従ってください。

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](includes/ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](includes/configure-persistent-storage-azure-spring-cloud.md)]

### <a name="migrate-all-certificates-to-keyvault"></a>すべての証明書を KeyVault に移行する

Azure Spring Cloud は JRE キーストアへのアクセスを提供しないため、Azure KeyVault に証明書を移行し、KeyVault 内の証明書にアクセスするようにアプリケーション コードを変更する必要があります。 詳細については、「[Key Vault 証明書の概要](/azure/key-vault/certificates/certificate-scenarios)」と [Java 用の Azure Key Vault の証明書クライアント ライブラリ](/java/api/overview/azure/security-keyvault-certificates-readme)に関するページを参照してください。

### <a name="remove-application-performance-management-apm-integrations"></a>アプリケーション パフォーマンス管理 (APM) 統合を削除する

APM ツール/エージェントとの統合をすべて削除します。 Azure Monitor を使用したパフォーマンス管理の構成については、「[移行後](#post-migration)」セクションを参照してください。

### <a name="disable-metrics-clients-and-endpoints-in-your-applications"></a>アプリケーションのメトリック クライアントおよびエンドポイントを無効にする

使用しているメトリック クライアント、またはアプリケーションで公開されているメトリック エンドポイントを削除します。

### <a name="deploy-the-application"></a>アプリケーションの配置

移行した各マイクロサービス (Spring Cloud Config および Registry のサーバーは含みません) をデプロイします。「[クイック スタート:Azure portal を使用して既存の Azure Spring Cloud アプリケーションを起動する](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)」の手順に従ってください。

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
* ID プロバイダーがオンプレミスの Active Directory フォレストである場合は、Azure Active Directory を使用したハイブリッド ID ソリューションの実装を検討します。 詳細については、[ハイブリッド ID のドキュメント](/azure/active-directory/hybrid/)を参照してください。
* ID プロバイダーが別のオンプレミス ソリューション (PingFederate など) である場合は、「[Azure AD Connect のカスタム インストール](/azure/active-directory/hybrid/how-to-connect-install-custom)」トピックを参照して、Azure Active Directory とのフェデレーションを構成します。 または、Spring Security を使用して [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) または [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 経由で ID プロバイダーを使用することを検討します。

### <a name="expose-the-application"></a>アプリケーションを公開する

既定では、Azure Spring Cloud にデプロイされたアプリケーションは外部からは見えません。 次のコマンドでパブリックにすることでアプリケーションを公開できます。

```azurecli
az spring-cloud app update -n <application name> --is-public true
```

Spring Cloud Gateway を使用しているか、使用する予定であれば、この手順をスキップしてください (これに関する詳細は次のセクションにあります)。

## <a name="post-migration"></a>移行後

これで移行が完了したので、アプリケーションが予想どおりに動作することを確認します。 その後、次の推奨事項を利用することでアプリケーションをよりクラウドネイティブにすることができます。

* Spring Cloud レジストリと連動するようにアプリケーションを有効にすることを検討してください。 これにより、デプロイされた他のマイクロサービスやクライアントでアプリケーションを動的に検出できます。 詳細については、[Java Spring アプリをデプロイ用に準備する](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)」を参照してください。 次に、Spring Client ロード バランサーを使用するようにアプリケーション クライアントを変更します。 これにより、クライアントでは実行中のすべてのアプリケーション インスタンスのアドレスを取得し、別のインスタンスが壊れたか、応答しなくなった場合に動作するインスタンスを見つけることができます。 詳細については、Spring ブログの「[Spring Tips:Spring Cloud Loadbalancer](https://spring.io/blog/2020/03/25/spring-tips-spring-cloud-loadbalancer)」(Spring に関するヒント: Spring Cloud ロード バランサー) を参照してください。

* アプリケーションをパブリックにする代わりに、[Spring Cloud Gateway](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/current/reference/html/) インスタンスの追加を検討してください。 Spring Cloud Gateway は、Azure Spring Cloud インスタンスにデプロイされたすべてのアプリケーションまたはマイクロサービスの単一エンドポイントとなります。 Spring Cloud Gateway が既にデプロイされている場合は、確実に新しくデプロイされたアプリケーションにトラフィックを送信するように構成します。

* Spring Cloud Config サーバーを追加し、すべての Spring Cloud マイクロサービスの構成を、バージョン管理も含め、一元的に管理することを検討してください。 まず、構成を格納するための Git リポジトリを作成し、それを使用するように Azure Spring Cloud インスタンスを構成します。 詳細については、[自分のサービス向けに Spring Cloud Config Server インスタンスを設定する](/azure/spring-cloud/spring-cloud-tutorial-config-server)」を参照してください。 次に、以下の手順で構成を移行します。

  1. Azure Spring Cloud インスタンスに定義したアプリケーションと同じ名前で、構成 Git リポジトリにディレクトリを作成します。

  1. このディレクトリの中に、次の内容で *bootstrap.yml* ファイルを作成します。

     ```yml
     spring:
       application:
         name: <Your the application name used in the previous step>
     ```

  1. 上記のディレクトリ内に *application.yml* ファイルを作成し、そこにアプリケーション設定を移動します。 設定が以前、 *.properties* ファイルに含まれていた場合は、YAML に変換する必要があります。

  1. これらの変更を Git リポジトリにコミットし、プッシュします。

* 一貫性のある自動デプロイのためにデプロイ パイプラインを追加することを検討します。 [Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd)、[GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions)、 [Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service) についての手順が用意されています。

* ステージング環境のデプロイを使用してコードの変更を運用環境でテストした後に、一部またはすべてのエンド ユーザーがそれらを使用できるようにすることを検討します。 詳細については、「[Azure Spring Cloud でステージング環境を設定する](/azure/spring-cloud/spring-cloud-howto-staging-environment)」を参照してください。

* サポートされている Azure データベースにアプリケーションを接続するために、サービス バインドを追加することを検討します。 これらのサービス バインドにより、資格情報などの接続情報を Spring Cloud アプリケーションに提供する必要がなくなります。

* アプリケーションのパフォーマンスおよび相互作用を監視するために、分散トレースと Azure App Insights の使用を検討します。 詳細については、「[Azure Spring Cloud で分散トレースを使用する](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)」を参照してください。

* 異常な状態を迅速に検出して対処するために、Azure Monitor のアラート ルールおよびアクション グループを追加することを検討します。 詳細については、[アラートとアクション グループを使用して Spring Cloud のリソースを監視する](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)」を参照してください。

* 待機時間を短くし、信頼性とフォールト トレランスを高めるために、別のリージョンに Azure Spring Cloud デプロイをレプリケートすることを検討します。 [Azure Traffic Manager](/azure/traffic-manager) を使用してデプロイ間で負荷を分散するか、[Azure Front Door](/azure/frontdoor) を使用して SSL オフロードと DDoS 保護付きの Web アプリケーション ファイアウォールを追加します。

* geo レプリケーションが不要な場合は、[Azure Application Gateway](/azure/application-gateway) を追加して、SSL オフロードと DDoS 保護付きの Web アプリケーション ファイアウォールを追加することを検討します。
