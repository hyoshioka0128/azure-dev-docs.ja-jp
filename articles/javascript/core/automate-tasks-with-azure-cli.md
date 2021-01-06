---
title: Azure CLI を使用した Web アプリの自動化タスク
description: Azure タスクの自動化は、ホスト環境への継続的デプロイのための一般的な要件です。 タスクを管理し、任意の場所からデプロイするために、JavaScript 開発者には Azure CLI をお勧めします。
ms.topic: conceptual
ms.date: 12/16/2020
ms.custom: devx-track-js
ms.openlocfilehash: 7cfce90d8d0daf861dab9ba02e46ce489ae10742
ms.sourcegitcommit: 0d2ea78f18430c845a32e0d2311427ab81033465
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/23/2020
ms.locfileid: "97754058"
---
# <a name="automate-tasks-with-azure-cli"></a>Azure CLI を使用してタスクを自動化する

Azure タスクの自動化は、ホスト環境への継続的デプロイのための一般的な要件です。 タスクを管理し、任意の場所からデプロイするために、JavaScript 開発者には [Azure CLI](/cli/azure/) をお勧めします。

JavaScript 開発者のための一般的なタスク コマンドについて説明します。 

## <a name="automation-with-azure-cli"></a>Azure CLI を使用した自動化

Azure CLI を自動化するには、環境に CLI をインストールする必要があります。 一般的な方法を次に示します。 

* [Azure CLI をローカルにインストールする](/cli/azure/install-azure-cli)
* [Docker コンテナーからコマンドを実行する](/cli/azure/run-azure-cli-docker)

## <a name="using-the-example-commands"></a>サンプル コマンドの使用 

1. 山かっこ `<...>` 内の変数を実際の値に置き換えます。 
1. `<MY_GITHUB_DEFAULT_BRANCH_NAME>` に対するご自身の GitHub リポジトリの値は、使用するリポジトリに固有です。 現在、一般的な値は `main` または `default` です。 以前のリポジトリでは、`master` を使用することもあります。 

## <a name="log-in-for-automated-tasks-with-azure-cli"></a>Azure CLI による自動化されたタスクのためのログイン

Azure CLI がインストールされた後、続いて Azure CLI コマンドを実行するにはログインする必要があります。 自動化のために、Azure CLI に対して認証を行うことができます。

**リファレンス ドキュメント**: [az login](/cli/azure/reference-index?view=azure-cli-latest#az-login)

認証には、[マネージド ID](/cli/azure/authenticate-azure-cli#sign-in-with-a-managed-identity) をお勧めします。

```azurecli
az login --identity
```

[サービス プリンシパルが作成された](../core/node-sdk-azure-authenticate-principal.md#create-a-service-principal-using-the-azure-cli-20)後に、[ユーザーのサービス プリンシパルでログインします](/cli/azure/authenticate-azure-cli#sign-in-with-a-service-principal)。 

```dotnetcli
read -sp "Azure password: " AZ_PASS && echo && \ 
    az login --service-principal \
    -u <MY-SP-APP-URL> \
    -p $AZ_PASS \
    --tenant <MY-TENANT>
```


[ユーザーの資格情報を使用: ユーザー名とパスワード](/cli/azure/authenticate-azure-cli#sign-in-with-credentials-on-the-command-line)

```dotnetcli
az login -u <MY_AZURE_USERNAME> -p <MY_AZURE_PASSWORD>
```    

## <a name="create-resource-group-for-resources"></a>リソースのリソースグループを作成する

リソース グループは、Azure リソースの論理的なコレクションです。 論理グループは、プロジェクトの特定のリージョンで必要とされるサービスに基づいています。 [名前付け規則](/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming)について確認してください。

**リファレンス ドキュメント**: [az group create](/cli/azure/group?view=azure-cli-latest#az_group_create)

```azurecli
az group create \
    --name <MY-AZURE-RESOURCE_GROUP_NAME> \
    --location <AZURE_REGION_LOCATION>
```

## <a name="static-web-apps-with-azure-cli"></a>Azure CLI を使用した静的な Web アプリ

静的 Web アプリには、次のものに対するコードが含まれています。

* GitHub リポジトリに含まれるフロントエンド アプリケーション
* (オプション) `/API` ディレクトリ内の既存の Azure Functions API [詳細情報](/azure/static-web-apps/add-api#create-the-api)

アプリはサーバーレス API に Azure 関数を使用できますが、静的 Web アプリではそれは必須ではありません。 

**リファレンス ドキュメント**: [az staticwebapp](/cli/azure/staticwebapp?view=azure-cli-latest)

### <a name="create-azure-static-web-app"></a>Azure Static Web アプリ を作成する 

**リファレンス ドキュメント**: [az staticwebapp create](/cli/azure/staticwebapp?view=azure-cli-latest#az_staticwebapp_create)

```azurecli
az staticwebapp create \
    --name <MY_AZURE_WEB_APP_NAME> \
    --resource-group <MY-AZURE-RESOURCE_GROUP_NAME> \
    --source https://github.com/<MY_GITHUB_ACCOUNT_NAME>/<MY_AZURE_WEB_APP_NAME> \
    --location <AZURE_REGION_LOCATION> \
    --branch <MY_GITHUB_DEFAULT_BRANCH_NAME> \
    --app-artifact-location "<MY_WEB_APP_BUILD_DIRECTORY_NAME>" \
    --token <MY_GITHUB_PERSONAL_ACCESS_TOKEN>
```

### <a name="deploy-azure-static-web-app"></a>Azure 静的 Web アプリをデプロイする 

アプリをデプロイするには、Git を使用して、前のセットのリソース作成時にリモートとブランチのセットにプッシュします。 

```bash
git push <REMOTE_NAME> <MY_GITHUB_DEFAULT_BRANCH_NAME>
```

このコマンドの例は次のとおりです。

```bash
git push origin main
```

### <a name="delete-static-web-app"></a>静的 Web アプリを削除する 

**リファレンス ドキュメント**: [az staticwebapp delete](/cli/azure/staticwebapp?view=azure-cli-latest#az_staticwebapp_delete)

```azurecli
az staticwebapp delete && \
    --name <MY_AZURE_WEB_APP_NAME> && \
    --resource-group <MY-AZURE-RESOURCE_GROUP_NAME>
```

## <a name="next-steps"></a>次のステップ

* [チュートリアル: 静的 Webアプリを構築して Azure にデプロイする](../tutorial/static-web-app/introduction.md)
