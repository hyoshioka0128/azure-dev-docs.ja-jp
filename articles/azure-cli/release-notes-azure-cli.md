---
title: Azure CLI リリース ノート
description: Azure CLI の最新情報について説明します
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 04/21/2020
ms.topic: article
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 4dcc1bcfb6e42089e221a1ebd7eb5abc0d37a4e3
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89374553"
---
# <a name="azure-cli-release-notes"></a>Azure CLI リリース ノート

## <a name="april-21-2020"></a>2020 年 4 月 21 日

バージョン 2.4.0

### <a name="acr"></a>ACR

* `az acr run --cmd`: 作業ディレクトリのオーバーライドを無効にします
* 専用データ エンドポイントをサポートします

### <a name="aks"></a>AKS

* プライベート クラスターの場合、`az aks list -o table` は privateFqdn を fqdn として表示します
* --uptime-sla を追加します
* containerservice パッケージを更新します
* ノードのパブリック IP サポートを追加します
* help コマンドの入力ミスを修正します

### <a name="appconfig"></a>AppConfig

* Kv list および export コマンドのキー コンテナー参照を解決します
* リスト キー値のバグの修正

### <a name="appservice"></a>AppService

* `az functionapp create`:dotnet の Linux 関数アプリに対する linuxFxVersion の設定方法を変更しました。 これにより、dotnet の Linux 使用アプリが作成されない原因となったバグが修正されます
* [破壊的変更] `az webapp create`: 既存の AppSettings を az webapp create で保持するように修正します
* [破壊的変更] `az webapp up`: -g フラグを使用する場合に az webapp up コマンドの RG を作成するように修正します
* [破壊的変更] `az webapp config`: az webapp config connection-string list で JSON 以外の出力の値を表示するように修正します

### <a name="arm"></a>ARM

* `az deployment create/validate`:ARM テンプレートの欠落したパラメーターのプロンプトをスキップすることをサポートするパラメーター `--no-prompt` を追加します
* `az deployment group/mg/sub/tenant validate`:デプロイ パラメーター ファイルのコメントをサポートします
* `az deployment`:パラメーター `--handle-extended-json-format` の `is_preview` を削除します
* `az deployment group/mg/sub/tenant cancel`:ARM テンプレートのデプロイのキャンセルをサポートします
* `az deployment group/mg/sub/tenant validate`:デプロイの検証が失敗したときのエラー メッセージを改善します
* `az deployment-scripts`:DeploymentScripts の新しいコマンドを追加します
* `az resource tag`:リソースへのタグの増分追加をサポートするパラメーター `--is-incremental` を追加します

### <a name="aro"></a>ARO

* `az aro`:Azure RedHat OpenShift V4 aro コマンド モジュールを追加します

### <a name="batch"></a>Batch

* Batch API を更新します

### <a name="compute"></a>Compute

* `az sig image-version create`:ストレージ アカウントの種類 Premium_LRS を追加します
* `az vmss update`:終了通知の更新の問題を修正します
* `az vm/vmss create`:特殊なイメージ バージョンのサポートを追加します
* SIG API バージョン2019-12-01
* `az sig image-version create`:--target-region-encryption を追加します
* keyvault 名がグローバル メモリ内キャッシュに複製されるため、シリアルで実行するとテストが失敗する問題を修正しました

### <a name="cosmosdb"></a>Cosmos DB

* `az cosmosdb private-link-resource/private-endpoint-connection` をサポートします

### <a name="iot-central"></a>IoT Central

* `az iotcentral` が非推奨になりました
* `az iot central` コマンド モジュールを追加します

### <a name="monitor"></a>モニター

* モニターのプライベート リンクのシナリオをサポートします
* test_monitor_general_operations.py の間違ったモック方法を修正します

### <a name="network"></a>ネットワーク

* public ip update コマンドの sku が非推奨になりました
* `az network private-endpoint`:プライベート dns ゾーン グループをサポートします
* vnet/subnet パラメーターのローカル コンテキスト機能を有効にします
* test_nw_flow_log_delete の間違った使用例を修正します

### <a name="packaging"></a>梱包

* Ubuntu/Disco パッケージのサポートを終了します

### <a name="rbac"></a>RBAC

* `az ad app create/update`: パラメーターとして --optional-claims をサポートします

### <a name="rdbms"></a>RDBMS

* PostgreSQL および MySQL 用の Azure Active Directory 管理者コマンドを追加します

### <a name="service-fabric"></a>Service Fabric

* #12891 の修正: `az sf application update --application-parameters` は、要求に含まれていない古いパラメーターを削除します
* #12470 az sf create cluster の修正。更新の持続性と信頼性に関するバグを修正し、ノード型名が指定されたコードを使用して vmss を正しく検出します

### <a name="sql"></a>SQL

* `az sql mi op list`、`az sql mi op get`、`az sql mi op cancel` を追加します
* `az sql midb`: 長期保持ポリシーを更新/表示し、長期保持バックアップを表示/削除し、長期保持バックアップを復元します

### <a name="storage"></a>ストレージ

* azure-mgmt-storage を 9.0.0 にアップグレードします
* `az storage logging off`:ストレージ アカウントのログ記録の無効化をサポートします
* `az storage account update`:CMK のキーの自動ローテーションを有効にします
* `az storage account encryption-scope create/update/list/show`:暗号化スコープをカスタマイズするためのサポートを追加します
* `az storage container create`:コンテナー レベルの暗号化スコープを設定するための --default-encryption-scope および --deny-encryption-scope-override を追加します

### <a name="survey"></a>アンケート

* アンケート リンクをオフにするスイッチを追加します

## <a name="april-01-2020"></a>2020 年 4 月 1 日

バージョン 2.3.1

### <a name="acr"></a>ACR

* Linux の azure-mgmt-containerregistry の間違ったバージョンを修正します

### <a name="profile"></a>プロファイル

* az login:`latest` 以外のクラウド プロファイルでのログイン エラーを修正します

## <a name="march-31-2020"></a>2020 年 3 月 31 日

バージョン 2.3.0

### <a name="acr"></a>ACR

* 'az acr task update': null ポインター例外
* `az acr import`:--source と --registry の使用法を明確にするために、ヘルプとエラー メッセージを修正します
* 引数 'registry_name' の検証コントロールを追加します
* `az acr login`: '--expose-token' のプレビュー フラグを削除します
* [破壊的変更] 'az acr task create/update' の Branch パラメーターは削除されています
* 'az acr task update' ではコンテキスト、git-token、トリガーを個別に更新できるようになりました
* 'az acr agentpool': 新機能

### <a name="aks"></a>AKS

* --api-server-authorized-ip-ranges の更新時の apiServerAccessProfile を修正します
* aks update:更新時に送信 IP を入力値でオーバーライドします
* MSI クラスターに SPN を作成しないようにし、MSI クラスターへの ACR のアタッチをサポートしてください

### <a name="ams"></a>AMS

* #12469 の修正: 'ask' パラメーターの問題が原因で、Fairplay content-key-policy の追加が失敗します

### <a name="appconfig"></a>AppConfig

* kv export に --skip-keyvault を追加します

### <a name="appservice"></a>AppService

* #12509 の修正:az webapp up へのタグを既定で削除します
* az functionapp create:--runtime-version ヘルプ メニューを更新し、ユーザーが dotnet に --runtime-version を指定した場合の警告を追加しました
* az functionapp create:javaVersion が Windows 関数アプリに設定される方法を更新しました

### <a name="arm"></a>ARM

* az deployment create/validate:既定で --handle-extended-json-format を使用します
* az lock create:ヘルプ ドキュメントにサブリソースの作成例を追加します
* az deployment {group/mg/sub/tenant} list:provisioningState フィルターをサポートします
* az deployment:最後の引数の後に書かれたコメントの解析バグを修正します

### <a name="backup"></a>バックアップ

* 複数ファイルの復元機能を追加しました
* OS ディスクのみのバックアップのサポートを追加しました
* アンマネージドとしての復元を指定するための restore-as-unmanaged-disk パラメーターを追加しました

### <a name="compute"></a>Compute

* az vm create:--nsg-rule の NONE オプションを追加します
* az vmss create/update: VMSS 自動修復のプレビュー タグを削除します
* az vm update:--workspace をサポートします
* VirtualMachineScaleSetExtension 初期化コードのバグを修正します
* VMAccessAgent のバージョンを 2.4 にアップグレードします
* az vmss set-orchestration-service-state: VMSS オーケストレーション サービス状態の設定をサポートします
* ディスク API のバージョンを 2019-11-01 にアップグレードします
* az disk create: --disk-iops-read-only、--disk-mbps-read-only、--max-shares、--image-reference、--image-reference-lun、--gallery-image-reference、--gallery-image-reference-lun を追加します

### <a name="cosmos-db"></a>Cosmos DB

* 非推奨リダイレクトの --type オプションの欠落を修正します

### <a name="docker"></a>Docker

* Alpine 3.11 および Python 3.6.10 へ更新します

### <a name="extension"></a>拡張機能

* パッケージを使用してシステム パスに拡張機能を読み込むことを許可します

### <a name="hdinsight"></a>HDInsight

* (az hdinsight create:)パラメーター `--minimal-tls-version`を使用した、最低限サポートされている tls バージョンの指定をサポートします。 使用可能な値は、1.0、1.1、1.2 です

### <a name="iot"></a>IoT

* codeowner を追加します
* az iot hub create:既定の SKU を F1 から S1 に変更します
* iot hub:2019-03-01-hybrid のプロファイルで IotHub をサポートします

### <a name="iotcentral"></a>IoTCentral

* エラーの詳細を更新し、既定のアプリケーション テンプレートと確認メッセージを更新します

### <a name="keyvault"></a>KeyVault

* 証明書のバックアップ/復元をサポートします
* keyvault create/update:--retention-days をサポートします
* 一覧表示中にマネージド キー/シークレットは表示されなくなります
* az keyvault create: コンテナーの作成中にネットワーク ルールを指定するための `--network-acls`、`--network-acls-ips`、`--network-acls-vnets`をサポートします

### <a name="lock"></a>ロック

* az lock delete のバグの修正: az lock delete は Microsoft.DocumentDB では機能しません

### <a name="monitor"></a>モニター

* az monitor clone: あるリソースから別のリソースへのメトリック ルールの複製をサポートします
* IcM179210086 の修正: Application Insights メトリックのカスタム メトリック アラートを作成できません

### <a name="netappfiles"></a>NetAppFiles

* az volume create:レプリケーション操作 (許可、保留、再開、状態、削除) を追加して、データ保護ボリュームを許可します

### <a name="network"></a>ネットワーク

* az network application-gateway waf-policy managed-rule rule-set add: Microsoft_BotManagerRuleSet をサポートします
* network watcher flow-log show:
* Application Gateway リスナーでホスト名をサポートします
* az network nat gateway: パブリック IP またはパブリック IP プレフィックスなしの空のリソースの作成をサポートします
* VPN ゲートウェイの世代をサポートします
* `az network dns record-set {} add-record`で `--if-none-match` をサポートします

### <a name="packaging"></a>梱包

* Python 3.5 のサポートは終了します

### <a name="profile"></a>プロファイル

* az login:MFA エラーの警告を表示します

### <a name="rdbms"></a>RDBMS

* PostgreSQL および MySQL 用のサーバー データ暗号化キー管理コマンドを追加します

## <a name="march-10-2020"></a>2020 年 3 月 10 日

バージョン 2.2.0

### <a name="acr"></a>ACR

* 修正: `az acr login` で誤ってエラーが発生する
* 新しいコマンド `az acr helm install-cli` を追加
* プライベート リンクと CMK のサポートの追加
* 'private-link-resource list' コマンドの追加

### <a name="aks"></a>AKS

* クラウド シェルでの aks browse の修正
* az aks:addon および agentpool NoneType エラーの監視を修正
* Azure Kubernetes クラスターの作成時にノード プールに --nodepool-tags を追加
* クラスターへの nodepool の追加または更新時に --tags を追加
* aks create: `--enable-private-cluster` の追加
* Azure Kubernetes クラスターの作成時に --nodepool-labels を追加
* Azure Kubernetes クラスターへの新しい nodepool 追加時に --labels を追加
* ダッシュボード URL に欠落していた / を追加
* マネージド ID を有効にする aks クラスターの作成のサポート
* az aks:ネットワーク プラグインが "azure" または "kubernet" のいずれかであることを検証
* az aks:aad セッション キーのサポートの追加
* [破壊的変更] az aks: omsagent での GF と BF に対する msi 変更のサポート (コンテナー監視)(#1)
* az aks use-dev-spaces:Azure Dev Spaces コントローラー上に作成されたエンドポイントをカスタマイズするために、use-dev-spaces コマンドにエンドポイントの種類オプションを追加

### <a name="appconfig"></a>AppConfig

* keyvault 参照および機能の追加のための "kv set" を使用したブロック解除

### <a name="appservice"></a>AppService

* az webapp create:--runtime を指定したコマンドの実行時の問題を修正
* az functionapp deployment source config-zip:リソース グループまたは関数名が無効であるか存在しない場合のエラー メッセージの追加
* functionapp create:`functionapp create` で表示される警告メッセージの修正 (現在、これは`--functions_version` フラグを示しますが、誤ってフラグ名の `-` ではなく `_` を使用します)
* az functionapp create:Linux 関数アプリに linuxFxVersion とコンテナー イメージ名が設定されていた方法を更新
* az functionapp deployment source config-zip:zip デプロイ中にアプリ設定の変更の競合状態によって発生する問題 (デプロイ時に 5xx エラーが表示される) を修正
* #5720946 の修正: az webapp backup で名前の設定に失敗する

### <a name="arm"></a>ARM

* az resource:リソース モジュールの例の改善
* az policy assignment list:管理グループ スコープでのポリシー割り当ての一覧表示をサポート
* リソース グループでのテンプレートのデプロイ用に `az deployment group` と `az deployment operation group` を追加。 これは `az group deployment` と `az group deployment operation` の複製です
* サブスクリプション スコープでのテンプレートのデプロイ用に `az deployment sub` と `az deployment operation sub` を追加。 これは `az deployment` と `az deployment operation` の複製です
* 管理グループでのテンプレートのデプロイ用に `az deployment mg` と `az deployment operation mg` を追加
* テナント スコープでのテンプレートのデプロイ用に `az deployment tenant` と `az deployment operation tenant` を追加
* az policy assignment create:`--location` パラメーターに対する説明の追加
* az group deployment create:クロス テナントをサポートするためにパラメーター `--aux-tenants` を追加

### <a name="cdn"></a>CDN

* CDN WAF コマンドの追加

### <a name="compute"></a>Compute

* az sig image-version: --data-snapshot-luns の追加
* az ppg show: 近接配置グループ内のすべてのリソースのコロケーションの状態をフェッチできるように、--colocation-status を追加
* az vmss create/update: 自動修復のサポート
* [破壊的変更] az image template: テンプレートの名前を builder に変更
* az image builder create: --image-template の追加

### <a name="cosmos-db"></a>Cosmos DB

* SQL ストアド プロシージャ、udf およびトリガーのコマンドレットの追加
* az cosmosdb create: キー コンテナーの暗号化情報の追加をサポートするために、--key-uri を追加

### <a name="keyvault"></a>KeyVault

* keyvault create: 論理的な削除を既定で有効化

### <a name="monitor"></a>モニター

* az monitor metrics alert create: `--condition` での `~` のサポート

### <a name="network"></a>ネットワーク

* az network application-gateway rewrite-rule create: URL の構成のサポート
* az network dns zone import: 今後、--zone-name は大文字と小文字を区別する
* az network private-endpoint/private-link-service: プレビュー ラベルの削除
* az network bastion: Bastion のサポート
* az network vnet list-available-ips: VNet 内で使用可能な IP の一覧のサポート
* az network watcher flow-log create/list/delete/update: Watcher フロー ログを管理するために新しいコマンドを追加し、Watcher を明示的に識別するために --location を公開
* az network watcher flow-log configure: 非推奨
* az network watcher flow-log show: ARM 形式の結果 (非推奨の古い形式の出力) を取得するために --location および--name をサポート

### <a name="policy"></a>ポリシー

* az policy assignment create:ポリシー割り当ての自動生成された名前が制限を超えるというバグの修正

### <a name="rbac"></a>RBAC

* az ad group show: RegEx の問題として扱われる --group 値の修正

### <a name="rdbms"></a>RDBMS

* azure-mgmt-rdbms SDK のバージョンを 2.0.0 にバージョンアップ
* az postgres private-endpoint-connection: Postgres プライベート エンドポイント接続の管理
* az postgres private-link-resource: Postgres プライベート リンク リソースの管理
* az mysql private-endpoint-connection: MySQL プライベート エンドポイント接続の管理
* az mysql private-link-resource: MySQL プライベート リンク リソースの管理
* az mariadb private-endpoint-connection: MariaDB プライベート エンドポイント接続の管理
* az mariadb private-link-resource: MariaDB プライベート リンク リソースの管理
* RDBMS プライベート エンドポイント テストを更新中

### <a name="sql"></a>SQL

* Sql midb Add: list-deleted、show-deleted、update-retention、show-retention
* (sql server create:)オプションの public-network-access 'Enable'/'Disable' フラグを sql server create に追加
* (sql server update:) 顧客向けの変更の実施
* MI および SQL DB 用の minimal_tls_version プロパティの追加

### <a name="storage"></a>ストレージ

* az storage blob delete-batch:`--dryrun` フラグの誤動作
* az storage account network-rule add (バグ修正): 追加操作は、べき等にする必要がある
* az storage account create/update:ルーティングの優先順位のサポートを追加
* azure-mgmt-storage のバージョンを 8.0.0 にアップグレード
* az storage container immutability create: --allow-protected-append-write パラメーターの追加
* az storage account private-link-resource list:ストレージ アカウントのプライベート リンク リソースを一覧表示するためのサポートを追加
* az storage account private-endpoint-connection approve/reject/show/delete:プライベート エンドポイント接続の管理のサポート
* az storage account blob-service-properties update: --enable-restore-policy と --restore-days の追加
* az storage blob restore:BLOB 範囲を復元するためのサポートを追加

## <a name="february-18-2020"></a>2020 年 2 月 18 日

バージョン 2.1.0

### <a name="acr"></a>ACR

* `az acr login` の新しい引数 `--expose-token` を追加
* `az acr task identity show -n Name -r Registry -o table` の正しくない出力を修正
* az acr login:Docker コマンドによって返されたエラーが返された場合は CLIError をスロー

### <a name="acs"></a>ACS

* aks create/update: `--vnet-subnet-id` の検証を追加

### <a name="aladdin"></a>Aladdin

* 生成された例をコマンドの _help.py として解析

### <a name="ams"></a>AMS

* az ams の一般提供を開始

### <a name="appconfig"></a>AppConfig

* サポートされていないキー/ラベル フィルターを除外するようにヘルプ メッセージを変更
* マネージド ID と機能フラグを除くほとんどのコマンドのプレビュー タグを削除
* ストアの更新時にカスタマー マネージド キーを追加

### <a name="appservice"></a>AppService

* az webapp list-runtimes:list-runtimes のバグを修正
* az webapp | functionapp config ssl create を追加
* v3 関数アプリおよびノード 12 のサポートを追加

### <a name="arm"></a>ARM

* az policy assignment create:`--policy` パラメーターが無効な場合のエラー メッセージを修正
* az group deployment create:サイズの大きな parameters.json ファイルの使用時の "stat: path too long for Windows" エラーを修正

### <a name="backup"></a>バックアップ

* OLR のアイテム レベルの回復フローを修正
* SQL Database および SAP Database のファイルとしての復元のサポートを追加

### <a name="compute"></a>Compute

* vm/vmss/availability-set update: ProximityPlacementGroup を更新できるように --ppg を追加
* vmss create: --data-disk-iops および --data-disk-mbps を追加
* az vm host: `vm host` および `vm host group` のプレビュー タグを削除
* [破壊的変更] #10728 を修正: `az vm create`: vnet が指定されている場合、サブネットが存在しなければサブネットが自動的に作成される
* vm イメージ リストの信頼性を向上

### <a name="eventhub"></a>Eventhub

* 2019-03-01-hybrid プロファイルに対する Azure Stack のサポート

### <a name="keyvault"></a>KeyVault

* az keyvault key create: パラメーター `--ops` の新しい値 `import` を追加
* az keyvault key list-versions: キー指定のためのパラメーター `--id` をサポート
* プライベート エンドポイントの接続をサポート

### <a name="network"></a>ネットワーク

* azure-mgmt-network 9.0.0 にバージョンアップ
* az network private-link-service update/create: support --enable-proxy-protocol
* 接続モニター V2 の機能を追加

### <a name="packaging"></a>梱包

* [破壊的変更] Python 2.7 のサポートを終了

### <a name="profile"></a>プロファイル

* プレビュー:サブスクリプション アカウントに新しい属性 `homeTenantId` および `managedByTenants` を追加。 変更を有効にするには、`az login` を再実行してください
* az login:1 つのサブスクリプションが複数のテナントからリストされる場合は警告を表示して最初のものに既定で設定。 このサブスクリプションにアクセスする際の特定のテナントを選択するには、`az login` に `--tenant` を含めてください

### <a name="role"></a>Role

* az role assignment create:表示名でサービス プリンシパルにロールを割り当てた場合に HTTP 400 が発生するエラーを修正

### <a name="sql"></a>SQL

* Update SQL Managed Instance のコマンドレット `az sql mi update` に 2 つのパラメーター tier および family を追加して更新

### <a name="storage"></a>ストレージ

* [破壊的変更] `az storage account create`:既定のストレージ アカウントの種類を StorageV2 に変更

## <a name="february-04-2020"></a>2020 年 2 月 4 日

バージョン 2.0.81

### <a name="acs"></a>ACS

* Standard Load Balancer でアウトバウンド割当てポートとアイドル タイムアウトを設定するためのサポートを追加します
* API バージョン 2019-11-01 への更新を行います

### <a name="acr"></a>ACR

* [破壊的変更] `az acr delete` でプロンプトが表示されます
* [破壊的変更] 'az acr task delete' でプロンプトが表示されます
* taskrun 管理用の新しいコマンド グループ 'az acr taskrun show/list/delete' を追加します

### <a name="aks"></a>AKS

* 各クラスターは、分離を向上させるために、個別のサービス プリンシパルを取得します

### <a name="appconfig"></a>AppConfig

* appservice との間での keyvault 参照のインポート/エクスポートをサポートします
* appconfig から appconfig へのすべてのラベルのインポート/エクスポートをサポートします
* 設定とインポートの前にキーと機能の名前を検証します
* 構成ストアの SKU 変更を公開します。
* マネージド ID のコマンド グループを追加します。

### <a name="appservice"></a>AppService

* Azure Stack: 2019-03-01-hybrid のプロファイルの下の surface コマンド
* functionapp:Linux で Java 関数アプリを作成する機能を追加します

### <a name="arm"></a>ARM

* 問題 #10246 を修正: 渡されたパラメーター `--ids` がリソース グループ ID の場合、`az resource tag` がクラッシュします
* 問題 #11658 を修正: `az group export` コマンド が `--query` と `--output` パラメーターをサポートしません
* 問題 #10279 を修正:検証が失敗した場合、`az group deployment validate` の終了コードは 0 になります
* 問題 #9916 を修正:`az resource list` コマンドのタグとその他のフィルター条件間の競合に関するエラー メッセージを改善します
* コマンド `az group create` の managedBy 情報の追加をサポートする新しいパラメーター `--managed-by` を追加します

### <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

* Azure Red Hat OpensShift クラスターで Log Analytics 監視を管理するための `monitor` サブグループを追加します

### <a name="botservice"></a>BotService

* 問題 #11697 を修正: `az bot create` がべき等ではありません
* 名前修正テストをライブ モードでのみ実行するように変更します

### <a name="cdn"></a>CDN

* rulesEngine 機能のサポートを追加します
* 規則を管理するための新しいコマンド グループ 'cdn endpoint rule' を追加します
* API バージョン 2019-04-15 を使用するように azure-mgmt-cdn バージョンを 4.0.0 に更新します

### <a name="deployment-manager"></a>Deployment Manager

* すべてのリソースのリスト操作を追加します。
* 新しい手順の種類に対する手順リソースを強化します。
* バージョン 0.2.0 を使用するように azure-mgmt-deploymentmanager パッケージを更新します。

### <a name="iot"></a>IoT

* 'IoT hub Job' コマンドを廃止します。

### <a name="iot-central"></a>IoT Central

* 新しい SKU 名 ST0、ST1、ST2 でのアプリの作成/更新をサポートします。

### <a name="key-vault"></a>Key Vault

* キーをダウンロードするための新しいコマンド `az keyvault key download` を追加します。

### <a name="misc"></a>その他

* #6371 を修正:Bash でのファイル名と環境変数の補完をサポートします

### <a name="network"></a>ネットワーク

* #2092 を修正: az network dns record-add/remove: レコード セットが見つからない場合に警告を追加します。 今後、この自動作成を確認するための追加の引数がサポートされる予定です。

### <a name="policy"></a>ポリシー

* 豊富なポリシー メタデータ リソースを取得するための新しいコマンド `az policy metadata` を追加します
* `az policy remediation create`:`--resource-discovery-mode` パラメーターを使用して、修復前にコンプライアンスを再評価する必要があるかどうかを指定します

### <a name="profile"></a>プロファイル

* `az account get-access-token`:サブスクリプションを指定することなく、テナントのトークンを直接取得するための `--tenant` パラメーターを追加します

### <a name="rbac"></a>RBAC

* [破壊的変更] #11883 を修正: `az role assignment create`: 空のスコープの場合にエラーが表示されます

### <a name="security"></a>Security

* ストレージ アカウントの高度な脅威保護設定を表示および管理するための新しいコマンド `az atp show` と `az atp update` を追加します。

### <a name="sql"></a>SQL

* `sql dw create`: `--zone-redundant` と `--read-replica-count` パラメーターを廃止します。 これらのパラメーターは、DataWarehouse には適用されません。
* [破壊的変更] `az sql db create`:"az sql db create --sample-name" に対して使用可能な値としてドキュメントに記載されている "WideWorldImportersStd" と "WideWorldImportersFull" を削除します。 これらのサンプル データベースを使用すると、常に作成が失敗します。
* SQL データベースの機密性分類を管理するための新しいコマンド `sql db classification show/list/update/delete` と `sql db classification recommendation list/enable/disable` を追加します。
* `az sql db audit-policy`:空の監査アクションとグループの修正

### <a name="storage"></a>ストレージ

* Azure ファイル共有の管理操作に Microsoft.Storage リソース プロバイダーを使用するための新しいコマンド グループ `az storage share-rm` を追加します。
* 問題 #11415 を修正: `az storage blob update` のアクセス許可エラー
* Azcopy 10.3.3 を統合し、Win32 をサポートします。
* `az storage copy`:`--include-path`、`--include-pattern`、`--exclude-path`、および `--exclude-pattern` パラメーターを追加します
* `az storage remove`:`--inlcude` および `--exclude` パラメーターを `--include-path`、`--include-pattern`、`--exclude-path`、および `--exclude-pattern` パラメーターに変更します
* `az storage sync`:`--include-pattern`、`--exclude-path`、および `--exclude-pattern` パラメーターを追加します

### <a name="servicefabric"></a>ServiceFabric

* アプリケーションとサービスを管理するための新しいコマンドを追加します。

## <a name="january-13-2020"></a>2020 年 1 月 13 日

バージョン 2.0.80

### <a name="compute"></a>Compute

* disk update:--disk-encryption-set と --encryption-type を追加します
* snapshot create/update:--disk-encryption-set と --encryption-type を追加します

### <a name="storage"></a>ストレージ

* azure-mgmt-storage のバージョンを 7.1.0 にアップグレードします
* `az storage account create`:テーブルとキューの暗号化サービスをサポートするための `--encryption-key-type-for-table` と `--encryption-key-type-for-queue` を追加します

## <a name="january-07-2020"></a>2020 年 1 月 7 日

バージョン 2.0.79

### <a name="acr"></a>ACR

* [破壊的変更] 'acr build'、'acr task create/update'、'acr run'、および 'acr pack' の '--os' パラメーターを削除します。 代わりに、'--platform' を使用してください。

### <a name="appconfig"></a>AppConfig

* 機能フラグのインポート/エクスポートのサポートを追加します
* keyvault 参照を作成するための新しいコマンド 'az appconfig kv set-keyvault' を追加します
* 機能フラグをファイルにエクスポートするときに、さまざまな名前付け規則をサポートします

### <a name="appservice"></a>AppService

* 問題 #7154 を修正:単一引用符ではなく、アクサン グラーブを使用するように、コマンド < > のドキュメントを更新しています
* 問題 #11287 を修正: webapp up:既定で、up を使用して作成されたアプリを 'SSL 有効' にする必要がある
* 問題 #11592 を修正:HTML 静的サイトに az webapp up フラグを追加します

### <a name="arm"></a>ARM

* `az resource tag` を修正:Recovery Services コンテナー タグを更新できない

### <a name="backup"></a>バックアップ

* IaasVM ワークロードの論理的な削除機能を有効にするための新しいコマンド 'backup protection undelete' を追加しました
* backup-properties コマンドを設定するための新しいパラメーター '--soft-delete-feature-state' を追加しました
* IaasVM ワークロードのディスク除外のサポートを追加しました

### <a name="compute"></a>Compute

* Azure Stack プロファイルの `vm create` エラーを修正します。
* vm monitor metrics tail/list-definitions: VM のクエリ メトリックとリスト定義をサポートします。
* az vm の新しい再適用コマンド アクションを追加します

### <a name="hdinsight"></a>HDInsight

* Kafka REST プロキシを使用する Kafka クラスターの作成のサポート
* azure-mgmt-hdinsight を 1.3.0 にアップグレード

### <a name="misc"></a>その他

* 既定の JSON 形式 または --output によって構成された形式で Azure CLI モジュールと拡張機能のバージョンを表示するプレビュー コマンド `az version show` を追加します

### <a name="event-hubs"></a>Event Hubs

* [破壊的変更] コマンド 'az eventhubs eventhub update' と 'az eventhubs eventhub create' から 'ReceiveDisabled' status オプションを削除します。 このオプションは、イベント ハブ エンティティに対しては無効です。

### <a name="service-bus"></a>Service Bus

* [破壊的変更] コマンド 'az servicebus topic create'、'az servicebus topic update'、'az servicebus queue create'、および 'az servicebus queue update' から 'ReceiveDisabled' status オプションを削除します。 このオプションは、Service Bus のトピックとキューに対しては無効です。

### <a name="rbac"></a>RBAC

* #11712 を修正: アプリケーションまたはサービス プリンシパルが存在しない場合に `az ad app/sp show` で終了コード 3 が返されない

### <a name="storage"></a>ストレージ

* `az storage account create`:--enable-hierarchical-namespace パラメーターのプレビュー フラグを削除します
* API バージョン 2019-06-01 を使用するために azure-mgmt-storage バージョンを 7.0.0 に更新します
* ストレージ アカウント blob-service-properties の削除の保持ポリシーの管理をサポートするために、新しいパラメーター `--enable-delete-retention` と `--delete-retention-days` を追加します。

## <a name="december-17-2019"></a>2019 年 12 月 17 日

2.0.78

### <a name="acr"></a>ACR

* acr task run でのローカル コンテキストのサポートを追加しました

### <a name="acs"></a>ACS

* [破壊的変更] az openshift create: `--workspace-resource-id` の名前を `--workspace-id` に変更します。

### <a name="ams"></a>AMS

* リソースが見つからない場合に 3 を返すように表示コマンドを更新しました

### <a name="appconfig"></a>AppConfig

* 要求 URL に api-version を追加するときのバグを修正しました。 既存のソリューションは、改ページ位置の自動修正で機能しません。
* 英語以外の言語を表示するためのサポートを追加しました。これは、Microsoft のバックエンド サービスがグローバリゼーション向けに unicode をサポートするためです。

### <a name="appservice"></a>AppService

* 問題 #11217 を修正: webapp: az webapp config ssl upload はスロット パラメーターをサポートする必要がある
* 問題 #10965 を修正:エラー:名前を空にすることはできません。 ip_address とサブネットによる削除を許可します
* Key Vault `az webapp config ssl import` からの証明書のインポートのサポートを追加しました

### <a name="arm"></a>ARM

* 6\.0.0 を使用するように azure-mgmt-resource パッケージを更新しました
* 新しいパラメーター `--aux-subs` の追加による `az group deployment create` コマンドのクロス テナントのサポート
* ポリシー セット定義へのメタデータ情報の追加をサポートするために、新しいパラメーター `--metadata` を追加しました。

### <a name="backup"></a>バックアップ

* SQL および SAP Hana ワークロードのバックアップ サポートを追加しました。

### <a name="botservice"></a>BotService

* [破壊的変更] プレビューコマンド 'az bot create' から '--version' フラグを削除します。 v4 SDK ボットのみがサポートされます。
* 名前を使用できるかどうかのチェックを 'sz bot create' に追加しました。
* 'az bot update' を使用してボットのアイコン URL を更新するためのサポートを追加しました。
* 'az bot directline update' を使用して Direct Line チャネルを更新するためのサポートを追加しました。
* 'az bot directline create' に '--enable-enhanced-auth' フラグのサポートを追加しました。
* コマンド グループ 'az bot authsetting' は、プレビュー段階ではなく、GA です。
* 'az bot' の次のコマンドは、プレビュー段階ではなく、GA です: 'create'、'prepare-deploy'、'show'、'delete'、'update'。
* 'az bot prepare-deploy' が修正され、 '--proj-file-path' 値が小文字に変更されます (例:"Test.csproj" が "test.csproj" に)。

### <a name="compute"></a>Compute

* vmss create/update:--scale-in-policy を追加しました。これは、VMSS がスケールインされるときに削除対象として選択される仮想マシンを決定します。
* vm/vmss update:--priority を追加しました。
* vm/vmss update:--max-price を追加しました。
* disk-encryption-set コマンド グループ (create、show、update、delete、list) を追加しました。
* disk create:--encryption-type と --disk-encryption-set を追加しました。
* vm/vmss create: --os-disk-encryption-set と --data-disk-encryption-sets を追加しました。

### <a name="core"></a>コア

* Python 3.4 のサポートを削除しました
* 複数のコマンドでの HaTS 調査のプラグイン

### <a name="dls"></a>DLS

* ADLS SDK バージョンを更新しました (0.0.48)。

### <a name="install"></a>インストール

* インストール スクリプトで python 3.8 がサポートされます

### <a name="iot"></a>IoT

* [破壊的変更] manual-failover から --failover-region パラメーターを削除しました。 これで、割り当てられた geo ペアのセカンダリ リージョンにフェールオーバーします。

### <a name="key-vault"></a>Key Vault

* #8095 を修正: `az keyvault storage remove`: ヘルプ メッセージを改善します
* #8921 を修正: `az keyvault key/secret/certificate list/list-deleted/list-versions`: パラメーター `--maxresults` の検証のバグを修正します
* #10512 を修正: `az keyvault set-policy`: `--object-id`、`--spn`、`--upn` のいずれも指定されていない場合のエラー メッセージを改善します
* #10846 を修正: `az keyvault secret show-deleted`: `--id` を指定した場合、`--name/-n` は必要ありません
* #11084 を修正: `az keyvault secret download`: パラメーター `--encoding` のヘルプ メッセージを改善します

### <a name="network"></a>ネットワーク

* az network application-gateway probe:作成および更新時にバックエンド サーバーをプローブするためのポートを指定する --port オプションのサポートを追加しました
* az network application-gateway url-path-map create/update: `--waf-policy` のバグ修正
* az network application-gateway:`--rewrite-rule-set` のサポートを追加しました
* az network list-service-aliases:サービス エンドポイント ポリシーに使用できるサービス別名を一覧表示するためのサポートを追加しました
* az network dns zone import:レコード名での .@ のサポートを追加しました

### <a name="packaging"></a>梱包

* pip インストール用のバック エッジ ビルドを追加しました
* Ubuntu eoan パッケージを追加しました

### <a name="policy"></a>ポリシー

* ポリシー API バージョン 2019-09-01 のサポートを追加しました。
* az policy set-definition:`--definition-groups` パラメーターを使用したポリシー セット定義内のグループ化のサポートを追加しました

### <a name="redis"></a>Redis

* `az redis create` コマンドにプレビュー パラメーター `--replicas-per-master` を追加しました
* azure-mgmt-redis を 6.0.0 から 7.0.0rc1 に更新しました

### <a name="servicefabric"></a>ServiceFabric

* #10963 を含む node-type 追加ロジックを修正:持続性レベル Gold で新しいノード タイプを追加すると、常に CLI エラーがスローされる
* 作成テンプレートの ServiceFabricNodeVmExt バージョンを 1.1 に更新しました

### <a name="sql"></a>SQL

* 読み取りスケール管理をサポートするために、sql db create および update コマンドに "--read-scale" と "--read-replicas" パラメーターを追加しました。

### <a name="storage"></a>ストレージ

* GA リリースでの storage account create および update コマンド用の大きいファイルの共有プロパティ
* GA リリースでのユーザー委任 SAS トークンのサポート
* ストレージ アカウントの Blob service のプロパティを管理するための新しいコマンド `az storage account blob-service-properties show` と `az storage account blob-service-properties update --enable-change-feed` が追加されました。
* [今後の破壊的変更] `az storage copy`: `*` 文字は URL 内のワイルドカードとしてサポートされなくなりましたが、新しいパラメーターの --include-pattern と --exclude-pattern が `*` ワイルドカードをサポートして追加されます。
* 問題 #11043 を修正:`az storage remove` コマンドでコンテナー/共有全体を削除するためのサポートを追加しました

## <a name="november-26-2019"></a>2019 年 11 月 26 日

バージョン 2.0.77

### <a name="acr"></a>ACR

* acr task create/update からパラメーター `--branch` が非推奨になりました

### <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

* 監視を使用して Azure Red Hat Openshift クラスターを作成できるように、`--workspace-resource-id` フラグを追加しました
* 監視を使用して Azure Red Hat OpenShift クラスターを作成するために `monitor_profile` を追加しました

### <a name="aks"></a>AKS

* "az aks rotate-certs" を使用したクラスター証明書ローテーション操作のサポートを追加しました。

### <a name="appconfig"></a>AppConfig

* `as az appconfig kv import` の区切り文字に ":" を使用するためのサポートを追加しました
* null ラベルを含む複数のラベルを持つキー値の一覧表示に関する問題を修正しました。 
* 管理プレーン SKD である azure-mgmt-appconfiguration を バージョン 0.3.0 に更新しました。 

### <a name="appservice"></a>AppService

* 問題 #11100 を修正:サービス プランの作成時の az webapp up の AttributeError
* az webapp up:サポートされている言語のサイトに作成またはデプロイを強制します。既定値は使用されません。
* App Service Environment のサポートを追加しました: az appservice ase show | list | list-addresses | list-plans | create | update | delete

### <a name="backup"></a>バックアップ

* az backup policy list-associated-items の問題を修正しました。 省略可能な BackupManagementType パラメーターを追加しました。

### <a name="compute"></a>Compute

* コンピューティング、ディスク、スナップショットの API バージョンを 2019-07-01 にアップグレードしました
* vmss create: --orchestration-mode の改善
* sig image-definition create: --os-state を追加して、このイメージの下に作成された仮想マシンを "一般化" するか、または "特殊化" するかを指定できるようにしました
* sig image-definition create: --hyper-v-generation を追加して、ハイパーバイザーの生成を指定できるようにしました
* sig image-version create: --os-snapshot と --data-snapshots のサポートを追加しました
* image create: --data-disk-caching を追加して、データ ディスクのキャッシュ設定を指定できるようにしました
* Python Compute SDK を 10.0.0 にアップグレードしました
* vm/vmss create: 'Priority' 列挙型プロパティに 'Spot' を追加しました
* [破壊的変更] Swagger と Powershell のコマンドレットとの一貫性を維持するために、VM と VMSS の両方で '--max-billing' パラメーターを '--max-price' に変更しました
* vm monitor log show: リンクされた Log Analytics ワークスペースに対してログのクエリを実行するためのサポートを追加しました。

### <a name="iot"></a>IoT

* #2531 の修正: ハブの更新に便利な引数を追加しました。
* #8323 の修正: ストレージ カスタム エンドポイントを作成するために不足しているパラメーターを追加しました。
* 回帰バグの修正: 既定のストレージ エンドポイントをオーバーライドする変更を元に戻しました。

### <a name="key-vault"></a>Key Vault

* #11121 を修正しました: `az keyvault certificate list` を使用する場合、`--include-pending` を渡すときに `true` または `false` の値が必須ではなくなりました。

### <a name="netappfiles"></a>NetAppFiles

* azure-mgmt-netapp を 0.7.0 にアップグレードしました。これには、今後のレプリケーション操作に関連した追加のボリューム プロパティが含まれています。

### <a name="network"></a>ネットワーク

* application-gateway waf-config: 非推奨になりました
* application-gateway waf-policy: 管理されているルール セットと除外ルールを管理するために、サブグループ managed-rules を追加しました
* application-gateway waf-policy: waf-policy のグローバル構成を管理するために、サブグループ policy-setting を追加しました
* [破壊的変更] application-gateway waf-policy: subgroup rule を custom-rule に名前変更しました
* application-gateway http-listener: 作成時の --firewall-policy を追加しました
* application-gateway url-path-map rule: 作成時の --firewall-policy を追加しました

### <a name="packaging"></a>梱包

* az ラッパーを Python で書き直しました
* Python 3.8 のサポートを追加しました
* RPM パッケージで Python 3 に変更しました

### <a name="profile"></a>プロファイル

* Microsoft アカウントで `az login -u {} -p {}` を実行するときのエラーを改善しました
* 自己署名ルート証明書使用してプロキシの背後で `az login` を実行するときの `SSLError` を改善しました
* #10578 を修正: Windows または WSL で同時に複数のインスタンスを起動すると `az login` がハングする
* #11059 を修正: テナントにサブスクリプションがあると `az login --allow-no-subscriptions` が失敗する
* #11238 を修正: サブスクリプションの名前を変更した後、MSI を使用してログインすると、同じサブスクリプションが 2 回表示される

### <a name="rbac"></a>RBAC

* #10996 を修正: `--password` が指定されていないときの `az ad user update` での `--force-change-password-next-login` のエラーを改善

### <a name="redis"></a>Redis

* #2902 を修正: Basic SKU キャッシュの更新中、メモリ構成を設定しない

### <a name="reservations"></a>Reservations

* SDK バージョンを 0.6.0 にアップグレードしました
* Gat-Gatalogs を呼び出した後の billingplan の詳細情報を追加しました
* 予約の価格を計算するための新しいコマンド `az reservations reservation-order calculate` を追加しました
* 新しい予約を購入するための新しいコマンド `az reservations reservation-order purchase` を追加しました

### <a name="rest"></a>REST
* `az rest` を GA に変更しました

### <a name="sql"></a>SQL

* azure-mgmt-sql をバージョン 0.15.0 に更新しました。

### <a name="storage"></a>ストレージ

* storage account create: Blob service でのファイルシステムのセマンティクスをサポートするために、--enable-hierarchical-namespace を追加しました。
* エラー メッセージから関連性のない例外を削除しました
* "この操作を実行するのに必要なアクセス許可がありません" という誤ったエラー メッセージが ネットワーク ルールまたは AuthenticationFailed によってブロックされた場合に表示される問題を修正しました。

## <a name="november-4-2019"></a>2019 年 11 月 4 日

バージョン 2.0.76

### <a name="acr"></a>ACR

* コマンド `az acr pack build` にプレビュー パラメーター `--pack-image-tag` を追加しました。
* レジストリ作成時の監査を有効にするためのサポートを追加しました
* レジストリ スコープが設定された RBAC のサポートを追加しました

### <a name="aks"></a>AKS

* `az aks create` コマンドに `--enable-cluster-autoscaler`、`--min-count`、および `--max-count` を追加しました。これにより、ノード プールのクラスター オートスケーラーが有効になります。
* 上記のフラグに加えて、`--update-cluster-autoscaler` および `--disable-cluster-autoscaler` を `az aks update` コマンドに追加し、クラスター オートスケーラーを更新できるようにしました。

### <a name="appconfig"></a>AppConfig

* App Configuration に格納される機能フラグを管理するために、appconfig feature コマンド グループを追加しました。
* ファイルに対する appconfig kv export コマンドの軽微なバグを修正しました。 エクスポート中に宛先ファイルの内容を読み取らないようにしました。

### <a name="appservice"></a>AppService

* `az appservice plan create`:appservice plan create に "persitescaling" を設定するためのサポートを追加しました。
* webapp config ssl bind 操作がリソースから既存のタグを削除する問題を修正しました
* 関数アプリのデプロイ時にリモート ビルド アクションをサポートするために、`az functionapp deployment source config-zip` に `--build-remote` フラグを追加しました。
* Windows 向けの関数アプリの既定のノードバージョンを 10 に変更しました
* `az functionapp create` に `--runtime-version` プロパティを追加しました

### <a name="arm"></a>ARM

* `az deployment/group deployment validate`:デプロイ時に、json テンプレートで複数行とコメントをサポートするために `--handle-extended-json-format` パラメーターを追加しました。
* azure-mgmt-resource を 2019-07-01 に引き上げました

### <a name="backup"></a>バックアップ

* AzureFiles のバックアップ サポートを追加しました

### <a name="compute"></a>Compute

* `az vm create`:高速ネットワークと既存の NIC を一緒に指定した場合の警告を追加しました。
* `az vm create`:仮想マシンを割り当てる必要がある既存の仮想マシン スケール セットを指定するための `--vmss` を追加しました。
* `az vm/vmss create`:イメージ エイリアス ファイルのローカル コピーを追加し、制限付きネットワーク環境でそれにアクセスできるようにしました。
* `az vmss create`:スケール セットによる仮想マシンの管理方法を指定するための `--orchestration-mode` を追加しました。
* `az vm/vmss update`:Ultra SSD 設定を更新できるように `--ultra-ssd-enabled` を追加しました。
* [破壊的変更] `az vm extension set`:ユーザーが `--ids` を使用して、VM に拡張機能を設定できないバグを修正しました。
* Azure Marketplace イメージの使用条件を管理するための新しいコマンド `az vm image terms accept/cancel/show` を追加しました。
* VMAccessForLinux をバージョン 1.5 に更新しました

### <a name="cosmosdb"></a>Cosmos DB

* [破壊的変更] `az sql container create`:`--partition-key-path` を必須パラメーターに変更しました
* [破壊的変更] `az gremlin graph create`:`--partition-key-path` を必須パラメーターに変更しました
* `az sql container create`:`--unique-key-policy` および `--conflict-resolution-policy` を追加しました
* `az sql container create/update`:既定のスキーマ `--idx` を更新しました
* `gremlin graph create`:`--conflict-resolution-policy` を追加しました
* `gremlin graph create/update`:既定のスキーマ `--idx` を更新しました
* ヘルプ メッセージの誤りを修正しました
* データベース:非推奨の情報を追加しました
* コレクション:非推奨の情報を追加しました

### <a name="iot"></a>IoT

* 新しいルーティング ソースの種類を追加しました: DigitalTwinChangeEvents
* `az iot hub create` の不十分な機能を修正しました

### <a name="key-vault"></a>Key Vault

* 証明書ファイルが存在しない場合の予期しないエラーを修正しました
* `az keyvault recover/purge` が機能しない問題を修正しました

### <a name="netappfiles"></a>NetAppFiles

* API バージョン 2019-07-01 を使用するために azure-mgmt-netapp を 0.6.0 にアップグレードしました。 この新しい API バージョンには以下の変更内容が含まれます。

    - ボリューム作成の `--protocol-types` が "NFSv4" ではなく "NFSv 4.1" を受け入れるようになりました
    - ボリューム エクスポート ポリシー プロパティの名前が "nfsv4" ではなく "nfsv41" に変更されました
    - ボリュームの `--creation-token` の名前が `--file-path` に変更されました
    - スナップショット作成日の名前が単に "created" に変更されました

### <a name="network"></a>ネットワーク

* `az network private-dns link vnet create/update`:テナント間の仮想ネットワーク リンクがサポートされています。
* [破壊的変更] `az network vnet subnet list`:`--resource-group` と `--vnet-name` を必須に変更しました。
* `az network public-ip prefix create`:作成時に IP アドレスのバージョン (IPv4、IPv6) を指定するためのサポートを追加しました
* azure-mgmt-network を 7.0.0 に、api-version を 2019-09-01 に引き上げました
* `az network vrouter`:新しいサービスの仮想ルーターと仮想ルーター ピアリングのサポートを追加しました
* `az network express-route gateway connection`:`--internet-security` のサポートを追加しました

### <a name="profile"></a>プロファイル

* `az account get-access-token --resource-type ms-graph` が機能しない問題を修正しました
* `az login` からの警告を削除しました

### <a name="rbac"></a>RBAC

* `az ad app update --id {} --display-name {}` が機能しない問題を修正しました

### <a name="servicefabric"></a>ServiceFabric

* `az sf cluster create`:Service Fabric の Linux と Windows の template.json コンピューティング vmss を標準からマネージド ディスクに変更することによって問題を修正しました

### <a name="sql"></a>SQL

* 次の新しい SQL Database オファリングの CRUD 操作をサポートするために、`--compute-model`、`--auto-pause-delay`、および `--min-capacity` パラメーターを追加しました:サーバーレス コンピューティング モデル。

### <a name="storage"></a>ストレージ

* `az storage account create/update`:Azure Files Active Directory Domain Services 認証をサポートするために、--enable-files-adds パラメーターと Azure Active Directory プロパティ引数グループを追加しました
* ストレージ アカウントの Kerberos キーの一覧表示または再生成をサポートするために、`az storage account keys list/renew` を拡張しました。

## <a name="october-15-2019"></a>2019 年 10 月 15 日

バージョン 2.0.75

### <a name="aks"></a>AKS

* Kubernetes のバージョンでサポートされている場合、`--load-balancer-sku` の既定値を `standard` に変更しました
* Kubernetes のバージョンでサポートされている場合、`--vm-set-type` の既定値を `virtualmachinescalesets` に変更しました

### <a name="ams"></a>AMS

* [破壊的変更]`job start` の名前を `job create` に変更しました
* [破壊的変更]`content-key-policy create` の `--ask` パラメーターが UTF8 ではなく 32 文字の 16 進文字列を使用するように変更しました

### <a name="appservice"></a>AppService

* コマンド `webapp config access-restriction show|set|add|remove` を追加しました
* より優れたエラー処理を `webapp up` に追加しました
* `Isolated` SKU のサポートを `appservice plan update` に追加しました

### <a name="arm"></a>ARM

* JSON テンプレートで複数行とコメントをサポートするための `--handle-extended-json-format` パラメーターを `deployment create` に追加しました

### <a name="compute"></a>Compute

* `--enable-agent` パラメーターを `vm create` に追加しました
* ゾーンを使用するときに標準のパブリック IP SKU を自動的に使用するように `vm create` を変更しました
* VM の有効なコンピューター名が指定されていない場合に自動的に作成されるように `vm create` を変更しました
* VMSS 内の仮想マシンのカスタム コンピューター名プレフィックスをサポートするために `--computer-name-prefix` パラメーターを `vmss create` に追加しました
* ログ分析ワークスペースを自動的に有効にする `--workspace` パラメーターを `vm create` に追加します
* ギャラリーの API バージョンが 2019-07-01 に更新されました

### <a name="core"></a>コア

* 汎用の update コマンドで `--set` パラメーターの構文チェックを追加しました

### <a name="iot"></a>IoT

* `iot hub show` で "リソースが見つかりません" のエラーが誤って発生する問題を修正しました

### <a name="monitor"></a>モニター

* CRUD のサポートを `monitor log-analytics workspace` に追加しました

### <a name="network"></a>ネットワーク

* クロステナントの仮想リンクのサポートを `network private-dns link vnet [create|update]` に追加しました
* [破壊的変更]`network vnet subnet list` で `--resource-group` および `--vnet-name` パラメーターが必須に変更になりました

### <a name="sql"></a>SQL

* マネージ インスタンスの AAD 管理者の設定をサポートするコマンドを `sql mi ad-admin` に追加しました

### <a name="storage"></a>ストレージ

* サービス間のコピー中にアクセス層を保持する `--preserve-s2s-access-tier` パラメーターを `storage copy` に追加しました
* ストレージ アカウントで大容量ファイルの共有をサポートするために `--enable-large-file-share` パラメーターを `storage account [create|update]` に追加しました

## <a name="september-24-2019"></a>2019 年 9 月 24 日

バージョン 2.0.74

### <a name="acr"></a>ACR

* 必須の `--type` パラメーターを `acr config retention update` に追加しました
* [破壊的変更] `acr config` コマンド グループでパラメーター `--name -n` の名前が `--registry -r ` に変更されました

### <a name="aks"></a>AKS

* `--load-balancer-sku` パラメーターを `aks create` コマンドに追加しました。これにより、SLB を使用する AKS クラスターを作成できます
* `--load-balancer-managed-outbound-ip-count`、`--load-balancer-outbound-ips`、`--load-balancer-outbound-ip-prefixes` パラメーターを `aks [create|update]` コマンドに追加しました。これにより、SLB を使用する AKS クラスターのロード バランサー プロファイルを更新できます
* `--vm-set-type` パラメーターを `aks create` コマンドに追加しました。これにより、AKS クラスターの VM の種類 (vmas または vmss) を指定できます

### <a name="arm"></a>ARM

* JSON テンプレートで複数行とコメントをサポートするための `--handle-extended-json-format` パラメーターを `group deployment create` コマンドに追加しました

### <a name="compute"></a>Compute

* 終了のスケジュール化されたイベントを構成しやすくするために `--terminate-notification-time` パラメーターを `vmss [create|update]` コマンドに追加しました
* 終了のスケジュール化されたイベントを構成しやすくするために `--enable-terminate-notification` パラメーターを `vmss update` コマンドに追加しました
* `--priority,` `--eviction-policy,` `--max-billing` パラメーターを `[vm|vmss] create` コマンドに追加しました
* `disk create` を変更し、ディスク アップロードの正確なサイズを指定できるようにしました
* マネージド ディスクの増分スナップショットのサポートを `snapshot create` に追加しました

### <a name="cosmos-db"></a>Cosmos DB

* キー、読み取り専用キー、または接続文字列を表示する `--type <key-type>` パラメーターを `cosmosdb keys list` コマンドに追加しました
* `cosmosdb keys regenerate` コマンドを追加しました
* [非推奨]`cosmosdb list-connection-strings`、`cosmosdb regenerate-key`、`cosmosdb list-read-only-keys` コマンドを非推奨にしました

### <a name="eventgrid"></a>EventGrid

* 正しいパラメーターを参照するようにエンドポイントのヘルプ テキストを修正しました

### <a name="key-vault"></a>Key Vault

* テナントを使用してログインすると (`login -t`)、`keyvault create` が失敗する問題を修正しました

### <a name="monitor"></a>モニター

* `monitor metrics alert create` の `--condition` 引数で `:` 文字を使用できないという問題を修正しました

### <a name="policy"></a>ポリシー

* Policy API バージョン 2019-06-01 のサポートを追加しました
* `policy assignment create` コマンドに `--enforcement-mode` パラメーターを追加しました

### <a name="storage"></a>ストレージ

* `az storage copy` コマンドに `--blob-type` パラメーターを追加しました

## <a name="september-10-2019"></a>2019 年 9 月 10 日

### <a name="acr"></a>ACR

* アイテム保持ポリシーを構成するためのコマンド グループ `acr config retention` を追加しました

### <a name="aks"></a>AKS

* 次のコマンドを使用した ACR 統合のサポートが追加されました。
  * AKS クラスターに ACR をアタッチするための `--attach-acr` パラメーターを `aks [create|update]` に追加しました
  * AKS クラスターから ACR をデタッチするための `--detach-acr` パラメーターを `aks update` に追加しました

### <a name="arm"></a>ARM

* API バージョン 2019-05-10 を使用するように更新しました

### <a name="batch"></a>Batch

* `batch pool create` の `--json-file` に新しい JSON 構成設定を追加しました。
  * ファイル システム マウント用の `MountConfigurations` を追加しました (詳細については https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body を参照してください)
  * プール上のパブリック IP 用に、`NetworkConfiguration` にオプションのプロパティ `publicIPs` を追加しました (詳細については https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body を参照してください)
* 共有イメージ ギャラリーのサポートを `--image` に追加しました
* [破壊的変更]`batch pool create` の `--start-task-wait-for-success` の既定値を `true` に変更しました
* [破壊的変更]`AutoUserSpecification` の `Scope` の既定値が常に Pool になるように変更しました (以前は Windows ノードでは `Task`、Linux ノードでは `Pool` でした)
  * この引数は、`--json-file` を使用して JSON 構成からのみ設定できます。

### <a name="hdinsight"></a>HDInsight

* GA リリース
* [破壊的変更]`az hdinsight resize` のパラメーター `--workernode-count/-c` が必須に変更されました。

### <a name="key-vault"></a>Key Vault

* ネットワーク ルールからサブネットを削除できなかった問題を修正しました
* 重複したサブネットと IP アドレスをネットワーク ルールに追加できる問題を修正しました

### <a name="network"></a>ネットワーク

* トラフィック分析間隔の値を設定する `--interval` パラメーターを `network watcher flow-log` に追加しました
* ゲートウェイ ID を管理するための `network application-gateway identity` を追加しました
* Key Vault ID を設定するためのサポートを `network application-gateway ssl-cert` に追加しました
* `network express-route peering peer-connection [show|list]` を追加しました

### <a name="policy"></a>ポリシー

* API バージョン 2019-01-01 を使用するように更新しました

## <a name="august-27-2019"></a>2019 年 8 月 27 日

バージョン 2.0.72

### <a name="acr"></a>ACR

* [破壊的変更]`classic` SKU のサポートを削除しました

### <a name="api-management"></a>API Management

* [プレビュー] `apim` コマンド グループを追加しました

### <a name="appservice"></a>AppService

* スロットを指定したときの `webapp webjob continuous start` コマンドに関する問題を修正しました
* `env` フォルダーを検出してデプロイに使用されているファイルから削除するように `webapp up` を変更しました

### <a name="keyvault"></a>KeyVault

* `--expires` 引数を無視する `keyvault secret set` のバグを修正しました

### <a name="network"></a>ネットワーク

* `--private-ip-address-version` 引数に IPv6 アドレスのサポートを追加しました
* プライベート エンドポイントの管理用に新しいコマンド `network private-endpoint [create|update|list-types]` を追加しました
* コマンド グループ `network private-link-service` を追加しました
* `--private-endpoint-network-policies` 引数と `--private-link-service-network-policies` 引数を `network vnet subnet update` に追加しました

### <a name="rbac"></a>RBAC

* ホームページが更新されない `ad app update --homepage` に関する問題を修正しました

### <a name="servicefabric"></a>ServiceFabric

* キー コンテナーの大文字と小文字が混在する名前のサポートを追加しました
* Key Vault で証明書を使用したときの問題を修正しました
* PFX 証明書ファイルの使用に関する問題を修正しました
* Key Vault リソースグループが指定されていなかったときの `sf cluster certificate add` に関する問題を修正しました
* `sf cluster set` が動作しない問題を修正しました

### <a name="signalr"></a>SignalR

* 次の新しいコマンドを追加しました
  * `signalr cors`:SignalR CORS の管理
  * `signalr restart`:SignalR Service の再起動
  * `signalr update`:SignalR Service の更新
* `--service-mode` 引数を `signalr create` に追加しました

### <a name="storage"></a>ストレージ

* `storage account revoke-delegation-keys` コマンドを追加しました

## <a name="august-13-2019"></a>2019 年 8 月 13 日

バージョン 2.0.71

### <a name="appservice"></a>AppService

* `webapp webjob continuous` コマンドがスロットで失敗する問題を修正しました

### <a name="botservice"></a>BotService

* [破壊的変更] v3 SDK ボットの作成のサポートを削除しました

### <a name="cognitiveservices"></a>CognitiveServices

* `cognitiveservices account network-rule` コマンドを追加しました

### <a name="cosmos-db"></a>Cosmos DB

* 複数の書き込み場所を更新するときの警告を削除しました
* CosmosDB SQL、MongoDB、Cassandra、Gremlin、およびテーブル リソースとリソースのスループットのための CRUD コマンドを追加しました

### <a name="hdinsight"></a>HDInsight

このリリースには、多数の破壊的変更が含まれています。

* [破壊的変更]`hdinsight create` のパラメーターの名前を変更しました。
  * `--storage-default-container` から `--storage-container` への名称変更
  * `--storage-default-filesystem` から `--storage-filesystem` への名称変更
* [破壊的変更]`application create` の `--name` 引数が、クラスター名の代わりにアプリケーション名を表すように変更されました
* `--cluster-name` 引数が `application create` に追加され、以前の `--name` の機能に置き換わりました
* [破壊的変更]`application create` のパラメーターの名前を変更しました。
  * `--application-type` から `--type` への名称変更
  * `--marketplace-identifier` から `--marketplace-id` への名称変更
  * `--https-endpoint-access-mode` から `--access-mode` への名称変更
  * `--https-endpoint-destination-port` の名前を `--destination-port` に変更しました
* [破壊的変更]`application create` のパラメーターを削除しました。
  * `--https-endpoint-location`
  * `--https-endpoint-public-port`
  * `--ssh-endpoint-destination-port`
  * `--ssh-endpoint-location`
  * `--ssh-endpoint-public-port`
* [破壊的変更] `hdinsight resize`の `--target-instance-count` の名前を `--workernode-count` に変更しました
* [破壊的変更]`hdinsight script-action` グループのすべてのコマンドが、スクリプト アクションの名前として `--name` パラメーターを使用するように変更されました。
* `--cluster-name` 引数がすべての `hdinsight script-action` コマンドに追加され、以前の `--name` の機能に置き換わりました
* [破壊的変更] すべての `hdinsight script-action` コマンドで `--script-execution-id` の名前を `--execution-id` に変更しました
* [破壊的変更] 名前を `hdinsight script-action show` から `hdinsight script-action show-execution-details` に変更しました
* [破壊的変更] `hdinsight script-action execute --roles` に対するパラメーターを、コンマ区切りではなくスペース区切りにしました
* [破壊的変更]`hdinsight script-action list`の `--persisted` パラメーターを削除しました
* ローカル JSON ファイルへのパスまたは JSON 文字列を受け入れるように `hdinsight create --cluster-configurations` パラメーターを変更しました
* `hdinsight script-action list-execution-history` コマンドを追加しました
* Log Analytics ワークスペース ID またはワークスペース名を受け入れるように `hdinsight monitor enable --workspace` を変更しました
* `hdinsight monitor enable --primary-key` 引数を追加しました。これは、ワークスペース ID がパラメーターとして指定されている場合に必要です
* 例をさらに追加し、ヘルプ メッセージの説明を更新しました

### <a name="interactive"></a>Interactive

* 読み込みエラーを修正しました

### <a name="kubernetes"></a>Kubernetes

* ダッシュボードのコンテナー ポートで `https` が使用されている場合に `https` を使用するように変更しました

### <a name="network"></a>ネットワーク

* `--yes` 引数を `network dns record-set cname delete` に追加しました

### <a name="profile"></a>プロファイル

* リソースのアクセス トークンを取得するための `account get-access-token` に `--resource-type` 引数を追加しました

### <a name="servicefabric"></a>ServiceFabric

* sf cluster create でサポートされているすべての OS バージョンを追加しました
* プライマリ証明書の検証のバグを修正しました

### <a name="storage"></a>ストレージ

* `storage copy` コマンドを追加しました

## <a name="july-30-2019"></a>2019 年 7 月 30 日

バージョン 2.0.70

### <a name="acr"></a>ACR

* 問題 #9952 (`acr pack build` コマンドでの回帰) を修正しました
* `acr pack build` の既定のビルダー イメージ名を削除しました

### <a name="appservice"></a>Appservice

* リソースが見つからない場合にメッセージ表示するように `webapp config ssl` を変更しました
* `functionapp create` がストレージ アカウントの種類 `Standard_RAGRS` を受け入れない問題を修正しました
* 古いバージョンの python を使用して `webapp up` を実行すると失敗する問題を修正しました

### <a name="network"></a>ネットワーク

* `network nic ip-config add` から無効なパラメーター `--ids` を削除しました (#9861 の修正)
* #9604 を修正しました。 信頼されたルート証明書へのユーザーの関連付けをサポートするために、`network application-gateway http-settings [create|update]` に `--root-certs` パラメーターを追加しました。
* `network dns record-set ns create` の引数 `--subscription` を修正しました (#9965)

### <a name="rbac"></a>RBAC

* `user update` コマンドを追加しました
* [非推奨] ユーザー関連コマンドで `--upn-or-object-id` を非推奨にしました
    * 代替引数の `--id` を使用してください
* ユーザー関連コマンドに `--id` 引数を追加しました

### <a name="sql"></a>SQL

* マネージド インスタンス キーおよび TDE 保護機能の管理コマンドを追加しました

### <a name="storage"></a>ストレージ

* `storage remove` コマンドを追加しました
* `storage blob update` での問題を修正しました

### <a name="vm"></a>VM

* ゾーンの詳細を出力するための新しい API バージョンを使用するように `list-skus` を変更しました
* `vmss create` の `--single-placement-group` の既定値を`false` に変更しました
* `[snapshot|disk] create` において ZRS ストレージ SKU を選択する機能を追加しました
* 専用ホストをサポートするために、新しいコマンド グループ `vm host` を追加しました
* VM 専用ホストを設定するためのパラメーター `--host` と `--host-group` を `vm create` に追加しました

## <a name="july-16-2019"></a>2019 年 7 月 16 日

バージョン 2.0.69

### <a name="appservice"></a>Appservice

* ResourceGroupName または App の名前が無効な場合に適切なエラー メッセージを返すように `webapp identity` コマンドが変更されました
* ResourceGroup が指定されなかった場合に numberOfSites の正しい値を返すように `webapp list` が修正されました
* `appservice plan create` と `webapp create` の副作用が修正されました

### <a name="core"></a>コア

* `--subscription` が適用不可にもかかわらず表示される問題が修正されました

### <a name="batch"></a>Batch

* [破壊的変更]`batch pool node-agent-skus list` が `batch pool supported-images list` に置き換えられました
* `batch pool create network` の `--json-file` オプションを使用するときに、トラフィックのソース ポートに基づいてプールへのネットワーク アクセスをブロックするセキュリティ規則のサポートが追加されました
* `batch task create`の `--json-file` オプションを使用するときに、コンテナーの作業ディレクトリまたは Batch タスクの作業ディレクトリでタスクを実行するためのサポートが追加されました
* `batch pool create`の `--application-package-references` オプションが、既定値でのみ動作するというエラーが修正されました

### <a name="eventhubs"></a>イベント ハブ

* `authorizationrule`コマンドのパラメーター `--rights` の検証が追加されました

### <a name="rdbms"></a>RDBMS

* レプリカ作成コマンドでレプリカ SKU を指定するためのオプション パラメーターが追加されました
* MySQL レプリカの作成で CI テストが失敗する問題が修正されました

### <a name="relay"></a>リレー

* クライアント承認が無効になっているときのハイブリッド接続の問題が修正されました ([#8775](https://github.com/azure/azure-cli/issues/8775))
* パラメーター `--requires-transport-security` を `relay wcfrelay create` に追加しました

### <a name="servicebus"></a>ServiceBus

* `authorizationrule`コマンドのパラメーター `--rights` の検証が追加されました

### <a name="storage"></a>ストレージ

* ストレージ アカウントの更新で Files AADDS を有効にします
* 修正された問題 `storage blob service-properties update --set`

## <a name="july-2-2019"></a>2019 年 7 月 2 日

バージョン 2.0.68

### <a name="core"></a>コア

* コマンド モジュールが再頒布可能な 1 つの Python コードに統合されました。 これにより、PyPI での多くの `azure-cli-` パッケージの直接使用が廃止されます。
  またこれにより、インストール サイズが削減され、`pip` 経由で直接インストールしたユーザーにのみ影響が出ます。

### <a name="acr"></a>ACR

* タスクへのタイマー トリガーのサポートが追加されました

### <a name="appservice"></a>Appservice

* 既定でアプリケーション インサイトを有効にするように `functionapp create` を変更しました
* [破壊的変更] 非推奨の `functionapp devops-build` コマンドを削除しました。
  *  代わりに新しいコマンド `az functionapp devops-pipeline` を使用
* Linux 従量課金プランの関数アプリのプラン サポートを `functionapp deployment config-zip` に追加しました

### <a name="cosmos-db"></a>Cosmos DB

* ID の無効化に対応するようになりました

### <a name="dls"></a>DLS

* ADLS バージョンを更新しました (0.0.45)

### <a name="feedback-command"></a>フィードバック コマンド

* 失敗した拡張機能コマンドを報告するときに、`az feedback` はインデックスからの拡張機能のプロジェクト/リポジトリの url にブラウザーを開こうとするようになりました

### <a name="hdinsight"></a>HDInsight

* [破壊的変更]`oms` コマンド グループ名を `monitor` に変更しました
* [破壊的変更]`--http-password/-p` が必須パラメーターになりました 
* `--cluster-admin-account` および `cluster-users-group-dns` パラメーター入力候補の入力候補を追加しました 
* `cluster-users-group-dns` パラメーターを `—esp` が存在するときに必要となるように変更しました
* 既存の引数自動オートコンプリートすべてにタイムアウトを追加しました
* リソース名をリソース id に変換する際のタイムアウトを追加しました
* 任意のリソース グループからリソースを選択するようにオートコンプリートを変更しました。 `-g` で指定されるリソース グループとは別のものにすることができます。
* `hdinsight application create` コマンドで `--sub-domain-suffix` および `--disable_gateway_auth` パラメーターのサポートを追加しました

### <a name="managed-services"></a>マネージド サービス

* プレビューにマネージド サービス コマンド モジュールを導入

### <a name="profile"></a>プロファイル
* ログアウト コマンドの `--subscription` 引数を抑制

### <a name="rbac"></a>RBAC

* [破壊的変更]`create-for-rbac` の `--password` 引数を削除しました
* AAD グラフ サーバー レプリケーションの待機時間によって発生する断続的なエラーを回避するために `--assignee-principal-type` パラメーターを `create` コマンドに追加しました
* 所有オブジェクトの一覧表示時の `ad signed-in-user` のクラッシュの問題を修正しました
* `ad sp` によってサービス プリンシパルから適切なアプリケーションが検出されない問題を修正しました

### <a name="rdbms"></a>RDBMS

* MariaDB のレプリケーション サポートを追加しました

### <a name="sql"></a>SQL

* `sql db create --sample-name` に使用できる値を文書化しました

### <a name="storage"></a>ストレージ

* `--as-user` を使用した `storage blob generate-sas` に対するユーザーの委任 SAS トークンのサポートを追加しました 
* `--as-user` を使用した `storage container generate-sas` に対するユーザーの委任 SAS トークンのサポートを追加しました 

### <a name="vm"></a>VM

* `--no-wait` による実行時に `vmss create` によってエラー メッセージが返されるバグを修正しました
* `vmss create --single-placement-group` のクライアント側の検証を削除しました。 `--single-placement-group` が `true` に設定され、`--instance-count` が 100 より大きいか、可用性ゾーンが指定されていても失敗にはならず、この検証はコンピューティング サービスに任されます
* `--latest` と共に使用したときに `[vm|vmss] extension image list` が失敗するバグを修正しました


## <a name="june-18-2019"></a>2019 年 6 月 18 日

バージョン 2.0.67

### <a name="core"></a>コア

このリリースでは、新しい [プレビュー] タグが導入され、コマンド グループ、コマンド、または引数がプレビュー状態である場合に、お客様により明確に通知できるようになりました。 以前は、これはヘルプ テキストで通知されていたか、コマンド モジュールのバージョン番号によって暗黙的に通知されていました。
CLI では、将来、個々のパッケージのバージョン番号が削除される予定です。 コマンドがプレビュー状態の場合は、そのすべての引数もプレビュー状態です。 コマンド グループにプレビュー状態のラベルが付いている場合、すべてのコマンドと引数もプレビュー状態と見なされます。

この変更の結果、このリリースでは、一部のコマンド グループが "突然" プレビュー状態になったように見える場合があります。 ほとんどのパッケージがプレビュー状態にあったのですが、このリリースでは一般提供と見なされていることがその真相です

### <a name="acr"></a>ACR
* 'acr check-health' コマンドを追加しました
* AAD トークンおよび外部コマンドの取得に関するエラー処理を改善しました

### <a name="acs"></a>ACS
* 非推奨の ACS コマンドがヘルプ ビューで非表示になりました

### <a name="ams"></a>AMS
* [破壊的変更] archive-window-length および key-frame-interval-duration の ISO 8601 時間文字列を返すように変更しました

### <a name="appservice"></a>AppService
* `webapp deleted list` および `webapp deleted restore` に対してロケーション ベースのルーティングを追加しました
* Azure Cloud Shell で Web アプリのログに記録されたターゲット URL ("You can launch the app at...") がクリックできない問題を修正しました
* 一部の SKU でのアプリ作成が AlwaysOn エラーで失敗するという問題を修正しました
* `[appservice|webapp] create` に事前検証を追加しました
* 正しい actionHostName を使用するように `[webapp|functionapp] traffic-routing` を修正しました
* `functionapp` コマンドにスロット サポートを追加しました

### <a name="batch"></a>Batch
* 共有キー認証の過度のエラー報告による AAD 認証の回帰を修正しました

### <a name="batchai"></a>BatchAI
* BatchAI コマンドが非推奨となり、非表示になりました

### <a name="botservice"></a>BotService
* v3 SDK をサポートするコマンドに "廃止されたサポート"/"保守モード" 警告メッセージを追加しました

### <a name="cosmosdb"></a>Cosmos DB
* [非推奨]`cosmosdb list-keys` コマンドを非推奨にしました
* `cosmosdb keys list` コマンドを追加しました。`cosmosdb list-keys` に置き換わるものです
* `cosmsodb create/update`:"isZoneRedundant" プロパティを設定できるように --location の新しいフォーマットを追加しました 古いフォーマットを非推奨にしました

### <a name="eventgrid"></a>EventGrid
* ドメイン CRUD 操作のための `eventgrid domain` コマンドを追加しました
* ドメイン トピック CRUD 操作のための `eventgrid domain topic` コマンドを追加しました
* OData 構文を使用して結果をフィルタリングするための `--odata-query` 引数を `eventgrid [topic|event-subscription] list` に追加しました
* `event-subscription create/update`:`--endpoint-type` パラメーターの新しい値として servicebusqueue を追加しました
* [破壊的変更]`eventgrid event-subscription [create|update]` での `--included-event-types All` のサポートを削除しました

### <a name="hdinsight"></a>HDInsight
* `hdinsight create` コマンドでの `--ssh-public-key` パラメーターのサポートを追加しました

### <a name="iot"></a>IoT
* 承認ポリシー キーの再生成のサポートを追加しました
* DigitalTwin リポジトリ プロビジョニング サービスの SDK およびサポートを追加しました

### <a name="network"></a>ネットワーク
* Nat Gateway に対するゾーン サポートを追加しました
* `network list-service-tags` コマンドを追加しました
* ユーザーがワイルドカード A レコードをインポートできない `dns zone import` の問題を修正しました
* 特定のリージョンでフロー ロギングを有効にできない `watcher flow-log configure` の問題を修正しました

### <a name="resource"></a>リソース
* REST 呼び出しを行うための `az rest` コマンドを追加しました
* リソース グループまたはサブスクリプション レベル`--scope` で `policy assignment list` を使用する場合のエラーを修正しました

### <a name="servicebus"></a>ServiceBus
* `servicebus topic create --max-size` での問題 [#9319](https://github.com/azure/azure-cli/issues/9319) を修正しました

### <a name="sql"></a>SQL
* `--location` を `sql [server|mi] create` のオプションに変更しました - 指定されていない場合、リソース グループの場所を使用します
* `sql db list-editions --available` の "'NoneType' object is not iterable" エラーを修正しました

### <a name="sqlvm"></a>SQLVm
* [破壊的変更] `--license-type` パラメーターを必要とするように `sql vm create` を変更しました
* sql vm の作成または更新時に SQL イメージ SKU を設定できるように変更しました

### <a name="storage"></a>ストレージ
* `storage container generate-sas` のアカウント キーが見つからない問題を修正しました
* Linux での `storage blob sync` に関する問題を修正しました

### <a name="vm"></a>VM
* [プレビュー] VM イメージを構築するための `vm image template` コマンドを追加しました

## <a name="june-4-2019"></a>2019 年 6 月 4 日

バージョン 2.0.66

### <a name="core"></a>コア
* `--output yaml` が `--query` と共に使用された場合、コマンドが正常に実行されないバグを修正しました

### <a name="acr"></a>ACR
* Buildpack を使用して簡単なビルド タスクを作成するための 'acr pack' コマンド グループを追加しました。

### <a name="acs"></a>ACS
* AKS kube-dashboard アドオンを有効化/無効化できるようになりました
* サブスクリプションが Azure Red Hat OpenShift を使用するようにホワイトリストに登録されていない場合、わかりやすいメッセージが出力されます

### <a name="batch"></a>Batch
* アカウント \[[#9165](https://github.com/Azure/azure-cli/issues/9165)\]\[[#8978](https://github.com/Azure/azure-cli/issues/8978)\] にログインしていないときのエラー処理を改善しました

### <a name="iot"></a>IoT
* 手動フェールオーバーのサポートを追加しました

### <a name="network"></a>ネットワーク
* カスタム WAF 規則をサポートするように `network application-gateway waf-policy` コマンドを追加しました。
* `--waf-policy` 引数と `--max-capacity` 引数を `network application-gateway [create|update]` に追加しました 

### <a name="resource"></a>リソース
* 使用できる TTY がない場合の `deployment create` からのエラー メッセージを改善しました

### <a name="role"></a>Role
* ヘルプ テキストを更新しました。

### <a name="compute"></a>Compute
* データディスク LUN が 0 から始まらないまたは番号をスキップするマネージド イメージの VM 用の `vm create` のサポートを追加しました

## <a name="may-21-2019"></a>2019 年 5 月 21 日

バージョン 2.0.65

### <a name="core"></a>コア
* 認証エラーのより有意義なフィードバックを追加しました
* CLI でそのコア バージョンと互換性がない拡張機能が読み込まれる問題を修正しました
* `clouds.config` が破損したときの起動時の問題を修正しました

### <a name="acr"></a>ACR
* タスクに対するマネージド ID のサポートを追加しました

### <a name="acs"></a>ACS
* お客様の AAD クライアントでの使用時の `openshift create` コマンドを修正しました

### <a name="appservice"></a>AppService
* [非推奨]`functionapp devops-build` コマンドを非推奨にしました - 次のリリースから削除されます
* 詳細モードで Azure DevOps からビルド ログをフェッチするように `functionapp devops-pipeline` を変更しました
* [破壊的変更]`--use_local_settings` フラグを `functionapp devops-pipeline` コマンドから削除しました (操作なし)
* `--logs` が使用されていないときに、JSON 出力を返すように `webapp up` を変更しました
* `webapp up` 用のローカル構成に既定のリソースを書き込むためのサポートを追加しました
* `--location` 引数を使用しないでアプリを再デプロイするために `webapp up` にサポートを追加しました
* SKU 値が機能しないため Linux Free SKU ASP 作成が無料で使用される問題を修正しました

### <a name="botservice"></a>BotService
* コマンドの `--lang` パラメーターに大文字小文字のすべてが許可されるように変更しました
* コマンド モジュールの説明を更新しました

### <a name="consumption"></a>従量課金
* `consumption usage list --billing-period-name` の実行時に存在しない必須パラメーターを追加しました

### <a name="iot"></a>IoT
* すべてのキーを一覧表示するようサポートを追加しました

### <a name="network"></a>ネットワーク
* [破壊的変更]: Removed `network interface-endpoints` command group - use `network private-endpoints` 
* NAT ゲートウェイへのアタッチ用に `--nat-gateway` 引数を `network vnet subnet [create|update]` に追加しました
* レコード名がレコードの種類と一致しない `dns zone import` の問題を修正しました

### <a name="rdbms"></a>RDBMS
* geo レプリケーション用の postgres と mysql のサポートを追加しました

### <a name="rbac"></a>RBAC
* 管理グループ スコープのサポートを `role assignment` に追加しました

### <a name="storage"></a>ストレージ
* `storage blob sync`: ストレージ BLOB 用の同期コマンドを追加

### <a name="compute"></a>Compute
* VM のコンピューター名設定用に `--computer-name` を `vm create` に追加しました
* `[vm|vmss] create` について `--ssh-key-value` を `--ssh-key-values` に変更しました。複数の ssh 公開キーの値またはパスが受け入れられるようになりました
  * __注__:これは、破壊的変更 "**ではありません**" - `--ssh-key-values` にのみ一致するため `--ssh-key-value` は 適切に解析されます
* `ppg create` の `--type` 引数をオプションに変更しました

## <a name="may-6-2019"></a>2019 年 5 月 6 日

バージョン 2.0.64

### <a name="acs"></a>ACS
* [破壊的変更]`--fqdn` フラグを `openshift` コマンドから削除しました
* Azure Red Hat Openshift GA API Version を使用するように変更しました
* `customer-admin-group-id` フラグを `openshift create` に追加しました
* [GA] `aks create` オプション `--network-policy` から `(PREVIEW)` を削除しました

### <a name="appservice"></a>Appservice
* [非推奨]`functionapp devops-build` コマンドを非推奨にしました
  * 名前を `functionapp devops-pipeline` に変更しました
* `webapp up` が失敗する原因となっていた問題を修正し、CloudShell の正しいユーザー名が取得されるようにしました
* サポートされる appserviceplans を反映するために `appservice plan --sku` のドキュメントを更新しました
* リソース グループとプランの省略可能な引数を `webapp up` に追加しました
* `AZURE_CLI_DISABLE_CONNECTION_VERIFICATION` 環境変数を尊重するために、`webapp ssh` に対するサポートを追加しました
* Linux の無料 SKU に `appserviceplan create` のサポートを追加しました
* kudu コールド スタートを処理するように `SCM_DO_BUILD_DURING_DEPLOYMENT=true` appsetting を設定した後に、`webapp up` が 30 秒間スリープ状態になるように変更しました
* Windows 上で `functionapp create` を実行するために `powershell` ランタイムに対するサポートを追加しました
* `create-remote-connection` コマンドを追加しました

### <a name="batch"></a>Batch
* `--application-package-references` オプションの検証コントロールのバグを修正しました

### <a name="botservice"></a>Botservice
* [破壊的変更] 空の Web アプリ ボットを既定で作成するように `bot create -v v4 -k webapp` を変更しました (つまり、アプリ サービスにデプロイされるボットはありません)
* `-v v4` により以前の動作を使用するように `--echo` フラグを `bot create` に追加しました
* [破壊的変更]`--version` の既定値を `v4` に変更しました
  * __注:__ `bot prepare-publish` ではその以前の既定値を引き続き使用します
* [破壊的変更] 既定値が `Csharp` にならないように `--lang` を変更しました。 コマンドで `--lang` が必要で、これが指定されないと、コマンドでエラーが出力されます
* [破壊的変更]`bot create` が必要になるように、および `ad app create` を通じて作成されるように `--appid` と `--password` 引数を変更しました
* `--appid` および `--password` 検証を追加しました
* [破壊的変更] ストレージ アカウントまたは Application Insights を作成または使用しないように `bot create -v v4` を変更しました
* [破壊的変更] Application Insights を使用できるリージョンを必要とするように `bot create -v v3` を変更しました
* [破壊的変更] ボットの特定のプロパティのみに影響するように `bot update` を変更しました
* [破壊的変更]`Node` の代わりに `Javascript` を受け入れるように `--lang` フラグを変更しました
* [破壊的変更] 許可された `--lang` 値としての `Node` を削除しました
* [破壊的変更]`SCM_DO_BUILD_DURING_DEPLOYMENT` が true に設定されないように `bot create -v v4 -k webapp` を変更しました。 Kudu を通じたすべてのデプロイがその既定の動作に従って動作します
* `.bot` ファイルのないボットで言語固有の構成ファイルが、ボット用のアプリケーション設定からの値により作成されるように `bot download` を変更しました
* `bot prepare-deploy` に `Typescript` のサポートを追加しました
* `--code-dir` に `package.json` が含まれない場合に備えて `Javascript` および `Typescript` ボット用に `bot prepare-deploy` に警告メッセージを追加しました
* 成功した場合に `true` を返すように `bot prepare-deploy` を変更しました
* 詳細ログ記録を `bot prepare-deploy` に追加しました
* さらに多くの使用可能な Application Insights リージョンを `az bot create -v v3` に追加しました

### <a name="configure"></a>構成
* フォルダー ベースの引数の既定値の構成のサポートを追加しました

### <a name="eventhubs"></a>イベント ハブ
* `namespace network-rule` コマンドを追加しました
* ネットワーク ルールの `--default-action` 引数を `namespace [create|update]` に追加しました

### <a name="network"></a>ネットワーク
* [破壊的変更]`vnet [create|update]` 用に `--cache` 引数を `--defer` に置き換えました 

### <a name="policy-insights"></a>ポリシーの分析情報
* リソースに関するポリシー評価の詳細にクエリを実行するために `--expand PolicyEvaluationDetails` のサポートを追加しました

### <a name="role"></a>Role
* [非推奨]`create-for-rbac` '--password' 引数を非表示にするように変更しました。サポートは 2019 年 5 月に削除されます

### <a name="service-bus"></a>Service Bus
* `namespace network-rule` コマンドを追加しました
* ネットワーク ルールの `--default-action` 引数を `namespace [create|update]` に追加しました
* Premium SKU で値 10、20、40、および 80 GB の `--max-size` サポートを許可するように `topic [create|update]` を修正しました

### <a name="sql"></a>SQL
* `sql virtual-cluster [list|show|delete]` コマンドを追加しました

### <a name="vm"></a>VM
* VMSS VM インスタンスの保護ポリシーへの更新を有効にするように `--protect-from-scale-in` および `--protect-from-scale-set-actions` を `vmss update` に追加しました
* VMSS VM インスタンスの汎用的な更新を有効にするように `--instance-id` を `vmss update` に追加しました
* `--instance-id` を `vmss wait` に追加しました
* 近接通信配置グループの管理用に新しい `ppg` コマンド グループを追加しました
* PPG の管理用に `--ppg` を `[vm|vmss] create` および `vm availability-set create` に追加しました
* `--hyper-v-generation` パラメーターを `image create` に追加しました

## <a name="april-23-2019"></a>2019 年 4 月 23 日

バージョン 2.0.63

### <a name="acs"></a>ACS
* 重複する値の上書きを求めるように `aks get-credentials` を変更しました
* Dev Spaces コマンド "aks use-dev-spaces" および "aks remove-dev-spaces" から `(PREVIEW)` を削除しました

### <a name="ams"></a>AMS
* 資産およびアカウント フィルターの更新プログラムによりバグを修正しました

### <a name="appservice"></a>AppService
* ASE のサポートと `webapp ssh` へのタイムアウトを追加しました
* Github リポジトリから関数アプリへ CI CD を確立するためのサポートを Azure DevOps パイプラインに追加しました
* Github 個人用アクセス トークンを受け入れるために、`functionapp devops-build create` に `--github-pat` 引数を追加しました
* functionapp ソース コード を含む Github リポジトリを受け入れるために、`functionapp devops-build create` に `--github-repository` 引数を追加しました
* `az webapp up --logs` がエラーで失敗し、既定の .NETCORE バージョンを 2.1 に更新する問題を修正しました
* 従量課金プランでの関数アプリ作成時の不要な functionapp 設定を削除しました
* SKU オプションに基づいて新しい ASP を作成するために、既定の ASP 文字列の末尾に数字を追加するように `webapp up` を変更しました
* ブラウザーでアプリを起動するためのオプションとして、`webapp up` に `-b` を追加しました
* `AZURE_CLI_DISABLE_CONNECTION_VERIFICATION` 環境変数を処理するように `webapp deployment source config zip` を変更しました

### <a name="deployment-manager"></a>Deployment Manager
* [プレビュー] 展開をサポートする成果物を作成して管理します

### <a name="lab"></a>ラボ
* 早期終了の原因となっていたバグを修正しました

### <a name="network"></a>ネットワーク
* 子ゾーン作成時のネーム サーバーの自動委任を親の `dns zone create` に追加しました

### <a name="resource"></a>リソース
* [非推奨]`resource link` の引数 `--link-id`、`--target-id`、`--filter-string` を廃止しました
  * 代わりに、引数 `--link`、`--target`、および `--filter` を使用します
* `resource link [create|update]` コマンドが機能しない問題を修正しました
* リソース ID を使用した削除がエラーでクラッシュする問題を修正しました

### <a name="sql"></a>SQL
* マネージド インスタンスでのカスタム タイム ゾーンのサポートを追加しました
* `sql db update` でのエラスティック プール名の使用を許可するように変更しました
* `sql server [create|update]` に `--no-wait` のサポートを追加しました
* `sql server wait` コマンドを追加しました

### <a name="storage"></a>ストレージ
* `storage blob generate-sas` での二重エンコードされた SAS トークンの問題を修正しました

### <a name="vm"></a>VM
* シャットダウンせずに VM の電源をオフにするために、`--skip-shutdown`フラグを `vm|vmss stop` に追加しました
* 発行プロファイルのアカウントの種類を設定するために、`sig image-version create` に `--storage-account-type` 引数を追加しました
* リージョン固有のストレージ アカウントの種類を設定できるようにするために、`sig image-version create` に `--target-regions` 引数を追加しました

## <a name="april-9-2019"></a>2019 年 4 月 9 日

### <a name="core"></a>コア
* 一部の拡張機能のバージョンが `Unknown` と表示され、更新できなかった問題を修正しました

### <a name="acr"></a>ACR
* コンテキストと無関係なイメージ実行のサポートが追加されました

### <a name="ams"></a>AMS
* [非推奨]: Deprecated the `--bitrate` parameter of `account-filter` and `asset-filter`
* [破壊的変更]: Renamed the `--bitrate` parameter to `--first-quality`
* `ams streaming-policy create` に新しい暗号化パラメーターのサポートを追加しました
* `ams streaming-locator create` に新しいパラメーター `--filters` を追加しました

### <a name="appservice"></a>AppService
* `webapp up` に `--logs` のサポートを追加しました
* `functionapp devops-build create` コマンドでの `azure-pipelines.yml` の生成に関する問題を修正しました
* `unctionapp devops-build create` のエラー処理とインジケーターを改善しました
* [破壊的変更]`devops-build` コマンドの `--local-git` フラグが削除され、Azure DevOps パイプラインを作成する際のローカル Git の検出と処理は強制となりました
* Linux 関数プラン作成のサポートを追加しました
* `functionapp update --plan` を使用して、関数アプリの基盤になっているプランを切り替える機能を追加しました
* Azure Functions Premium プランのスケールアウト設定のサポートを追加しました

### <a name="cdn"></a>CDN
* `Microsoft_Standard` と `Standard_ChinaCdn` のサポートを追加しました

### <a name="feedback-command"></a>フィードバック コマンド
* 最近実行されたコマンドのメタデータを表示するように `feedback` を変更しました
* ブラウザーを開き、問題テンプレートを使用して、問題作成プロセスの支援をユーザーに促すよう `feedback` を変更しました
* "--verbose" を指定して実行すると、問題の本文が出力されるように `feedback` を変更しました

### <a name="monitor"></a>モニター
* `metrics alert [create|update]` で "Count" が許容値ではなかった問題を修正しました 

### <a name="network"></a>ネットワーク
* `vnet-gateway list-bgp-peer-status` で表形式が表示されない問題を修正しました
* `application-gateway rewrite-rule` に `list-request-headers` コマンドと `list-response-headers` コマンドを追加しました
* `application-gateway rewrite-rule condition` に `list-server-variables` コマンドを追加しました
* ExpressRoute Port のリンク状態を更新すると、属性不明例外 `express-route port update` がスローされる問題を修正しました

### <a name="privatedns"></a>プライベート DNS
* プライベート DNS ゾーンに `network private-dns` を追加しました

### <a name="resource"></a>リソース
* 問題を修正しました空のパラメーター セットが含まれるパラメーター ファイルが機能しない、`deployment create` と `group deployment create` に関する問題を修正しました

### <a name="role"></a>Role
* `create-for-rbac` で `--years` が正しく処理されるように修正しました
* [破壊的変更] サブスクリプションのすべての割り当てを無条件に削除する場合、プロンプトを表示するように `role assignment delete` を修正しました

### <a name="sql"></a>SQL
* `sql mi [create|update]` を proxyOverride プロパティと publicDataEndpointEnabled プロパティで更新しました

### <a name="storage"></a>ストレージ
* [破壊的変更]`storage blob delete` の結果を削除しました
* SAS を使用して BLOB の完全な URI を作成できるように `storage blob generate-sas` に `--full-uri` を追加しました
* スナップショットからファイルをコピーできるように `storage file copy start` に `--file-snapshot` を追加しました
* NoPendingCopyOperation の例外ではなくエラーのみを表示するように `storage blob copy cancel` を変更しました

## <a name="march-26-2019"></a>2019 年 3 月 26 日


### <a name="core"></a>コア
* dev 拡張機能の非互換性の問題を修正しました
* エラー処理でお客様が問題のページに誘導されるようになりました

### <a name="cloud"></a>クラウド
* `cloud set` の 'サブスクリプションが見つかりません' エラーを修正しました

### <a name="acr"></a>ACR
* イメージ インポートの冗長なソースを修正しました
* `--auth-mode` を `acr build`、`acr run`、`acr task create`、および `acr task update` コマンドに追加しました
* タスク用の資格情報を管理するための 'acr task credential' コマンド グループを追加しました
* '--no-wait' を `acr build` コマンドに追加しました

### <a name="appservice"></a>AppService
* `webapp up` が空のディレクトリから、または不明なコード シナリオでの実行を正しく処理しないバグを修正しました
* スロットが `[webapp|functionapp] config ssl bind` に機能しないバグを修正しました

### <a name="bot-service"></a>ボット サービス
* `webapp` を介したボットのデプロイに備えるため `bot prepare-deploy` を追加しました
* パスワードが指定されていないときにパスワードを表示するように `bot create --kind registration` を変更しました
* [破壊的変更] 要求されるのではなく、既定で空の文字列になるように `bot create --kind registration` で `--endpoint` を変更しました
* v4 Web アプリ ボット用の ARM テンプレートのアプリケーション設定に `SCM_DO_BUILD_DURING_DEPLOYMENT` を追加しました

### <a name="cdn"></a>CDN
* `--no-wait` のサポートを `cdn endpoint [create|update|start|stop|delete|load|purge]` に追加しました  
* [破壊的変更]:`cdn endpoint create` のクエリ文字列の既定のキャッシュ動作を変更しました "IgnoreQueryString" は既定値ではなくなりました。 これはサービスによって設定されるようになりました

### <a name="cosmosdb"></a>Cosmosdb
* アカウントの更新で `--enable-multiple-write-locations` のサポートを追加しました
* Cosmos DB アカウントの VNET ルールを管理するために、`add`、`remove`、および `list` コマンドに `network-rule` サブグループを追加しました

### <a name="interactive"></a>Interactive
* azdev を通じてインストールされたインタラクティブ拡張機能の非互換性の問題を修正しました

### <a name="monitor"></a>モニター
* ディメンション値 `*` を許可するように `monitor metrics alert [create|update]` を変更しました

### <a name="network"></a>ネットワーク
* `rewrite-rule` コマンド グループを `application-gateway` に追加しました

### <a name="profile"></a>プロファイル
* マネージド サービス ID に対応するテナント レベル アカウントのサポートを `login` に追加しました

### <a name="postgres"></a>Postgres 
* postgresql`replica` コマンドと `restart server` コマンドを追加しました
* サーバーの作成用に提供されていない場合にリソース グループから規定の場所を取得し、リテンション期間の日数の検証を追加するための変更を行いました

### <a name="resource"></a>リソース
* `deployment [create|list|show]` のテーブル出力を強化しました
* 型 secureObject が認識されない `deployment [create|validate]` の問題を修正しました

### <a name="graph"></a>グラフ
* `--end-date` のサポートを `ad [app|sp] credential reset` に追加しました
* `ad app permission add` にアクセス許可の追加サポートを追加しました
* アクセス許可がない `ad app permission list` のバグを修正しました
* 現在のアカウントにサブスクリプションがない場合にロール割り当ての削除をスキップするように `ad sp delete` を変更しました
* 指定されていない場合、`--identifier-uris` が既定で空のリストを指定するように `ad app create` を変更しました

### <a name="storage"></a>storage
* 共有スナップショットからダウンロードするように `--snapshot` を `storage file download-batch` に追加しました
* より簡潔に、現在の BLOB を示すように `storage blob [download-batch|upload-batch]` 進行状況バーを変更しました
* 暗号化パラメーターの更新時の `storage account update` の問題を修正しました
* OAuth (`--auth-mode=login`) の使用時に `storage blob show` が失敗する問題を修正しました

### <a name="vm"></a>VM
* `image update` コマンドを追加しました

## <a name="march-12-2019"></a>2019 年 3 月 12 日

バージョン 2.0.60

### <a name="core"></a>コア

* サブスクリプションが見つからないという `cloud set` の正しくないエラーを修正しました

### <a name="acr"></a>ACR

* イメージ インポートの冗長なソースを修正しました

### <a name="acs"></a>ACS

* kubectl でサポートされていない場合、`aks browse` の `--listen-address`パラメーターを無視するように変更しました 

### <a name="appservice"></a>AppService

* Kudu の公開 URL とその資格情報を取得するための `[webapp|functionapp] deployment list-publishing-credentials` を追加しました
* `webapp auth update` の誤った print ステートメントを削除しました
* Linux App Service プランのランタイム用に正しいイメージを設定するように `functionapp` を修正しました
* `webapp up` のプレビュー タグを削除し、コマンドを改善しました

### <a name="botservice"></a>Botservice

* v4 Web アプリ ボット用の ARM テンプレートのアプリケーション設定に `SCM_DO_BUILD_DURING_DEPLOYMENT` を追加しました
* v4 Web アプリ ボット用の ARM テンプレートのアプリケーション設定に `Microsoft-BotFramework-AppId` と `Microsoft-BotFramework-AppPassword` を追加しました
* `bot create` の最後で `bot publish` コマンドの出力から一重引用符を削除しました
* `bot publish` を非同期に変更しました

### <a name="container"></a>コンテナー

* `--no-wait` 引数を `container [start|restart]` に追加しました

### <a name="eventhub"></a>EventHub

* キャプチャで空のアーカイブをサポートするために、`--skip-empty-archives` フラグを `eventhub create|update` に追加しました

### <a name="find"></a>Find

* 主要な機能更新

### <a name="hdinsight"></a>HDInsight

* ADLS Gen2 MSI をサポートするために、`--storage-account-managed-identity` パラメーターを `hdinsight create` に追加しました

### <a name="network"></a>ネットワーク

* 異なるサブスクリプションのゲートウェイ間の VPN 接続を更新できない `vpn-connection update` の問題を修正しました

### <a name="rdbms"></a>Rdbms

* サーバーの作成用に提供されていない場合にリソース グループから規定の場所を取得し、リテンション期間の日数の検証を追加するための軽微な修正を行いました

### <a name="role"></a>Role

* 定義を正しく解決するために ID を使用するように `role definition update` を修正しました
* アプリのサービス プリンシパルが常に存在するという前提を除去するように `ad app credential reset` を変更しました

### <a name="service-fabric"></a>Service Fabric

* `sf cluster list` が反復可能ではないことに関する問題を修正しました

## <a name="february-26-2019"></a>2019 年 2 月 26 日

バージョン 2.0.59

### <a name="core"></a>コア

* 一部のインスタンスで `--subscription NAME` を使用すると例外がスローされる問題を修正しました

### <a name="acr"></a>ACR

* `acr build`、`acr task create`、`acr task update` コマンドに `--target` パラメーターを追加しました
* Azure にログインしていない場合の、ランタイム コマンドのエラー処理を改善しました

### <a name="acs"></a>ACS

* `--listen-address` オプションを `aks port-forward` に追加しました

### <a name="appservice"></a>AppService

* `functionapp devops-build` コマンドを追加しました

### <a name="batch"></a>Batch
* [破壊的変更]`batch pool upgrade os` コマンドを削除しました
* [破壊的変更]`Application` の応答から `Pacakges` プロパティを削除しました
* アプリケーションのパッケージを一覧表示する `batch application package list` コマンドを追加しました
* [破壊的変更] すべての `batch application` コマンドで `--application-id` を `--application-name` に変更しました 
* 生の API 応答を要求するためのコマンドに `--json-file` 引数を追加しました
* すべてのエンドポイントに自動的に `https://` を含めるように検証を更新しました (抜けている場合)

### <a name="cosmosdb"></a>Cosmos DB

* Cosmos DB アカウントの VNET ルールを管理するために、`add`、`remove`、および `list` コマンドに `network-rule` サブグループを追加しました

### <a name="kusto"></a>Kusto

* [破壊的変更] データベースの `hot_cache_period` および `soft_delete_period` 型を ISO8601 の期間形式に変更しました

### <a name="network"></a>ネットワーク

* `--express-route-gateway-bypass` 引数を `vpn-connection [create|update]` に追加しました
* `express-route` 拡張機能のコマンド グループを追加しました
* `express-route gateway` および `express-route port` コマンド グループを追加しました
* `--legacy-mode` 引数を `express-route peering [create|update]` に追加しました 
* `--allow-classic-operations` 引数を `--express-route-port` および `express-route [create|update]` に追加しました
* `--gateway-default-site` 引数を `vnet-gateway [create|update]` に追加しました
* `ipsec-policy` コマンドを `vnet-gateway` に追加しました

### <a name="resource"></a>リソース

* 型フィールドで大文字と小文字が区別されていた `deployment create` の問題を修正しました
* `policy assignment create` に URI ベースのパラメーター ファイルのサポートを追加しました
* `policy set-definition update` に URI ベースのパラメーターおよび定義のサポートを追加しました
* `policy definition update` のパラメーターと規則の処理を修正しました
* クロス サブスクリプション ID でサブスクリプション ID が正しく優先されなかった `resource show/update/delete/tag/invoke-action` の問題を修正しました

### <a name="role"></a>Role

* アプリ ロールのサポートを `ad app [create|update]` に追加しました

### <a name="vm"></a>VM

* Ubuntu 18.0 で --accelerated-networking が既定で有効になっていなかった `vm create where ` の問題を修正しました

## <a name="february-12-2019"></a>2019 年 2 月 12 日

バージョン 2.0.58

### <a name="core"></a>コア

* 更新可能なパッケージがある場合、`az --version` で通知が表示されるようになりました
* JSON 出力で `--ids` を使用できなくなる回帰を修正しました

### <a name="acr"></a>ACR
* [破壊的変更]`acr build-task` コマンド グループを削除しました
* [破壊的変更]`acr repository delete` から `--tag` オプションおよび `--manifest` オプションを削除しました

### <a name="acs"></a>ACS
* `aks [enable-addons|disable-addons]` に、大文字と小文字を区別しない名前のサポ―トを追加しました
* `aks update-credentials --reset-aad` を使用した Azure Active Directory 更新操作のサポートを追加しました
* `aks get-credentials` のために `--output` が無視されることの説明を追加しました

### <a name="ams"></a>AMS
* `ams streaming-endpoint [start | stop | create | update] wait` コマンドを追加しました
* `ams live-event [create | start | stop | reset] wait` コマンドを追加しました

### <a name="appservice"></a>Appservice
* ACR のコンテナーを使用して関数を作成および構成する機能を追加しました
* JSON 経由で Web アプリの構成を更新するためのサポートが追加されました
* `appservice-plan-update` のヘルプを強化しました
* functionapp create に対する App Insights のサポートを追加しました
* Web アプリの SSH の問題を修正しました

### <a name="botservice"></a>Botservice
* `bot publish` の UX を強化しました
* `az bot publish` の間に `npm install` を実行した場合のタイムアウトの警告を追加しました
* `az bot create` の `--name` から無効な文字 `.` を削除しました
* Azure Storage、App Service プラン、関数または Web アプリ、Application Insights を作成するときに、リソース名のランダム化を停止するように変更しました
* [非推奨]`--proj-file-path`を優先して、`--proj-name` を非推奨にしました
* 存在しなかった場合にフェッチされた IIS Node.js デプロイ ファイルを削除するように、`az bot publish` を変更しました
* App Service で `node_modules` フォルダーを削除しないように、`az bot publish` に `--keep-node-modules` 引数を追加しました
* Azure Functions ボットまたは Web アプリ ボットの作成時の、`az bot create` からの出力に `"publishCommand"` キーと値のペアを追加しました
  * `"publishCommand"` の値は、新しく作成したボットを発行するために必要なパラメーターが入力された `az bot publish` コマンドです
* 8\.9.4 ではなく 10.14.1 を使用するように、v4 SDK ボット用の ARM テンプレートで `"WEBSITE_NODE_DEFAULT_VERSION"` を更新しました

### <a name="key-vault"></a>Key Vault
* `--id` の使用時に一部のユーザーに `unexpected_keyword` エラーが発生する `keyvault secret backup` の問題を修正しました

### <a name="monitor"></a>モニター
* ディメンション値 `*` を許可するように `monitor metrics alert [create|update]` を変更しました

### <a name="network"></a>ネットワーク
* エクスポートされた CNAME が必ず FQDN になるように `dns zone export` を変更しました
* アプリケーション ゲートウェイのバックエンド アドレス プールをサポートするように、`--gateway-name` パラメーターを `nic ip-config address-pool [add|remove]` に追加しました
* Log Analytics ワークスペースによるトラフィック分析をサポートするために、`--traffic-analytics` 引数と `--workspace` 引数を `network watcher flow-log configure` に追加しました
* `--idle-timeout` と `--floating-ip` を `lb inbound-nat-pool [create|update]` に追加しました

### <a name="policy-insights"></a>ポリシーの分析情報
* リソース ポリシーの修復機能をサポートするために、`policy remediation` コマンドを追加しました

### <a name="rdbms"></a>RDBMS
* ヘルプ メッセージとコマンド パラメーターを強化しました

### <a name="redis"></a>Redis
* ファイアウォール規則を管理 (作成、更新、削除、表示、一覧表示) するためのコマンドを追加しました
* サーバーのリンクを管理 (作成、削除、表示、一覧表示) するためのコマンドを追加しました
* パッチ スケジュールを管理 (作成、更新、削除、表示) するためのコマンドを追加しました
* redis create に、Availability Zones と TLS 最小バージョンのサポートを追加しました
* [破壊的変更]`redis update-settings` コマンドおよび `redis list-all` コマンドを削除しました
* [破壊的変更]`redis create` のパラメーター 'tenant settings' は、key[=value] 形式では使用できなくなりました
* [非推奨]`redis import-method` コマンドが非推奨であることを示す警告メッセージを追加しました

### <a name="role"></a>Role
* [破壊的変更]`az identity` コマンドを `vm` のコマンドからここに移動しました

### <a name="sql-vm"></a>SQL VM
* [非推奨] 誤りがあったため、`--boostrap-acc-pwd` 引数を非推奨にしました

### <a name="vm"></a>VM
* `--all true` の代わりに `--all` を使用できるように `vm list-skus` を変更しました
* `vmss run-command [invoke | list | show]` を追加しました
* 以前に実行していた場合に `vmss encryption enable` が失敗するバグを修正しました
* [破壊的変更]`az identity` コマンドを `role` のコマンドに移動しました

## <a name="january-31-2019"></a>2019 年 1 月 31 日

バージョン 2.0.57

### <a name="core"></a>コア

* [問題 8399](https://github.com/Azure/azure-cli/issues/8399) 用のホット フィックス。

## <a name="january-28-2019"></a>2019 年 1 月 28 日

バージョン 2.0.56

### <a name="acr"></a>ACR
* VNet ルールまたは IP 規則のサポートを追加しました

### <a name="acs"></a>ACS
* 仮想ノードのプレビューを追加しました
* マネージド OpenShift コマンドを追加しました
* `aks update-credentials -reset-service-principal` を使用したサービス プリンシパル更新操作に対するサポートを追加しました

### <a name="ams"></a>AMS
* [破壊的変更] 名前を `ams asset get-streaming-locators` から `ams asset list-streaming-locators` に変更しました
* [破壊的変更] 名前を `ams streaming-locator get-content-keys` から `ams streaming-locator list-content-keys` に変更しました

### <a name="appservice"></a>Appservice
* `functionapp create` での App Insights のサポートを追加しました
* Function App に、App Service プラン作成 (エラスティック Premium を含む) のサポートを追加しました
* エラスティック Premium プランのアプリ設定に関する問題を修正しました

### <a name="container"></a>コンテナー
* `container start` コマンドを追加しました
* コンテナーの作成時に CPU に 10 進数値を使用できるように変更しました

### <a name="eventgrid"></a>EventGrid
* `--deadletter-endpoint` パラメーターを `event-subscription [create|update]` に追加しました
* "event-subscription [create|update] --endpoint-type" の新しい値として、storagequeue と hybridconnection を追加しました
* イベントの再試行ポリシーを指定するために、`--max-delivery-attempts` パラメーターと `--event-ttl` パラメーターを `event-subscription create` に追加しました
* イベント サブスクリプションの保存先として Webhook が使用されている場合、`event-subscription [create|update]` に警告メッセージを追加しました
* すべてのイベント サブスクリプションに関連するコマンドに source-resource-id パラメーターを追加し、その他のすべてのソース リソース関連パラメーターを非推奨としてマークしました

### <a name="hdinsight"></a>HDInsight
* [破壊的変更]`hdinsight [application] create` から `--virtual-network` パラメーターと `--subnet-name` パラメーターを削除しました
* [破壊的変更] BLOB エンドポイントではなく、ストレージ アカウントの名前または ID を受け入れるように、`hdinsight create --storage-account` を変更しました
* `--vnet-name` および `--subnet-name` パラメーターを `hdinsight create` に追加しました
* Enterprise セキュリティ パッケージとディスク暗号化のサポートが `hdinsight create` に追加されました 
* `hdinsight rotate-disk-encryption-key` コマンドを追加しました
* `hdinsight update` コマンドを追加しました

### <a name="iot"></a>IoT
* routing-endpoint コマンドにエンコード形式を追加しました

### <a name="kusto"></a>Kusto
* プレビュー リリース

### <a name="monitor"></a>モニター
* 大文字と小文字が区別されないように、ID 比較を変更しました

### <a name="profile"></a>プロファイル
* `login` でマネージド サービス ID に対応するテナント レベル アカウントを有効にしました

### <a name="network"></a>ネットワーク
* `--bandwidth` 引数が無視される `express-route update` の問題を修正しました
* set comprehension によってスタック トレースが引き起こされる `ddos-protection update` の問題を修正しました

### <a name="resource"></a>リソース
* `group deployment create` に URI パラメーター ファイルのサポートを追加しました
* `policy assignment [create|list|show]` にマネージド ID のサポートを追加しました

### <a name="sql-virtual-machine"></a>SQL 仮想マシン
* プレビュー リリース

### <a name="storage"></a>ストレージ
* 同じオブジェクトで変更されるプロパティのみを更新するように修正プログラムを変更しました
* #8021 を修正しました。バイナリ データが返されるとき、base 64 でエンコードされます

### <a name="vm"></a>VM
* ディスク暗号化 keyvault と、キー暗号化 keyvault が存在することを検証するように `vm encryption enable` を変更しました
* `--force` フラグを `vm encryption enable` に追加しました

## <a name="january-15-2019"></a>2019 年 1 月 15 日

バージョン 2.0.55

### <a name="acr"></a>ACR
* 存在しない Helm チャートを強制プッシュできるように変更しました
* ARM 要求なしでランタイム操作できるように変更しました
* [非推奨] 次のコマンドの `--resource-group` パラメーターを非推奨にしました
  * `acr login`
  * `acr repository`
  * `acr helm`

### <a name="acs"></a>ACS
* 新しい ACI リージョンのサポートを追加しました

### <a name="appservice"></a>Appservice
* ASE RG とアプリ RG の異なる ASE でホストされているアプリの証明書のアップロードに関する問題を修正しました
* Linux 用の既定値として SKU P1V1 を使用するように `webapp up` を変更しました
* デプロイが失敗したときに、適切なエラー メッセージが表示されるように `[webapp|functionapp] deployment source config-zip` を修正しました 
* `webapp ssh` コマンドを追加しました

### <a name="botservice"></a>Botservice
* デプロイ状態の更新プログラムを `bot create` に追加しました

### <a name="configure"></a>構成
* 構成可能な出力形式として `none` を追加しました

### <a name="cosmosdb"></a>Cosmos DB
* 共有スループットを使用したデータベース作成のサポートを追加しました

### <a name="hdinsight"></a>HDInsight
* アプリケーションを管理するためのコマンドを追加しました
* スクリプト アクションを管理するためのコマンドを追加しました
* Operations Management Suite (OMS) を管理するためのコマンドを追加しました
* リージョンの使用状況を一覧表するためのサポートを `hdinsight list-usage` に追加しました
* [破壊的変更] 既定のクラスターの種類を `hdinsight create` から削除しました

### <a name="network"></a>ネットワーク
* `--custom-headers` 引数と `--status-code-ranges` 引数を `traffic-manager profile [create|update]` に追加しました
* 新しいルーティングの種類として、サブネットと複数値を追加しました
* `--custom-headers` 引数と `--subnets` 引数を `traffic-manager endpoint [create|update]` に追加しました  
* `ddos-protection update` に `--vnets ""` を指定するとエラーが発生する問題を修正しました

### <a name="role"></a>Role
* [非推奨]`create-for-rbac` の `--password` 引数を非推奨にしました。 代わりに、CLI によって生成された安全なパスワードを使用してください

### <a name="security"></a>Security
* 最初のリリース

### <a name="storage"></a>ストレージ
* [破壊的変更] 既定の結果数が 5,000 になるように `storage [blob|file|container|share] list` を変更しました。 すべての結果を返す元の動作が必要な場合は `--num-results *` を使用してください
* `--marker` パラメーターを `storage [blob|file|container|share] list` に追加しました
* 次のページを表すログ マーカーを `storage [blob|file|container|share] list` の STDERR に追加しました 
* 静的 Web サイトをサポートする `storage blob service-properties update` コマンドを追加しました

### <a name="vm"></a>VM
* より一貫性のあるパラメーターが指定されるように `vm [disk|unmanaged-disk]` と `vmss disk` を変更しました
* テナント イメージ相互参照のサポートを `[vm|vmss] create` に追加しました
* `vm diagnostics get-default-config --windows-os` の既定の構成に関するバグを修正しました
* 拡張機能を設定する前に拡張機能をプロビジョニングしておく必要があるかを定義するために、`--provision-after-extensions` 引数を `vmss extension set` に追加しました
* 既定のレプリケーション数を設定できるように、`--replica-count` 引数を `sig image-version update` に追加しました
* 完全なリソース ID を指定しているにもかかわらず、ソース OS ディスクが同じ名前の VM と取り違えられる `image create --source` のバグを修正しました

## <a name="december-20-2018"></a>2018 年 12 月 20 日

バージョン 2.0.54
### <a name="appservice"></a>Appservice
* `webapp up` で再デプロイが失敗する問題を修正しました
* Web アプリのスナップショットの一覧表示および復元のサポートを追加しました
* `--runtime` フラグのサポートを Windows 関数アプリに追加しました

### <a name="iotcentral"></a>IoTCentral
* 更新コマンドの API 呼び出しを修正しました

### <a name="role"></a>Role
* [破壊的変更] 既定では最初の 100 個のオブジェクトのみを一覧表示するように、`ad [app|sp] list` を変更しました

### <a name="sql"></a>SQL
* マネージド インスタンスでカスタム照合順序のサポートを追加しました

### <a name="vm"></a>VM
* `---os-type` パラメーターを `disk create` に追加しました

## <a name="december-18-2018"></a>2018 年 12 月 18 日

バージョン 2.0.53
### <a name="acr"></a>ACR
* 外部のコンテナー レジストリからのイメージのインポートのサポートを追加しました
* タスク一覧のテーブルのレイアウトを縮小しました
* Azure DevOps の URL のサポートを追加しました

### <a name="acs"></a>ACS
* 仮想ノードのプレビューを追加しました
* `aks create` の AAD 引数から "(プレビュー)" を削除しました
* [非推奨]`az acs` コマンドを非推奨にしました。 ACS サービスは 2020 年 1 月 31 日に廃止予定
* 新しい AKS クラスターの作成時のネットワーク ポリシーのサポートを追加しました
* nodepool が 1 つのみの場合に `aks scale` に求められる `--nodepool-name` 引数の要件を削除しました

### <a name="appservice"></a>Appservice
* `webapp config container` で `--slot` パラメーターが許可されない問題を修正しました

### <a name="botservice"></a>Botservice
* `bot show` の呼び出し時に `.bot` ファイルの解析のサポートを追加しました
* AppInsights のプロビジョニングのバグを修正しました
* ファイル パスを処理するときの空白文字のバグを修正しました
* Kudu ネットワーク呼び出しを削減しました
* 一般的なコマンド UX を改善しました

### <a name="consumption"></a>従量課金
* 予算 API での通知の表示のバグを修正しました

### <a name="cosmosdb"></a>Cosmos DB
* マルチマスターからシングルマスターへのアカウントの更新のサポートを追加しました

### <a name="maps"></a>マップ
* S1 SKU のサポートを `maps account [create|update]` に追加しました

### <a name="network"></a>ネットワーク
* `--format` と `--log-version` のサポートを `watcher flow-log configure` に追加しました
* 解決仮想ネットワークと登録仮想ネットワークをクリアするために "" を使用しても機能しないときの `dns zone update` に関する問題を修正しました

### <a name="resource"></a>リソース
* `policy assignment [create|list|delete|show|update]` の管理グループのスコープ パラメーターの処理を修正しました 
* 新しいコマンド `resource wait` を追加しました

### <a name="storage"></a>ストレージ
*  `storage logging update` でストレージ サービスのログ スキーマのバージョンを更新する機能を追加しました

### <a name="vm"></a>VM
* 指定された VM に割り当てられているマネージド サービス ID がない場合の `vm identity remove` でのクラッシュを修正しました

## <a name="december-4-2018"></a>2018 年 12 月 4 日

バージョン 2.0.52
### <a name="core"></a>コア
* マルチテナント サービス プリンシパルのクロス テナント リソース プロビジョニングのサポートを追加しました
* tsv 出力するコマンドからパイプ処理された ID が適切に解析されないバグを修正しました

### <a name="appservice"></a>Appservice
* [プレビュー] コンテンツを作成してアプリに展開する際に役立つ `webapp up` コマンドを追加しました
* バックエンドの変更に起因するコンテナー ベースの Windows アプリのバグを修正しました

### <a name="network"></a>ネットワーク
* WAF 除外をサポートするために、`application-gateway waf-config set` に `--exclusion` 引数を追加しました

### <a name="role"></a>Role
* パスワード資格情報のカスタム識別子のサポートを追加しました 

### <a name="vm"></a>VM
* [非推奨]`vm extension [show|wait] --expand` パラメーターを非推奨にしました
* 応答しない VM を再デプロイするために、`vm restart` に `--force` パラメーターを追加しました
* パスワードと SSH 認証の両方を使用して VM を作成するために、"all" を受け入れるように `[vm|vmss] create --authentication-type` を変更しました
* イメージの OS ディスク キャッシュを設定するための `image create --os-disk-caching` パラメーターを追加しました

## <a name="november-20-2018"></a>2018 年 11 月 20 日

バージョン 2.0.51
### <a name="core"></a>コア
* ID のサブスクリプション名を再利用しないように MSI ログインを変更しました

### <a name="acr"></a>ACR
* タスク ステップにコンテキスト トークンを追加しました
* ACR タスクをミラーリングするために、ACR 実行でシークレットを設定するためのサポートを追加しました
* `show-tags` および `show-manifests` コマンドの `--top` と `--orderby` のサポートを改善しました

### <a name="appservice"></a>Appservice
* ZIP デプロイの既定のタイムアウトを変更し、状態のポーリングを 5 分に増やしました。また、この値をカスタマイズするためのタイムアウト プロパティを追加しました
* 既定の `node_version` を更新しました。 2 フェーズのスワップ時にスロット スワップ アクションをリセットしても、appsettings と接続文字列はすべて保持されます
* Linux App Service プラン作成時のクライアント側の SKU チェックを削除しました
* zipdeploy の状態を取得しようとしたときのエラーを修正しました

### <a name="iotcentral"></a>IotCentral
* IoT Central アプリケーションの作成時にサブドメインの可用性チェックを追加しました

### <a name="keyvault"></a>KeyVault
* エラーが無視される場合があるバグを修正しました

### <a name="network"></a>ネットワーク
* 信頼されたルート証明書を処理するために、`application-gateway` に `root-cert` サブコマンドを追加しました
* `application-gateway [create|update]` に、`--min-capacity` および `--custom-error-pages` オプションを追加しました
* `application-gateway create` に、可用性ゾーンをサポートするための `--zones` を追加しました 
* `application-gateway waf-config set` に、引数 `--file-upload-limit`、`--max-request-body-size`、`--request-body-check` を追加しました

### <a name="rdbms"></a>Rdbms
* mariadb vnet コマンドを追加しました

### <a name="rbac"></a>Rbac
* `ad app update` で変更できない資格情報を更新しようとする問題を修正しました
* 近い将来の `ad [app|sp] list` の破壊的変更を伝えるための出力に関する警告を追加しました 

### <a name="storage"></a>ストレージ
* ストレージ コピー コマンドのまれに発生するケースの処理を改善しました
* コピー先アカウントとコピー元アカウントが同じである場合にログイン資格情報を使用しない、`storage blob copy start-batch` に関する問題を修正しました
* `sas_token` が URL に組み込まれない `storage [blob|file] url` のバグを修正しました
* `[blob|container] list` に破壊的変更に関する警告を追加しました。既定で最初の 5000 個の結果のみがすぐに出力されます

### <a name="vm"></a>VM
* `[vm|vmss] create --storage-sku` に、マネージド OS ディスクとデータ ディスクのストレージ アカウント SKU を個別に指定するためのサポートを追加しました
* `sig image-version` のバージョン名パラメーターを `--image-version -e` に変更しました
* `sig image-version` の引数 `--image-version-name` が非推奨になり、`--image-version` に置き換えられました
* `[vm|vmss] create --ephemeral-os-disk` に、ローカル OS ディスクを使用するためのサポートを追加しました
* `--no-wait` のサポートを `snapshot create/update` に追加しました
* `snapshot wait` コマンドを追加しました
* `[vm|vmss] extension set --extension-instance-name` でインスタンス名を使用するためのサポートを追加しました

## <a name="november-6-2018"></a>2018 年 11 月 6 日

バージョン 2.0.50

### <a name="core"></a>コア
* サービス プリンシパル インスタンスと発行者による認証に対するサポートを追加しました

### <a name="acr"></a>ACR
* タスク ソース トリガーのコミットおよび pull request の Git イベントに対するサポートを追加しました
* ビルド コマンドで指定されていない場合、既定の Dockerfile を使用するように変更しました

### <a name="acs"></a>ACS
* [破壊的変更] 既定で 'az aks browse' が有効になるように、`enable_cloud_console_aks_browse` を削除しました

### <a name="advisor"></a>Advisor
* GA リリース

### <a name="ams"></a>AMS
* 次の新しいコマンド グループを追加しました
  *  `ams account-filter`
  *  `ams asset-filter`
  *  `ams content-key-policy`
  *  `ams live-event`
  *  `ams live-output`
  *  `ams streaming-endpoint`
  *  `ams mru`
* 次の新しいコマンドを追加しました
  * `ams account check-name`
  * `ams job update`
  * `ams asset get-encryption-key`
  * `ams asset get-streaming-locators`
  * `ams streaming-locator get-content-keys`
* 暗号化パラメーターのサポートを `ams streaming-policy create` に追加しました
* `ams transform output remove` にサポートを追加しました。削除する出力インデックスを渡すことで実行できるようになりました
* `--correlation-data` と `--label` の各引数を `ams job` コマンド グループに追加しました
* `--storage-account` と `--container` の各引数を `ams asset` コマンド グループに追加しました
* `ams asset get-sas-url` コマンドに有効期限 (現在 + 23 時間) とアクセス許可 (読み取り) の既定値を追加しました 
* [破壊的変更]`ams streaming locator` コマンドを `ams streaming-locator` で置き換えました
* [破壊的変更]`ams streaming locator` の `--content-keys` 引数を更新しました
* [破壊的変更]`ams streaming locator` コマンドの `--content-policy-name` の名前を `--content-key-policy-name` に変更しました
* [破壊的変更]`ams streaming policy` コマンドを `ams streaming-policy` で置き換えました
* [破壊的変更]`ams transform` コマンド グループの `--preset-names` 引数を `--preset` で置き換えました。 現在同時に設定できるのは、1 つの出力/プリセットのみです (さらに追加するには `ams transform output add` を実行する必要があります)。 また、カスタムの JSON にパスを渡すことで、カスタム StandardEncoderPreset を設定することもできます
* [破壊的変更]`ams job start` コマンドの `--output-asset-names ` の名前を `--output-assets` に変更しました。 "assetName=label" 形式の、スペース区切りのアセットのリストを受け入れるようになりました。 "assetName=" のようなラベルのないアセットを送信できます

### <a name="appservice"></a>AppService
* バックアップ スケジュールがまだ設定されていない場合はバックアップ スケジュールを設定できないという `az webapp config backup update` のバグを修正しました

### <a name="configure"></a>構成
* YAML を出力形式のオプションに追加しました

### <a name="container"></a>コンテナー
* YAML にコンテナー グループをエクスポートするときに、ID を表示するように変更しました

### <a name="eventhub"></a>EventHub
* `eventhub namespace [create|update]` で、Kafka をサポートするように `--enable-kafka` フラグを追加しました

### <a name="interactive"></a>Interactive
* 対話型で `interactive` 拡張機能をインストールするようになりました。これにより、迅速な更新とサポートが可能になります

### <a name="monitor"></a>モニター
* `monitor metrics alert [create|update]` の `--condition` で、フォワードスラッシュ (/) とピリオド (.) の文字を含むメトリック名がサポートされるようになりました

### <a name="network"></a>ネットワーク
* `network private-endpoint` を優先して、`network interface-endpoint` コマンド名を非推奨にしました
* `express-route peering connection create` の `--peer-circuit` 引数が ID を受け入れない問題を修正しました
* `public-ip create` で `--ip-tags` が正しく機能しない問題を修正しました 

### <a name="profile"></a>プロファイル
* 証明書の自動ロールでサービス プリンシパルのログインが可能になるように `--use-cert-sn-issuer` を `az login` に追加しました

### <a name="rdbms"></a>RDBMS
* mysql replica コマンドを追加しました

### <a name="resource"></a>リソース
* 管理グループとサブスクリプションのサポートを `policy definition|set-definition` コマンドに追加しました

### <a name="role"></a>Role
* API アクセス許可の管理、サインインしているユーザー、およびアプリケーションのパスワードと証明書の資格情報管理のサポートが追加されました
* displayName とサービス プリンシパル名を取り違えないように、`ad sp create-for-rbac` を変更しました
* AAD アプリにアクセス許可を付与するサポートを追加しました

### <a name="storage"></a>ストレージ
* アカウント名またはキーなしで、SAS とエンドポイントのみを使用してストレージ サービスに接続するサポートを追加しました (`Configure Azure Storage connection strings <https://docs.microsoft.com/azure/storage/common/storage-configure-connection-string>` を参照してください)

### <a name="vm"></a>VM
* イメージの既定のストレージ アカウントの種類を設定できるように、`storage-sku` 引数を `image create` に追加しました
* `--no-wait` オプションでコマンドがクラッシュする `vm resize` のバグを修正しました
* 状態が表示されるように `vm encryption show` のテーブル出力形式を変更しました
* json/jsonc 出力を要求するように `vm secret format` を変更しました。 望ましくない出力形式が選択された場合、ユーザーに警告し、既定値が json 出力になります
* `vm create --image` の引数検証を改善しました

## <a name="october-23-2018"></a>2018 年 10 月 23 日

バージョン 2.0.49

### <a name="core"></a>コア
* `--subscription` が `--ids` のサブスクリプションよりも優先される場合の `--ids` に関する問題を修正しました
* `--ids` を使用してパラメーターを無視するときの明示的な警告を追加しました

### <a name="acr"></a>ACR
* Python2 の ACR ビルドのエンコードの問題を修正しました

### <a name="cdn"></a>CDN
* [破壊的変更]`cdn endpoint create` のクエリ文字列の既定のキャッシュ動作が変更され、"IgnoreQueryString" が既定値ではなくなりました。 これはサービスによって設定されるようになりました

### <a name="container"></a>コンテナー
* "--ip-address" に渡す有効な型として、`Private` を追加しました
* サブネット ID のみを使用して、コンテナー グループの仮想ネットワークを設定できるように変更しました
* VNet 名またはリソース ID を使用して、さまざまなリソース グループの VNet を使用できるように変更しました
* コンテナー グループに MSI ID を追加するための `--assign-identity` を追加しました
* システム割り当て MSI ID のロールの割り当てを作成するために、`--scope` を追加しました
* イメージを使用して、長時間実行プロセスなしでコンテナー グループを作成するときの警告を追加しました
* `list` コマンドと `show` コマンドのテーブル出力の問題を修正しました

### <a name="cosmosdb"></a>Cosmos DB
* `cosmosdb create` に `--enable-multiple-write-locations` のサポートを追加しました

### <a name="interactive"></a>Interactive
* パラメーターにグローバル サブスクリプション パラメーターが確実に表示されるように変更しました

### <a name="iot-central"></a>IoT Central
* IoT Central アプリケーションを作成するためのテンプレートと表示名のオプションを追加しました
* [破壊的変更] F1 SKU のサポートを削除しました。代わりに S1 SKU を使用します

### <a name="monitor"></a>モニター
* `monitor activity-log list` の変更点:
  * すべてのイベントをサブスクリプション レベルで一覧表示するサポートを追加しました
  * 時間クエリをより簡単に作成できるように、`--offset` パラメーターを追加しました
  * 幅広い ISO8601 形式とユーザー フレンドリな datetime 形式を使用するために、`--start-time` と `--end-time` の検証を改善しました
  * 非推奨の `--resource-provider` オプションのエイリアスとして、`--namespace` を追加しました
  * 厳密に型指定されたオプションを使用する値以外はサービスでサポートされていないため、`--filters` を非推奨にしました
* `monitor metrics list` の変更点:
  * 時間クエリをより簡単に作成できるように、`--offset` パラメーターを追加しました
  * 幅広い ISO8601 形式とユーザー フレンドリな datetime 形式を使用するために、`--start-time` と `--end-time` の検証を改善しました
* `monitor diagnostic-settings create` の引数 `--event-hub` と `--event-hub-rule` の検証を改善しました

### <a name="network"></a>ネットワーク
* NIC へのアプリケーション ゲートウェイ バックエンド アドレス プールの追加をサポートするために、`nic create` に引数 `--app-gateway-address-pools` と `--gateway-name` を追加しました
* NIC へのアプリケーション ゲートウェイ バックエンド アドレス プールの追加をサポートするために、`nic ip-config create/update` に引数 `--app-gateway-address-pools` と `--gateway-name` を追加しました

### <a name="servicebus"></a>ServiceBus
* Service Bus Standard から Premium 名前空間への移行の現在の状態を表示するために、MigrationConfigProperties に読み取り専用の `migration_state` を追加しました

### <a name="sql"></a>SQL
* 手動フェールオーバー ポリシーで機能するように、`sql failover-group create` と `sql failover-group update` を修正しました

### <a name="storage"></a>ストレージ
* すべての項目に正しい "サービス" キーが表示されるように、`az storage cors list` の出力書式設定を修正しました
* 不変ポリシーによってブロックされたコンテナーの削除を可能にする `--bypass-immutability-policy` パラメーターを追加しました

### <a name="vm"></a>VM
* `[vm|vmss] create` で、Lv/Lv2 シリーズのマシンに対して、ディスク キャッシュ モードが強制的に `None` に設定されます
* `vm create` でネットワーク アクセラレータをサポートすることにより、サポートされているサイズのリストを更新しました
* `disk create` の ultrassd iops と mbps configs に、厳密に型指定された引数を追加しました

## <a name="october-16-2018"></a>2018 年 10 月 16 日

バージョン 2.0.48

### <a name="vm"></a>VM
* Homebrew のインストールが失敗する原因となっていた SDK の問題を修正しました

## <a name="october-9-2018"></a>2018 年 10 月 9 日

バージョン 2.0.47

### <a name="core"></a>コア
* "無効な要求" エラーのエラー処理を改善しました

### <a name="acr"></a>ACR
* Helm クライアントと似たテーブル形式のサポートを追加しました

### <a name="acs"></a>ACS
* nodepool 名を構成するために `aks [create|scale] --nodepool-name` を追加しました。12 文字に切り詰められており、既定値は nodepool1 です 
* Parimiko が失敗したときに "scp" にフォールバックするように修正しました
* `--aad-tenant-id` が省略可能になるように `aks create` を変更しました
* 重複するエントリが存在するときの Kubernetes 資格情報のマージを改善しました

### <a name="container"></a>コンテナー
* 特定のランタイムでの Linux 従量課金プランの種類の作成をサポートするように `functionapp create` を変更しました
* [プレビュー] Windows コンテナーでの Web アプリのホストのサポートを追加しました

### <a name="event-hub"></a>イベント ハブ
* `eventhub update` コマンドを修正しました
* [破壊的変更] 空のリストを表示するのではなく、一般的な方法でリソースのエラー NotFound(404) を処理するように `list` コマンドを変更しました

### <a name="extensions"></a>拡張機能
* 既にインストールされている拡張機能を追加しようとする問題を修正しました

### <a name="hdinsight"></a>HDInsight
* 最初のリリース

### <a name="iot"></a>IoT
* 拡張機能のインストール コマンドを初回実行時のバナーに追加しました

### <a name="keyvault"></a>KeyVault
* keyvault storage コマンドが最新の API プロファイルに制限されるように変更しました

### <a name="network"></a>ネットワーク
* `network dns zone create` を修正しました:ユーザーが既定の場所を構成している場合でも、コマンドは成功します。 #6052 を参照してください
* `network vnet peering create` を使用できるため `--remote-vnet-id` を非推奨にしました
* `--remote-vnet` を `network vnet peering create` に追加しました。これは名前または ID を指定できます
* `--subnet-prefixes` で `network vnet create` への複数のサブネット プレフィックスのサポートを追加しました
* `--address-prefixes` で `network vnet subnet [create|update]` への複数のサブネット プレフィックスのサポートを追加しました
* `WAF_v2` または `Standard_v2` SKU でのゲートウェイの作成を妨げていた `network application-gateway create` の問題を修正しました
* `--service-endpoint-policy` の利便性の引数を `network vnet subnet update` に追加しました

### <a name="role"></a>Role
* Azure AD アプリ所有者を一覧表示するためのサポートを `ad app owner` に追加しました
* Azure AD サービス プリンシパル所有者を一覧表示するためのサポートを `ad sp owner` に追加しました
* ロール定義の作成および更新コマンドが複数のアクセス許可構成を確実に受け入れるように変更しました
* ホーム ページの URI が確実かつ常に "https" になるように `ad sp create-for-rbac` を変更しました

### <a name="service-bus"></a>Service Bus
* [破壊的変更] 空のリストを表示するのではなく、一般的な方法でリソースのエラー NotFound(404) を処理するように `list` コマンドを変更しました

### <a name="vm"></a>VM
* `disk grant-access` の空の `accessSas` フィールドを修正しました
* オーバープロビジョニングを処理するのに十分なフロントエンド ポート範囲が予約されるように `vmss create` を変更しました
* `sig` の更新コマンドを修正しました
* `sig` のイメージ バージョンを管理するための `--no-wait` のサポートを追加しました
* パブリック IP アドレスの可用性ゾーンが表示されるように `vm list-ip-addresses` を変更しました
* ディスクの既定の LUN が最初の使用可能なスポットに設定されるように `[vm|vmss] disk attach` を変更しました

## <a name="september-21-2018"></a>2018 年 9 月 21 日

バージョン 2.0.46

### <a name="acr"></a>ACR
* ACR タスクのコマンドを追加しました
* クイック実行コマンドを追加しました
* `build-task` コマンド グループを非推奨にしました
* ACR での Helm チャートの管理をサポートするように `helm` コマンド グループを追加しました
* マネージド レジストリについて、べき等作成のサポートを追加しました
* ビルド ログを表示するために形式のないフラグを追加しました

### <a name="acs"></a>ACS
* AKS マスター FQDN を設定するように `install-connector` コマンドを変更しました
* サービス プリンシパルと skip-role-assignemnt を指定しない場合の vnet-subnet-id に対するロールの割り当て作成を修正しました

### <a name="appservice"></a>AppService

* Web ジョブの (継続的およびトリガーされた) 運用管理のサポートを追加しました
* az webapp config set で --fts-state プロパティがサポートされました。また、az functionapp config set および az functionapp config show のサポートを追加しました
* Web アプリについて Bring Your Own Storage のサポートを追加しました
* 削除された Web アプリの一覧表示および復元のサポートを追加しました

### <a name="batch"></a>Batch
* AddTaskCollectionParameter 構文をサポートするように `--json-file` を使用したタスク追加を変更しました
* 承認済み `--json-file` 形式のドキュメントを更新しました
* `--max-tasks-per-node-option` を `batch pool create` に追加しました
* オプションが指定されていない場合に、現在ログインしているアカウントを表示するように `batch account` の動作を変更しました

### <a name="batch-ai"></a>Batch AI 
* `batchai cluster create` コマンドの自動ストレージ アカウント作成エラーを修正しました

### <a name="cognitive-services"></a>Cognitive Services
* `--sku`、`--kind`、`--location` 引数の入力候補を追加しました
* `cognitiveservices account list-usage` コマンドを追加しました
* `cognitiveservices account list-kinds` コマンドを追加しました
* `cognitiveservices account list` コマンドを追加しました
* `cognitiveservices list` を非推奨にしました
* `cognitiveservices account list-skus` について `--name` を省略可能に変更しました

### <a name="container"></a>コンテナー
* 実行中のコンテナー グループを再起動および停止する機能を追加しました
* ネットワーク プロファイルに渡すための `--network-profile` を追加しました
* VNet でのコンテナー グループの作成できるように `--subnet`、`--vnet_name` を追加しました
* コンテナー グループの状態を表示するようにテーブル出力を変更しました

### <a name="datalake"></a>DataLake
* 仮想ネットワーク規則用のコマンドを追加しました

### <a name="interactive-shell"></a>対話型シェル
* Windows でコマンドが正常に実行されないというエラーを修正しました
* 非推奨のオブジェクトによって発生した、対話形式でのコマンド読み込みの問題を修正しました

### <a name="iot"></a>IoT
* IoT ハブのルーティングのサポートを追加しました

### <a name="key-vault"></a>Key Vault
* RSA キーの Key Vault のキー インポートを修正しました

### <a name="network"></a>ネットワーク
* パブリック IP プレフィックス機能をサポートするように `network public-ip prefix` コマンドを追加しました
* サービス エンドポイント ポリシー機能をサポートするように `network service-endpoint` コマンドを追加しました
* Standard Load Balancer 送信規則の作成をサポートするように `network lb outbound-rule` コマンドを追加しました
* パブリック IP プレフィックスを使用したフロントエンド IP 構成をサポートするように `--public-ip-prefix` を `network lb frontend-ip create/update` に追加しました
* `--enable-tcp-reset` を `network lb rule/inbound-nat-rule/inbound-nat-pool create/update` に追加しました
* `--disable-outbound-snat` を `network lb rule create/update` に追加しました
* `network watcher flow-log show/configure` をクラシック NSG で使用できるようにしました
* `network watcher run-configuration-diagnostic` コマンドが追加されました
* `network watcher test-connectivity` コマンドを修正し、`--method`、`--valid-status-codes`、および `--headers` プロパティを追加しました
* `network express-route create/update`:`--allow-global-reach` フラグを追加しました
* `network vnet subnet create/update`:`--delegation` のサポートを追加しました
* `network vnet subnet list-available-delegations` コマンドを追加しました
* `network traffic-manager profile create/update`:監視の構成について `--interval`、`--timeout`、および `--max-failures` のサポートを追加しました。オプション `--path`、`--port`、`--protocol` を優先し、`--monitor-path`、`--monitor-port`、および `--monitor-protocol` を非推奨にしました
* `network lb frontend-ip create/update`:プライベート IP 割り当て方法の設定ロジックを修正しました。プライベート IP アドレスが指定されている場合、割り当ては静的になります。プライベート IP アドレスが指定されていない場合、またはプライベート IP アドレスに対して空の文字列が指定されている場合、割り当ては動的です。
* `dns record-set * create/update`:`--target-resource` のサポートを追加しました
* インターフェイス エンドポイント オブジェクトにクエリを実行する `network interface-endpoint` コマンドを追加しました
* ネットワーク プロファイルの部分的な管理用に `network profile show/list/delete` を追加しました
* ExpressRoute 間のピアリング接続を管理する `network express-route peering connection` コマンドを追加しました

### <a name="rdbms"></a>RDBMS
* MariaDB サービスのサポートを追加しました

### <a name="reservation"></a>予約
* 予約されたリソース列挙型に CosmosDB を追加しました
* Patch モデルに name プロパティを追加しました

### <a name="manage-app"></a>アプリの管理
* Marketplace マネージド アプリのインスタンス作成がクラッシュする原因となっている `managedapp create --kind MarketPlace` のバグを修正しました
* サポートされているプロファイルに制限されるように `feature` コマンドを変更しました

### <a name="role"></a>Role
* ユーザーのグループ メンバーシップ表示のサポートを追加しました

### <a name="signalr"></a>SignalR
* 最初のリリース

### <a name="storage"></a>ストレージ
* BLOB およびキュー承認用のユーザー ログイン資格情報に使用する `--auth-mode login` パラメーターを追加しました
* 不変ストレージを管理するための `storage container immutability-policy/legal-hold` を追加しました

### <a name="vm"></a>VM
* 公開キー ファイルが見つからない場合に `vm create --generate-ssh-keys` によって秘密キー ファイルが上書きされる問題を修正しました (#4725、#6780)
* `az sig` によって共有イメージ ギャラリーのサポートを追加しました

## <a name="august-28-2018"></a>2018 年 8 月 28 日

バージョン 2.0.45

### <a name="core"></a>コア

* 空の構成ファイルの読み込みに関する問題を修正しました
* Azure Stack におけるプロファイル `2018-03-01-hybrid` のサポートを追加しました

### <a name="acr"></a>ACR

* ARM 要求がないランタイム操作に対する回避策を追加しました
* `build` コマンドで、アップロードされた tar からバージョン コントロール ファイル (git、gitignore など) が既定で除外されるように変更しました

### <a name="acs"></a>ACS

* `Standard_DS2_v2` VM が既定で設定されるように `aks create` を変更しました
* 新しい API を呼び出して、クラスター資格情報を取得するように `aks get-credentials` を変更しました

### <a name="appservice"></a>AppService

* 関数アプリおよび Web アプリで CORS のサポートを追加しました
* 作成コマンドに ARM タグのサポートを追加しました
* リソースが見つからないときにコード 3 で終了するように `[webapp|functionapp] identity show` を変更しました

### <a name="backup"></a>バックアップ

* リソースが見つからないときにコード 3 で終了するように `backup vault backup-properties show` を変更しました

### <a name="bot-service"></a>ボット サービス

* 初期ボット サービス CLI リリース

### <a name="cognitive-services"></a>Cognitive Services

* 一部のサービスの作成に必要な新しいパラメーター `--api-properties,` を追加しました

### <a name="iot"></a>IoT

* リンクされたハブの関連付けに関する問題を修正しました

### <a name="monitor"></a>モニター

* ほぼリアルタイムのメトリック アラートのための `monitor metrics alert` コマンドを追加しました
* `monitor alert` コマンドを非推奨にしました

### <a name="network"></a>ネットワーク

* リソースが見つからないときにコード 3 で終了するように `network application-gateway ssl-policy predefined show` を変更しました

### <a name="resource"></a>リソース

* リソースが見つからないときにコード 3 で終了するように `provider operation show` を変更しました

### <a name="storage"></a>ストレージ

* リソースが見つからないときにコード 3 で終了するように `storage share policy show` を変更しました

### <a name="vm"></a>VM

* リソースが見つからないときにコード 3 で終了するように `vm/vmss identity show` を変更しました 
* `vm create` を使用できるため `--storage-caching` を非推奨にしました

## <a name="auguest-14-2018"></a>2018 年 8 月 14 日

バージョン 2.0.44

### <a name="core"></a>コア

* `table` 出力の数値表示を修正しました
* YAML 出力形式を追加しました

### <a name="telemetry"></a>テレメトリ

* テレメトリ レポートを改善しました

### <a name="acr"></a>ACR

* `content-trust policy` コマンドを追加しました
* `.dockerignore` が適切に処理されない問題を修正しました

### <a name="acs"></a>ACS

* Windows で `%USERPROFILE%\.azure-kubectl` にインストールされるように `az acs/aks install-cli` を変更しました
* クラスターに RBAC が含まれるかどうかを検出し、ACI コネクタを適切に構成するように `az aks install-connector` を変更しました
* サブネットが指定されている場合、そのサブネットにロールが割り当てられるように変更しました
* サブネットが指定されている場合、そのサブネットについて "ロールの割り当てをスキップ" する新しいオプションを追加しました                                 
* サブネットへのロールの割り当てが既に存在する場合、その割り当てをスキップするように変更しました                

### <a name="appservice"></a>AppService

* 外部のリソース グループのストレージ アカウントを使用した関数アプリの作成を妨げるバグを修正しました
* zip デプロイ上のクラッシュを修正しました

### <a name="batchai"></a>BatchAI

* "resource *group*" が指定されるように、自動ストレージ アカウント作成のロガー出力を変更しました。        

### <a name="container"></a>コンテナー

* セキュリティで保護された環境変数をコンテナーに渡すための `--secure-environment-variables` を追加しました      

### <a name="iot"></a>IoT

* [破壊的変更] iot 拡張機能に移動された非推奨のコマンドを削除しました
* `azure-devices.net` ドメインを想定しないように要素を更新しました

### <a name="iot-central"></a>Iot Central

* IoT Central モジュールの最初のリリース

### <a name="keyvault"></a>KeyVault


* ストレージ アカウントと SAS 定義を管理するためのコマンドを追加しました
* network-rules 用のコマンドを追加しました                                                           
* `--id` パラメーターをシークレット、キー、および証明書の操作に追加しました
* KV 管理マルチ API バージョンのサポートを追加しました
* KV データ プレーン マルチ API バージョンのサポートを追加しました

### <a name="relay"></a>リレー

* 最初のリリース

### <a name="sql"></a>Sql

* `sql failover-group` コマンドを追加しました

### <a name="storage"></a>ストレージ

* [破壊的変更]`--location` パラメーターを必要とするように `storage account show-usage` を変更しました。リージョンごとに表示されます
* `storage account` コマンドで省略できるように `--resource-group` パラメーターを変更しました
* バッチ コマンドの個々のエラーを表す "前提条件が満たされていない" という警告を削除し、1 つのメッセージに集約しました
* null の配列が出力されないように `[blob|file] delete-batch` コマンドを変更しました
* コンテナー URL から SAS トークンが読み取られるように `blob [download|upload|delete-batch]` コマンドを変更しました

### <a name="vm"></a>VM

* 使いやすくするために、共通フィルターを `vm list-skus` に追加しました

## <a name="july-31-2018"></a>2018 年 7 月 31日

バージョン 2.0.43

### <a name="acr"></a>ACR

* `--with-secure-properties` フラグを `acr build-task show` コマンドに追加しました
* `acr build-task update-build` コマンドを追加しました

### <a name="acs"></a>ACS

* Ctrl + C キーを押して `az aks browse` を終了すると、戻り値 0 (成功) が返されるように変更しました

### <a name="batch"></a>Batch

* CloudShell で AAD トークンを表示する際のバグを修正しました

### <a name="container"></a>コンテナー

* サブスクリプションの設定時に、名または ID が求められる `--log-analytics-workspace-key` の要件を削除しました

### <a name="network"></a>ネットワーク

* Azure Stack の 2017-03-09-profile に DNS のサポートを追加しました 

### <a name="resource"></a>リソース

* エラー時に既知の正常なデプロイが実行されるように、`group deployment create` に `--rollback-on-error` を追加しました
* `group deployment create` を指定した `--parameters {}` がエラーになる問題を修正しました

### <a name="role"></a>Role

* Stack プロファイル 2017-03-09-profile のサポートを追加しました
* `app update` に対する汎用更新パラメーターが正しく機能しない問題を修正しました

### <a name="search"></a>検索

* Azure Search サービスのコマンドを追加しました

### <a name="service-bus"></a>Service Bus

* Service Bus Standard から Premium に名前空間を移行する移行コマンド グループを追加しました
* Service Bus キューおよびサブスクリプションに、新しい省略可能なプロパティを追加しました
  *  `queue` の `--enable-batched-operations` および `--enable-dead-lettering-on-message-expiration`
  *  `subscriptions` の `--dead-letter-on-filter-exceptions`

### <a name="storage"></a>ストレージ

* 1 つの接続を使用した大きなファイルのダウンロードがサポートされるようになりました
* リソースが見つからないときに終了コード 3 で失敗しそこなっていた `show` コマンドを変換しました

### <a name="vm"></a>VM

* サブスクリプションごとに可用性セットを一覧表示できるようになりました
* `StandardSSD_LRS` のサポートを追加しました
* VM スケール セットの作成でアプリケーション セキュリティ グループのサポートを追加しました
* [破壊的変更] ディクショナリ形式でユーザー割り当て ID を出力するように、`[vm|vmss] create`、`[vm|vmss] identity assign`、および `[vm|vmss] identity remove` を変更しました

## <a name="july-18-2018"></a>2018 年 7 月 18 日

バージョン 2.0.42

### <a name="core"></a>コア

* WSL bash ウィンドウでのブラウザー ベースのログインのサポートを追加しました
* すべての汎用更新コマンドに `--force-string` フラグを追加しました
* [破壊的変更] リソースが見つからないときに、エラー メッセージを記録し、終了コード 3 で失敗するように "show" コマンドを変更しました

### <a name="acr"></a>ACR

* [破壊的変更] "acr build" コマンドの "--no-push" を純粋なフラグに更新しました
* `acr repository` グループに、`show` コマンドと `update` コマンドを追加しました
* `show-manifests` および `show-tags` に、詳細情報を表示する `--detail` フラグを追加しました
* ビルドの詳細またはログのイメージによる取得をサポートするために、`--image` パラメーターを追加しました

### <a name="acs"></a>ACS

* `--max-pods` が 5 未満の場合にエラーが出力されるように `az aks create` を変更しました

### <a name="appservice"></a>AppService

* PremiumV2 SKU のサポートを追加しました

### <a name="batch"></a>Batch

* Cloud Shell モードでトークン資格情報を使用する際のバグを修正しました
* 大文字と小文字が区別されないように JSON 入力を変更しました

### <a name="batch-ai"></a>Batch AI

* `az batchai job exec` コマンドを修正しました

### <a name="container"></a>コンテナー

* DockerHub 以外のレジストリのユーザー名とパスワードの要件を削除しました
* yaml ファイルからコンテナー グループを作成するときのエラーを修正しました

### <a name="network"></a>ネットワーク

* `network nic [create|update|delete]` に `--no-wait` のサポートを追加しました 
* `network nic wait` を追加しました
* `network vnet [subnet|peering] list` の `--ids` 引数を非推奨にしました
* `network nsg rule list` の出力に既定のセキュリティ規則を含める `--include-default` フラグを追加しました  

### <a name="resource"></a>リソース

* `group deployment delete` に `--no-wait` のサポートを追加しました
* `deployment delete` に `--no-wait` のサポートを追加しました
* `deployment wait` コマンドを追加しました
* 2017-03-09-profile プロファイルにサブスクリプション レベルの `az deployment` コマンドが誤って表示される問題を修正しました

### <a name="sql"></a>SQL

* `sql db copy` コマンドおよび `sql db replica create` コマンドにエラスティック プール名を指定したときの、"The provided resource group name ... did not match the name in the Url"\(指定されたリソース グループ名 ... が URL 内の名前と一致しませんでした\) というエラーを修正しました
* `az configure --defaults sql-server=<name>` を実行して、既定の SQL Server を構成できるようになりました
* `sql server`、`sql server firewall-rule`、`sql list-usages`、`sql show-usage` の各コマンドのテーブル フォーマッタを実装しました

### <a name="storage"></a>ストレージ

* `storage blob show` の出力に、ページ BLOB 用に設定される `pageRanges` プロパティを追加しました

### <a name="vm"></a>VM

* [破壊的変更] 既定のインスタンス サイズとして `Standard_DS1_v2` を使用するように `vmss create` を変更しました
* `--no-wait` のサポートを `vm extension [set|delete]` および `vmss extension [set|delete]` に追加しました
* `vm extension wait` を追加しました

## <a name="july-3-2018"></a>2018 年 7 月 3 日

バージョン 2.0.41

### <a name="aks"></a>AKS

* サブスクリプション ID を使用するように監視を変更しました

## <a name="july-3-2018"></a>2018 年 7 月 3 日

バージョン 2.0.40

### <a name="core"></a>コア

* 対話型ログインの新しい承認コード フローを追加しました

### <a name="acr"></a>ACR

* ビルド状態のポーリングを追加しました
* 大文字と小文字が区別されない列挙型の値のサポ―トを追加しました
* `show-manifests` の `--top` および `--orderby` パラメーターを追加しました

### <a name="acs"></a>ACS

* [破壊的変更] Kubernetes のロールベースのアクセス制御を既定で有効にしました
* `--disable-rbac` 引数を追加しました。また、`--enable-rbac` は現在の既定値なので非推奨にしました
* `aks browse` コマンドのオプションを更新しました。 `--listen-port` のサポートを追加しました
* `aks install-connector` コマンドの既定の Helm チャート パッケージを更新しました。 virtual-kubelet-for-aks-latest.tgz を使用します
* 既存のクラスターを更新するための `aks enable-addons` コマンドと `aks disable-addons` コマンドを追加しました

### <a name="appservice"></a>AppService

* `webapp identity remove` による ID の無効化に対応するようになりました
* ID 機能の `preview` タグを削除しました

### <a name="backup"></a>バックアップ

* モジュール定義を更新しました

### <a name="batchai"></a>BatchAI

* `batchai cluster node list` コマンドと `batchai job node list` コマンドのテーブル出力を修正しました

### <a name="cloud"></a>クラウド

* `acr login` サーバー サフィックスをクラウド構成に追加しました

### <a name="container"></a>コンテナー

* `container create` の既定値が実行時間の長い操作に設定されるように変更しました
* Log Analytics の `--log-analytics-workspace` パラメーターと `--log-analytics-workspace-key` パラメーターを追加しました
* 使用するネットワーク プロトコルを指定する `--protocol` パラメーターを追加しました

### <a name="extension"></a>拡張機能

* CLI バージョンと互換性のある拡張機能のみが表示されるように `extension list-available` を変更しました

### <a name="network"></a>ネットワーク

* レコードの種類で大文字と小文字が区別される問題 ([#6602](https://github.com/Azure/azure-cli/issues/6602)) を修正しました

### <a name="rdbms"></a>Rdbms

* `[postgres|myql] server vnet-rule` コマンドを追加しました

### <a name="resource"></a>リソース

* 新しい操作グループ `deployment` を追加しました

### <a name="vm"></a>VM

* システム割り当て ID の削除に対応するようになりました

## <a name="june-25-2018"></a>2018 年 6 月 25 日

バージョン 2.0.39

### <a name="cli"></a>CLI

* 拡張機能のインストールの問題を解決するために MSI インストーラーのファイルのトリミングを更新しました

## <a name="june-19-2018"></a>2018 年 6 月 19 日

バージョン 2.0.38

### <a name="core"></a>コア

* `--subscription` のグローバル サポートをほとんどのコマンドに追加しました

### <a name="acr"></a>ACR

* `azure-storage-blob` を依存関係として追加しました
* 2 コアが使用されるように `acr build-task create` での既定の CPU 構成を変更しました

### <a name="acs"></a>ACS

* `aks use-dev-spaces` コマンドのオプションを更新しました。 `--update` のサポートを追加しました
* `$HOME/.kube/config` のユーザー コンテキストが置き換えられないように `aks get-credentials --admin` を変更しました
* マネージド クラスターで読み取り専用の `nodeResourceGroup` プロパティを公開しました
* `acs browse` コマンド エラーを修正しました
* `aks install-connector`、`aks upgrade-connector`、および `aks remove-connector` について、`--connector-name` を省略可能にしました
* `aks install-connector` の新しい Azure コンテナー インスタンス リージョンを追加しました
* Helm のリリース名とノード名への正規化された場所を `aks install-connector` に追加しました

### <a name="appservice"></a>AppService

* urllib の新しいバージョンのサポートを追加しました
* 外部リソース グループから appservice プランが使用されるようにサポートを `functionapp create` に追加しました

### <a name="batch"></a>Batch

* `azure-batch-extensions` 依存関係を削除しました

### <a name="batch-ai"></a>Batch AI

* ワークスペースのサポートを追加しました。 ワークスペースを使用すると、クラスター、ファイル サーバー、および実験をグループ化できるため、作成できるリソースの数に対する制限がなくなります
* 実験のサポートを追加しました。 実験を使用すると、ジョブをグループ化できるため、作成されたジョブの数に対する制限がなくなります
* Docker コンテナーで実行されているジョブに対して `/dev/shm` を構成するためのサポートを追加しました
* `batchai cluster node exec` コマンドと `batchai job node exec` コマンドを追加しました。 これらのコマンドを使用すると、すべてのコマンドをノードで直接実行して、ポート フォワーディングの機能を提供できます。
* `--ids` のサポートを `batchai` コマンドに追加しました
* [破壊的変更] すべてのクラスターおよびファイルサーバーをワークスペースに作成する必要があります
* [破壊的変更] ジョブを実験に作成する必要があります
* [破壊的変更]`--nfs-resource-group` を `cluster create` コマンドおよび `job create` コマンドから削除しました。 別のワークスペース/リソース グループに属している NFS をマウントするには、`--nfs` オプションによってファイル サーバーの ARM ID を指定します
* [破壊的変更]`--cluster-resource-group` を `job create` コマンドから削除しました。 別のワークスペース/リソース グループに属しているクラスターでジョブを送信するには、`--cluster` オプションによってクラスターの ARM ID を指定します
* [破壊的変更]`location` 属性をジョブ、クラスター、およびファイル サーバーから削除しました。 現在の場所はワークスペースの属性です。
* [破壊的変更]`--location` を `job create` コマンド、`cluster create` コマンド、および `file-server create` コマンドから削除しました
* [破壊的変更] インターフェイスの一貫性を向上させるために短いオプションの名前を変更しました。
  - [`--config`, `-c`] の名前を [`--config-file`, `-f`] に変更しました
  - [`--cluster`, `-r`] の名前を [`--cluster`, `-c`] に変更しました
  - [`--cluster`, `-n`] の名前を [`--cluster`, `-c`] に変更しました
  - [`--job`, `-n`] の名前を [`--job`, `-j`] に変更しました

### <a name="maps"></a>マップ

* [破壊的変更] 対話形式のプロンプトまたは `--accept-tos` フラグによるサービス利用規約への同意を必要とするように `maps account create` を変更しました

### <a name="network"></a>ネットワーク

* `https` のサポートを `network lb probe create` に追加しました ([#6571](https://github.com/Azure/azure-cli/issues/6571))
* `--endpoint-status` で大文字と小文字が区別されていた問題を修正しました。 [#6502](https://github.com/Azure/azure-cli/issues/6502)

### <a name="reservations"></a>Reservations

* [破壊的変更] 必要なパラメーター `ReservedResourceType` を `reservations catalog show` に追加しました
* パラメーター `Location` を `reservations catalog show` に追加しました
* [破壊的変更]`kind` を `ReservationProperties` から削除しました
* [破壊的変更]`Catalog` で `capabilities` の名前を `sku_properties` に変更しました
* [破壊的変更]`Catalog` から `size` プロパティおよび `tier` プロパティを削除しました
* パラメーター `InstanceFlexibility` を `reservations reservation update` に追加しました

### <a name="role"></a>Role

* エラー処理を改善しました

### <a name="sql"></a>SQL

* ご自身のサブスクリプションで利用できない場所で `az sql db list-editions` 実行しているときに発生するわかりにくいエラーを修正しました

### <a name="storage"></a>ストレージ

* `storage blob download` のテーブル出力を読みやすくしました

### <a name="vm"></a>VM

* `vm create` で高速化されたネットワークをサポートするために、VM サイズの絞り込みチェックを改善しました
* 既定の VM サイズが `Standard_D1_v2` から `Standard_DS1_v2` に切り替わるこという `vmss create` に対する警告を追加しました
* 構成が変更されていない場合でも拡張機能が更新されるように `--force-update` を `[vm|vmss] extension set` に追加しました

## <a name="june-13-2018"></a>2018 年 6 月 13 日

バージョン 2.0.37

### <a name="core"></a>コア

* 対話形式の利用統計情報を改善しました

## <a name="june-13-2018"></a>2018 年 6 月 13 日

バージョン 2.0.36

### <a name="aks"></a>AKS

* 高度なネットワーク オプションを `aks create` に追加しました
* 引数を `aks create` に追加して、監視と HTTP ルーティングを有効にしました
* `--no-ssh-key` 引数を `aks create` に追加しました
* `--enable-rbac` 引数を `aks create` に追加しました
* [プレビュー] Azure Active Directory 認証のサポートを `aks create` に追加しました

### <a name="appservice"></a>AppService

* 互換性のない urllib バージョンに関する問題を修正しました

## <a name="june-5-2018"></a>2018 年 6 月 5 日

バージョン 2.0.35

### <a name="interactive"></a>Interactive

* 対話モードの依存関係に対する制限を追加しました

## <a name="june-5-2018"></a>2018 年 6 月 5 日

バージョン 2.0.34

### <a name="core"></a>コア

* テナント リソース相互参照のサポートを追加しました
* 製品利用統計情報のアップロードの信頼性を強化しました

### <a name="acr"></a>ACR

* リモート ソースの場所として VSTS のサポートを追加しました
* `acr import` コマンドを追加しました

### <a name="aks"></a>AKS

* より安全なファイルシステム アクセス許可を使用して kube 構成ファイルが作成されるように `aks get-credentials` を変更しました

### <a name="batch"></a>Batch

* プール リスト テーブルのフォーマットのバグ [[Issue #4378](https://github.com/Azure/azure-cli/issues/4378)] を修正しました

### <a name="iot"></a>IoT

* Basic レベルの IoT ハブを作成するためのサポートを追加しました

### <a name="network"></a>ネットワーク

* `network vnet peering` を強化しました

### <a name="policy-insights"></a>ポリシーの分析情報

* 最初のリリース

### <a name="arm"></a>ARM

* `account management-group` コマンドを追加しました。

### <a name="sql"></a>SQL

* 新しいマネージド インスタンス コマンドを追加しました。
  * `sql mi create`
  * `sql mi show`
  * `sql mi list`
  * `sql mi update`
  * `sql mi delete`
* 新しいマネージド データベース コマンドを追加しました。
  * `sql midb create`
  * `sql midb show`
  * `sql midb list`
  * `sql midb restore`
  * `sql midb delete`

### <a name="storage"></a>ストレージ

* ファイル名拡張子から JSON および JavaScript が推論されるように、mimetype を追加しました

### <a name="vm"></a>VM

* 固定列が使用されるように `vm list-skus` を変更し、`Tier` と `Size` が削除されることを知らせる警告を追加しました
* `--accelerated-networking` オプションを `vm create` に追加しました
* `--tags` を `identity create` に追加しました

## <a name="may-22-2018"></a>2018 年 5 月 22 日

バージョン 2.0.33

### <a name="core"></a>コア

* ファイル名で `@` を展開するためのサポートを追加しました

### <a name="acs"></a>ACS

* 新しい Dev Space コマンド `aks use-dev-spaces` と `aks remove-dev-spaces` を追加しました
* ヘルプ メッセージの誤りを修正しました

### <a name="appservice"></a>AppService

* 汎用的な更新コマンドを強化しました
* `webapp deployment source config-zip` の非同期サポートを追加しました

### <a name="container"></a>コンテナー

* YAML 形式のコンテナー グループをエクスポートするためのサポートを追加しました
* YAML ファイルを使用してコンテナー グループを作成/更新するためのサポートを追加しました

### <a name="extension"></a>拡張機能

* 拡張機能の削除機能を強化しました

### <a name="interactive"></a>Interactive

* ミュート パーサーへの完了のログ記録を変更しました
* 正しくないヘルプ キャッシュの処理を強化しました

### <a name="keyvault"></a>KeyVault

* ID を持つ VM またはクラウド シェルで動作するように KeyVault コマンドを修正しました

### <a name="network"></a>ネットワーク

* `network watcher show-topology` が VNet またはサブネット名で動作しない問題 ([#6326](https://github.com/Azure/azure-cli/issues/6326)) を修正しました
* 実際は有効になっているリージョンについて Network Watcher が、一部の `network watcher` コマンドによって、有効になっていないと通知される問題 ([#6264](https://github.com/Azure/azure-cli/issues/6264)) を修正しました

### <a name="sql"></a>SQL

* [破壊的変更]`db` コマンドおよび `dw` コマンドから返される応答オブジェクトを変更しました。
    * `serviceLevelObjective` プロパティの名前を `currentServiceObjectiveName` に変更しました
    * `currentServiceObjectiveId` プロパティと `requestedServiceObjectiveId` プロパティを削除しました
    * `maxSizeBytes` プロパティを、文字列ではなく整数値に変更しました
* [破壊的変更] 次の `db` プロパティと `dw` プロパティを読み取り専用に変更しました。
    * `requestedServiceObjectiveName`.  更新するには、`--service-objective` パラメーターを使用するか、`sku.name` プロパティを設定します
    * `edition`. 更新するには、`--edition` パラメーターを使用するか、`sku.tier` プロパティを設定します
    * `elasticPoolName`. 更新するには、`--elastic-pool` パラメーターを使用するか、`elasticPoolId` プロパティを設定します
* [破壊的変更] 次の `elastic-pool` プロパティを読み取り専用に変更しました。
    * `edition`. 更新するには、`--edition` パラメーターを使用します
    * `dtu`. 更新するには、`--capacity` パラメーターを使用します
    *  `databaseDtuMin`. 更新するには、`--db-min-capacity` パラメーターを使用します
    *  `databaseDtuMax`. 更新するには、`--db-max-capacity` パラメーターを使用します
* `--family` パラメーターと `--capacity` パラメーターを、`db`、`dw`、`elastic-pool` の各コマンドに追加しました。
* テーブル フォーマッタを、`db`、`dw`、`elastic-pool` の各コマンドに追加しました。

### <a name="storage"></a>ストレージ

* `--account-name` 引数の入力候補を追加しました
* `storage entity query` の問題を修正しました

### <a name="vm"></a>VM

* [破壊的変更]`--write-accelerator` を `vm create` から削除しました。 同じサポートに、`vm update` または `vm disk attach` を使用してアクセスできます
* `[vm|vmss] extension` で一致する拡張機能イメージを修正しました
* ブート ログがキャプチャされるように `--boot-diagnostics-storage` を `vm create` に追加しました
* `--license-type` を `[vm|vmss] update` に追加しました

## <a name="may-7-2018"></a>2018 年 5 月 7 日

バージョン 2.0.32

### <a name="core"></a>コア

* 証明書を使用してサービス プリンシパル アカウントからシークレットを取得する際のハンドルされない例外を修正しました
* 位置引数の制限付きサポートを追加しました
* `--ids` で `--query` を使用できない問題を修正しました。 [#5591](https://github.com/Azure/azure-cli/issues/5591)
* `--ids` を使用するときのコマンドのパイプ処理シナリオを改善しました。 `-o tsv` (クエリが指定されている場合) と `-o json` (クエリが指定されていない場合) がサポートされます
* ユーザーが入力したコマンドに誤りがある場合のエラーにコマンドの候補を追加しました
* ユーザーが「`az ''`」と入力したときのエラーを改善しました
* コマンド モジュールとコマンド拡張機能でのカスタム リソースの種類のサポートを追加しました

### <a name="acr"></a>ACR

* ACR ビルドのコマンドを追加しました
* リソースが見つからないというエラー メッセージを改善しました
* リソース作成のパフォーマンスとエラー処理を改善しました
* 非標準コンソールと WSL での ACR ログインを改善しました
* リポジトリ コマンドのエラー メッセージを改善しました
* テーブルの列と順序付けを更新しました

### <a name="acs"></a>ACS

* `az aks` がプレビュー サービスであることを示す警告を追加しました
* `--aci-resource-group` が指定されていないときの `aks install-connector` でのアクセス許可の問題を修正しました

### <a name="ams"></a>AMS

* 最初のリリース - Azure Media Services リソースの管理

### <a name="appservice"></a>Appservice

* `--slot` を指定したときの `webapp delete` のバグを修正しました
* `webapp auth update` から `--runtime-version` を削除しました
* min\_tls\_version と https2.0 のサポートを追加しました
* 複数コンテナーのサポートを追加しました

### <a name="batch-ai"></a>Batch AI

* クラスターの構成ファイルで構成されている VM 優先度を優先するように `batchai create cluster` を変更しました

### <a name="cognitive-services"></a>Cognitive Services

* `cognitiveservices account create` の例の誤りを修正しました ([#5603](https://github.com/Azure/azure-cli/issues/5603))

### <a name="consumption"></a>従量課金

* budget API の新しいコマンドを追加しました

### <a name="container"></a>コンテナー

* イメージ名にレジストリ サーバーが含まれている場合の `container create` での `--registry-server` の要件を削除しました

### <a name="cosmos-db"></a>Cosmos DB

* Azure CLI (Cosmos DB) での VNET サポートを導入しました

### <a name="dms"></a>DMS

* 最初のリリース - SQL から Azure SQL への移行シナリオのサポートを追加しました

### <a name="extension"></a>拡張機能

* 拡張機能メタデータが表示されなくなるバグを修正しました

### <a name="interactive"></a>Interactive

* 対話型の入力候補が位置引数で機能するようになりました
* ユーザーが「\'」と入力したときの出力をわかりやすくしました
* ヘルプのないパラメーターの入力候補を修正しました
* コマンド グループの説明を修正しました

### <a name="lab"></a>ラボ

* knack 変換からの回帰を修正しました

### <a name="network"></a>ネットワーク

* [破壊的変更] 次の要素の `--ids` パラメーターを削除しました
  * `express-route auth list`
  * `express-route peering list`
  * `nic ip-config list`
  * `nsg rule list`
  * `route-filter rule list`
  * `route-table route list`
  * `traffic-manager endpoint list`

### <a name="profile"></a>プロファイル

* `disk create` のソース検出を修正しました
* [破壊的変更]`--msi-port` と `--identity-port` は使用されなくなったため削除しました
* `account get-access-token` の概要の誤りを修正しました

### <a name="redis"></a>Redis

* `redis patch-schedule show` を優先して、`redis patch-schedule patch-schedule show` を非推奨にしました
* `redis list-all` を非推奨にしました。 この機能は `redis list` に組み込まれました
* `redis import` を優先して、`redis import-method` を非推奨にしました
* さまざまなコマンドに `--ids` のサポートを追加しました

### <a name="role"></a>Role

* [破壊的変更] 非推奨の `ad sp reset-credentials` を削除しました

### <a name="storage"></a>ストレージ

* ソース SAS とアカウント キーが指定されていない場合、コピー先の SAS トークンを BLOB コピーのソースに適用できます
* BLOB のアップロードとダウンロードのための --socket-timeout を公開しました
* パスの区切り記号で始まる BLOB 名は相対パスとして扱います
* 先頭の文字が "?" のクエリで `storage blob copy --source-sas` を使用できます
* key=values のリストを受け入れるように `storage entity query --marker` を修正しました

### <a name="vm"></a>VM

* 非管理対象の BLOB URI の無効な検出ロジックを修正しました
* ユーザーが指定したサービス プリンシパルを使用しないディスク暗号化のサポートを追加しました
* [破壊的変更] MSI をサポートするために VM の "ManagedIdentityExtension" を使用しないでください
* `vmss` に削除ポリシーのサポートを追加しました
* [破壊的変更] 次の要素から `--ids` を削除しました
  * `vm extension list`
  * `vm secret list`
  * `vm unmanaged-disk list`
  * `vmss nic list`
* 書き込みアクセラレータのサポートを追加しました
* `vmss perform-maintenance` を追加しました
* VM の OS の種類が確実に検出されるように `vm diagnostics set` を修正しました
* 要求されたサイズが現在設定されているサイズと異なるかどうかをチェックし、変更時にのみ更新するように `vm resize` を変更しました


## <a name="april-10-2018"></a>2018 年 4 月 10 日

バージョン 2.0.31

### <a name="acr"></a>ACR

* wincred フォールバックのエラー処理を改善しました

### <a name="acs"></a>ACS

* AKS で作成した SPN を変更して 5 年間有効にしました

### <a name="appservice"></a>Appservice

* [破壊的変更]: Removed `assign-identity`
* 存在しない webapp プランのキャッチされない例外を修正しました

### <a name="batchai"></a>BatchAI

* 2018-03-01 API のサポートを追加しました

  - ジョブ レベルのマウント
  - シークレット値を含む環境変数
  - パフォーマンス カウンターの設定
  - ジョブ固有のパス セグメントのレポート
  - ファイルを一覧表示する API のサブフォルダーのサポート
  - 使用状況と制限のレポート
  - NFS サーバーのキャッシュの種類が指定可能
  - カスタム イメージのサポート
  - pyTorch ツールキットのサポートを追加しました

* `job wait` コマンドを追加しました。このコマンドは、ジョブ完了を待機できるようにして、ジョブ終了コードをレポートします
* `usage show` コマンドを追加しました。このコマンドは、さまざまなリージョンにおける現在の Batch AI リソースの使用状況と制限を一覧表示します
* National Clouds がサポートされています
* 構成ファイルに加え、ジョブ レベルでファイルシステムをマウントするジョブ コマンド ライン引数を追加しました
* VM 優先順位、サブネット、自動スケール クラスターの初期ノード数、カスタム イメージの指定など、クラスターのカスタマイズ オプションを追加しました
* Batch AI マネージド NFS のキャッシュの種類を指定するコマンド ライン オプションを追加しました
* 構成ファイルでのファイルシステム マウントの指定を簡素化しました。 Azure ファイル共有および Azure BLOB コンテナーの資格情報を省略できます。不足している資格情報は、CLI によって、コマンド ライン パラメーターを介して提供された、または環境変数によって指定されたストレージ アカウント キーを使用して設定されるか、Azure Storage からキーが照会されます (ストレージ アカウントが現在のサブスクリプションに属している場合)
* ジョブ ファイル ストリーム コマンドがオートコンプリートされるようになりました (成功、失敗、終了、または削除)
* `show` 操作の `table` 出力を改善しました
* クラスター作成の `--use-auto-storage` オプションを追加しました。 このオプションによって、ストレージ アカウントの管理と、クラスターへの Azure ファイル共有および Azure BLOB コンテナーのマウントがシンプルになります
* `--generate-ssh-keys` オプションを `cluster create` と `file-server create` に追加しました
* コマンド ラインでノードのセットアップ タスクを提供できるようにしました
* [破壊的変更]`job file` グループで `job stream-file` および `job list-files` コマンドを移行しました
* [破壊的変更]`cluster create` コマンドに合わせて、`file-server create` コマンドの `--admin-user-name` の名前を `--user-name` に変更しました

### <a name="billing"></a>課金

* 登録アカウント コマンドを追加しました

### <a name="consumption"></a>従量課金

* `marketplace` コマンドを追加しました
* [破壊的変更] 名前を `reservations summaries` から `reservation summary` に変更しました
* [破壊的変更] 名前を `reservations details` から `reservation detail` に変更しました
* [破壊的変更]`reservation` コマンドの `--reservation-order-id` および `--reservation-id` の短いオプションを削除しました
* [破壊的変更]`reservation summary` コマンドの `--grain` の短いオプションを削除しました
* [破壊的変更]`pricesheet` コマンドの `--include-meter-details` の短いオプションを削除しました

### <a name="container"></a>コンテナー

* Git リポジトリのボリューム マウント パラメーター `--gitrepo-url`、`--gitrepo-dir`、`--gitrepo-revision`、および `--gitrepo-mount-path` を追加しました
* [#5926](https://github.com/Azure/azure-cli/issues/5926) (--container-name が指定されたときに `az container exec` が失敗する) を修正しました

### <a name="extension"></a>拡張機能

* ディストリビューション チェック メッセージをデバッグ レベルに変更しました

### <a name="interactive"></a>Interactive

* 認識できないコマンドが入力されたときに入力候補が停止するように変更しました
* コマンド サブツリーの作成前および作成後にイベント フックを追加しました
* `--ids` パラメーターの入力候補を追加しました

### <a name="network"></a>ネットワーク

* [#5936](https://github.com/Azure/azure-cli/issues/5936) (`application-gateway create` タグを設定できない) を修正しました
* `application-gateway http-settings [create|update]` の認証証明書をアタッチする引数 `--auth-certs` を追加しました。 [#4910](https://github.com/Azure/azure-cli/issues/4910)
* DDoS 保護プランを作成する `ddos-protection` コマンドを追加しました
* `--ddos-protection-plan` のサポートを `vnet [create|update]` に追加して、VNet を DDoS 保護プランに関連付けました
* `network route-table [create|update]` の `--disable-bgp-route-propagation` フラグに関する問題を修正しました
* `network lb [create|update]` のダミー引数 `--public-ip-address-type` および `--subnet-type` を削除しました
* RFC 1035 エスケープ シーケンスを含む TXT レコードのサポートを `network dns zone [import|export]` および `network dns record-set txt add-record` に追加しました

### <a name="profile"></a>プロファイル

* `account list` に Azure クラシック アカウントのサポートを追加しました
* [破壊的変更]`--msi` & `--msi-port` 引数を削除しました

### <a name="rdbms"></a>RDBMS

* `georestore` コマンドを追加しました
* ストレージ サイズの制限を `create` コマンドから削除しました

### <a name="resource"></a>リソース

* `--metadata` のサポートを `policy definition create` に追加しました
* `--metadata`、`--set`、`--add`、`--remove` のサポートを `policy definition update` に追加しました

### <a name="sql"></a>SQL

* `sql elastic-pool op list` および `sql elastic-pool op cancel` を追加しました

### <a name="storage"></a>ストレージ

* 無効な形式の接続文字列のエラー メッセージを改善しました

### <a name="vm"></a>VM

* プラットフォーム障害ドメイン数の構成サポートを `vmss create` に追加しました
* ゾーンベースの大規模な、または single-placement-group が無効なスケールセットについて、`vmss create` の既定値が Standard LB になるように変更しました
* [破壊的変更]: Removed `vm assign-identity`, `vm remove-identity and `vm format-secret`
* Public-IP SKU のサポートを `vm create` に追加しました
* コマンドでコンテナー ID を解決できないシナリオをサポートするように、`--keyvault` 引数と `--resource-group` 引数を `vm secret format` に追加しました。 [#5718](https://github.com/Azure/azure-cli/issues/5718)
* リソース グループの場所でゾーンがサポートされていない場合の、`[vm|vmss create]` のエラーを改善しました


## <a name="march-27-2018"></a>2018 年 3 月 27 日

バージョン 2.0.30

### <a name="core"></a>コア

* ヘルプでプレビュー版としてマークされている拡張機能に関するメッセージを表示します

### <a name="acs"></a>ACS

* Cloud Shell の `aks install-cli` での SSL 証明書の検証エラーを修正しました

### <a name="appservice"></a>Appservice

* `webapp update` に HTTPS 専用サポートを追加しました
* `az webapp identity [assign|show]` と `az functionapp identity [assign|show]` にスロットのサポートを追加しました

### <a name="backup"></a>バックアップ

* 新しいコマンド `az backup protection isenabled-for-vm` を追加しました。 このコマンドは、VM がサブスクリプション内の任意のコンテナーによってバックアップされるかどうかを確認するときに使用できます
* 次のコマンドの `--resource-group` パラメーターと `--vault-name` パラメーターに対して Azure のオブジェクト ID を有効にしました
  * `backup container show`
  * `backup item set-policy`
  * `backup item show`
  * `backup job show`
  * `backup job stop`
  * `backup job wait`
  * `backup policy delete`
  * `backup policy get-default-for-vm`
  * `backup policy list-associated-items`
  * `backup policy set`
  * `backup policy show`
  * `backup protection backup-now`
  * `backup protection disable`
  * `backup protection enable-for-vm`
  * `backup recoverypoint show`
  * `backup restore files mount-rp`
  * `backup restore files unmount-rp`
  * `backup restore restore-disks`
  * `backup vault delete`
  * `backup vault show`
* `backup ... show` コマンドからの出力形式を受け入れるように、`--name` パラメーターを変更しました

### <a name="container"></a>コンテナー

* `container exec` コマンドを追加しました。 実行中のコンテナー グループのコンテナーでコマンドを実行します
* コンテナー グループの作成および更新のために、テーブル出力が許可されます

### <a name="extension"></a>拡張機能

* 拡張機能がプレビュー段階である場合に、`extension add` のメッセージを追加しました
* `--show-details` を使用して完全な拡張機能データを表示できるように、`extension list-available` を変更しました
* [破壊的変更] 簡略化された拡張機能データを既定で表示するように、`extension list-available` を変更しました

### <a name="interactive"></a>Interactive

* コマンド テーブルの読み込みが完了するとすぐに入力候補がアクティブになるように変更しました
* `--style` パラメーター使用時のバグを修正しました
* 存在しない場合、コマンド テーブルのダンプ後に対話型レクサーがインスタンス化されました
* 入力候補のサポートを強化しました

### <a name="lab"></a>ラボ

* `create environment` コマンドに関するバグを修正しました

### <a name="monitor"></a>モニター

* `--top`、`--orderby`、および `--namespace` のサポートを `metrics list` に追加しました ([#5785](https://github.com/Azure/azure-cli/issues/5785))
* [#4529](https://github.com/Azure/azure-cli/issues/5785) を修正しました:`metrics list` は、取得するメトリックのスペース区切りリストを受け入れます
* `--namespace` のサポートを `metrics list-definitions` に追加しました ([#5785](https://github.com/Azure/azure-cli/issues/5785))

### <a name="network"></a>ネットワーク

* プライベート DNS ゾーンのサポートを追加しました

### <a name="profile"></a>プロファイル

* `--identity-port` と `--msi-port` の警告を `login` に追加しました

### <a name="rdbms"></a>RDBMS

* ビジネス モデル GA API バージョン 2017-12-01 を追加しました

### <a name="resource"></a>リソース

* [破壊的変更]: Changed `provider operation [list|show]` to not require `--api-version`

### <a name="role"></a>Role

* 必要なアクセス権の構成とネイティブ クライアントのサポートを `az ad app create` に追加しました
* オブジェクトの解決時に 1000 未満の ID を返すように、`rbac` コマンドを変更しました
* 資格情報管理コマンド `ad sp credential [reset|list|delete]` を追加しました
* [破壊的変更]`az role assignment [list|show]` の出力から 'properties' を削除しました
* `dataActions` アクセス許可と `notDataActions` アクセス許可のサポートを `role definition` に追加しました

### <a name="storage"></a>ストレージ

* サイズが 195 ～ 200 GB のファイルをアップロードするときの問題を修正しました
* [#4049](https://github.com/Azure/azure-cli/issues/4049) を修正しました:追加 BLOB のアップロードで条件のパラメーターが無視される問題

### <a name="vm"></a>VM

* インスタンス数が 100 を超えるセットに対する今後の破壊的変更に向けて、`vmss create` に警告を追加しました
* ゾーン回復性のサポートを `vm [snapshot|image]` に追加しました
* より適切に暗号化状態がレポートされるように、ディスクのインスタンス ビューを変更しました
* [破壊的変更] 出力を返さないように `vm extension delete` を変更しました

## <a name="march-13-2018"></a>2018 年 3 月 13 日

バージョン 2.0.29

### <a name="acr"></a>ACR

* `--image` パラメーターのサポートを `repository delete` に追加しました
* `repository delete` コマンドの `--manifest` および `--tag` パラメーターを非推奨にしました
* データを削除せずに、タグを削除する `repository untag` コマンドを追加しました

### <a name="acs"></a>ACS

* 既存のコネクタをアップグレードする `aks upgrade-connector` コマンドを追加しました
* より読み取りやすいブロック スタイルの YAML を使用するように `kubectl` 構成ファイルを変更しました

### <a name="advisor"></a>Advisor

* [破壊的変更] 名前を `advisor configuration get` から `advisor configuration list` に変更しました
* [破壊的変更] 名前を `advisor configuration set` から `advisor configuration update` に変更しました
* [破壊的変更]`advisor recommendation generate` を削除しました
* `--refresh` パラメーターを `advisor recommendation list` に追加しました
* `advisor recommendation show` コマンドを追加しました

### <a name="appservice"></a>Appservice

* `[webapp|functionapp] assign-identity` を非推奨にしました
* 管理対象 ID コマンド `webapp identity [assign|show]` および `functionapp identity [assign|show]` を追加しました

### <a name="eventhubs"></a>イベント ハブ

* 最初のリリース

### <a name="extension"></a>拡張機能

* 使用しているディストリビューションがパッケージ ソース ファイルに格納されているものと異なる場合は、エラーが発生する可能性があるため、ユーザーに警告するチェックを追加しました

### <a name="interactive"></a>Interactive

* [#5625](https://github.com/Azure/azure-cli/issues/5625) を修正しました:異なるセッション間で履歴が保持されます
* [#3016](https://github.com/Azure/azure-cli/issues/3016) を修正しました:スコープ内にある間は履歴が記録されませんでした
* [#5688](https://github.com/Azure/azure-cli/issues/5688) を修正しました:コマンド テーブルの読み込み中に例外が発生した場合、入力候補が表示されませんでした
* 実行時間の長い操作の進行状況インジケーターを修正しました

### <a name="monitor"></a>モニター

* `monitor autoscale-settings` コマンドを非推奨にしました
* `monitor autoscale` コマンドを追加しました
* `monitor autoscale profile` コマンドを追加しました
* `monitor autoscale rule` コマンドを追加しました

### <a name="network"></a>ネットワーク

* [破壊的変更]`route-filter rule create` から `--tags` パラメーターを削除しました
* 次のコマンドの不適切な既定値を削除しました。
  * `network express-route update`
  * `network nsg rule update`
  * `network public-ip update`
  * `traffic-manager profile update`
  * `network vnet-gateway update`
* `network watcher connection-monitor` コマンドを追加しました
* `--vnet` および `--subnet` パラメーターを `network watcher show-topology` に追加しました

### <a name="profile"></a>プロファイル

* `az login` の `--msi` パラメーターを非推奨にしました
* `az login` の `--msi` パラメーターの代わりに `--identity` パラメーターを追加しました

### <a name="rdbms"></a>RDBMS

* [プレビュー] API 2017-12-01-preview を使用するように変更しました

### <a name="service-bus"></a>Service Bus

* 最初のリリース

### <a name="storage"></a>ストレージ

* [#4971](https://github.com/Azure/azure-cli/issues/4971) を修正しました: `storage blob copy` では他の Azure クラウドをサポートするようになりました
* [#5286](https://github.com/Azure/azure-cli/issues/5286) を修正しました:Batch コマンド `storage blob [delete-batch|download-batch|upload-batch]` の前提条件が満たされていなくても、エラーがスローされなくなりました

### <a name="vm"></a>VM

* 被管理対象データ ディスクを接続し、キャッシュを構成できるように `[vm|vmss] create` へのサポートを追加しました
* `[vm|vmss] assign-identity` および `[vm|vmss] remove-identity` を非推奨にしました
* 非推奨のコマンドの代わりに `vm identity [assign|remove|show]` および `vmss identity [assign|remove|show]` コマンドを追加しました
* `vmss create` での既定の優先順位を None に変更しました

## <a name="february-27-2018"></a>2018 年 2 月 27 日

バージョン 2.0.28

### <a name="core"></a>コア

* [#5184](https://github.com/Azure/azure-cli/issues/5184) を修正しました:Homebrew のインストールの問題
* カスタム キーを使用した拡張機能のテレメトリのサポートを追加しました
* HTTP ログを `--debug` に追加しました

### <a name="acs"></a>ACS

* 既定で `aks install-connector` の `virtual-kubelet-for-aks` Helm チャートを使用するように変更しました
* 修正された問題:ACI コンテナー グループを作成するためのアクセス許可がサービス プリンシパルに不足している問題
* `--aci-container-group`、`--location`、および `--image-tag` の各パラメーターを `aks install-connector` に追加しました
* 非推奨に関する通知を `aks get-versions` から削除しました

### <a name="appservice"></a>Appservice

* 新しい SDK バージョン (azure-mgmt-web 0.35.0) の更新
* [#5538](https://github.com/Azure/azure-cli/issues/5538) を修正: 無効な SKU として報告された `Free`

### <a name="cognitive-services"></a>Cognitive Services

* 新しい Cognitive Services アカウントを作成するときの "通知" を更新しました

### <a name="consumption"></a>従量課金

* pricesheet API の新しいコマンドを追加しました
* 既存の使用方法の詳細と予約の詳細の形式を更新しました

### <a name="container"></a>コンテナー

* ACI でシークレットを使用するために `--secrets` と `--secrets-mount-path` の各引数を `container create` に追加しました

### <a name="network"></a>ネットワーク

* [#5559](https://github.com/Azure/azure-cli/issues/5559) を修正:`network vnet-gateway vpn-client generate` でクライアントが見つからない

### <a name="resource"></a>リソース

* エラー発生時にテンプレートとエラーの一部が表示されるように `group deployment export` を変更しました

### <a name="role"></a>Role

* サービス プリンシパル ロールの監査を許可するために `role assignment list-changelogs` を追加しました

### <a name="sql"></a>SQL

* 作成および更新時のデータベースとエラスティック プールのゾーン冗長性に対するサポートを追加しました

### <a name="storage"></a>ストレージ

* `storage blob [upload-batch|download-batch]` のターゲット パス/プレフィックスの指定を有効にしました

### <a name="vm"></a>VM

* 1 つの VMSS インスタンス上でのディスクの接続/デタッチのサポートを追加しました


## <a name="february-13-2018"></a>2018 年 2 月 13 日

バージョン 2.0.27

### <a name="core"></a>コア

* MSI ログインのサブスクリプション ID と名前の両方で認証をキーに変更しました

### <a name="acs"></a>ACS

* [破壊的変更] 精度のために名前を `aks get-versions` から `aks get-upgrades` に変更しました
* `aks create` で使用可能な Kubernetes バージョンが表示されるように `aks get-versions` を変更しました
* サーバーで Kubernetes のバージョンを選択できるように `aks create` 既定値を変更しました
* AKS によって生成されたサービス プリンシパルを参照するヘルプ メッセージを更新しました
* `aks create` の既定のノード サイズを "Standard\_D1\_v2" から "Standard\_DS1\_v2" に変更しました
* `az aks browse` のダッシュボード ポッドを特定するときの信頼性が向上しました
* Kubernetes 構成ファイルを読み込むときの Unicode エラーを処理するように `aks get-credentials` を修正しました
* メッセージを `az aks install-cli` に追加しました。これは `$PATH` での `kubectl` の取得に役立ちます

### <a name="appservice"></a>Appservice

* null 参照のために `webapp [backup|restore]` が失敗する問題を修正しました
* `az configure --defaults appserviceplan=my-asp` による既定のアプリ サービス プランのサポートを追加しました

### <a name="cdn"></a>CDN

* `cdn custom-domain [enable-https|disable-https]` コマンドを追加しました

### <a name="container"></a>コンテナー

* ログ ストリーミングのために `--follow` オプションを `az container logs` に追加しました
* ローカル標準出力とエラー ストリームをコンテナー グループのコンテナーにアタッチする `container attach` コマンドを追加しました

### <a name="cosmosdb"></a>Cosmos DB

* 機能の設定のサポートを追加しました

### <a name="extension"></a>拡張機能

* `--pip-proxy` パラメーターのサポートを `az extension [add|update]` コマンドに追加しました
* `--pip-extra-index-urls` 引数のサポートを `az extension [add|update]` コマンドに追加しました

### <a name="feedback-command"></a>フィードバック コマンド

* 拡張機能の情報を利用統計情報に追加しました

### <a name="interactive"></a>Interactive

* Cloud Shell で対話モードを使用するときにユーザーのログインが求められる問題を修正しました
* パラメーター不足での完了による回帰を修正しました

### <a name="iot"></a>IoT

* 成功時に `iot dps access policy [create|update]` が "検出不可" エラーを返す問題を修正しました
* 成功時に `iot dps linked-hub [create|update]` が "検出不可" エラーを返す問題を修正しました
* `--no-wait` のサポートを `iot dps access policy [create|update]` および `iot dps linked-hub [create|update]` に追加しました
* パーティション数の指定を許可するように `iot hub create` を変更しました

### <a name="monitor"></a>モニター

* `az monitor log-profiles create` コマンドを修正しました

### <a name="network"></a>ネットワーク

* 次のコマンドの `--tags` オプションを修正しました。
  * `network public-ip create`
  * `network lb create`
  * `network local-gateway create`
  * `network nic create`
  * `network vnet-gateway create`
  * `network vpn-connection create`

### <a name="profile"></a>プロファイル

* 対話モードで `az login` を有効にしました

### <a name="resource"></a>リソース

* `feature show` を再び追加しました

### <a name="role"></a>Role

* `--available-to-other-tenants` 引数を `ad app update` に追加しました

### <a name="sql"></a>SQL

* `sql server dns-alias` コマンドを追加しました
* `sql db rename` を追加しました
* `--ids` 引数のサポートをすべての SQL コマンドに追加しました

### <a name="storage"></a>ストレージ

* 論理的な削除を有効にする `storage blob service-properties delete-policy` コマンドと `storage blob undelete` コマンドを追加しました

### <a name="vm"></a>VM

* VM 暗号化の一部を初期化できないときのクラッシュを修正しました
* MSI を有効にするときのプリンシパル ID 出力を追加しました
* `vm boot-diagnostics get-boot-log` (固定)


## <a name="january-31-2018"></a>2018 年 1 月 31 日

バージョン 2.0.26

### <a name="core"></a>コア

* MSI コンテキストでの未処理トークン取得のサポートを追加しました
* Windows cmd.exe での LRO 終了後のインジケーター文字列ポーリングを削除しました
* 構成された既定値の使用が情報レベル エントリに変更されたときに表示される警告を追加しました。 確認するには `--verbose` を使用します
* 待機コマンドの進行状況のインジケーターを追加します

### <a name="acs"></a>ACS

* `--disable-browser` 引数を明確化しました
* `--vm-size` 引数のタブ補完を強化しました

### <a name="appservice"></a>Appservice

* `webapp log [tail|download]` (固定)
* WebApps と機能で `kind` チェックを削除しました

### <a name="cdn"></a>CDN

* `cdn custom-domain create` のクライアントが見つからない問題を修正しました

### <a name="cosmosdb"></a>Cosmos DB

* フェールオーバー ポリシーのパラメーターの説明を修正しました

### <a name="interactive"></a>Interactive

* コマンド オプションの完了が表示されなくなった問題を修正しました

### <a name="network"></a>ネットワーク

* `--cert-password` の保護を `application-gateway create` に追加しました
* `--sku` が誤って既定値を適用する `application-gateway update` の問題を修正しました
* `--shared-key` の保護を `--authorization-key` および `vpn-connection create` に追加しました
* `asg create` のクライアントが見つからない問題を修正しました
* エクスポートされた名前の `--file-name / -f` パラメーターを `dns zone export` に追加しました
* `dns zone export` の次の問題を修正しました。
  * 長い TXT レコードが誤ってエクスポートされる問題を修正しました
  * 引用符で囲まれた TXT レコードが、エスケープされた引用符なしで誤ってエクスポートされる問題を修正しました
* 特定のレコードが `dns zone import` で 2 回インポートされる問題を修正しました
* `vnet-gateway root-cert` コマンドと `vnet-gateway revoked-cert` コマンドを復元しました

### <a name="profile"></a>プロファイル

* ID を使用して VM 内で動作するように `get-access-token` を修正しました

### <a name="resource"></a>リソース

* テンプレートの "type" フィールドに大文字の値が含まれているときに警告が誤って表示される `deployment [create|validate]` のバグを修正しました

### <a name="storage"></a>ストレージ

* ストレージ V1 からストレージ V2 へのアカウント移行の問題を修正しました
* すべてのアップロード/ダウンロード コマンドについて、進行状況レポートを追加しました
* `storage account check-name` で "-n" 引数オプションが妨げられるバグを修正しました
* "snapshot" 列を `blob [list|show]` のテーブル出力に追加しました
* 整数として解析する必要があるさまざまなパラメーターのバグを修正しました

### <a name="vm"></a>VM

* 追加料金でイメージからの VM 作成を許可する `vm image accept-terms` コマンドを追加しました
* 署名されていない証明書を使用して、コマンドをプロキシで確実に実行できるように `[vm|vmss create]` を修正しました
* [プレビュー] "低" 優先度のサポートを VMSS に追加しました
* `--admin-password` の保護を `[vm|vmss] create` に追加しました


## <a name="january-17-2018"></a>2018 年 1 月 17 日

バージョン 2.0.25

### <a name="acr"></a>ACR

* Windows 資格情報エラーの acr ログイン フォールバックを追加しました
* レジストリ ログを有効にしました

### <a name="acs"></a>ACS

* `get-credentials` コマンドを修正しました
* SPN のロール要件を削除しました

### <a name="appservice"></a>Appservice

* `hosting_environment_profile` が null になる `config ssl upload` のバグを修正しました
* `browse` にカスタム URL のサポートを追加しました
* `log tail` のスロットのサポートを修正しました

### <a name="backup"></a>バックアップ

* `backup item list` の `--container-name` オプションを省略可能に変更しました
* `backup restore restore-disks` にストレージ アカウント オプションを追加しました
* `backup protection enable-for-vm` での場所のチェックを、大文字と小文字が区別されないように修正しました
* 無効なコンテナー名でコマンドが失敗する問題を修正しました
* 既定で "正常性状態" が含まれるように `backup item list` を変更しました

### <a name="batch"></a>Batch

* 認証の詳細を返すように `batch login` を変更しました

### <a name="cloud"></a>クラウド

* クラウドで `--profile` を設定するときに、エンドポイントを不要にするように変更しました

### <a name="consumption"></a>従量課金

* 予約の新しいコマンドとして、`consumption reservations summaries` と `consumption reservations details` を追加しました

### <a name="event-grid"></a>Event Grid

* [破壊的変更]`az eventgrid topic event-subscription` コマンドを `eventgrid event-subscription` に移行しました
* [破壊的変更]`az eventgrid resource event-subscription` コマンドを `eventgrid event-subscription` に移行しました
* [破壊的変更]`eventgrid event-subscription show-endpoint-url` コマンドを削除しました 代わりに `eventgrid event-subscription show --include-full-endpoint-url` を使用してください
* `eventgrid topic update` コマンドを追加しました
* `eventgrid event-subscription update` コマンドを追加しました
* `eventgrid topic` コマンドの `--ids` パラメーターを追加しました
* トピック名のタブ補完のサポートを追加しました

### <a name="interactive"></a>Interactive

* Python 2.x で対話モードが機能しない問題を解決しました
* 起動時のエラーを修正しました
* 対話モードで実行されない一部のコマンドの問題を修正しました

### <a name="iot"></a>IoT

* デバイス プロビジョニング サービスのサポートを追加しました
* コマンドとコマンドのヘルプに非推奨を示すメッセージを追加しました
* ユーザーに IoT 拡張機能を通知する IoT チェックを追加しました

### <a name="monitor"></a>モニター

* 複数診断設定のサポートを追加しました `az monitor diagnostic-settings create` で `--name` パラメーターが必須になりました
* 診断設定カテゴリを取得する `monitor diagnostic-settings categories` コマンドを追加しました

### <a name="network"></a>ネットワーク

* `vnet-gateway update` でのアクティブ/スタンバイ モードの切り替え時の問題を修正しました
* `application-gateway [create|update]` に HTTP2 のサポートを追加しました

### <a name="profile"></a>プロファイル

* ユーザーが割り当てた ID でのログインのサポートを追加しました

### <a name="role"></a>Role

* グラフ クエリをバイパスするために、`role assignment create` に `--assignee-object-id` 引数を追加しました

### <a name="service-fabric"></a>Service Fabric

* クラスターの作成時の検証応答に詳細なエラーを追加しました
* 一部のコマンドでのクライアントが見つからない問題を修正しました

### <a name="vm"></a>VM

* [プレビュー] `vmss` のクロス ゾーンのサポート
* [破壊的変更] 単一ゾーン `vmss` の既定値を "Standard" ロード バランサーに変更しました
* [破壊的変更] EMSI の `externalIdentities` を `userAssignedIdentities` に変更しました
* [プレビュー] OS ディスク スワップのサポートを追加しました
* 他のサブスクリプションの VM イメージの使用のサポートを追加しました
* `--plan-name`、`--plan-product`、`--plan-promotion-code`、`--plan-publisher` の各引数を `[vm|vmss] create` に追加しました
* `[vm|vmss] create` でのエラーの問題を修正しました
* `vm image list --all` に起因するリソースの過剰使用を修正しました

## <a name="december-19-2017"></a>2017 年 12 月 19 日

バージョン 2.0.23

* ユーザーが割り当てた ID でのログインのサポートを追加しました

### <a name="container"></a>コンテナー

* コンテナー ログのパラメーターのパラメーターの不適切な順序を修正しました

### <a name="network"></a>ネットワーク

* `--disable-bgp-route-propagation` 引数を `route-table [create|update]` に追加しました
* `--ip-tags` 引数を `public-ip [create|update]` に追加しました

### <a name="storage"></a>ストレージ

* ストレージ V2 のサポートを追加しました

### <a name="vm"></a>VM

* [プレビュー] VM と VMSS のユーザーが割り当てた ID のサポートを追加しました


## <a name="december-5-2017"></a>2017 年 12 月 5 日

バージョン 2.0.22

* `az component` コマンドを削除しました。 代わりに `az extension` を使用してください

### <a name="core"></a>コア
* `AZURE_US_GOV_CLOUD` AAD 機関エンドポイントを、login.microsoftonline.com から login.microsoftonline.us に変更しました
* テレメトリが連続的に再送信される問題を修正しました

### <a name="acs"></a>ACS

* `aks install-connector` コマンドと `aks remove-connector` コマンドを追加しました
* `acs create` のエラー報告を改善しました
* 完全修飾パスなしでの `aks get-credentials -f` の使用方法を修正しました

### <a name="advisor"></a>Advisor

* 最初のリリース

### <a name="appservice"></a>Appservice

* `webapp config ssl upload` による証明書名の生成を修正しました
* 正しいアプリを表示するように、`webapp [list|show]` と `functionapp [list|show]` を修正しました
* `WEBSITE_NODE_DEFAULT_VERSION` の既定値を追加しました

### <a name="consumption"></a>従量課金

* API バージョン 2017-11-30 のサポートを追加しました

### <a name="container"></a>コンテナー

* 既定のポート回帰を修正しました

### <a name="monitor"></a>モニター

* メトリック コマンドに多次元のサポートを追加しました

### <a name="resource"></a>リソース

* `--include-response-body` 引数を `resource show` に追加しました

### <a name="role"></a>Role

* `role assignment list` に、"従来の" 管理者の既定の割り当ての表示を追加しました
* `ad sp reset-credentials` で、上書きではなく、資格情報を追加できるようにしました
* `ad sp create-for-rbac` のエラー報告を改善しました

### <a name="sql"></a>SQL

* `sql db list-usages` コマンドと `sql db show-usage` コマンドを追加しました
* `sql server conn-policy show` コマンドと `sql server conn-policy update` コマンドを追加しました

### <a name="vm"></a>VM

* `az vm list-skus` にゾーン情報を追加しました


## <a name="november-14-2017"></a>2017 年 11 月 14 日

バージョン 2.0.21

### <a name="acr"></a>ACR

* レプリケーション リージョンで webhook を作成するためのサポートを追加しました


### <a name="acs"></a>ACS

* AKS の "エージェント" という用語をすべて "ノード" に変更しました
* `acs create` の `--orchestrator-release` オプションを非推奨にしました
* `Standard_D1_v2` に対する AKS の既定 VM サイズを変更しました
* Windows での `az aks browse` を修正しました
* Windows での `az aks get-credentials` を修正しました

### <a name="appservice"></a>Appservice

* Web アプリおよび関数アプリ用のデプロイ ソース `config-zip` を追加しました
* `--docker-container-logging` オプションを `az webapp log config` に追加しました
* `storage` オプションを `az webapp log config` のパラメーター `--web-server-logging` から削除しました
* `deployment user set` のエラー メッセージを改善しました
* Linux 関数アプリを作成するためのサポートを追加しました
* `list-locations` (固定)

### <a name="batch"></a>Batch

* `--image` を指定してリソース ID を使用した場合のプール作成コマンドのバグを修正しました

### <a name="batchai"></a>Batchai

* `file-server create` コマンドで VM サイズを指定する場合の `--vm-size` の短いオプション `-s` を追加しました
* ストレージ アカウント名とキー引数を `cluster create` パラメーターに追加しました
* `job list-files` および `job stream-file` のドキュメントを修正しました
* `job create` コマンドでクラスター名を指定する場合の `--cluster-name` の短いオプション `-r` を追加しました

### <a name="cloud"></a>クラウド

* 必要なエンドポイントのないクラウドの登録を防ぐために `cloud [register|update]` を変更しました

### <a name="container"></a>コンテナー

* 複数のポートを開くためのサポートを追加しました
* コンテナー グループ再起動ポリシーを追加しました
* Azure ファイル共有をボリュームとしてマウントするためのサポートを追加しました
* ヘルパー ドキュメントを更新しました

### <a name="data-lake-analytics"></a>Data Lake Analytics

* より簡潔な情報を返すように `[job|account] list` を変更しました

### <a name="data-lake-store"></a>Data Lake Store

* より簡潔な情報を返すように `account list` を変更しました

### <a name="extension"></a>拡張機能

* 公式の Microsoft 拡張機能を一覧表示できるようにするための `extension list-available` を追加しました
* 拡張機能を名前別にインストールできるように、`--name` を `extension [add|update]` に追加しました

### <a name="iot"></a>IoT

* 証明機関 (CA) および証明書チェーンのサポートを追加しました

### <a name="monitor"></a>モニター

* `activity-log alert` コマンドを追加しました

### <a name="network"></a>ネットワーク

* CAA DNS レコードのサポートを追加しました
* エンドポイントを `traffic-manager profile update` で更新できない問題を修正しました
* VNET の作成方法によっては `vnet update --dns-servers` が機能しない問題を修正しました
* 相対 DNS 名が `dns zone import` によって正しくインポートされない問題を修正しました

### <a name="reservations"></a>Reservations

* 初期プレビュー リリース

### <a name="resource"></a>リソース

* リソース ID のサポートを `--resource` パラメーターとリソースレベル ロックに追加しました

### <a name="sql"></a>SQL

* `--ignore-missing-vnet-service-endpoint` パラメーターを `sql server vnet-rule [create|update]` に追加しました

### <a name="storage"></a>ストレージ

* SKU `Standard_RAGRS` を既定値として使用するように `storage account create` を変更しました
* 非 ASCII 文字を含むファイル/BLOB 名を処理する場合のバグを修正しました
* `storage [blob|file] copy start-batch` での `--source-uri` の使用を妨げるバグを修正しました
* `storage [blob|file] delete-batch` で複数のオブジェクトを glob および削除するコマンドを追加しました
* `storage metrics update` でメトリックを有効にする場合の問題を修正しました
* `storage blob upload-batch` の使用時に 200 GB を超えるファイルにおける問題を修正しました
* `--bypass` と `--default-action` が `storage account [create|update]` によって無視される問題を修正しました

### <a name="vm"></a>VM

* `Basic` サイズ レベルの使用を妨げていり `vmss create` のバグを修正しました
* 課金情報を含むカスタム イメージ用に `--plan` 引数を `[vm|vmss] create` に追加しました
* `vm secret `[add|remove|list]` コマンドを追加しました
* `vm format-secret` から `vm secret format` への名称変更
* `--encrypt format` 引数を `vm encryption enable` に追加しました

## <a name="october-24-2017"></a>2017 年 10 月 24 日

バージョン 2.0.20

### <a name="core"></a>コア

* `MGMT_STORAGE` API バージョン `2016-01-01` を使用するように `2017-03-09-profile` を更新しました

### <a name="acr"></a>ACR

* `2017-10-01` API バージョンを指すようにリソース管理を更新しました
* "Bring Your Own Storage" SKU をクラシックに変更しました
* レジストリの SKU の名前を Basic、Standard、Premium に変更しました

### <a name="acs"></a>ACS

* [プレビュー] `az aks` コマンドを追加しました
* Kubernetes の `get-credentials` を修正しました

### <a name="appservice"></a>Appservice

* ダウンロードした `webapp` ログが無効である可能性のある問題を修正しました

### <a name="component"></a>コンポーネント

* すべてのインストーラー向けのより明確な非推奨メッセージと、確認プロンプトを追加しました

### <a name="monitor"></a>モニター

* `action-group` コマンドを追加しました

### <a name="resource"></a>リソース

* `group export` における msrest の依存関係の最新バージョンとの非互換性を修正しました
* 組み込みのポリシー定義とポリシーセットの定義を使用するように `policy assignment create` を修正しました

### <a name="vm"></a>VM

* `--accelerated-networking` 引数を `vmss create` に追加しました


## <a name="october-9-2017"></a>2017 年 10 月 9 日

バージョン 2.0.19

### <a name="core"></a>コア

* 末尾にスラッシュが付いている ADFS 権限 URL の処理を Azure Stack に追加しました

### <a name="appservice"></a>Appservice

* 新しいコマンド `webapp update` による全体的な更新を追加しました

### <a name="batch"></a>Batch

* Batch SDK 4.0.0 に更新しました
* publish:offer:sku:version に加えて ARM イメージ参照をサポートするように VirtualMachineConfiguration の `--image` オプションを更新しました
* Batch 拡張機能のコマンドの新しい CLI 拡張モデルのサポートを追加しました
* コンポーネント モデルから Batch のサポートを削除しました

### <a name="batchai"></a>Batchai

* Batch AI モジュールの最初のリリース

### <a name="keyvault"></a>KeyVault

* Azure Stack で ADFS を使用したときの Key Vault の認証の問題を修正しました。 [(#4448)](https://github.com/Azure/azure-cli/issues/4448)

### <a name="network"></a>ネットワーク

* アドレス プールを空にできるように `application-gateway address-pool create` の `--server` 引数を省略可能に変更しました
* 最新機能をサポートするように `traffic-manager` を更新しました

### <a name="resource"></a>リソース

* リソース グループ名に関する `--resource-group/-g` オプションのサポートを `group` に追加しました
* サブスクリプション レベルのロックを処理するための `account lock` 用のコマンドを追加しました
* グループ レベルのロックを処理するための `group lock` 用のコマンドを追加しました
* リソース レベルのロックを処理するための `resource lock` 用のコマンドを追加しました

### <a name="sql"></a>Sql

* SQL Transparent Data Encryption (TDE) および Bring Your Own Key による TDE のサポートを追加しました
* `db list-deleted` コマンドと `db restore --deleted-time` パラメーターを追加し、削除したデータベースを検索して復元できるようにしました。
* `db op list` と `db op cancel` を追加し、データベースに対して実行中の操作を一覧表示およびキャンセルできるようにしました。

### <a name="storage"></a>ストレージ

* ファイル共有スナップショットのサポートを追加しました

### <a name="vm"></a>VM

* `-d` を使用すると存在しないプライベート IP アドレスでクラッシュする `vm show` のバグを修正しました
* [プレビュー] ローリング アップグレードのサポートを `vmss create` に追加しました
* `vm encryption enable` による暗号化設定の更新のサポートを追加しました
* `--os-disk-size-gb` パラメーターを `vm create` に追加しました
* Windows 用の `--license-type` パラメーターを `vmss create` に追加しました


## <a name="september-22-2017"></a>2017 年 9 月 22 日

バージョン 2.0.18

### <a name="resource"></a>リソース

* 組み込みのポリシー定義を表示するためのサポートを追加しました
* ポリシー定義を作成するためのサポート モード パラメーターを追加しました
* UI の定義とテンプレートのサポートを `managedapp definition create` に追加しました
* [破壊的変更]`managedapp` のリソースの種類を `appliances` から `applications`、`applianceDefinitions` から `applicationDefinitions` に変更しました

### <a name="network"></a>ネットワーク

* 可用性ゾーンのサポートを `network lb` および `network public-ip` サブコマンドに追加しました
* IPv6 Microsoft ピアリングのサポートを `express-route` に追加しました
* `asg` アプリケーション セキュリティ グループのコマンドを追加しました
* `--application-security-groups` 引数を `nic [create|ip-config create|ip-config update]` に追加しました
* `--source-asgs` 引数と `--destination-asgs` 引数を `nsg rule [create|update]` に追加しました
* `--ddos-protection` 引数と `--vm-protection` 引数を `vnet [create|update]` に追加しました
* `network [vnet-gateway|vpn-client|show-url]` コマンドを追加しました

### <a name="storage"></a>ストレージ

* SDK の更新後に `storage account network-rule` コマンドが失敗する可能性がある問題を修正しました

### <a name="eventgrid"></a>Eventgrid

* 新しい API バージョン "2017-09-15-preview" を使用するように Azure Event Grid Python SDK を更新しました

### <a name="sql"></a>SQL

* `sql server list` の引数 `--resource-group` を省略可能に変更しました。 指定しなかった場合は、サブスクリプション内のすべての SQL Server が返されます
* `--no-wait` パラメーターを `db [create|copy|restore|update|replica create|create|update]` と `dw [create|update]` に追加しました

### <a name="keyvault"></a>KeyVault

* プロキシの内側からの KeyVault コマンドのサポートを追加しました

### <a name="vm"></a>VM

* 可用性ゾーンに対するサポートを `[vm|vmss|disk] create` に追加しました
* `vmss create` で `--app-gateway ID` を使用するとエラーになる問題を修正しました
* `--asgs` 引数を `vm create` に追加しました
* `vm run-command` を使用して VM でコマンドを実行するためのサポートを追加しました
* [プレビュー] `vmss encryption` による VMSS ディスク暗号化のサポートを追加しました
* `vm perform-maintenance` を使用して VM のメンテナンスを行うためのサポートを追加しました

### <a name="acs"></a>ACS

* ACS プレビューのリージョン用に `--orchestrator-release` 引数を `acs create` に追加しました

### <a name="appservice"></a>Appservice

* `webapp auth [update|show]` で認証設定を更新および表示する機能を追加しました

### <a name="backup"></a>バックアップ

* プレビュー リリース


## <a name="september-11-2017"></a>2017 年 9 月 11 日

バージョン 2.0.17

### <a name="core"></a>コア

* テレメトリで独自の関連付け ID を設定するコマンド モジュールを有効にしました
* テレメトリが診断モードに設定された際に発生する JSON ダンプの問題を修正しました

### <a name="acs"></a>ACS

* `acs list-locations` コマンドを追加しました
* `ssh-key-file` を予想される既定値と共に使用されるようにしました

### <a name="appservice"></a>Appservice

* アクティブなサービス プラン以外のリソース グループで Web アプリを作成する機能を追加しました

### <a name="cdn"></a>CDN

* `cdn custom-domain create` の "CustomDomain is not interable" バグを修正しました

### <a name="extension"></a>拡張機能

* 最初のリリース

### <a name="keyvault"></a>KeyVault

* `keyvault set-policy` について、アクセス許可の大文字と小文字が区別される問題を修正しました

### <a name="network"></a>ネットワーク

* `vnet list-private-access-services` から `vnet list-endpoint-services` への名称変更
* `vnet subnet create/update` の `--private-access-services` 引数の名前を `--service-endpoints` に変更しました
* `nsg rule create/update` に対する複数の IP 範囲およびポート範囲のサポートを追加しました
* `lb create` に対する SKU のサポートを追加しました
* `public-ip create` に対する SKU のサポートを追加しました

### <a name="resource"></a>リソース

* `policy definition create` と `policy definition update` でリソース ポリシーのパラメーター定義を渡せるようにします
* `policy assignment create` でパラメーター値を渡せるようにします
* すべてのパラメーターについて JSON またはファイルを渡せるようにします
* API バージョンを増やしました

### <a name="sql"></a>SQL

* `sql server vnet-rule` コマンドを追加しました

### <a name="vm"></a>VM

* 固定:`--scope` が指定されないとアクセス権が割り当てられない
* 固定:ポータルと同じ拡張機能の名前付けが行われる
* `[vm|vmss] create` 出力から `subscription` を削除しました
* 修正済み: `[vm|vmss] create` ストレージ SKU がイメージ付きのデータ ディスクに適用されない
* 修正済み: `vm format-secret --secrets` で、改行によって分かれた ID を使用できない

## <a name="august-31-2017"></a>2017 年 8 月 31 日

バージョン 2.0.16

### <a name="keyvault"></a>KeyVault

* `secret download` でシークレット エンコードを自動的に解決しようとする際に発生するバグを修正しました

### <a name="sf"></a>SF

* すべてのコマンドが非推奨とされ、Service Fabric CLI (sfctl) が優先されます

### <a name="storage"></a>ストレージ

* ネットワーク ACL 機能がサポートされていないリージョンでストレージ アカウントを作成できない問題を修正しました
* コンテンツの種類とエンコードがどちらも指定されていない場合、BLOB とファイルのアップロード中にコンテンツの種類とエンコードを決定します

## <a name="august-28-2017"></a>2017 年 8 月 28 日

バージョン 2.0.15

### <a name="cli"></a>CLI

* `--version` に法的事項を追加しました

### <a name="acs"></a>ACS

* プレビュー リージョンを修正しました
* 既定の `dns_name_prefix` を正しい形式にしました
* ACS コマンド出力を最適化しました

### <a name="appservice"></a>Appservice

* [破壊的変更]`az webapp config appsettings [delete|set]` の出力の不整合を修正しました
* `az webapp config container set --docker-custom-image-name` の `-i` の新しいエイリアスを追加しました
* `az webapp log show` を公開しました
* App Service プラン、メトリック、または DNS 登録を保持するために、`az webapp delete` の新しい引数を公開しました
* 固定:スロット設定の正常な検出

### <a name="iot"></a>IoT

* #3934 を修正しました:ポリシーの作成で既存のポリシーが消去されなくなりました

### <a name="network"></a>ネットワーク

* [破壊的変更] 名前を `vnet list-private-access-services` から `vnet list-endpoint-services` に変更しました
* [破壊的変更]`vnet subnet [create|update]` のオプション `--private-access-services` の名前を `--service-endpoints` に変更しました
* `nsg rule [create|update]` に対する複数 IP およびポート範囲のサポートを追加しました
* `lb create` に対する SKU のサポートを追加しました
* `public-ip create` に対する SKU のサポートを追加しました

### <a name="profile"></a>プロファイル

* 仮想マシンの ID を使用してログインするために、`--msi` と `--msi-port` を公開しました

### <a name="service-fabric"></a>Service Fabric

* プレビュー リリース
* コマンドに対するレジストリのユーザー/パスワード規則を簡略化しました
* パラメーターを渡した後でもユーザーにパスワードの入力が求められる問題を修正しました
* 空の `registry_cred` のサポートを追加しました

### <a name="storage"></a>ストレージ

* BLOB 層の設定を有効にしました
* サービス トンネリングをサポートするために、`--bypass` 引数と `--default-action` 引数を `storage account [create|update]` に追加しました
* VNET ルールと IP ベースのルールを `storage account network-rule` に追加するためのコマンドを追加しました
* 顧客管理キーによるサービスの暗号化を有効にしました
* [破壊的変更]`az storage account create and az storage account update` コマンドの `--encryption` オプションの名前を `--encryption-services` に変更しました
* 修正済み #4220: `az storage account update encryption` - 構文の不一致

### <a name="vm"></a>VM

* `vmss get-instance-view` で `--instance-id *` を使用した場合に、余分な間違えた情報が表示される問題を修正しました
* `vmss create` に `--lb-sku` のサポートを追加しました。
* `[vm|vmss] create` の管理者名ブロック リストから人物名を削除しました
* イメージからプラン情報を抽出できない場合に `[vm|vmss] create` がエラーをスローする問題を修正しました
* 内部 LB で vmms scaleset を作成するときのクラッシュを修正しました
* `vm availability-set create` で `--no-wait` 引数が機能しない問題を修正しました


## <a name="august-15-2017"></a>2017 年 8 月 15 日

バージョン 2.0.14

### <a name="acs"></a>ACS

* kubernetes の sshMaster0 ポート番号を修正しました

### <a name="appservice"></a>Appservice

* 新しい Git ベースの Linux Web アプリを作成するときの例外を修正しました

### <a name="event-grid"></a>Event Grid

* SDK 依存関係を追加しました

## <a name="august-11-2017"></a>2017 年 8 月 11 日

バージョン 2.0.13

### <a name="acs"></a>ACS

* プレビュー リージョンを追加しました

### <a name="batch"></a>Batch

* Batch SDK 3.1.0 と Batch Management SDK 4.1.0 に更新しました
* ジョブのタスク数を表示する新しいコマンドを追加しました
* リソース ファイルの SAS URL 処理のバグを修正しました
* Batch アカウントのエンドポイントで、オプションの "https://" プレフィックスがサポートされるようになりました
* ジョブに対する 100 を超えるタスクのリストの追加をサポートします
* 拡張機能コマンド モジュールの読み込みのデバッグ ログを追加しました

### <a name="component"></a>コンポーネント

* "az component" コマンドに非推奨警告を追加しました

### <a name="container"></a>コンテナー

* `create`:環境変数内で等号を使用できない問題を修正しました


### <a name="data-lake-store"></a>Data Lake Store

* プログレス コントロールを有効にしました

### <a name="event-grid"></a>Event Grid

* 最初のリリース

### <a name="network"></a>ネットワーク

* `lb`:特定の子リソース名が省略された場合に正しく解決されない問題を修正しました
* `application-gateway {subresource} delete`:`--no-wait` が受け入れられない問題を修正しました
* `application-gateway http-settings update`:`--connection-draining-timeout` を無効にできない問題を修正しました
* `az network vpn-connection ipsec-policy add` での予期しないキーワード引数 `sa_data_size_kilobyes` のエラーを修正しました

### <a name="profile"></a>プロファイル

* `account list`:サーバーの最新のサブスクリプションを同期する `--refresh` を追加しました

### <a name="storage"></a>ストレージ

* システムによって割り当てられた ID によるストレージ アカウントの更新を有効にします

### <a name="vm"></a>VM

* `availability-set`:変換時の障害ドメイン数を公開しました
* `list-skus` コマンドを公開しました
* ロールの割り当てを作成しない ID の割り当てをサポートします
* データ ディスクのアタッチ時にストレージ SKU を適用します
* 管理ディスクを使用する場合の既定の os-disk 名とストレージ SKU を削除しました


## <a name="july-28-2017"></a>2017 年 7 月 28 日

バージョン 2.0.12

* コンテナー コマンドを追加しました
* 請求および使用量モジュールを追加しました

```text
azure-cli (2.0.12)

acr (2.0.9)
acs (2.0.11)
appservice (0.1.11)
batch (3.0.3)
billing (0.1.3)
cdn (0.0.6)
cloud (2.0.7)
cognitiveservices (0.1.6)
command-modules-nspkg (2.0.1)
component (2.0.6)
configure (2.0.10)
consumption (0.1.3)
container (0.1.7)
core (2.0.12)
cosmosdb (0.1.11)
dla (0.0.10)
dls (0.0.11)
feedback (2.0.6)
find (0.2.6)
interactive (0.3.7)
iot (0.1.10)
keyvault (2.0.8)
lab (0.0.9)
monitor (0.0.8)
network (2.0.11)
nspkg (3.0.1)
profile (2.0.9)
rdbms (0.0.5)
redis (0.2.7)
resource (2.0.11)
role (2.0.9)
sf (1.0.5)
sql (2.0.8)
storage (2.0.11)
vm (2.0.11)
```

### <a name="core"></a>コア

* サービス プリンシパルの SDK 認証情報と証明書を出力します
* デプロイの進行状況の例外を修正しました
* サブスクリプション クライアントを作成するために、現在のクラウドの ARM エンドポイントを使用します
* clouds.config ファイルの同時処理機能が向上しました (#3636)
* コマンド実行のたびにクライアント要求 ID を更新します
* 正しい SDK プロファイルでサブスクリプション クライアントを作成します (#3635)
* テンプレート デプロイの進行状況レポート (#3510)
* jmespath クエリを通じてテーブル出力フィールドを選択するサポートを追加しました (#3581)
* 解析引数のミュートを改善し、ジェスチャによる履歴を追加しました (#3434)
* 正しい SDK プロファイルでサブスクリプション クライアントを作成します
* すべての既存のレコーディング ファイルを最新のフォルダーに移動します
* VM/VMSS 作成のべき等を修正しました (#3586)
* コマンド パスで大文字と小文字が区別されなくなりました
* 特定のブール型パラメーターで大文字と小文字が区別されなくなりました
* Azure Stack のような ADFS オンプレミス サーバーへのログインをサポートします
* clouds.config への同時書き込みを修正しました (#3255)

### <a name="acr"></a>ACR

* 管理対象レジストリ用の `show-usage` コマンドを追加しました
* 管理対象レジストリの SKU 更新をサポートします
* 管理対象レジストリと管理 SKU を追加しました
* 管理対象レジストリの webhook と acr webhook コマンド モジュールを追加しました
* AAD 認証と acr login コマンドを追加しました
* Docker リポジトリ、マニフェスト、タグ用の delete コマンドを追加しました

### <a name="acs"></a>ACS

* API バージョン 2017-07-01 をサポートします

### <a name="appservice"></a>Appservice

* Linux Web アプリの一覧表示で何も返されないバグを修正しました
* acr からの資格情報の取得をサポートします
* `appservice web` のすべてのコマンドを削除します
* コマンド出力で Docker レジストリのパスワードをマスクします (#3656)
* macOS でエラーにならずに既定のブラウザーが使用されます (#3623)
* `webapp log tail` と `webapp log download` のヘルプを改善します (#3624)
* 静的ルーティングを構成するための `traffic-routing` コマンドを公開しました (#3566)
* ソース管理の構成で信頼性のための修正が追加されました (#3245)
* Windows Web アプリ用の `webapp config update` から、サポートされていない `--node-version` 引数を削除しました。 代わりに `webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=...` を使用してください

### <a name="batch"></a>Batch

* Batch SDK 3.0.0 に更新して、プール内の優先度が低い VM がサポートされるようにしました
* `pool create` のオプション `--target-dedicated` の名前を `--target-dedicated-nodes` に変更しました
* `pool create` のオプションの `--target-low-priority-nodes` と `--application-licenses` を追加しました

### <a name="cdn"></a>CDN

* `--profile-name` によって指定されたプロファイルが存在しない場合に表示される `cdn endpoint list` のエラー メッセージを、より適切な内容に変更しました

### <a name="cloud"></a>クラウド

* クラウド メタデータ エンドポイントの API バージョンを YYYY-MM-DD 形式に変更しました
* ギャラリー エンドポイントは必須ではありません
* ARM リソース マネージャー エンドポイントだけでのクラウドの登録をサポートします
* `cloud set` で現在のクラウドを選択するときにプロファイルを選択できるオプションを提供しました
* `endpoint_vm_image_alias_doc` を公開しました

### <a name="cosmosdb"></a>Cosmos DB

* カスタム パーティション キーでコレクションを作成できるように修正しました
* 既定のコレクション TTL のサポートを追加しました

### <a name="data-lake-analytics"></a>Data Lake Analytics

* コンピューティング ポリシー管理用のコマンドを `dla account compute-policy` 見出しの下に追加しました
* `dla job pipeline show` を追加しました
* `dla job recurrence list` を追加しました

### <a name="data-lake-store"></a>Data Lake Store

* ユーザー管理のキー コンテナーのキー ローテーションのサポートを `dls account update` に追加しました
* 基になる Data Lake Store ファイル システム SDK のバージョンを更新して、パフォーマンスの問題に対応しました
* コマンド `dls enable-key-vault` を追加しました。 このコマンドでは、Data Lake Store アカウント内でデータの暗号化を使用するために、ユーザー指定のキー コンテナーの有効化が試行されます

### <a name="interactive"></a>Interactive

* キャッシュされたコマンドを使用することで、起動時間が向上しました
* テスト カバレッジが増えました
* "?" ジェスチャが次のコマンドにも挿入されるよう強化しました
* プロファイル 2017-03-09-profile-preview との対話エラーを修正しました (#3587)
* `--version` を対話モードのパラメーターとして使用できるようにしました (#3645)
* 対話モードで検証の完了時にエラーがスローされないようにします (#3570)
* テンプレート デプロイの進行状況レポート (#3510)
* `--progress` フラグを追加しました
* 入力候補から `--debug` と `--verbose` を削除しました
* 入力候補から `interactive` を削除しました (#3324)

### <a name="iot"></a>IoT

* ポリシーの作成で既存のポリシーが消去されないように修正しました。 (#3934)

### <a name="key-vault"></a>Key Vault

* キー コンテナーの回復機能のために、以下のコマンドを追加しました。
  * `keyvault` サブコマンドの `purge`、`recover`、`keyvault list-deleted`
  * `keyvault secret` サブコマンドの `backup`、`restore`、`purge`、`recover`、`list-deleted`
  * `keyvault certificate` サブコマンドの `purge`、`recover`、`list-deleted`
  * `keyvault key` サブコマンドの `purge`、`recover`、`list-deleted`
* サービス プリンシパルのキー コンテナーの統合を追加しました (#3133)
* キー コンテナー データプレーンを 0.3.2 に更新しました。 (#3307)

### <a name="lab"></a>ラボ

* `az lab vm claim` を通じてラボ内の任意の VM を要求するためのサポートを追加しました
* `az lab vm list` と `az lab vm show` のテーブル出力フォーマッタを追加しました

### <a name="monitor"></a>モニター

* `monitor autoscale-settings get-parameters-template` コマンドのテンプレート ファイルを修正しました (#3349)
* `monitor alert-rule-incidents list` から `monitor alert list-incidents` への名称変更
* `monitor alert-rule-incidents show` から `monitor alert show-incident` への名称変更
* `monitor metric-defintions list` から `monitor metrics list-definitions` への名称変更
* `monitor alert-rules` から `monitor alert` への名称変更
* `monitor alert create` を次のように変更しました。
  * `condition` および `action` サブコマンドが JSON を受け入れなくなりました
  * ルール作成プロセスを簡略化するために多数のパラメーターを追加します
  * `location` が必須ではなくなりました
  * ターゲットの名前と ID のサポートを追加します
  * `--alert-rule-resource-name` を削除します
  * `is-enabled` の名前を `enabled` に変更し、省略可能にしました
  * `description` の既定値が、指定された条件に基づいて設定されるようになりました
  *  新しい形式をわかりやすく示すための例を追加します
* `monitor metric` コマンドの名前または ID をサポートします
* `monitor alert rule update` に便利な引数と例を追加しました

### <a name="network"></a>ネットワーク

* `list-private-access-services` コマンドを追加しました
* `--private-access-services` 引数を `vnet subnet create` と `vnet subnet update` に追加しました
* `application-gateway redirect-config create` が失敗する問題を修正しました
* `application-gateway redirect-config update` で `--no-wait` を指定しても機能しない問題を修正しました
* `application-gateway address-pool create` と `application-gateway address-pool update` で `--servers` 引数を使用する場合のバグを修正しました
* `application-gateway redirect-config` コマンドを追加しました
* `list-options`、`predefined list`、`predefined show` の各コマンドを `application-gateway ssl-policy` に追加しました
* `--name`、`--cipher-suites`、`--min-protocol-version` の各引数を `application-gateway ssl-policy set` に追加しました
* `--host-name-from-backend-pool`、`--affinity-cookie-name`、`--enable-probe`、`--path` の各引数を `application-gateway http-settings create` と `application-gateway http-settings update` に追加しました
* `--default-redirect-config` 引数と `--redirect-config` 引数を `application-gateway url-path-map create` と `application-gateway url-path-map update` に追加しました
* `--redirect-config` 引数を `application-gateway url-path-map rule create` に追加しました
* `--no-wait` のサポートを `application-gateway url-path-map rule delete` に追加しました
* `--host-name-from-http-settings`、`--min-servers`、`--match-body`、`--match-status-codes` の各引数を `application-gateway probe create` と `application-gateway probe update` に追加しました
* `--redirect-config` 引数を `application-gateway rule create` と `application-gateway rule update` に追加しました
* `--accelerated-networking` のサポートを `nic create` と `nic update` に追加しました
* `nic create` から `--internal-dns-name-suffix` 引数を削除しました
* `--dns-servers` のサポートを `nic update` と `nic create` に追加しました:--dns-servers のサポートを追加しました
* `local-gateway create` で `--local-address-prefixes` が無視されるバグを修正しました
* `--dns-servers` のサポートを `vnet update` に追加しました
* `express-route peering create` でルート フィルター処理をせずにピアリングを作成するときのバグを修正しました
* `express-route update` で `--provider` および `--bandwidth` 引数が機能しないバグを修正しました
* `network watcher show-topology` の既定値設定ロジックのバグを修正しました
* `network list-usages` の出力書式設定を改善しました
* `application-gateway http-listener create` に既定のフロントエンド IP を使用します (1 つしかない場合)
* `application-gateway rule create` に既定のアドレス プール、HTTP 設定、および HTTP リスナーを使用します (1 つしかない場合)
* `lb rule create` に既定のフロントエンド IP およびバックエンド プールを使用します (1 つしかない場合)
* `lb inbound-nat-rule create` に既定のフロントエンド IP を使用します (1 つしかない場合)

### <a name="profile"></a>プロファイル

* 管理対象 ID での VM 内のログインをサポートします
* `account show` で SDK 認証ファイル形式での出力をサポートします
* "--expanded-view" を使用した場合に非推奨警告を表示します
* 生の AAD トークンを提供するための `get-access-token` コマンドを追加しました
* サブスクリプションが関連付けられていないユーザー アカウントでのログインをサポートします

### <a name="rdbms"></a>RDBMS

* サブスクリプション全体のサーバーの一覧表示をサポートします (#3417)
* `% server_type` が見つからないために `%s` が処理されない問題を修正しました (#3393)
* ドキュメント ソース マップを修正し、検証するための CI タスクを追加しました (#3361)
* MySQL および PostgreSQL のヘルプを修正しました (#3369)

### <a name="resource"></a>リソース

* `group deployment create` の不足しているパラメーターの指定を求めるメッセージを改善しました
* `--parameters KEY=VALUE` 構文の解析を改善しました
* `group deployment create` のパラメーター ファイルが `@<file>` 構文で認識されなくなった問題を修正しました
* `resource` および `managedapp` コマンドで `--ids` 引数をサポートします
* いくつかの解析およびエラー メッセージを修正しました (#3584)
* `<resource-namespace>` と `<resource-type>` を受け入れるよう、`lock` コマンドの `--resource-type` の解析を修正しました
* テンプレート リンク テンプレートのパラメーター チェックを追加しました (#3629)
* `KEY=VALUE` 構文でデプロイ パラメーターを指定するためのサポートを追加しました

### <a name="role"></a>Role

* `create-for-rbac` で SDK 認証ファイル形式での出力をサポートします
* サービス プリンシパルを削除するときに、ロールの割り当てと、関連する AAD アプリケーションがクリーンアップされるようになりました (#3610)
* `app create` の引数 `--start-date` および `--end-date` の説明に時刻形式を含めます
* `--expanded-view` を使用した場合に非推奨警告を表示します
* キー コンテナーの統合を `create-for-rbac` および `reset-credentials` コマンドに追加しました

### <a name="service-fabric"></a>Service Fabric
* アプリケーション内の大きなファイルがアップロード時に切り詰められる問題を修正しました (#3666)
* Service Fabric コマンドのテストを追加しました (#3424)
* 多数の Service Fabric コマンドを修正しました (#3234)

### <a name="sql"></a>SQL

* 壊れた `sql server create` `--identity` パラメーターを削除しました
* `sql server create` および `sql server update` コマンドの出力からパスワード値を削除しました
* `sql db list-editions` および `sql elastic-pool list-editions` コマンドを追加しました

### <a name="storage"></a>ストレージ

* `storage blob list`、`storage container list`、`storage share list` の各コマンドから `--marker` オプションを削除しました (#3745)
* https 専用ストレージ アカウントの作成を有効にしました
* storage metrics、logging、cors の各コマンドを更新しました (#3495)
* CORS add からの例外メッセージを言い換えました (#3638) (#3362)
* ジェネレーターをドライ ラン モードのダウンロード バッチ コマンド内の一覧に変換しました (#3592)
* BLOB ダウンロード バッチのドライ ランの問題を修正しました (#3640) (#3592)

### <a name="vm"></a>VM

* nsg の構成をサポートします
* DNS サーバーが正しく構成されないバグを修正しました
* 管理対象のサービス ID をサポートします
* 既存のロード バランサーを使用する `cmss create` で `--backend-pool-name` が必要だった問題を修正しました
* `vm image create` で作成された datadisks の lun を 0 から開始します


## <a name="may-10-2017"></a>2017 年 5 月 10 日

バージョン 2.0.6

* documentdb から cosmosdb に名前が変更されました
* rdbms (mysql、postgres) が追加されました
* Data Lake Analytics モジュールと Data Lake Store モジュールが追加されました
* Cognitive Services モジュールが追加されました
* Service Fabric モジュールが追加されました
* 対話型モジュールが追加されました (az-shell の名前変更)
* CDN コマンドに対応するようになりました
* コンテナー モジュールが削除されました
* "az --version" のショートカットとして "az -v" が追加されました ([#2926](https://github.com/Azure/azure-cli/issues/2926))
* パッケージ読み込みとコマンド実行のパフォーマンスが向上しています ([#2819](https://github.com/Azure/azure-cli/issues/2819))

```text
azure-cli (2.0.6)

acr (2.0.4)
acs (2.0.6)
appservice (0.1.6)
batch (2.0.4)
cdn (0.0.2)
cloud (2.0.2)
cognitiveservices (0.1.2)
command-modules-nspkg (2.0.0)
component (2.0.4)
configure (2.0.6)
core (2.0.6)
cosmosdb (0.1.6)
dla (0.0.6)
dls (0.0.6)
feedback (2.0.2)
find (0.2.2)
interactive (0.3.1)
iot (0.1.5)
keyvault (2.0.4)
lab (0.0.4)
monitor (0.0.4)
network (2.0.6)
nspkg (3.0.0)
profile (2.0.4)
rdbms (0.0.1)
redis (0.2.3)
resource (2.0.6)
role (2.0.4)
sf (1.0.1)
sql (2.0.3)
storage (2.0.6)
vm (2.0.6)
```

### <a name="core"></a>コア

* コア: 登録されていないプロバイダーによる例外がキャプチャされ、自動登録されます
* パフォーマンス: プロセスが終了するまで ADAL トークン キャッシュがメモリに保持されます ([#2603](https://github.com/Azure/azure-cli/issues/2603))
* 16 進フィンガープリント -o tsv から返されるバイトが修正されました ([#3053](https://github.com/Azure/azure-cli/issues/3053))
* Key Vault 証明書ダウンロードの強化と AAD SP 統合 ([#3003](https://github.com/Azure/azure-cli/issues/3003))
* Python の場所が "az —version" に追加されました ([#2986](https://github.com/Azure/azure-cli/issues/2986))
* ログイン: サブスクリプションがないときのログインに対応するようになりました ([#2929](https://github.com/Azure/azure-cli/issues/2929))
* コア: サービス プリンシパルを 2 回使用してログインするときのエラーが修正されました ([#2800](https://github.com/Azure/azure-cli/issues/2800))
* コア:accessTokens.json のファイル パスを環境変数で構成できます ([#2605](https://github.com/Azure/azure-cli/issues/2605))
* コア:構成済み既定値を省略可能な引数に適用できます ([#2703](https://github.com/Azure/azure-cli/issues/2703))
* コア:パフォーマンスの向上
* コア:カスタム CA 証明書 - REQUESTS_CA_BUNDLE 環境変数の設定を利用できます
* コア:クラウド構成 - "管理" エンドポイントが設定されていない場合に、"リソース マネージャー" エンドポイントが使用されます

### <a name="acs"></a>ACS

* マスター数とエージェント数が文字列から整数に修正されました
* 非同期作成用に "az acs create --no-wait" と "az acs wait" が公開されました
* ドライラン検証用に "az acs create --validate" が公開されました
* スケール コマンドの PUT 呼び出しの前の Windows プロファイルが削除されます ([#2755](https://github.com/Azure/azure-cli/issues/2755))

### <a name="appservice"></a>AppService

* 関数アプリ: 作成、表示、リスト、削除、ホスト名、SSL など、関数アプリに完全に対応するようになりました
* Team Services (VSTS) が継続的デリバリー オプションとして "appservice web source-control config" に追加されました
* "az webapp" が作成され "az app service web" が置き換えられています (下位互換性のために "az app service web" は 2 リリースだけ保持されます)
* WebApp 作成時にデプロイと "ランタイム スタック" を構成するための引数が公開されました
* "webapp list-runtimes" が公開されました
* 接続文字列の構成がサポートされています ([#2647](https://github.com/Azure/azure-cli/issues/2647))
* プレビューでのスロット スワップがサポートされています
* appservice コマンドからのポーランド語のエラー ([#2948](https://github.com/Azure/azure-cli/issues/2948))
* 証明書の操作に App Service プランのリソース グループが使用されます ([#2750](https://github.com/Azure/azure-cli/issues/2750))

### <a name="cosmosdb"></a>Cosmos DB

* documentdb モジュールから cosmosdb に名前が変更されました
* DocumentDB データプレーン API: データベースとコレクション管理に対応するようになりました
* データベース アカウントにおける自動フェールオーバーの有効化に対応するようになりました
* 新しい一貫性ポリシー ConsistentPrefix に対応するようになりました

### <a name="data-lake-analytics"></a>Data Lake Analytics

* ジョブ リストの結果と状態に対するフィルター処理でエラーがスローされるバグが修正されました
* 新しいカタログ項目の種類: パッケージに対応するようになりました。 `az dla catalog package` を介してアクセスされます
* 次のカタログ項目の一覧をデータベース内から表示できます (スキーマ仕様は不要です)。

  * テーブル
  * テーブル値関数
  * 表示
  * テーブルの統計。 スキーマと一緒に表示することもできますが、テーブル名を指定する必要はありません

### <a name="data-lake-store"></a>Data Lake Store

* 基になるファイルシステム SDK のバージョンが更新され、サーバー側スロットル処理への対応が強化されました
* パッケージ読み込みとコマンド実行のパフォーマンスが向上しています ([#2819](https://github.com/Azure/azure-cli/issues/2819))
* access show のヘルプがなかったため、 追加しました ([#2743](https://github.com/Azure/azure-cli/issues/2743))

### <a name="find"></a>Find

* 検索結果を改善し、検索インデックスのバージョン管理を可能にしました

### <a name="keyvault"></a>KeyVault

* BC: `az keyvault certificate download` によって -e が文字列またはバイナリから PEM または DER に変更され、オプションの表現が向上しました
* BC:--expires パラメーターと --not-before パラメーターはサービスでサポートされていないため、`keyvault certificate create` から削除されました
* --validity パラメーターが `keyvault certificate create` に追加され、--policy の値が選択的にオーバーライドされます
* "expires" と "not_before" が公開され、"validity_in_months" が公開されなかった場合に `keyvault certificate get-default-policy` で発生する問題が修正されました
* KeyVault における pem と pfx のインポートが修正されました ([#2754](https://github.com/Azure/azure-cli/issues/2754))

### <a name="lab"></a>ラボ

* ラボ環境用の作成、表示、削除、およびリスト コマンドが追加されました
* ラボで ARM テンプレートを表示するための表示およびリスト コマンドが追加されました
* ラボ環境で VM をフィルター処理するための --environment フラグが `az lab vm list` に追加されました
* ラボの数式内でアーティファクト スキャフォールディングをエクスポートするための便利なコマンド `az lab formula export-artifacts` が追加されました
* ラボ内でシークレットを管理するためのコマンドが追加されました

### <a name="monitor"></a>モニター

* バグの修正: JSON 文字列を使用するため `az alert-rules create` の `--actions` がモデル化されています ([#3009](https://github.com/Azure/azure-cli/issues/3009))
* バグの修正: diagnostic settings create が、表示コマンドからのログ/メトリックを受け付けません ([#2913](https://github.com/Azure/azure-cli/issues/2913))

### <a name="network"></a>ネットワーク

* `network watcher test-connectivity` コマンドが追加されました
* `network watcher packet-capture create` の `--filters` パラメーターに対応するようになりました
* Application Gateway 接続のドレインに対応するようになりました
* Application Gateway WAF 規則セットの構成に対応するようになりました
* ExpressRoute ルート フィルターとルールに対応するようになりました
* TrafficManager の地理的なルーティングに対応するようになりました
* VPN 接続ポリシー ベースのトラフィック セレクターに対応するようになりました
* VPN 接続 IPSec ポリシーに対応するようになりました
* `--no-wait` パラメーターまたは `--validate` パラメーターを使用するときの `vpn-connection create` のバグが修正されました
* アクティブ/アクティブ VNet ゲートウェイに対応するようになりました
* `network vpn-connection list/show` コマンドの出力から null 値が削除されました
* BC:`vpn-connection create` の出力のバグが修正されました
* "vpn-connection create" の "--key-length" 引数が適切に解析されないバグが修正されました
* `dns zone import` でレコードが適切にインポートされないバグが修正されました
* `traffic-manager endpoint update` が動作しないバグを修正しました
* "Network Watcher" プレビューのコマンドが追加されました

### <a name="profile"></a>プロファイル

* サブスクリプションが見つからないときのログインに対応するようになりました ([#2560](https://github.com/Azure/azure-cli/issues/2560))
* az account set --subscription で短いパラメーター名に対応するようになりました ([#2980](https://github.com/Azure/azure-cli/issues/2980))

### <a name="redis"></a>Redis

* Redis Cache のスケール機能も追加する更新コマンドが追加されました
* "update-settings" コマンドが廃止されました

### <a name="resource"></a>リソース

* managedapp と managedapp の定義コマンドが追加されました ([#2985](https://github.com/Azure/azure-cli/issues/2985))
* "provider operation" コマンドに対応するようになりました ([#2908](https://github.com/Azure/azure-cli/issues/2908))
* 汎用リソースの作成に対応するようになりました ([#2606](https://github.com/Azure/azure-cli/issues/2606))
* リソース解析および API バージョンの検索が修正されました ([#2781](https://github.com/Azure/azure-cli/issues/2781))
* az lock update のドキュメントが追加されました ([#2702](https://github.com/Azure/azure-cli/issues/2702))
* 存在しないグループのリソースの一覧を表示しようとするとエラーが出力されます ([#2769](https://github.com/Azure/azure-cli/issues/2769))
* [コンピューティング] VMSS と VM の可用性セットの更新に関する問題が修正されました。 ([#2773](https://github.com/Azure/azure-cli/issues/2773))
* parent-resource-path が None の場合のロックの作成と削除が修正されました ([#2742](https://github.com/Azure/azure-cli/issues/2742))

### <a name="role"></a>Role

* create-for-rbac: SP の終了日が、確実に証明書の有効期限の前に設定されます ([#2989](https://github.com/Azure/azure-cli/issues/2989))
* RBAC: "ad group" が完全にサポートされるようになりました ([#2016](https://github.com/Azure/azure-cli/issues/2016))
* ロール: ロール定義の更新に関する問題が修正されました ([#2745](https://github.com/Azure/azure-cli/issues/2745))
* create-for-rbac: ユーザーによって指定されたパスワードが確実に取得されます

### <a name="sql"></a>SQL

* az sql server list-usages コマンドと az sql db list-usages コマンドが追加されました
* SQL - リソース プロバイダーに直接接続できます ([#2832](https://github.com/Azure/azure-cli/issues/2832))

### <a name="storage"></a>ストレージ

* `storage account create` のリソース グループの場所に対する既定の場所
* 増分 BLOB コピーに対応するようになりました
* 大きなブロック BLOB アップロードに対応するようになりました
* アップロードするファイルが 200 GB を超える場合にブロック サイズが 100 MB に変更されます

### <a name="vm"></a>VM

* avail-set: UD&FD ドメイン数が省略可能になりました

  注:独立したクラウドの VM コマンド。次のマネージド ディスク関連機能は避けるようにしてください。
  1. az disk/snapshot/image
  2. az vm/vmss disk
  3. "az vm/vmss create" 内では、"—use-unmanaged-disk" を使用して管理対象ディスクを回避します。他のコマンドは機能します
* vm/vmss: SSH キー ペアを生成するときの警告テキストが向上します
* vm/vmss: プラン情報を必要とするマーケットプレイス イメージからの作成をサポートします ([#1209](https://github.com/Azure/azure-cli/issues/1209))


## <a name="april-3-2017"></a>2017 年 4 月 3 日

バージョン 2.0.2

このリリースでは、ACR、Batch、KeyVault、SQL コンポーネントをリリースしました

```text
azure-cli (2.0.2)

acr (2.0.0)
acs (2.0.2)
appservice (0.1.2)
batch (2.0.0)
cloud (2.0.0)
component (2.0.0)
configure (2.0.2)
container (0.1.2)
core (2.0.2)
documentdb (0.1.2)
feedback (2.0.0)
find (0.0.1b1)
iot (0.1.2)
keyvault (2.0.0)
lab (0.0.1)
monitor (0.0.1)
network (2.0.2)
nspkg (2.0.0)
profile (2.0.2)
redis (0.1.1b3)
resource (2.0.2)
role (2.0.1)
sql (2.0.0)
storage (2.0.2)
vm (2.0.2)
```

### <a name="core"></a>コア

* 既定の一覧に acr、lab、monitor、find モジュールを追加
* ログイン: 誤ったテナントをスキップ ([#2634](https://github.com/Azure/azure-cli/pull/2634))
* ログイン: 既定のサブスクリプションを "Enabled" の状態のサブスクリプションに設定 ([#2575](https://github.com/Azure/azure-cli/pull/2575))
* wait コマンドと --no-wait のサポートをより多くのコマンドに追加 ([#2524](https://github.com/Azure/azure-cli/pull/2524))
* コア: 証明書を持つサービス プリンシパルを使用したログインをサポート ([#2457](https://github.com/Azure/azure-cli/pull/2457))
* 不足しているテンプレート パラメーターの指定を求めるメッセージを追加 ([#2364](https://github.com/Azure/azure-cli/pull/2364))
* 既定のリソース グループ、既定の Web、既定の VM など、一般的な引数の既定値の設定をサポート
* 特定のテナントへのログインをサポート

### <a name="acs"></a>ACS

* [ACS] 既定の ACS クラスターを構成するためのサポートを追加 ([#2554](https://github.com/Azure/azure-cli/pull/2554))
* ssh キー パスワードの入力要求のサポートを追加 ([#2044](https://github.com/Azure/azure-cli/pull/2044))
* Windows クラスターのためのサポートを追加 ([#2211](https://github.com/Azure/azure-cli/pull/2211))
* 所有者から共同作成者へのロールの切り替え ([#2321](https://github.com/Azure/azure-cli/pull/2321))

### <a name="appservice"></a>AppService

* appservice: DNS A レコードで使用される外部 IP アドレスの取得をサポート ([#2627](https://github.com/Azure/azure-cli/pull/2627))
* appservice: ワイルドカード証明書のバインドをサポート ([#2625](https://github.com/Azure/azure-cli/pull/2625))
* appservice: 発行プロファイルの一覧表示をサポート ([#2504](https://github.com/Azure/azure-cli/pull/2504))
* AppService - 構成後にソース管理の同期をトリガー ([#2326](https://github.com/Azure/azure-cli/pull/2326))

### <a name="datalake"></a>DataLake

* Data Lake Analytics モジュールの最初のリリース
* Data Lake Store モジュールの最初のリリース

### <a name="docuemntdb"></a>DocuemntDB

* DocumentDB:接続文字列を一覧表示するためのサポートを追加 ([#2580](https://github.com/Azure/azure-cli/pull/2580))

### <a name="vm"></a>VM

* [コンピューティング] 仮想マシン スケール セットを作成するための AppGateway サポートを追加 ([#2570](https://github.com/Azure/azure-cli/pull/2570))
* [VM/VMSS] ディスク キャッシュのサポートを改善しました ([#2522](https://github.com/Azure/azure-cli/pull/2522))
* VM/VMSS: ポータルで使用される資格情報検証ロジックを組み込む ([#2537](https://github.com/Azure/azure-cli/pull/2537))
* wait コマンドと --no-wait のサポートを追加 ([#2524](https://github.com/Azure/azure-cli/pull/2524))
* 仮想マシン スケール セット: VM 間にわたるインスタンス ビューの一覧表示をサポート * ([#2467](https://github.com/Azure/azure-cli/pull/2467))
* VM と仮想マシン スケール セット用の --secrets を追加 ([#2212](<https://github.com/Azure/azure-cli/pull/2212>))
* 特殊化した VHD による VM 作成を許可 ([#2256](https://github.com/Azure/azure-cli/pull/2256))

## <a name="february-27-2017"></a>2017 年 2 月 27 日

バージョン 2.0.0

Azure CLI 2.0 のこのリリースは、最初の "一般公開" リリースです。一般公開に該当するのは、以下のコマンド モジュールです。
- コンテナー サービス (acs)
- コンピューティング (Resource Manager、VM、仮想マシン スケール セット、Managed Disks など)
- ネットワーク
- ストレージ

これらのコマンド モジュールは、運用環境で使用することができ、標準の Microsoft SLA でサポートされています。問題は、Microsoft サポートで直接開くことも、[GitHub の問題一覧](https://github.com/azure/azure-cli/issues/)で開くこともできます。[azure-cli タグを使用して StackOverflow](http://stackoverflow.com/questions/tagged/azure-cli) で質問したり、製品チーム ([azfeedback@microsoft.com](mailto:azfeedback@microsoft.com)) に問い合わせたりすることもできます。`az feedback` コマンドを使用すると、コマンド ラインからフィードバックを送ることができます。

これらのモジュールのコマンドは安定しており、Azure CLI のこのバージョンの今後のリリースで構文が変更されることは想定されていません

CLI のバージョンを確認するには、`az --version` を使用します。出力では、CLI 自体のバージョン (このリリースでは 2.0.0)、個々のコマンド モジュール、および使用している Python と GCC のバージョンが示されます。

```text
azure-cli (2.0.0)

acs (2.0.0)
appservice (0.1.1b5)
batch (0.1.1b4)
cloud (2.0.0)
component (2.0.0)
configure (2.0.0)
container (0.1.1b4)
core (2.0.0)
documentdb (0.1.1b2)
feedback (2.0.0)
iot (0.1.1b3)
keyvault (0.1.1b5)
network (2.0.0)
nspkg (2.0.0)
profile (2.0.0)
redis (0.1.1b3)
resource (2.0.0)
role (2.0.0)
sql (0.1.1b5)
storage (2.0.0)
vm (2.0.0)

Python (Darwin) 2.7.10 (default, Jul 30 2016, 19:40:32)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.34)]
```

> [!Note]
> コマンド モジュールには、"b*n*" または "rc*n*" という接尾辞が付いているものがあります。これらのコマンド モジュールはまだプレビュー段階であり、今後一般公開される予定です

CLI のナイトリー プレビュー ビルドもあります。詳細については、[ナイトリー ビルドの入手](https://github.com/Azure/azure-cli#nightly-builds)に関する手順と、[開発者向けセットアップおよび共同作成コード](https://github.com/Azure/azure-cli#developer-setup)に関する手順を参照してください

ナイトリー プレビュー ビルドの問題は、次の方法で報告することができます。
- [github の問題一覧](https://github.com/azure/azure-cli/issues/)で問題を報告する。
- 製品チーム ([azfeedback@microsoft.com](mailto:azfeedback@microsoft.com)) に問い合わせる
- `az feedback` コマンドを使用してコマンド ラインからフィードバックを送る
