---
title: JavaScript 開発者向けの Azure の主要タスク
description: 現在のタスクの例を参照してください。
ms.topic: reference
ms.date: 12/08/2020
ms.custom: devx-track-js
ms.openlocfilehash: 396544121a964cf7fd8cbde01012e7466a76eb37
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97690797"
---
# <a name="common-top-tasks-for-javascript-developers"></a>JavaScript 開発者向けの一般的な主要タスク

現在のタスクの例を参照してください。

## <a name="application-settings"></a>アプリケーションの設定

|タスク|using|
|--|--|
|ローカル環境変数を設定する|[Bash](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#add-environment-variables-to-your-local-environment)|
|Storage コンテナーの Shared Access Signature (SAS) トークンを作成する|[ポータル](../tutorial/browser-file-upload-azure-storage-blob.md#5-generate-your-shared-access-signature-sas-token)|
|コードに SAS トークンを設定する|[TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#set-sas-token-in-code-file)|
|Storage 用に CORS を構成する|[ポータル](../tutorial/browser-file-upload-azure-storage-blob.md#6-configure-cors-for-azure-storage-resource)|

## <a name="azure-resource-groups"></a>Azure リソース グループ

|タスク|using|
|--|--|
|リソース グループの作成|[Azure CLI](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|リソース グループの削除|[Azure CLI](../tutorial/static-web-app/clean-up-resources.md#remove-all-the-resources-by-removing-resource-group)|

## <a name="static-web-apps"></a>静的 Web アプリ

|タスク|using|
|--|--|
|静的 Web アプリを作成する|[Visual Studio Code 拡張機能](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|サイトの参照|[Visual Studio Code 拡張機能](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-azure-static-web-site-in-browser)|

## <a name="storage"></a>ストレージ

|タスク|using|
|--|--|
|リソースを作成する|[Visual Studio Code 拡張機能](../tutorial/browser-file-upload-azure-storage-blob.md#3-create-storage-resource-with-visual-studio-extension)|
|Delete resource|[Visual Studio Code 拡張機能](../tutorial/browser-file-upload-azure-storage-blob.md#clean-up-resources)|

### <a name="blobs"></a>BLOB

|タスク|using|
|--|--|
|[`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob) を使用してストレージにコンテナーを作成する|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#create-storage-client-and-manage-steps)|
|[`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob) を使用してファイルをストレージにアップロードする|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#upload-button-functionality)|
|[`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob) を使用して Storage コンテナー内のファイルを一覧表示する|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#get-list-of-blobs)|

## <a name="cognitive-services"></a>Cognitive Services

|タスク|using|
|--|--|
|Cognitive Services _ComputerVision_ リソースを作成する|[Azure CLI](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Cognitive Services _ComputerVision_ リソースを取得する|[Azure CLI](../tutorial/static-web-app/create-computer-vision-resource-use-in-code.md#create-azure-resources)|
|Azure SDK をインストールする|[Bash](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-to-local-react-app)|
|[`@azure/cognitiveservices-computervision`](https://www.npmjs.com/package/@azure/cognitiveservices-computervision) を使用して画像を分析する|[Visual Studio Code](../tutorial/static-web-app/add-computer-vision-react-app.md#add-computer-vision-code-as-separate-module)|

## <a name="github-actions"></a>GitHub のアクション 

|タスク|using|
|--|--|
|シークレットを追加する|[Visual Studio Code](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#create-a-static-web-app-resource)|
|ビルド プロセスを表示する|[GitHub Web サイト](../tutorial/static-web-app/create-static-web-app-visual-studio-code-extension.md#view-the-github-action-build-process)|

## <a name="visual-studio-code"></a>Visual Studio Code

|タスク|using|
|--|--|
|GitHub リポジトリを複製する|[Visual Studio Code](../tutorial/browser-file-upload-azure-storage-blob.md#2-clone-and-run-the-initial-react-app)|

## <a name="next-steps"></a>次のステップ

* [Web アプリをデプロイする](deploy-web-app.md)