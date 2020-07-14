---
title: Azure で開発するためのローカル JavaScript 環境を構成する
description: エディター、Azure SDK ライブラリ、オプションのツール、ライブラリ認証に必要な資格情報など、Azure で作業するためのローカル JavaScript 開発環境を設定する方法。
ms.date: 07/01/2020
ms.topic: conceptual
ms.openlocfilehash: 2285e79ea62d2de961fd7d4dc7647fec2312b83c
ms.sourcegitcommit: 7be67fb768fb5e19f7de573068cc1376b3d90d1f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85911191"
---
# <a name="configure-your-local-javascript-dev-environment-for-azure"></a>Azure 用のローカル JavaScript 開発環境を構成する

クラウド アプリケーションを作成する場合、開発者は通常、Azure などのクラウド環境にコードをデプロイする前に、ローカル ワークステーションでそのコードをテストしたいと考えます。 ローカル開発には、使い慣れた開発環境と共に幅広いツールを利用できるという利点があります。

この記事では、Azure での JavaScript に適したローカル開発環境を作成および検証するためのセットアップ手順について説明します。

## <a name="one-time-subscription-creation"></a>1 回限りのサブスクリプションの作成

Azure リソースは、Azure を使用するための課金単位であるサブスクリプション内に作成されます。 無料のリソースを作成することはできますが (各サブスクリプションには、ほとんどのサービスに無料のリソースが用意されています)、リソースを運用環境にデプロイする予定の場合は、有料階層のリソースを作成する必要があります。

* 既にサブスクリプションをお持ちの場合は、新しいものを作成する必要はありません。 [Azure portal](https://portal.azure.com) を使用して、既存のサブスクリプションにアクセスしてください。
* [無料試用版サブスクリプションを開始する]()

## <a name="one-time-installation"></a>1 回限りのインストール

ローカル ワークステーションで JavaScript と共に Azure リソースを使用して開発するには、次のものを 1 回インストールする必要があります。

|名前またはインストーラー|説明|
|--|--|
|[Node.js]()|ローカル ワークステーション開発用の最新の長期サポート (LTS) ランタイム環境をインストールします。 |
| NPM (最新バージョンの Node.js と共にインストールされます) または [Yarn]()|Azure SDK ライブラリをインストールするためのパッケージ マネージャー。|
|[VSCode](https://aka.ms/vscode-deploy)| VSCode は、優れた JavaScript 統合とコーディングのエクスペリエンスを提供しますが、必須ではありません。 任意のコード エディターを使用することができます。 このドキュメントにおいては、別のエディターを使用している場合、Azure との統合を確認するか、Azure CLI を使用してください。|
|[Azure CLI]()|Azure CLI を使用すると、コマンド ライン、ターミナル、または bash シェルから Azure リソースを再作成したり管理したりすることができます。|

> [!CAUTION]
> Azure Web アプリや Azure Container Instance など、コードのランタイム環境として Azure リソースを使用する予定がある場合は、ローカルの Node.js 開発環境が、使用する予定の Azure リソース ランタイムと一致していることを確認する必要があります。

### <a name="optional-local-installations"></a>オプションのローカル インストール

次の一般的なローカル ワークステーションのインストールは、ローカルの開発タスクに役立つオプションです。

|名前またはインストーラー|説明|
|--|--|
| [git](https://git-scm.com/downloads) | ソース管理用のコマンドライン ツール。 必要に応じて、別のソース管理ツールを使用できます。 |

## <a name="one-time-configuration-of-service-principal"></a>サービス プリンシパルの 1 回限りの構成

各 Azure サービスには認証メカニズムがあります。 これには、キーとエンドポイント、接続文字列、またはその他のメカニズムが含まれる場合があります。 ベスト プラクティスに準拠するには、リソースを作成し、サービス プリンシパルを使用してリソースに対して認証を行います。 サービス プリンシパルを使用すると、当面の開発ニーズに対するアクセス スコープを具体的に定義できます。

概念的には、サービス プリンシパルを作成して使用する手順は次のとおりです。

* joe@microsoft.com など、個々のユーザー アカウントを使用して Azure にログインします。
* 特定のスコープを持つ名前付きサービス プリンシパルを作成します。 ほとんどのクイックスタートでは Azure リソースを作成するように求められるため、サービス プリンシパルにはリソースを作成する機能が必要です。
* 自分のユーザー アカウントで Azure からログオフします。
* サービス プリンシパルを使用して、プログラムで Azure に対する認証を行います。
* サービス プリンシパルは、Azure リソースを作成し、該当のサービスに関連付けられているサービスを使用します。

### <a name="create-service-principal"></a>サービス プリンシパルの作成

サービス プリンシパルの作成を簡単にするには、次の手順と提供されるスクリプトを使用して、Azure クイックスタートで使用するサービス プリンシパルを作成します。 次の手順では、ユーザー名の例として `JOE` という名前を使用します。 これを独自の名前または電子メール エイリアスに置き換えてください。

1. VSCode を開き、[Azure CLI ツール](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)拡張機能をインストールします。 この拡張機能を使用すると、スクリプト ファイルから Azure CLI コマンドを 1 行ずつ実行できます。 各コマンドを実行すると、隣接するドキュメントが VSCode で開き、結果が表示されます。

1. `create-service-principal.sh` という名前の新しいファイルを作成し、次の Azure コマンドをこのファイルにコピーします。

    ```azurecli
    # Replace ALL-CAPS variables with your own values

    ####################################
    # Login as you
    ####################################

    # Login - command opens browser, select your account to finish authentication, then close browser
    az login

    ####################################
    # Optional, set default subscription
    ####################################

    # If you have more than 1 subscription, use the `list` command to find the subscription, then use the `set` command to set the default by name or id
    az account list
    az account set --subscription MYCOMPANYSUBSCRIPTION

    ####################################
    # Create service principal
    ####################################

    # Create a service principal with a name that indicates its purpose and owner - the response includes the `appId` which is necessary in some of the remaining commands
    az ad sp create-for-rbac --name JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS --skip-assignment

    ####################################
    # Add role of contributor
    ####################################

    # Add contributor role to service principal so it can create Azure resources
    az role assignment create --assignee APP-ID --role CONTRIBUTOR

    ####################################
    # Optional, verify role assignment
    ####################################

    # Verify role assignment for service principal
    az role assignment list --assignee APP-ID

    ####################################
    # Logout
    ####################################

    # Logout off Azure CLI
    az logout
    ```

    この手順の残りのステップについては、そのファイル内の `#` で始まって**いない**各行について、その行に VSCode カーソルを置き、**右クリック**して **[Run Line in Editor]\(エディターで行を実行\)** を選択します。

    :::image type="content" source="media/development-setup/vscode-rightclick-run-line-in-editor.png" alt-text="この手順の残りのステップでは、そのファイル内の `#` で始まっていない各行について、その行に VSCode カーソルを置き、右クリックして [Run Line in Editor]\(エディターで行を実行\) を選択します。":::

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、Azure CLI を用いて、自分のユーザー アカウントで Azure に対する認証を行います。 このコマンドにより、インターネット ブラウザーが開きます。 Azure アカウントを選択します。 アカウントが認証されたら、そのブラウザー ウィンドウを閉じます。これは、残りのタスクでは必要ありません。

    ```azurecli
    az login
    ```

    応答には、アクセス権のあるすべてのサブスクリプションが含まれ、別の VSCode ドキュメント ウィンドウに JSON 配列として表示されます。 `name` または `id` プロパティを見つけます。 残りのコマンドで、これらの値のいずれかが必要です。

    ```json
    [  {
    "cloudName": "AzureCloud",
    "id": "320d9379-aaaa-bbbb-cccc-52f2b0fc40ac",
    "isDefault": false,
    "name": "contoso-development-team",
    "state": "Enabled",
    "tenantId": "72f988bf-aaaa-bbbb-cccc-2d7cd011db47",
    "user": {
      "name": "joe@contoso.com",
      "type": "user"
    }
    }]
    ```

    `isDefault: true` でマークされたサブスクリプションは、残りのコマンドを受け取るサブスクリプションです。 既定のサブスクリプションを変更する必要がある場合は、`az account set --subscription <name or id>` コマンドを使用します。


<a name='create-service-principal-command'></a>

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、自分のユーザー アカウントに関連付けられたサービス プリンシパルを作成します。 `--skip-assignment` パラメーターにより、このサービス プリンシパルには、スコープ付きアクセス許可がまだありません。


    ```azurecli
    az ad sp create-for-rbac --name JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS --skip-assignment
    ```

    サービス プリンシパル名は `JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS` です。 Azure portal の Active Directory サービスのアプリケーション一覧の下に、自分の Azure ユーザー アカウントに関連付けられたすべてのサービス プリンシパルの一覧が表示されます。

    この結果には、必要な情報である `appId` と `password` が含まれています。 このファイルを `create-service-principal.json` という名前で保存します

    ```json
    {
      "appId": "93453d56-aaaa-bbbb-cccc-db600ecc4f6a",
      "displayName": "JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS",
      "name": "http://JOE-SERVICEPRINCIPAL-DOCUMENT-QUICKSTARTS",
      "password": "d88b21e0-aaaa-bbbb-cccc-e1e9b06d50f6",
      "tenant": "72f988bf-aaaa-bbbb-cccc-2d7cd011db47"
    }
    ```

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、Azure リソースを作成するためのスコープ付きアクセス許可を割り当てます。 `CONTRIBUTOR` スコープにより、サービス プリンシパルは Azure リソースを作成できます。

    ```azurecli
    az role assignment create --assignee APP-ID --role CONTRIBUTOR
    ```

    結果は次のようになります。

    ```json
    {
      "canDelegate": null,
      "id": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0/providers/Microsoft.Authorization/roleAssignments/3a155db5-aaaa-bbbb-cccc-0cbfebf75464",
      "name": "3a155db5-aaaa-bbbb-cccc-0cbfebf75464",
      "principalId": "c05d56c9-aaaa-bbbb-cccc-0535d6167ed4",
      "principalType": "ServicePrincipal",
      "roleDefinitionId": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0/providers/Microsoft.Authorization/roleDefinitions/b24988ac-aaaa-bbbb-cccc-20f7382dd24c",
      "scope": "/subscriptions/a5b1ca8b-aaaa-bbbb-cccc-4cf7ec4791a0",
      "type": "Microsoft.Authorization/roleAssignments"
    }
    ```

    この時点で、サービス プリンシパルを使用する準備ができました。

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、次のコマンドで Azure CLI からログアウトします。

    ```azurecli
    az logout
    ```

## <a name="steps-for-each-new-development-project-setup"></a>新しい開発プロジェクトのセットアップごとの手順

Azure SDK ライブラリはサービスごとに個別に提供されるため、すべての Azure リソースにアクセスするための、単一のダウンロード可能なパッケージはありません。 各ライブラリは、使用する Azure サービスに基づいてインストールします。

Azure を使用する新しいプロジェクトでは、それぞれ次のことを行う必要があります。
- Azure リソースを作成するか、既存の Azure リソースの認証情報を検索する
- Azure SDK ライブラリを NPM または Yarn からインストールする。 [ライブラリのバージョン](#library-versions)について確認してください。
- プロジェクト内の認証情報を安全に管理する。 一般的な方法の 1 つとして、 **[Dotenv](https://www.npmjs.com/package/dotenv)** を使用して、`.env` ファイルから環境変数を読み取る方法があります。 `.env` ファイルがソース管理にチェックインされないように、必ず `.env` ファイルを `.gitignore` ファイルに追加してください。

### <a name="library-versions"></a>ライブラリのバージョン

すべての Azure ライブラリは `@azure` スコープに移行中です。

| ライブラリの種類 | 説明|
|--|--|
|モダン|`@azure` (たとえば、[@azure/storage-blob](https://www.npmjs.com/package/@azure/storage-blob) および [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos)) にスコープが設定され、TypeScript 型が含まれます。|
|古いパッケージ|通常は `azure-` で始まります。 多くのパッケージはこの名前で始まりますが、それらは Microsoft によって生成されたものではありません。 パッケージの所有者が Microsoft または Azure のいずれかであることを確認してください。|

### <a name="create-resource-using-service-principal"></a>サービス プリンシパルを使用してリソースを作成する

次のセクションでは、サービス プリンシパルを使用して Azure サービス リソースを作成する方法の例を示します。 サービス プリンシパルを使用してサインインするには、「[サービス プリンシパルの作成](#create-service-principal)」手順で `create-service-principal.json`に保存した `appId`、`tenant`、および `password` が必要です。

1. VSCode を開き、前にインストールした [Azure CLI ツール](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)拡張機能を使用します。 この拡張機能を使用すると、スクリプト ファイルから Azure CLI コマンドを 1 行ずつ実行できます。 各コマンドを実行すると、隣接するドキュメントが VSCode で開き、結果が表示されます。

1. `create-service-resource.sh` という名前の新しいファイルを作成し、次の Azure コマンドをこのファイルにコピーします。

    ```azurecli
    ####################################
    # Login as service principal
    ####################################
    # User name for command is the app id
    az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID

    ####################################
    # Create resource group
    ####################################

    # Create resource group in westus region - check your quickstart if it requires a specific region, then change this value to the appropriate region
    # Common naming convention for resource group is `USERNAME-REGION-PURPOSE`
    az group create --location WESTUS --name JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP

    ####################################
    # Create specific service resource
    ####################################

    # Create resource in westus
    # This is an example of creating a Cognitive Services LUIS resource
    # Review your quickstart to find the exact command
    az SERVICENAME account create --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP --kind LUIS --sku F0 --location WESTUS --yes

    ####################################
    # Get resource keys
    ####################################

    # Get resource keys
    az cognitiveservices account keys list --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、サービス プリンシパルでログインします。 すべて大文字の変数が、[前のサービス プリンシパル作成コマンド](#create-service-principal-command)からの応答で返されました。

    ```azurecli
    az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID
    ```

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、クイックスタート用に作成する必要のあるすべてのリソースのリソース グループを作成します。 リソース グループのリージョンには、そのリージョンのリソースのみを含めることができます。

    ```azurecli
    az group create --location WESTUS --name JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

    クイックスタート リソースの使用が終了したら、リソース グループを削除できます。これは、1 回のアクションでリソースに対して削除が行われます。

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、Cognitive Services LUIS リソースを作成します。 これは例です。独自のリソースには別のコマンドを使用することになります。

    ```azurecli
    az SERVICENAME account create --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP --kind LUIS --sku F0 --location WESTUS --yes
    ```

    LUIS リソースではキーとエンドポイントが使用されます。これらは、LUIS のクイックスタートを使用するために必要です。

1. 次の行で右クリック/[Run Line in Editor]\(エディターで行を実行\) を使用して、LUIS キーとエンドポイントを取得します。 LUIS サービスに対する認証では、キーとエンドポイントが使用されます。

    ```azurecli
    az cognitiveservices account keys list --name JOE-WESTUS-COGNITIVESERVICES-LUIS --resource-group JOE-WESTUS-QUICKSTARTS-RESOURCEGROUP
    ```

### <a name="create-environment-variables-for-the-azure-libraries"></a>Azure ライブラリの環境変数を作成する

Azure SDK ライブラリに必要な Azure 設定を使用して Azure クラウドにアクセスするには、最も一般的な値を環境変数に設定します。 次のコマンドは、環境変数をローカル ワークステーションに設定します。 もう 1 つの一般的なメカニズムは、`DOTENV` NPM パッケージを使用して、これらの設定用の `.env` ファイルを作成することです。 `.env` を使用する場合は、このファイルをソース管理にチェックインしないようにしてください。 これらの設定がソース管理にチェックインされるようにするための標準的な方法は、`.env` ファイルを git の `.ignore` ファイルに追加することです。

次の例では、クライアント ID は、サービス プリンシパル ID とサービス プリンシパル シークレットです。

# <a name="bash"></a>[bash](#tab/bash)

```bash
AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
AZURE_TENANT_ID="00112233-7777-8888-9999-aabbccddeeff"
AZURE_CLIENT_ID="12345678-1111-2222-3333-1234567890ab"
AZURE_CLIENT_SECRET="abcdef00-4444-5555-6666-1234567890ab"
```

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
set AZURE_SUBSCRIPTION_ID="aa11bb33-cc77-dd88-ee99-0918273645aa"
set AZURE_TENANT_ID=00112233-7777-8888-9999-aabbccddeeff
set AZURE_CLIENT_ID=12345678-1111-2222-3333-1234567890ab
set AZURE_CLIENT_SECRET=abcdef00-4444-5555-6666-1234567890ab
```

---

これらのコマンドに示されている値を、お客様固有のサービス プリンシパルの値で置き換えます。

### <a name="install-npm-packages"></a>NPM パッケージのインストール

すべてのプロジェクトについて、次の手順に従って、常に個別のフォルダーと独自の `package.json` ファイルを作成することをお勧めします。

1. ターミナル、コマンド プロンプト、または bash シェルを開き、プロジェクトに新しいフォルダーを作成します。 その後、その新しいフォルダーに移動します。

    ```console
    mkdir MY-NEW-PROJECT && cd MY-NEW-PROJECT
    ```

1. パッケージ ファイルを初期化します。

    ```console
    npm init -y
    ```

    このコマンドは、NPM コマンドを実行して、package.json ファイルを作成し、最小プロパティを初期化します。 NPM または Yarn を使用して Azure SDK ライブラリ パッケージをインストールすると、package.json ファイルにそのインストール情報が取り込まれます。

1. クイックスタートに必要な Azure SDK ライブラリをインストールします。 次のコマンドは例です。

    ```console
    npm install @azure/cognitiveservices-luis-runtime
    ```

## <a name="use-source-control"></a>ソース管理を使用する

プロジェクトを開始するときは常にソース管理リポジトリを作成するという習慣を身に付けることをお勧めします。 Git がインストールされている場合は、次のコマンドを実行します。

```bash
git init
```

そこから、`git add` や `git commit` などのコマンドを実行して変更をコミットできます。 変更を定期的にコミットすることにより、コミット履歴が作成されるので、以前の状態に戻すことができるようになります。

プロジェクトのオンライン バックアップを作成するには、リポジトリを [GitHub](https://github.com) または [Azure DevOps](/azure/devops/user-guide/code-with-git?view=azure-devops) にアップロードすることをお勧めします。 最初にローカル リポジトリを初期化した場合は、`git remote add` を使用して、ローカル リポジトリを GitHub または Azure DevOps にアタッチします。

Git のドキュメントは、[git-scm.com/docs](https://git-scm.com/docs) と、インターネット上のさまざまな場所で参照できます。

Visual Studio Code には、数多くの組み込み Git 機能が含まれています。 詳細については、「[VS Code でのバージョン管理の使用](https://code.visualstudio.com/docs/editor/versioncontrol)」を参照してください。

他の任意のソース管理ツールを使用することもできます。Git は最も広く使用され、サポートされているソース管理ツールの 1 つにすぎません。

## <a name="next-steps"></a>次のステップ

* [Visual Studio Code から静的 Web サイトを Azure にデプロイする](tutorial-vscode-static-website-node-01.md)