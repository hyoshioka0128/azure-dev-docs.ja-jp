---
title: Azure CLI を操作するためのサービス間リンク
description: 人気のあるチュートリアル、クイックスタート、サンプル、概念、ハウツー ガイド、Azure CLI、仮想マシン、Azure Kubernetes Service (AKS)、Batch、Azure CLI (コア)、Azure Resource Manager、Key Vault、Azure Stack Hub、Functions、Database、Event Hubs、App Configuration、アプリの構成、Germany、セキュリティ、ガバナンス、インサイト、IoT、モノのインターネット、DevOps、Virtual Network、Compute、ネットワーク、開発者ツール、データベース、Analytics、管理とガバナンス、ハイブリッド、ストレージ、セキュリティ、AI、AI + 機械学習、Linux、Windows、Ubuntu、オートメーション、アプリケーション、Web アプリ、スクリプトへのリンク
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 03/01/2020
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 3a0b2e315e0eaf6c352aa2737f33da043b5feb7d
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031254"
---
# <a name="popular-articles-using-the-azure-cli"></a>Azure CLI を使用した人気のある記事

Azure CLI は多くの Azure サービスで使用されるため、記事は複数のドキュメント リポジトリに散在しています。  このページでは、人気のある一部の記事へのリンクを示します。  

## <a name="compute"></a>Compute

| | | | |
|-|-|-|-|
|Virtual Machines | チュートリアル:Linux | [Azure CLI で Linux 仮想マシンを作成する](azure-cli-vm-tutorial.yml) | 仮想マシンを作成します。  出力クエリと環境変数の設定について説明します。
|Virtual Machines | クイック スタート:Linux | [Azure CLI を使用して Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-cli) | Linux 仮想マシンを作成してデプロイします。  Web トラフィック用のポートを開き、Web サーバーをインストールします。
|Virtual Machines | 攻略ガイド: Linux |[仮想マシンまたは VHD の Linus イメージを作成する](/azure/virtual-machines/linux/capture-image) | 既存の仮想マシンのプロビジョニングを解除し、イメージを作成して、キャプチャしたイメージから新しい仮想マシンを作成します。
|Virtual Machines | 攻略ガイド: Linux | [Azure CLI を使用して VHD を Azure にアップロードする](/azure/virtual-machines/linux/disks-upload-vhd-to-managed-disk-cli) | 空のマネージド ディスクを作成し、ローカル VHD ファイルをアップロードして、マネージド ディスクをコピーします。
|Virtual Machines | 攻略ガイド: Linux | [Azure CLI を使用して共有イメージ ギャラリーを作成する](/azure/virtual-machines/linux/shared-images) | 組織内、リージョン内またはリージョン間、または Azure Active Directory テナント内で、他のユーザーとの間でカスタム VM イメージの共有イメージ ギャラリーを作成します。
|Virtual Machines | 攻略ガイド: Linux | [Azure CLI を使用してスポット VM をデプロイする (プレビュー)](/azure/virtual-machines/linux/spot-cli) | 価格に基づいて削除されない Linux スポット仮想マシンをデプロイする方法を示しています。
|Virtual Machines | クイック スタート:Windows | [Azure CLI を使用して Windows 仮想マシンを作成する](/azure/virtual-machines/windows/quick-create-cli) | Windows Server 2016 を実行する仮想マシンを Azure にデプロイします。
|Virtual Machines | Learn モジュール | [Azure CLI を使用して仮想マシンを管理する](https://docs.microsoft.com/learn/modules/manage-virtual-machines-with-azure-cli/) | 仮想マシンに関連する追加の管理タスクを作成、開始、停止、実行します。
|Azure Kubernetes Service (AKS)| クイック スタート | [Azure CLI を使用して Azure Kubernetes Service (AKS) クラスターをデプロイする](/azure/aks/kubernetes-walkthrough) | AKS クラスターをデプロイおよび管理します。  アプリケーションを実行するクラスターとポッドの正常性を監視する方法を示します。
|Azure Batch|サンプル | [Azure CLI を使用した Azure Batch でのジョブとタスクの実行](/azure/batch/scripts/batch-cli-sample-run-job) | Batch のジョブを作成してそのジョブに一連のタスクを追加します。 ジョブとそのタスクを監視します。
|Azure Batch|サンプル | [Azure CLI を使用した Azure Batch での Windows プールの作成と管理](/azure/batch/scripts/batch-cli-sample-manage-windows-pool) | Cloud Services の構成で Windows 計算ノードのプールを作成および管理します。
|Azure Container Instances|クイック スタート | [Azure CLI を使用してコンテナー インスタンスをデプロイする](/azure/container-instances/container-instances-quickstart) | 分離された Docker コンテナーをデプロイし、そのアプリケーションを完全修飾ドメイン名 (FQDN) を介して使用できるようにします。 1 つのデプロイ コマンドを実行してから、コンテナーで実行中のアプリケーションを参照しま。
|Azure 関数|クイック スタート |  [Azure CLI を使用して HTTP 要求に応答する関数を Azure で作成する](/azure/azure-functions/functions-create-first-azure-function-azure-cli) | コマンドライン ツールを使用して、HTTP 要求に応答する関数を作成します。 コードをローカルでテストした後、Azure Functions のサーバーレス環境に関数をデプロイします。

## <a name="networking"></a>ネットワーク

| | | | |
|-|-|-|-|
|Virtual Network|クイック スタート | [Azure CLI を使用した仮想ネットワークの作成](/azure/virtual-network/quick-create-cli) | 仮想ネットワークを作成し、仮想ネットワークに 2 つの仮想マシンをデプロイして、インターネットから仮想マシンに接続します。
|Virtual Network|ハウツー ガイド | [Azure CLI を使用して Linux 仮想マシン上で高速ネットワークを有効にする](/azure/virtual-network/create-vm-accelerated-networking-cli) | Linux 仮想マシンを作成し、仮想関数の動的バインドと失効を処理し、高速ネットワークを有効にします。

## <a name="internet-of-things"></a>モノのインターネット (IoT)

| | | | |
|-|-|-|-|
|IoT Hub|チュートリアル | [Azure CLI を使用して IoT Hub のメッセージ ルーティングを構成する](/azure/iot-hub/tutorial-routing) | IoT Hub でカスタム ルーティング クエリを設定して使用します。

## <a name="developer-tools"></a>開発者ツール

| | | | |
|-|-|-|-|
|Azure App Configuration|サンプル |[Azure App Configuration 用の Azure CLI のサンプル](/azure/azure-app-configuration/cli-samples) | Azure App Configuration 用の Azure CLI を使用する bash スクリプトへのリンクを取得します。
|Azure DevOps| 概要:DevOps パイプライン |[Azure CLI を使用して初めての Azure パイプラインを作成する](/azure/devops/pipelines/create-first-pipeline-cli) | 複製された GitHub ディレクトリに新しいパイプラインを作成し、パイプラインを管理して実行します。
|Azure DevOps| 攻略ガイド: DevOps パイプライン |[Azure CLI を使用した Azure パイプライン デプロイ タスク](/azure/devops/pipelines/tasks/deploy/azure-cli?view=azure-devops) | ビルドまたはリリース パイプラインで、Azure CLI を含むシェルまたはバッチ スクリプトを実行します。  コマンドは、Linux、macOS、または Windows オペレーティング システムで実行されているクロスプラットフォーム エージェント上で実行されます。
|Azure DevOps| チュートリアル:Jenkins パイプライン |[Jenkins で Azure CLI を使用して Azure App Service にデプロイする](/azure/jenkins/execute-cli-jenkins-pipeline) | Jenkins 仮想マシンの作成と構成、Azure での Web アプリの作成、および GitHub リポジトリの準備を行います。  Jenkins パイプラインを作成して実行します。

## <a name="databases"></a>データベース

| | | | |
|-|-|-|-|
|SQL Database| サンプル |[Azure CLI を使用して Azure SQL Database を構成する](/azure/sql-database/sql-database-cli-samples?tabs=single-database) | Azure SQL Database 用の Azure CLI サンプル。
|MySQL|クイック スタート |[Azure CLI を使用した Azure Database for MySQL サーバーの作成](/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli) | Azure Database for MySQL サーバーを作成します。  ファイアウォール規則と SSL 設定を構成します。  接続情報を取得して使用します。
|Cosmos DB |ハウツー ガイド |[Azure CLI を使用した Azure Cosmos リソースの管理](/azure/cosmos-db/manage-with-cli) | 共通コマンドを使用して、Azure Cosmos DB のアカウント、データベース、コンテナーの管理を自動化します。
|Cosmos DB |サンプル |[Azure Cosmos DB SQL (Core) API 用の Azure CLI サンプル](/azure/cosmos-db/cli-samples) | Azure Cosmos DB SQL (Core) API 用の Azure CLI サンプル スクリプトへのリンクを取得します。

## <a name="analytics"></a>Analytics

| | | | |
|-|-|-|-|
Azure Event Hub |クイック スタート |[Azure CLI を使用したイベント ハブの作成](/azure/event-hubs/event-hubs-quickstart-cli) | Event Hubs 名前空間とイベント ハブを作成します。
HDInsight |ハウツー ガイド |[Azure CLI を使用した HDInsight クラスターを作成する](/azure/hdinsight/hdinsight-hadoop-create-linux-clusters-azure-cli) | HDInsight 3.6 クラスターを作成します。
HDInsight |チュートリアル |[Azure CLI を使用した Azure HDInsight クラスターの管理](/azure/hdinsight/hdinsight-administer-use-command-line) | HDInsight クラスターの一覧表示、表示、削除、およびスケーリングを行います。

## <a name="management-and-governance"></a>管理とガバナンス

| | | | |
|-|-|-|-|
リソース マネージャーのテンプレート |ハウツー ガイド |[Azure Resource Manager テンプレートと Azure CLI を使用してリソースをデプロイする](/azure/azure-resource-manager/templates/deploy-cli) | テンプレートを使用してリソースを Azure にデプロイします。
Resource Manager グループ |ハウツー ガイド |[Azure CLI を使用した Azure Resource Manager リソース グループの管理](/azure/azure-resource-manager/management/manage-resource-groups-cli) | Azure Resource Manager を使用して Azure リソース グループを管理します。
リソース グラフ |クイック スタート |[Azure CLI を使用して最初の Resource Graph クエリを実行する](/azure/governance/resource-graph/first-query-azurecli) | Azure CLI インストールに Azure Resource Graph を追加し、最初の Resource Graph クエリを実行します。
ポリシー割り当て |クイック スタート |[Azure CLI を使用して準拠していないリソースを識別するためのポリシー割り当てを作成する](/azure/governance/policy/assign-policy-azurecli) | マネージド ディスクを使用していない仮想マシンを識別するためのポリシー割り当てを作成します。

## <a name="hybrid"></a>ハイブリッド

| | | | |
|-|-|-|-|
Azure Stack Hub| クイック スタート:Linux VM |[Azure CLI を使用して Azure Stack Hub に Linux サーバー仮想マシンを作成する](/azure-stack/user/azure-stack-quick-create-vm-linux-cli) | Ubuntu Server 16.04 LTS 仮想マシンを作成し、リモート クライアントを使用して仮想マシンに接続し、NGINX Web サーバーをインストールします。
Azure Stack Hub| クイック スタート:Windows VM |[Azure CLI を使用して Azure Stack Hub に Windows Server 仮想マシンを作成する](/azure-stack/user/azure-stack-quick-create-vm-windows-cli) |Windows Server 2016 仮想マシンを作成し、リモート クライアントを使用して仮想マシンに接続し、IIS Web サーバーをインストールします。
Azure Stack Hub| 攻略ガイド: ASDK リソース |[Azure CLI を使用した Azure Stack Hub へのリソースの管理とデプロイ](/azure-stack/user/azure-stack-version-profiles-azurecli2) | Linux、Mac、Windows クライアントのプラットフォームから Azure Stack Development Kit (ASDK) のリソースを管理するように Azure CLI を設定します。

## <a name="storage"></a>ストレージ

| | | | |
|-|-|-|-|
BLOB ストレージ |クイック スタート |  [Azure CLI を使用して BLOB を作成、ダウンロード、および一覧表示する](/azure/storage/blobs/storage-quickstart-blobs-cli) | Azure Blob Storage との間でデータをアップロードおよびダウンロードします。
BLOB ストレージ |ハウツー ガイド |[Azure CLI を使用して BLOB またはキュー データへのアクセスを承認する](/azure/storage/common/authorize-data-operations-cli) | データ操作を承認する方法を指定し、パラメーターの環境変数を設定します。
BLOB ストレージ |ハウツー ガイド |[Azure CLI を使用して Azure Data Lake Storage Gen2 のディレクトリ、ファイル、ACL を管理する (プレビュー)](/azure/storage/blobs/data-lake-storage-directory-file-acl-cli) | 階層型名前空間を持つストレージ アカウントでディレクトリ、ファイル、アクセス許可を作成および管理します。
File Storage |クイック スタート |[Azure CLI を使用した Azure ファイル共有の作成および管理](/azure/storage/files/storage-how-to-use-files-cli) | Azure ファイル共有を作成および使用します。  共有スナップショットを作成および管理します。

## <a name="security"></a>Security

| | | | |
|-|-|-|-|
サービス プリンシパル |ハウツー ガイド |[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli) | Azure CLI を使用して、サービス プリンシパルの作成、情報の取得、およびリセットを行います。
RBAC |ハウツー ガイド |[Azure RBAC と Azure CLI を使用してロールの割り当てを追加または削除する](/azure/role-based-access-control/role-assignments-cli) | Azure ロールベースのアクセス制御にロールを割り当てます。
Key Vault |ハウツー ガイド |[Azure CLI を使用して Key Vault を管理する](/azure/key-vault/key-vault-manage-with-cli2) | Azure Key Vault を作成および管理します。  アプリケーションの登録と承認、高度なアクセス ポリシーの設定、クロスプラットフォームのコマンドライン インターフェイス コマンドの学習を行います。
Key Vault |チュートリアル |[Key Vault と Azure CLI を使用してストレージ アカウント キーを管理する](/azure/key-vault/key-vault-ovw-storage-keys) | ストレージ アカウント キーを管理し、Shared Access Signature トークンを生成します。

## <a name="ai--machine-learning"></a>AI + 機械学習

| | | | |
|-|-|-|-|
Machine Learning |リファレンス |[Azure Machine Learning 用の Azure CLI 拡張機能を使用する](/azure/machine-learning/reference-azure-machine-learning-cli) | 実験を実行して機械学習モデルを作成し、顧客が使用するための機械学習モデルを登録します。  機械学習モデルのライフサイクルをパッケージ化、デプロイ、追跡します。
Cognitive Services |ハウツー ガイド |[Azure CLI を使用して Cognitive Services リソースを作成する](/azure/cognitive-services/cognitive-services-apis-create-account-cli) | Azure Cognitive Services にサインアップし、単一サービスまたはマルチサービス サブスクリプションを持つアカウントを作成します。  自動的に生成されたキーとエンドポイントを使用して、自分のアプリケーションの認証を行います。
Azure Monitor |ハウツー ガイド |[Azure CLI を使用して Log Analytics ワークスペースを作成する](/azure/azure-monitor/learn/quick-create-workspace-cli) | Log Analytics ワークスペースを作成してデプロイします。

## <a name="geographies"></a>地域

| | | | |
|-|-|-|-|
Azure Germany |開始するには |[Azure CLI を使用して Azure Germany に接続する](/azure/germany/germany-get-started-connect-with-cli) | Azure Germany を使用して、スクリプトによる大規模なサブスクリプションを管理したり、現在グローバル Azure portal にはない機能を利用したりします。
Azure Government|開始するには |[Azure CLI を使用した Azure Government への接続](/azure/azure-government/documentation-government-get-started-connect-with-cli)|Azure Government のリソースにアクセスして管理を開始します。

## <a name="see-also"></a>関連項目

* [Azure CLI の概要](get-started-with-azure-cli.md)
* [Azure CLI の完全なコマンド リファレンス一覧](/cli/azure/reference-index)
* [Azure CLI で管理できるサービス](azure-services-the-azure-cli-can-manage.md)
