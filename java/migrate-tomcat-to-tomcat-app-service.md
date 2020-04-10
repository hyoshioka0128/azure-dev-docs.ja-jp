---
title: Tomcat アプリケーションを Azure App Service 上の Tomcat に 移行する
description: このガイドでは、既存の Tomcat アプリケーションを移行し、Tomcat を使用して Azure App Service で実行する場合に知っておくべきことについて説明します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: 6e14e8a18f87b67eb0ecb5ce08541058a964c988
ms.sourcegitcommit: 951fc116a9519577b5d35b6fb584abee6ae72b0f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2020
ms.locfileid: "80612098"
---
# <a name="migrate-tomcat-applications-to-tomcat-on-azure-app-service"></a>Tomcat アプリケーションを Azure App Service 上の Tomcat に 移行する

このガイドでは、既存の Tomcat アプリケーションを移行し、Tomcat 9.0 を使用して Azure App Service で実行する場合に知っておくべきことについて説明します。

## <a name="before-you-start"></a>開始する前に

移行前の要件を満たすことができない場合は、以下の関連する移行ガイドを参照してください。

* [Azure Kubernetes Service のコンテナーに Tomcat アプリケーションを移行する](migrate-tomcat-to-containers-on-azure-kubernetes-service.md)
* Tomcat アプリケーションを Azure Virtual Machines に移行する (ガイド計画済)

## <a name="pre-migration"></a>移行前

### <a name="switch-to-a-supported-platform"></a>サポートされているプラットフォームに切り替える

App Service では、特定のバージョンの Java で特定のバージョンの Tomcat が提供されます。 互換性を確保するために、残りの手順に進む前に、現在の環境でサポートされているいずれかのバージョンの Tomcat および Java にアプリケーションを移行してください。 結果として得られた構成は、十分にテストしてください。 このようなテストでは、Linux ディストリビューションの安定した最新リリースを使用します。

[!INCLUDE [note-obtain-your-current-java-version](includes/migration/note-obtain-your-current-java-version.md)]

現在の Tomcat バージョンを取得するには、実稼働サーバーにサインインし、次のコマンドを実行します。

```bash
${CATALINA_HOME}/bin/version.sh
```

Azure App Service によって使用されている現在のバージョンを取得するには、Azure App Service で使用するバージョンに応じて、[Tomcat 9](https://tomcat.apache.org/download-90.cgi) をダウンロードします。

[!INCLUDE [inventory-external-resources](includes/migration/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/migration/inventory-secrets.md)]

[!INCLUDE [inventory-certificates](includes/migration/inventory-certificates.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

<!-- App-Service-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>動的または内部のコンテンツ

アプリケーションで頻繁に書き込みおよび読み取りされるファイル (一時データ ファイルなど) や、アプリケーションでのみ表示できる静的ファイルの場合、Azure Storage を App Service ファイル システムにマウントできます。 詳細については、「[Linux 上の App Service 内の Azure Storage からコンテンツを提供する](/azure/app-service/containers/how-to-serve-content-from-azure-storage)」を参照してください。

### <a name="identify-session-persistence-mechanism"></a>ID セッションの永続化メカニズム

使用されているセッション永続化マネージャーを特定するには、アプリケーション内の *context.xml* ファイルおよび Tomcat の構成を調べます。 `<Manager>` 要素を探して、`className` 属性の値を確認します。

Tomcat の組み込みの [PersistentManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html) の実装 ([StandardManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Standard_Implementation) や [FileStore](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Nested_Components) など) は、App Service のような分散型のスケーリングされたプラットフォームで使用するように設計されていません。 App Service では、複数のインスタンス間で負荷が分散され、任意の時点で任意のインスタンスが透過的に再起動される可能性があるため、変更可能な状態をファイル システムに保持することは推奨されません。

セッションの永続化が必要な場合は、代替の `PersistentManager` の実装を使用する必要があります。これは、Redis Cache を使用して Pivotal Session Manager などの外部データ ストアへの書き込みを行います。 詳細については、「[Tomcat を使用してセッション キャッシュとして Redis を使用する](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat)」を参照してください。

### <a name="special-cases"></a>特殊なケース

運用環境のシナリオによっては、追加の変更が必要な場合や、追加の制限が課される場合があります。 そのようなシナリオはめったに発生しませんが、それらがアプリケーションに適用されないこと、または正しく解決されることを確認することが重要です。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>スケジュールされたジョブにアプリケーションが依存しているかどうかを判断する

スケジュールされたジョブ (Quartz Scheduler タスクや cron ジョブなど) は、App Service では使用できません。 App Service では、スケジュールされたタスクを含むアプリケーションの内部でのデプロイが妨げられることはありません。 ただし、アプリケーションをスケールアウトすると、同じスケジュールされたジョブが、スケジュールされた期間に複数回実行されることがあります。 このような場合、意図しない結果になることがあります。

アプリケーション サーバーの内部または外部で、すべてのスケジュールされたジョブをインベントリします。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>アプリケーションに OS 固有のコードが含まれているかどうかを判断する

アプリケーションにホスト OS に依存するコードが含まれている場合は、それをリファクタリングしてそれらの依存関係を削除する必要があります。 たとえば、ファイル システムのパスで `/` または `\` を使用している場合、[`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) または [`Paths.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-) に置き換えることが必要になる可能性があります。

#### <a name="determine-whether-tomcat-clustering-is-used"></a>Tomcat クラスタリングが使用されているかどうかを判断する

[Tomcat クラスタリング](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html) は Azure App Service ではサポートされていません。 代わりに、Tomcat 固有の機能を使用せずに Azure App Service によってスケーリングと負荷分散を構成して管理できます。 セッション状態を別の場所に保持して、レプリカの間で使用できるようにすることができます。 詳細については、「[ID セッションの永続化メカニズム](#identify-session-persistence-mechanism)」を参照してください。

アプリケーションでクラスタリングが使用されているかどうかを判断するには、*server.xml* ファイルの `<Host>` または `<Engine>` 要素内で `<Cluster>` 要素を探します。

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>実稼働サーバーで実行されているすべての外部プロセス/デーモンを識別する

デーモンの監視など、アプリケーション サーバーの外部で実行されているプロセスは、他の場所に移行するか削除する必要があります。

#### <a name="determine-whether-non-http-connectors-are-used"></a>非 HTTP コネクタが使用されているかどうかを判断する

App Service では、1 つの HTTP コネクタのみがサポートされます。 アプリケーションで AJP コネクタなどの追加のコネクタが必要な場合は、App Service を使用しないでください。

アプリケーションによって使用されている HTTP コネクタを特定するには、Tomcat 構成の *server.xml* ファイル内で `<Connector>` 要素を探します。

#### <a name="determine-whether-memoryrealm-is-used"></a>MemoryRealm が使用されているかどうかを判断する

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) には、永続化された XML ファイルが必要です。 Azure AppService では、このファイルを */home* ディレクトリかそのサブディレクトリ、またはマウントされたストレージにアップロードする必要があります。 `pathName` パラメーターは適宜変更する必要があります。

`MemoryRealm` が現在使用されているかどうかを確認するには、*server.xml* および *context.xml* ファイルを調べ、`className` 属性が `org.apache.catalina.realm.MemoryRealm` に設定されている `<Realm>` 要素を検索します。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>SSL セッションの追跡が使用されているかどうかを判断する

App Service では、Tomcat ランタイムの外部でセッションのオフロードが実行されます。 そのため、[SSL セッションの追跡](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)は使用できません。 代わりに別のセッションの追跡モード (`COOKIE` または `URL`) を使用してください。 SSL セッションの追跡が必要な場合は、App Service を使用しないでください。

#### <a name="determine-whether-accesslogvalve-is-used"></a>AccessLogValve が使用されているかどうかを判断する

[AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html) を使用している場合は、`directory` パラメーターを `/home/LogFiles` またはそのサブディレクトリに設定する必要があります。

## <a name="migration"></a>移行

### <a name="parameterize-the-configuration"></a>構成のパラメーター化

多くの場合、移行前に、*server.xml* および *context.xml* ファイル内でシークレットや外部依存関係 (データソースなど) が識別されます。 そのため、識別された各項目について、ユーザー名、パスワード、接続文字列、または URL を環境変数に置き換えます。

たとえば、*context.xml* ファイルに次の要素が含まれているとします。

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="jdbc:postgresql://postgresdb.contoso.com/wickedsecret?ssl=true"
    driverClassName="org.postgresql.Driver"
    username="postgres"
    password="t00secure2gue$$"
/>
```

この場合は、次の例に示されているように変更できます。

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="${postgresdb.connectionString}"
    driverClassName="org.postgresql.Driver"
    username="${postgresdb.username}"
    password="${postgresdb.password}"
/>
```

### <a name="provision-an-app-service-plan"></a>App Service プランをプロビジョニングする

「[App Service の価格](https://azure.microsoft.com/pricing/details/app-service/linux/)」の利用可能なサービス プランのリストから、仕様が現在の実稼働のハードウェアの仕様を満たしているか超えているプランを選択します。

> [!NOTE]
> ステージング/カナリア デプロイを実行する場合、またはデプロイ スロットを使用する場合は、App Service プランにその追加容量が含まれている必要があります。 Java アプリケーションでは、Premium 以上のプランを使用することをお勧めします。 詳細については、「[Azure App Service でステージング環境を設定する](/azure/app-service/deploy-staging-slots)」を参照してください。

その後に、App Service プランを作成します。 詳細については、「[Azure で App Service プランを管理する](/azure/app-service/app-service-plan-manage)」を参照してください。

### <a name="create-and-deploy-web-apps"></a>Web アプリを作成してデプロイする

App Service プランでは、Tomcat サーバーにデプロイされている WAR ファイルごとに (ランタイム スタックとして Tomcat のバージョンを選択して) Web アプリを作成する必要があります。

> [!NOTE]
> 1 つの Web アプリに複数の WAR ファイルをデプロイすることは可能ですが、非常に望ましくない操作です。 1 つの Web アプリに複数の WAR ファイルをデプロイすると、各アプリケーションを独自の使用要件に従ってスケーリングできなくなります。 さらに、後続の配置パイプラインの複雑さも増大します。 1 つの URL で複数のアプリケーションを使用できるようにする必要がある場合は、[Azure Application Gateway](/azure/application-gateway/) などのルーティング ソリューションの使用を検討してください。

#### <a name="maven-applications"></a>Maven アプリケーション

アプリケーションが Maven POM ファイルから作成されている場合は、[Maven 用の Webapp プラグインを使用](/azure/app-service/containers/quickstart-java#configure-the-maven-plugin)して Web アプリを作成し、アプリケーションをデプロイします。

#### <a name="non-maven-applications"></a>Maven 以外のアプリケーション

Maven プラグインを使用できない場合は、次のような他のメカニズムを使用して Web アプリをプロビジョニングする必要があります。

* [Azure Portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [Azure CLI](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Web アプリが作成されたら、[利用可能なデプロイ メカニズム](/azure/app-service/deploy-zip)のいずれかを使用してアプリケーションをデプロイします。

### <a name="migrate-jvm-runtime-options"></a>JVM ランタイムオプションを移行する

アプリケーションで特定のランタイム オプションが必要な場合は、[最適なメカニズムを使用してそれらを指定します](/azure/app-service/containers/configure-language-java#set-java-runtime-options)。

### <a name="populate-secrets"></a>シークレットを取り込む

アプリケーション固有のシークレットを格納するには、アプリケーション設定を使用します。 複数のアプリケーションで同じシークレットを使用する場合、またはきめ細かなアクセス ポリシーと監査機能が必要な場合は、代わりに [Azure Key Vault を使用](/azure/app-service/containers/configure-language-java#use-keyvault-references) します。

[!INCLUDE [configure-custom-domain-and-ssl](includes/migration/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/migration/import-backend-certificates.md)]

### <a name="migrate-data-sources-libraries-and-jndi-resources"></a>データソース、ライブラリ、および JNDI リソースを移行する

データ ソースの構成手順については、「[Azure App Service 向けの Linux Java アプリを構成する](/azure/app-service/containers/configure-language-java)」の「[データ ソース](/azure/app-service/containers/configure-language-java#data-sources)」セクションを参照してください。

[!INCLUDE[Tomcat datasource additional instructions](includes/migration/tomcat-datasource-additional-instructions.md)]

追加のサーバー レベル クラスパスの依存関係を、[データ ソースの JAR ファイルと同じ手順](/azure/app-service/containers/configure-language-java#finalize-configuration)に従って移行します。

その他の[共有サーバー レベルの JDNI リソース](/azure/app-service/containers/configure-language-java#shared-server-level-resources)を移行します。

> [!NOTE]
> webapp ごとに 1 つの WAR という推奨アーキテクチャに従っている場合は、サーバー レベル クラスパス ライブラリと JNDI リソースをアプリケーションに移行することを検討してください。 これにより、コンポーネントのガバナンスと変更管理が大幅に簡素化されます。

### <a name="migrate-remaining-configuration"></a>残りの構成を移行する

前のセクションを完了すると、カスタマイズ可能なサーバー構成が */home/tomcat/conf* に格納されます。

追加の構成 ([Realm](https://tomcat.apache.org/tomcat-9.0-doc/config/realm.html) や [JASPIC](https://tomcat.apache.org/tomcat-9.0-doc/config/jaspic.html) など) をコピーして移行を完了します

[!INCLUDE [migrate-scheduled-jobs](includes/migration/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>再起動とスモーク テスト

最後に、Web アプリを再起動してすべての構成の変更を適用する必要があります。 再起動が完了したら、アプリケーションが正しく実行されていることを確認します。

## <a name="post-migration"></a>移行後

アプリケーションを Azure App Service に移行したので、期待どおりに動作することを確認する必要があります。 これを完了したら、アプリケーションをよりクラウド ネイティブにするための推奨事項がいくつかあります。

### <a name="recommendations"></a>Recommendations

* ファイル ストレージ用に */home* ディレクトリを使用することを選択した場合は、[それを Azure Storage に置き換える](/azure/app-service/containers/how-to-serve-content-from-azure-storage)ことを検討してください。

* 接続文字列、SSL キー、およびその他の機密情報が含まれている */home* ディレクトリ内に構成がある場合、可能であれば、[Azure Key Vault](/azure/app-service/app-service-key-vault-references) と [アプリケーション設定を使用するパラメーター インジェクション](/azure/app-service/configure-common#configure-app-settings)とを組み合わせて使用することを検討してください。

* ダウンタイムなしで信頼性の高いデプロイを行うには、[デプロイ スロットの使用](/azure/app-service/deploy-staging-slots)を検討してください。

* DevOps の戦略を設計し、実装します。 信頼性を維持しながら開発速度を向上させるには、[Azure Pipelines を使用してデプロイとテストを自動化する](/azure/devops/pipelines/ecosystems/java-webapp)ことを検討してください。 デプロイ スロットを使用する場合は、[スロットへのデプロイと後続のスロット スワップを自動化](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot)できます。

* 事業継続とディザスター リカバリー戦略を設計し、実装します。 ミッション クリティカルなアプリケーションの場合は、[複数リージョン デプロイのアーキテクチャ](/azure/architecture/reference-architectures/app-service-web-app/multi-region)を検討してください。
