---
title: Azure Kubernetes Service のコンテナーに Tomcat アプリケーションを移行する
description: このガイドでは、既存の Tomcat アプリケーションを移行して Azure Kubernetes Service コンテナーで実行する場合に知っておくべきことについて説明します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.custom: devx-track-java
ms.openlocfilehash: 2671f861e780aad0025f881311b9bc603d72bb90
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379686"
---
# <a name="migrate-tomcat-applications-to-containers-on-azure-kubernetes-service"></a>Azure Kubernetes Service のコンテナーに Tomcat アプリケーションを移行する

このガイドでは、既存の Tomcat アプリケーションを移行して Azure Kubernetes Service (AKS) で実行する場合に知っておくべきことについて説明します。

## <a name="pre-migration"></a>移行前

移行を確実に成功させるには、開始する前に、次のセクションで説明する評価とインベントリの手順を完了します。

[!INCLUDE [inventory-external-resources](includes/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/inventory-secrets.md)]

[!INCLUDE [determine-whether-and-how-the-file-system-is-used](includes/determine-whether-and-how-the-file-system-is-used.md)]

<!-- AKS-specific addendum to inventory-persistence-usage -->
[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

### <a name="identify-session-persistence-mechanism"></a>ID セッションの永続化メカニズム

使用されているセッション永続化マネージャーを特定するには、アプリケーション内の *context.xml* ファイルおよび Tomcat の構成を調べます。 `<Manager>` 要素を探して、`className` 属性の値を確認します。

Tomcat の組み込みの [PersistentManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html) の実装 ([StandardManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Standard_Implementation) や [FileStore](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Nested_Components) など) は、Kubernetes のような分散型のスケーリングされたプラットフォームで使用するように設計されていません。 AKS は、複数のポッド間で負荷を分散し、任意の時点で任意のポッドを透過的に再起動することがあります。変更可能な状態をファイル システムに保持することは推奨されません。

セッションの永続化が必要な場合は、代替の `PersistentManager` の実装を使用する必要があります。これは、Redis Cache を使用して Pivotal Session Manager などの外部データ ストアへの書き込みを行います。 詳細については、「[Tomcat を使用してセッション キャッシュとして Redis を使用する](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat)」を参照してください。

### <a name="special-cases"></a>特殊なケース

運用環境のシナリオによっては、追加の変更が必要な場合や、追加の制限が課される場合があります。 そのようなシナリオはめったに発生しませんが、それらがアプリケーションに適用されないこと、または正しく解決されることを確認することが重要です。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>スケジュールされたジョブにアプリケーションが依存しているかどうかを判断する

スケジュールされたジョブ (Quartz Scheduler タスクや cron ジョブなど) は、コンテナー化された Tomcat デプロイでは使用できません。 アプリケーションをスケールアウトすると、1 つのスケジュールされたジョブが、スケジュールされた期間に複数回実行されることがあります。 このような場合、意図しない結果になることがあります。

アプリケーション サーバーの内部または外部で、すべてのスケジュールされたジョブをインベントリします。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>アプリケーションに OS 固有のコードが含まれているかどうかを判断する

アプリケーションに、アプリケーションが実行されている OS に対応するコードが含まれている場合は、基盤の OS に依存しないようにアプリケーションをリファクターする必要があります。 たとえば、ファイル システム パスで `/` や `\` が使用されている場合は、[`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) または [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-) に置き換える必要があります。

#### <a name="determine-whether-memoryrealm-is-used"></a>MemoryRealm が使用されているかどうかを判断する

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) には、永続化された XML ファイルが必要です。 Kubernetes では、このファイルをコンテナー イメージに追加するか、[コンテナーで利用可能になっている共有ストレージ](#identify-session-persistence-mechanism)にアップロードする必要があります。 `pathName` パラメーターを適宜変更する必要があります。

`MemoryRealm` が現在使用されているかどうかを確認するには、*server.xml* および *context.xml* ファイルを調べ、`className` 属性が `org.apache.catalina.realm.MemoryRealm` に設定されている `<Realm>` 要素を検索します。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>SSL セッションの追跡が使用されているかどうかを判断する

コンテナー化されたデプロイでは、一般に SSL セッションはアプリケーション コンテナーの外部 に (通常はイングレス コント ローラーによって) オフロードされます。 アプリケーションで [SSL セッションの追跡](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)を必要とする場合は、SSL トラフィックがアプリケーション コンテナーに直接渡されるようにしてください。

#### <a name="determine-whether-accesslogvalve-is-used"></a>AccessLogValve が使用されているかどうかを判断する

[AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html) が使用されている場合、`directory` パラメーターを、[マウントされた Azure Files 共有](/azure/aks/azure-files-dynamic-pv)またはそのサブディレクトリのいずれかに設定する必要があります。

### <a name="in-place-testing"></a>インプレース テスト

コンテナー イメージを作成する前に、AKS で使用する予定のアプリケーションを JDK と Tomcat に移行します。 アプリケーションを十分にテストして、互換性とパフォーマンスを確認します。

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

## <a name="migration"></a>移行

最初の手順 (「コンテナー レジストリと AKS をプロビジョニングする」) を除いて、以下の手順を移行対象の各アプリケーション (WAR ファイル) に個別に実行することをお勧めします。

> [!NOTE]
> 一部の Tomcat デプロイでは、1 つの Tomcat サーバーで複数のアプリケーションが実行されている場合があります。 デプロイがこれに該当する場合は、各アプリケーションを個別のポッドで実行することを強くお勧めします。 これにより、各アプリケーションのリソース使用率を最適化しながら、複雑さと結合を最小限に抑えることができます。

### <a name="provision-container-registry-and-aks"></a>コンテナー レジストリと AKS をプロビジョニングする

コンテナーレジストリと、サービス プリンシパルがレジストリの閲覧者ロールを持つ Azure Kubernetes クラスターを作成します。 クラスターのネットワーク要件に応じて、[適切なネットワーク モデル](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)を選択してください。

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```

### <a name="prepare-the-deployment-artifacts"></a>デプロイの成果物を準備する

[コンテナー上の Tomcat のクイックスタート GitHub リポジトリ](https://github.com/Azure/tomcat-container-quickstart)を複製します。 これには、いくつかの推奨される最適化が適用された Dockerfile と Tomcat 構成ファイルが含まれています。 次の手順では、コンテナー イメージをビルドして AKS にデプロイする前に、これらのファイルに対して行う必要があると思われる変更について説明します。

#### <a name="open-ports-for-clustering-if-needed"></a>クラスタリング用のポートを開く (必要な場合)

AKS で [Tomcat クラスタリング](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html) を使用する場合は、必要なポート範囲が Dockerfile で公開されていることを確認してください。 *server.xml* でサーバーの IP アドレスを指定するには、コンテナーの起動時にポッドの IP アドレスに初期化された変数の値を使用してください。

または、セッション状態を[別の場所に保持](#identify-session-persistence-mechanism)して、レプリカ間で利用できるようにすることもできます。

アプリケーションでクラスタリングが使用されているかどうかを判断するには、*server.xml* ファイルの `<Host>` または `<Engine>` 要素内で `<Cluster>` 要素を探します。

#### <a name="add-jndi-resources"></a>JNDI リソースを追加する

*server.xml* を編集して、移行前の手順で準備したリソース (データ ソースなど) を追加します。

次に例を示します。

```xml
<!-- Global JNDI resources
      Documentation at /docs/jndi-resources-howto.html
-->
<GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml"
               />

    <!-- Migrated datasources here: -->
    <Resource
        name="jdbc/dbconnection"
        type="javax.sql.DataSource"
        url="${postgresdb.connectionString}"
        driverClassName="org.postgresql.Driver"
        username="${postgresdb.username}"
        password="${postgresdb.password}"
    />
    <!-- End of migrated datasources -->
</GlobalNamingResources>
```

[!INCLUDE[Tomcat datasource additional instructions](includes/tomcat-datasource-additional-instructions.md)]

### <a name="build-and-push-the-image"></a>イメージをビルドしてプッシュする

AKS で使用するためにイメージをビルドして Azure Container Registry (ACR) にアップロードする最も簡単な方法は、`az acr build` コマンドを使用することです。 このコマンドでは、コンピューターに Docker をインストールする必要はありません。 たとえば、上記の Dockerfile とアプリケーション パッケージ *petclinic.war* が現在のディレクトリにある場合は、次の 1 つの手順で ACR でコンテナー イメージをビルドできます。

```bash
az acr build -t "${acrName}.azurecr.io/petclinic:{{.Run.ID}}" -r $acrName --build-arg APP_FILE=petclinic.war --build-arg=prod.server.xml .
```

WAR ファイルの名前が *ROOT.war* の場合は、`--build-arg APP_FILE...` パラメーターを省略できます。 サーバー XML ファイルの名前が *server.xml* の場合は、`--build-arg SERVER_XML...` パラメーターを省略できます。 どちらのファイルも *Dockerfile* と同じディレクトリに存在する必要があります。

または、Docker CLI を使用してイメージをローカルにビルドすることもできます。 この方法を使用すると、ACR への初期デプロイの前に、イメージのテストと調整を簡単に行えます。 ただし、Docker CLI がインストールされ、Docker デーモンが実行されている必要があります。

```bash
# Build the image locally
sudo docker build . --build-arg APP_FILE=petclinic.war -t "${acrName}.azurecr.io/petclinic:1"

# Run the image locally
sudo docker run -d -p 8080:8080 "${acrName}.azurecr.io/petclinic:1"

# Your application can now be accessed with a browser at http://localhost:8080.

# Log into ACR
sudo az acr login -n $acrName

# Push the image to ACR
sudo docker push "${acrName}.azurecr.io/petclinic:1"
```

Azure でのコンテナー イメージの構築と格納の詳細については、個別の [Microsoft Learn コース](/learn/modules/build-and-store-container-images/)を参照してください。

### <a name="provision-a-public-ip-address"></a>パブリック IP アドレスをプロビジョニングする

内部ネットワークまたは仮想ネットワークの外部からアプリケーションにアクセスできるようにする場合は、パブリック静的 IP アドレスが必要になります。 この IP アドレスは、クラスターのノード リソース グループ内にプロビジョニングする必要があります。

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```

### <a name="deploy-to-aks"></a>AKS にデプロイする

[Kubernetes YAML ファイルを作成して適用します](/azure/aks/kubernetes-walkthrough#run-the-application)。 外部ロード バランサーを (アプリケーションまたはイングレス コントローラーに) 作成する場合は、前のセクションで `LoadBalancerIP` としてプロビジョニングした IP アドレスを必ず指定してください。

[外部化したパラメーターを環境変数として](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)含めます。 シークレット (パスワード、API キー、JDBC 接続文字列など) は含めないでください。 シークレットについては、「[KeyVault FlexVolume を構成する](#configure-keyvault-flexvolume)」セクションを参照してください。

### <a name="configure-persistent-storage"></a>永続ストレージを構成する

アプリケーションで非揮発性ストレージが必要な場合は、[永続ボリューム](/azure/aks/azure-disks-dynamic-pv) を 1 つ以上構成します。

Tomcat ログ ディレクトリ ( */tomcat_logs*) にマウントされた Azure Files を含む永続ボリュームを作成し、ログを一元的に保持することができます。 詳細については、「[Azure Kubernetes Service (AKS) で Azure Files を含む永続ボリュームを動的に作成して使用する](/azure/aks/azure-files-dynamic-pv)」を参照してください。

### <a name="configure-keyvault-flexvolume"></a>KeyVault FlexVolume を構成する

[Azure KeyVault を作成](/azure/key-vault/quick-create-cli)し、必要なすべてのシークレットを設定します。 次に、[KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md) を構成して、これらのシークレットにポッドがアクセスできるようにします。

コンテナーのローカル キーストアに証明書をインポートするために、スタートアップ スクリプト ([コンテナー上の Tomcat](https://github.com/Azure/tomcat-container-quickstart) GitHub リポジトリにある *startup.sh*) を変更する必要があります。

### <a name="migrate-scheduled-jobs"></a>スケジュールされたジョブを移行する

AKS クラスターでスケジュールされたジョブを実行するには、必要に応じて [Cron ジョブ](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)を定義します。

## <a name="post-migration"></a>移行後

アプリケーションを AKS に移行したので、期待どおりに動作することを確認する必要があります。 これを完了したら、アプリケーションをよりクラウド ネイティブにするための推奨事項がいくつかあります。

* イングレス コントローラーまたはアプリケーション ロード バランサーに割り当てられた IP アドレスに DNS 名を追加することを検討してください。 詳細については、「[AKS で静的パブリック IP アドレスを使用してイングレス コントローラーを作成する](/azure/aks/ingress-static-ip)」を参照してください。

* [アプリケーションに HELM チャートを追加](https://helm.sh/docs/topics/charts/)することを検討してください。 Helm チャートを使用すると、より多様な顧客によって使用およびカスタマイズされるように、アプリケーションのデプロイをパラメーター化できます。

* DevOps の戦略を設計し、実装します。 信頼性を維持しながら開発速度を向上させるには、[Azure Pipelines を使用してデプロイとテストを自動化する](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)ことを検討してください。

* [クラスターに対して Azure 監視](/azure/azure-monitor/insights/container-insights-enable-existing-clusters)を有効にし、コンテナー ログの収集や使用状況の追跡などを許可します。

* Prometheus を使用して、アプリケーション固有のメトリックを公開することを検討してください。 Prometheus は、Kubernetes コミュニティで広く採用されているオープンソースのメトリック フレームワークです。 独自の Prometheus サーバーをホストするのではなく、[Azure Monitor で Prometheus メトリックのスクレーピング](/azure/azure-monitor/insights/container-insights-prometheus-integration)を構成して、アプリケーションからのメトリックの集計と、異常な状態に対する自動応答またはエスカレーションを有効にできます。

* 事業継続とディザスター リカバリー戦略を設計し、実装します。 ミッション クリティカルなアプリケーションの場合は、[複数リージョン デプロイのアーキテクチャ](/azure/aks/operator-best-practices-multi-region)を検討してください。

* [Kubernetes バージョン サポート ポリシー](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy)を確認します。 常にサポートされているバージョンが実行されるように、継続的に [AKS クラスターを更新する](/azure/aks/upgrade-cluster)のは開発者の責任です。

* クラスター管理とアプリケーション開発を担当するすべてのチーム メンバーに、関連する [AKS のベスト プラクティス](/azure/aks/best-practices)を確認してもらいます。

* *logging.properties* ファイル内の項目を評価します。 パフォーマンスを改善するために、一部のログ出力をなくすか減らすことを検討します。

* さらにパフォーマンスを最適化するために、[コードのキャッシュ サイズを監視](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm)し、パラメーター `-XX:InitialCodeCacheSize` および `-XX:ReservedCodeCacheSize` を Dockerfile の `JAVA_OPTS` 変数に追加することを検討します。
