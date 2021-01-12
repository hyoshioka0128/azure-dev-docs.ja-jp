---
title: JavaScript 開発者向けの Azure の主要タスク
description: 現在のタスクの例を参照してください。
ms.topic: reference
ms.date: 01/05/2021
ms.custom: devx-track-js
ms.openlocfilehash: 2f19a660a60e91601dc31b829bb8fbc82f04b37d
ms.sourcegitcommit: 075f39972e390e79ed09a3fcfdbfc776727e08fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97952484"
---
# <a name="common-top-tasks-for-javascript-developers"></a>JavaScript 開発者向けの一般的な主要タスク

現在のタスクの例を参照してください。 タスクが見つからない場合は、タスクを要求するフィードバックを残しておきます。 

## <a name="app-registration"></a>アプリの登録

|タスク|using|
|--|--|
|アプリの登録を作成する|[ポータル](../tutorial/single-page-application-azure-login-button-sdk-msal.md#3-create-app-registration-for-authentication)|

## <a name="application-settings"></a>アプリケーションの設定

|タスク|using|
|--|--|
|ローカル環境変数を設定する|[Bash](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#add-environment-variables-to-your-local-environment)|
|Storage コンテナーの Shared Access Signature (SAS) トークンを作成する|[ポータル](../tutorial/browser-file-upload-azure-storage-blob.md#5-generate-your-shared-access-signature-sas-token)|
|コードに SAS トークンを設定する|[TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#set-sas-token-in-code-file)|
|Storage 用に CORS を構成する|[ポータル](../tutorial/browser-file-upload-azure-storage-blob.md#6-configure-cors-for-azure-storage-resource)|

## <a name="authentication"></a>認証

|タスク|using|
|--|--|
|`@azure/msal-browser` を使用した Microsoft ログイン/ログオフ ボタン|[React/TypeScript](../tutorial/single-page-application-azure-login-button-sdk-msal.md#5-add-login-and-logoff-buttons)|
|AAD のアクセス許可を取り消す|[https://myapplications.microsoft.com/](https://myapplications.microsoft.com/)|
|コンシューマーのアクセス許可を取り消す|[https://account.live.com/consent/manage](https://account.live.com/consent/manage)|

## <a name="azure-login"></a>Azure ログイン

|タスク|using|
|--|--|
|ログイン|[Azure CLI](../tutorial/deploy-deno-app-azure-app-service-azure-cli.md#2-sign-in-to-azure-cli)<br>[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md#sign-in-to-azure)|


## <a name="azure-resource-groups"></a>Azure リソース グループ

|タスク|using|
|--|--|
|リソース グループの作成|[Azure CLI](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|リソース グループの削除|[Azure CLI](../tutorial/static-web-app/clean-up-resources.md#remove-all-the-resources-by-removing-resource-group)|

## <a name="apps"></a>アプリ

### <a name="static-web-apps"></a>静的 Web アプリ

|タスク|using|
|--|--|
|Angular アプリの作成|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=angular)|
|Deno アプリの作成|[Bash](../tutorial/deploy-deno-app-azure-app-service-azure-cli.md#3-create-local-deno-api-app)|
|JavaScript 言語を対象とする React アプリの作成|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=react)|
|TypeScript 言語を対象とする React アプリの作成|[Bash](../tutorial/single-page-application-azure-login-button-sdk-msal.md#4-create-react-single-page-application-for-typescript)|
|Vue アプリの作成|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=vue)|
|Svelte アプリの作成|[Bash](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-02.md?tabs=svelte)|
|静的 Web アプリを作成する|[Visual Studio Code 拡張機能](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|ストレージでホストされる静的アプリの作成|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-03.md)|
|ストレージでホストされる静的アプリのデプロイ|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-04.md)|
|サイトの参照|[Visual Studio Code 拡張機能](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-azure-static-web-site-in-browser)|

### <a name="function-serverless-apps"></a>関数 (サーバーレス) アプリ

|タスク|using|
|--|--|
|Functions アプリをローカルで作成する|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-serverless-node-create-local.md)|
|HTTP トリガー コード|[JavaScript](../tutorial/tutorial-vscode-serverless-node-create-local.md#http-function-javascript-template-code)|
|関数をローカルでデバッグ/テストする|[Visual Studio Code](../tutorial/tutorial-vscode-serverless-node-test-local.md)|
|関数を Azure クラウドにデプロイする|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-serverless-node-deploy-hosting.md)|
|関数がパブリック URL で使用可能であることを確認する|[Visual Studio Code 拡張機能/ブラウザー](../tutorial/tutorial-vscode-serverless-node-deploy-hosting.md#verify-functions-app-is-publicly-available-with-browser)|
|関数アプリ リソースを削除する|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-serverless-node-remove-resource.md)|

### <a name="app-service---full-stack-server-only-or-client-only-apps"></a>アプリ サービス - フル スタック、サーバーのみ、またはクライアントのみのアプリ

|タスク|using|
|--|--|
|ローカル環境で Express.js アプリを作成する|[Bash](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#3-create-a-local-expressjs-app)|
|アプリ リソースの作成 - Express.js アプリのデプロイ、ログのストリーミングを含む|[Visual Studio Code 拡張機能](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#create-web-app-resource-and-deploy-expressjs-app)|
|アプリ リソースの作成 - Express.js アプリのデプロイ、アプリ設定の構成、npm インストールの実行、デプロイされた Web サイトの参照を含む|[Visual Studio Code 拡張機能](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#6-create-app-service-resource-in-visual-studio-code)|
|アプリ リソース計画を作成する|[Azure CLI](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md#create-app-service-plan)|
|アプリ リソースを作成する|[Azure CLI](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md)|
|デプロイの構成|[Azure CLI](../tutorial/deploy-deno-app-azure-app-service-azure-cli.md#5-configure-the-azure-app-service-webapp)
|アプリをデプロイする|[Azure CLI](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-04.md)|
|ブラウザーでアプリを表示する|[Azure CLI](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-03.md#browse-web-app)|
|アプリ リソースを削除する|[Visual Studio Code 拡張機能](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#clean-up-resources)<br>[Azure CLI](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-07.md)|
|リモート ログをストリーミングする|[Visual Studio Code 拡張機能](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#7-stream-remote-service-logs-in-visual-studio-code)<br>[Azure CLI](../tutorial/tutorial-vscode-azure-cli-node/tutorial-vscode-azure-cli-node-05.md)|

## <a name="cognitive-services"></a>Cognitive Services

|タスク|using|
|--|--|
|Cognitive Services _ComputerVision_ リソースを作成する|[Azure CLI](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Cognitive Services _ComputerVision_ リソースを取得する|[Azure CLI](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Azure SDK をインストールする|[Bash](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-to-local-react-app)|
|[`@azure/cognitiveservices-computervision`](https://www.npmjs.com/package/@azure/cognitiveservices-computervision) を使用して画像を分析する|[Visual Studio Code](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-code-as-separate-module)|

## <a name="containers"></a>Containers

探しているものが見つからない場合は、[Docker タスク](#docker)を確認してください。 

|タスク|using|
|--|--|
|コンテナー レジストリ リソースを作成する|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-02.md#create-an-azure-container-registry)|
|レジストリ リソースにイメージをプッシュする|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#push-the-image-to-a-registry)|
|レジストリへの管理者アクセスを有効にする|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-05.md#enable-admin-access-on-the-registry)|
|イメージをアプリ サービスにデプロイする|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-05.md?branch=main#deploy-image)|


## <a name="databases"></a>データベース

|タスク|using|
|--|--|
|CosmosDB の作成 - MongoDB リソース|[Visual Studio Code 拡張機能](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md)|
|CosmosDB 接続文字列を取得する|[Visual Studio Code 拡張機能](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#get-cosmosdb-connection-string)|

## <a name="docker"></a>Docker

探しているものが見つからない場合は、[コンテナー タスク](#containers)を確認してください。 

|タスク|using|
|--|--|
|Docker のバージョンを確認する|[Bash](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md#verify-docker-install)|
|Docker ファイルをローカル プロジェクトに追加する|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#add-docker-files)|
|ローカル プロジェクトから Docker イメージを構築する|[Bash](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#build-a-docker-image)|

## <a name="git"></a>Git

|タスク|using|
|--|--|
|ローカル Git リポジトリを初期化する|[Visual Studio Code 拡張機能](../tutorial/deploy-nodejs-azure-app-service-with-visual-studio-code.md?tabs=bash#5-initialize-git-in-visual-studio-code-for-current-app)|

## <a name="github"></a>GitHub 

### <a name="actions"></a>アクション 

|タスク|using|
|--|--|
|シークレットを追加する|[Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|ビルド プロセスを表示する|[GitHub Web サイト](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-the-github-action-build-process)|


## <a name="monitoring"></a>監視

|タスク|using|
|--|--|
|リソースを作成する|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/create-azure-monitoring-application-insights-web-resource.md#create-azure-monitor-resource-with-azure-cli)|



## <a name="storage"></a>ストレージ

|タスク|using|
|--|--|
|リソースを作成する|[Visual Studio Code 拡張機能](../tutorial/browser-file-upload-azure-storage-blob.md#3-create-storage-resource-with-visual-studio-extension)|
|Delete resource|[Visual Studio Code 拡張機能](../tutorial/browser-file-upload-azure-storage-blob.md#clean-up-resources)|
|ストレージでホストされる静的アプリの作成|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-03.md)|
|ストレージでホストされる静的アプリのデプロイ|[Visual Studio Code 拡張機能](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-04.md)|

### <a name="blobs"></a>BLOB

|タスク|using|
|--|--|
|[`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob) を使用してストレージにコンテナーを作成する|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#create-storage-client-and-manage-steps)|
|[`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob) を使用してファイルをストレージにアップロードする|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#upload-button-functionality)|
|[`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob) を使用して Storage コンテナー内のファイルを一覧表示する|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#get-list-of-blobs)|

## <a name="virtual-machines"></a>仮想マシン

|タスク|using|
|--|--|
|SSH を使用して VM に接続する|[Bash](../tutorial/nodejs-virtual-machine-vm/connect-linux-virtual-machine-ssh.md#connect-with-ssh-and-change-web-app)|
|Monitoring SDK のインストール|[Bash](../tutorial/nodejs-virtual-machine-vm/connect-linux-virtual-machine-ssh.md#install-monitoring-sdk)|
|Express.js アプリに監視コードを追加する|[JavaScript](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-nodejs-expressjs-code.md#edit-indexjs-for-logging-with-azure-monitor-application-insights)|
|cloud-init ファイルを作成する|[YAML](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#create-a-cloud-init-file-to-expedite-linux-virtual-machine-creation)|
|Linux VM リソースを作成する|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#create-a-virtual-machine-resource)|
|Linux VM のポートを開く|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#open-port-for-virtual-machine)|
|ログを表示する|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-nodejs-expressjs-code.md#viewing-the-vm-logs-for-nginx-and-pm2)<br>[ポータル](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-logs.md#view-application-traces-in-azure-portal)|


## <a name="visual-studio-code"></a>Visual Studio Code

|タスク|using|
|--|--|
|GitHub リポジトリを複製する|[Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#2-clone-and-run-the-initial-react-app)|

## <a name="samples-supporting-these-tasks"></a>これらのタスクをサポートするサンプル

|Name | 説明|
|--|--|
|静的 Webアプリを構築して Azure にデプロイする|GitHub アクションを使用して、ローカル環境で React/TypeScript クライアント アプリケーションを構築し、Azure Static Web アプリにデプロイします。<br>[チュートリアル](../tutorial/static-web-app/introduction.md) - [サンプル コード](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)|
|ファイルを Azure Storage BLOB にアップロードする|このサンプル プロジェクトは、Azure Storage BLOB にアップロードするファイルを選択するための HTML フォームを使用した、TypeScript React (create-react-app) フレームワーク クライアント アプリです。<br>[チュートリアル](../tutorial/browser-file-upload-azure-storage-blob.md) - [サンプル コード](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)|
|クライアント アプリケーションにログイン ボタンを追加する|このチュートリアルで構築する SPA は、次のタスクを行う React アプリ (create-react-app) です。<br>* Office 365 や Outlook.com など、Microsoft がサポートするログインを使用してログインする<br>* アプリケーションからログオフする<br>[チュートリアル](../tutorial/single-page-application-azure-login-button-sdk-msal.md) - [サンプル コード](https://github.com/Azure-Samples/js-e2e-client-azure-login-button)|
|App Service にデプロイされた CosmosDB (MongoDB)/Express.js アプリ|このチュートリアルでは、拡張機能を使用して VSCode でプロジェクトをローカルに読み込んで実行する方法と、App Service でコードをリモートで実行する方法について説明します。 このチュートリアルでは、Mongo API 用に CosmosDB リソースを作成し、接続情報を取得して、クラウド データベースに接続するように App Service 構成で設定します。<br>[チュートリアル](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md#get-cosmosdb-connection-string) - [サンプル コード](https://github.com/Azure-Samples/js-e2e-express-mongo)|

## <a name="next-steps"></a>次のステップ

* [Web アプリをデプロイする](deploy-web-app.md)