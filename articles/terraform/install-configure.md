---
title: クイックスタート - Azure リソースをプロビジョニングするための Terraform をインストールして構成する
description: Azure リソースを作成するために Terraform をインストールして構成する方法について説明します。
keywords: Azure DevOps Terraform インストール構成
ms.topic: quickstart
ms.date: 04/26/2020
ms.openlocfilehash: e395227171417ccf73ef073a6067732fb11a418d
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82169738"
---
# <a name="quickstart-install-and-configure-terraform-to-provision-azure-resources"></a>クイック スタート:Azure リソースを作成するために Terraform をインストールして構成する
 
Terraform は、[シンプルなテンプレート言語](https://www.terraform.io/docs/configuration/syntax.html)を使ってクラウド インフラストラクチャを簡単に定義、プレビュー、およびデプロイできるツールです。 この記事では、Terraform を使用して Azure にリソースをプロビジョニングするために必要な手順について説明します。

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="install-terraform"></a>Terraform のインストール

既定では、Terraform の最新バージョンは、[Azure Cloud Shell](/azure/cloud-shell/overview) で使用するためにインストールされます。 Terraform をローカルにインストールする場合は、この手順を完了してください。それ以外の場合は、「[Azure への Terraform のアクセスを構成する](#configure-terraform-access-to-azure)」に進んでください。

1. オペレーティング システムに適したパッケージを指定して [Terraform をインストール](https://www.terraform.io/downloads.html)します。
1. ダウンロードには、1 つの実行可能ファイルが含まれています。 オペレーティング システムに基づいて、実行可能ファイルへのグローバル パスを定義します。
    - [Linux または MacOS](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)
    - [Windows](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) です。
1. `terraform` コマンドでグローバル パス構成を確認します。 Terraform が見つかり、実行されると、使用可能な Terraform オプションの一覧が表示されます。

    ```console
    azureuser@Azure:~$ terraform
    Usage: terraform [--version] [--help] <command> [args]
    ```
    
## <a name="configure-terraform-access-to-azure"></a>Azure への Terraform のアクセスを構成する

Terraform で Azure にリソースをプロビジョニングできるようにするには、[Azure AD サービス プリンシパル](/cli/azure/create-an-azure-service-principal-azure-cli)を作成します。 サービス プリンシパルは、Terraform スクリプトが Azure サブスクリプションにリソースをプロビジョニングすることを許可します。

複数の Azure サブスクリプションがある場合は、先に [az account list](/cli/azure/account#az-account-list) を使用してアカウントに対するクエリを実行して、サブスクリプション ID とテナント ID の値を取得します。

```azurecli-interactive
az account list --query "[].{name:name, subscriptionId:id, tenantId:tenantId}"
```

選択したサブスクリプションを使用するには、[az account set](/cli/azure/account#az-account-set). を使用して、このセッション用のサブスクリプションを設定します。 使用するサブスクリプションから返された `id` フィールドの値を `SUBSCRIPTION_ID` 環境変数に設定します。

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

これで、Terraform で使用するサービス プリンシパルを作成できます。 次のように、[az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) を使用して、サブスクリプションの*スコープ*を設定します。

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

ご自分の `appId`、`password`、`sp_name`、および `tenant` が返されます。 `appId` と `password` を書き留めておきます。

## <a name="configure-terraform-environment-variables"></a>Terraform 環境変数の構成

Azure AD サービス プリンシパルを使用するように Terraform を構成するには、次の環境変数を設定します。これは、後で [Azure Terraform モジュール](https://registry.terraform.io/modules/Azure)によって使用されます。 Azure パブリック以外の Azure クラウドで作業している場合は、環境を設定することもできます。

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

これらの変数は、次のサンプル シェル スクリプトを使用して設定できます。

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>サンプル スクリプトの実行

空のディレクトリに `test.tf` というファイルを作成し、次のスクリプトを貼り付けます。

```hcl
provider "azurerm" {
  # The "feature" block is required for AzureRM provider 2.x. 
  # If you are using version 1.x, the "features" block is not allowed.
  version = "~>2.0"
  features {}
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

ファイルを保存し、Terraform のデプロイを初期化します。 この手順によって、Azure リソース グループを作成するために必要な Azure モジュールがダウンロードされます。

```bash
terraform init
```

出力は次の例のようになります。

```console
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

`terraform plan` を使用して、Terraform スクリプトによって完了されるアクションをプレビューできます。 リソース グループを作成する準備が完了したら、次のように、Terraform プランを適用します。

```bash
terraform apply
```

出力は次の例のようになります。

```console
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>次のステップ

この記事では、Terraform をインストールするか、Cloud Shell を使用して Azure の資格情報を構成し、Azure サブスクリプションでのリソースの作成を開始しました。 Azure でより完全な Terraform デプロイを作成するには、次の記事を参照してください。

> [!div class="nextstepaction"]
> [Terraform がインストールされている Azure VM を作成する](create-linux-virtual-machine-with-infrastructure.md)