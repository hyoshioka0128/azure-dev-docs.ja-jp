---
author: yevster
ms.author: yebronsh
ms.date: 8/25/2020
ms.openlocfilehash: 7970cae2b9ac39f7012e74b1334d12b2c5006af0
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062060"
---
### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>Azure Spring Cloud のインスタンスとアプリを作成する

存在していない場合、Azure サブスクリプションで Azure Spring Cloud インスタンスをプロビジョニングします。 その後、そこでアプリケーションを作成します。 詳細については、「[クイック スタート: Azure portal を使用して既存の Azure Spring Cloud アプリケーションを起動する](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)」の手順に従ってください。

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](configure-persistent-storage-azure-spring-cloud.md)]

[!INCLUDE [migrate-all-certificates-to-keyvault-azure-spring-cloud](migrate-all-certificates-to-keyvault-azure-spring-cloud.md)]

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

![Spring Cloud アプリの構成設定](../media/migrate-spring-cloud-to-azure-spring-cloud/spring-cloud-app-configuration-settings.png)

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
