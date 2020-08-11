---
title: クイック スタート - Azure Cloud Shell を使用して Terraform の使用を開始する
description: このクイックスタートでは、Azure リソースを作成するために Terraform をインストールして構成する方法について説明します。
keywords: Azure DevOps Terraform インストール 構成 Cloud Shell 初期化 プラン 適用 実行 portal ログイン RBAC サービス プリンシパル 自動スクリプト
ms.topic: quickstart
ms.date: 07/26/2020
ms.openlocfilehash: dbe290fbb7909d116d2ff0cec8e01a3b145ded30
ms.sourcegitcommit: e451e4360d9c5956cc6a50880b3a7a55aa4efd2f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2020
ms.locfileid: "87478592"
---
# <a name="quickstart-get-started-with-terraform-using-azure-cloud-shell"></a>クイック スタート:Azure Cloud Shell を使用して Terraform の使用を開始する
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

この記事では、[Azure で Terraform](https://www.terraform.io/docs/providers/azurerm/index.html) の使用を開始する方法について説明します。

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>環境を構成する

1. [Azure ポータル](https://portal.azure.com)にアクセスします。

1. まだログインしていない場合は、利用可能な Microsoft アカウントの一覧が Azure portal に表示されます。 1 つ以上のアクティブな Azure サブスクリプションに関連付けられている Microsoft アカウントを選択し、資格情報を入力して続行します。

1. Cloud Shell を開きます。

    ![Cloud Shell へのアクセス](media/install-configure/portal-cloud-shell.png)

1. Cloud Shell を以前に使用したことがない場合は、環境とストレージの設定を構成します。 この記事では、Bash 環境を使用します。

**注**:
- Cloud Shell には、Terraform の最新バージョンが自動的にインストールされます。 また、Terraform は、現在の Azure サブスクリプションの情報を自動的に使用します。 そのため、インストールや構成は必要ありません。

## <a name="authenticate-to-azure"></a>Azure に対して認証します

Cloud Shell は、Azure portal へのログインに使用した Microsoft アカウントで自動的に認証されます。 お使いのアカウントに複数の Azure サブスクリプションがある場合は、[他のいずれかのサブスクリプションに切り替える](#set-the-current-azure-subscription)ことができます。

Terraform では、Azure に対して認証を行うためのオプションがいくつかサポートされています。 この記事では、次の手法について説明します。

- Terraform を対話的に使用する場合は、[Microsoft アカウントを使用した認証](#authenticate-via-microsoft-account)をお勧めします。
- Terraform をコードから使用する場合は、[Azure サービス プリンシパルを使用した認証](#authenticate-via-azure-service-principal)が推奨される方法の 1 つです。

### <a name="authenticate-via-microsoft-account"></a>Microsoft アカウントを使用した認証

パラメーターを指定せずに `az login` を呼び出すと、URL とコードが表示されます。 URL を参照してコードを入力し、指示に従って、ご自身の Microsoft アカウントを使用して Azure にログインします。 ログインしたら、ポータルに戻ります。

```azurecli
az login
```

**注**:

- ログインが成功すると、`az login` により、ログインした Microsoft アカウントに関連付けられている Azure サブスクリプションの一覧が表示されます。
- 使用可能な Azure サブスクリプションごとにプロパティの一覧が表示されます。 `isDefault` プロパティは、使用している Azure サブスクリプションを識別します。 別の Azure サブスクリプションに切り替える方法については、「[現在の Azure サブスクリプションを設定する](#set-the-current-azure-subscription)」セクションを参照してください。

### <a name="authenticate-via-azure-service-principal"></a>Azure サービス プリンシパルを使用した認証

**Azure サービス プリンシパルを作成する**:サービス プリンシパルを使用して Azure サブスクリプションにログインするには、まずサービス プリンシパルへのアクセスが必要です。 サービス プリンシパルを既にお持ちの場合は、セクションのこの部分を省略できます。

Azure サービスをデプロイまたは使用する自動化ツール (Terraform など) のアクセス許可は、常に制限されている必要があります。 完全な特権を持つユーザーとしてアプリケーションをログインさせる代わりに、Azure にはサービス プリンシパルが用意されています。 しかし、ログインに使用するサービス プリンシパルがない場合はどうすればよいでしょうか。 そのようなシナリオの場合、ユーザー資格情報を使用してログインしてから、サービス プリンシパルを作成できます。 サービス プリンシパルが作成されると、その情報を今後のログイン試行に使用できるようになります。

[Azure CLI でサービス プリンシパルを作成](/cli/azure/create-an-azure-service-principal-azure-cli?)するときは、多くのオプションがあります。 この記事では、[az ad sp create-for-rbac](/cli/azure/ad/sp?#az-ad-sp-create-for-rbac) を作成して、**共同作成者**ロールを持つサービス プリンシパルを作成します。 **共同作成者**ロール (既定) には、Azure アカウントに対して読み取りと書き込みを行うための完全なアクセス許可があります。 ロールベースのアクセス制御 (RBAC) とロールの詳細については、[RBAC の組み込みのロール](/azure/active-directory/role-based-access-built-in-roles)に関するページをご覧ください。

次のコマンドを入力します。`<subscription_id>` を、使用するサブスクリプションアカウントの ID に置き換えます。

```azurecli
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_id>"
```

**注**:

- 正常に完了すると、`az ad sp create-for-rbac` によっていくつかの値が表示されます。 `name`、`password`、および `tenant` の値は、次のステップで使用します。
- このパスワードは、紛失した場合、取得できません。 したがって、パスワードは安全な場所に保管してください。 パスワードを忘れた場合は、[サービス プリンシパルの資格情報をリセット](/cli/azure/create-an-azure-service-principal-azure-cli#reset-credentials)する必要があります。

**Azure サービス プリンシパルを使用してログインする**: 次の `az login` への呼び出しで、プレースホルダーをご自身のサービス プリンシパルの情報に置き換えます。

```azurecli
az login --service-principal -u <service_principal_name> -p "<service_principal_password>" --tenant "<service_principal_tenant>"
```

## <a name="set-the-current-azure-subscription"></a>現在の Azure サブスクリプションを設定する

Microsoft アカウントは、複数の Azure サブスクリプションに関連付けることができます。 次の手順では、サブスクリプションを切り替える方法の概要を説明します。

1. 現在の Azure サブスクリプションを表示するには、[az account show](/cli/azure/account#az-account-show) を使用します。

    ```azurecli
    az account show
    ```

1. 複数の利用可能な Azure サブスクリプションにアクセスできる場合は、[az account list](/cli/azure/account#az-account-list) を使用して、サブスクリプション名 ID の値の一覧を表示します。

    ```azurecli
    az account list --query "[].{name:name, subscriptionId:id}"
    ```

1. 現在の Cloud Shell セッションで特定の Azure サブスクリプションを使用するには、[az account set](/cli/azure/account#az-account-set) を使用します。 `<subscription_id>` プレースホルダーを、使用するサブスクリプションの ID (または名前) に置き換えます。

    ```azurecli
    az account set --subscription="<subscription_id>"
    ```

    **注**:

    - `az account set` を呼び出すと、指定した Azure サブスクリプションに切り替えた結果が表示されません。 ただし、`az account show` を使用して、現在の Azure サブスクリプションが変更されたことを確認できます。

## <a name="create-a-terraform-configuration-file"></a>Terraform 構成ファイルを作成する

このセクションでは、Azure リソース グループを作成する Terraform 構成ファイルを作成する方法について説明します。

1. Cloud Shell での作業内容が保存されている、マウントされたファイル共有にディレクトリを変更します。 Cloud Shell でのファイルの保存方法の詳細については、「[Microsoft Azure Files ストレージの接続](/azure/cloud-shell/overview#connect-your-microsoft-azure-files-storage)」を参照してください。

    ```bash
    cd clouddrive
    ```

1. このデモ用の Terraform ファイルを格納するディレクトリを作成します。

    ```bash
    mkdir QuickstartTerraformTest
    ```

1. ディレクトリを demo ディレクトリに変更します。

    ```bash
    cd QuickstartTerraformTest
    ```

1. 任意のエディターを使用して、Terraform 構成ファイルを作成します。 この記事では、組み込みの [Cloud Shell エディター](/azure/cloud-shell/using-cloud-shell-editor)を使用します。

    ```bash
    code QuickstartTerraformTest.tf
    ```
 
1. 次の HCL を新しいファイルに貼り付けます。

    ```hcl
    provider "azurerm" {
      # The "feature" block is required for AzureRM provider 2.x.
      # If you are using version 1.x, the "features" block is not allowed.
      version = "~>2.0"
      features {}
    }
    resource "azurerm_resource_group" "rg" {
            name = "QuickstartTerraformTest-rg"
            location = "eastus"
    }
    ```

    **注**:

    - `provider` ブロックは、[Azure プロバイダー (`azurerm`)](https://www.terraform.io/docs/providers/azurerm/index.html) が使用されることを指定しています。
    - `azurerm` プロバイダー ブロック内には、`version` と `features` 属性が設定されています。 コメントに記載されているように、その使用方法はバージョン固有です。 ご使用環境に合わせてこれらの属性を設定する方法の詳細については、[AzureRM プロバイダーの v2.0](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html) に関するページを参照してください。
    - 唯一の[リソース宣言](https://www.terraform.io/docs/configuration/resources.html)は、[azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html) のリソースの種類に対するものです。 `azure_resource_group` に必要な 2 つの引数は `name` と `location` です。

1. ファイルを保存します ( **&lt;Ctrl>S**)。

1. エディターを終了します ( **&lt;Ctrl>Q**)。

## <a name="create-and-apply-a-terraform-execution-plan"></a>Terraform 実行プランを作成して適用する

構成ファイルを作成したら、このセクションでは、"*実行プラン*" を作成し、それをクラウド インフラストラクチャに適用する方法について説明します。

1. [terraform init](https://www.terraform.io/docs/commands/init.html) を使用して Terraform のデプロイを初期化します。 この手順によって、Azure リソース グループを作成するために必要な Azure モジュールがダウンロードされます。

    ```bash
    terraform init
    ```

1. [terraform plan](https://www.terraform.io/docs/commands/plan.html) を実行して、実行プランを作成し、その結果をプレビューします。

    ```bash
    terraform plan
    ```

    **注**:

    - `terraform plan` コマンドは、実行プランを作成しますが、実行はしません。 代わりに、構成ファイルに指定された構成を作成するために必要なアクションを決定します。
    - `terraform plan` コマンドを使用すると、実際のリソースに変更を加える前に、実行プランが自分が期待しているものと一致しているかどうかを確認できます。
    - 省略可能な `-out` パラメーターを使用すると、プランの出力ファイルを指定できます。 `-out` パラメーターの使用方法の詳細については、「[後のデプロイのために実行プランを保存する](#persist-an-execution-plan-for-later-deployment)」セクションを参照してください。

1. [terraform apply](https://www.terraform.io/docs/commands/apply.html) を使用して、実行プランを適用します。

    ```bash
    terraform apply
    ```

1. Terraform は、実行プランを適用した場合に何が起こるかを示し、その実行を確認するよう求めます。 `yes` を入力し、**Enter** キーを押して、コマンドを確認します。

1. プランの実行を確認したら、[az group show](/cli/azure/group?#az-group-show) を使用して、リソース グループが正常に作成されたことをテストします。

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    **注**:

    - 成功すると、`az group show` により、新しく作成されたリソース グループのさまざまなプロパティが表示されます。

## <a name="persist-an-execution-plan-for-later-deployment"></a>後のデプロイのために実行プランを保存する

前のセクションでは、[terraform plan](https://www.terraform.io/docs/commands/plan.html) を実行して実行プランを作成する方法を確認しました。 次に、[terraform apply](https://www.terraform.io/docs/commands/apply.html) を使用してそのプランを適用することを確認しました。 このパターンは、手順が対話型で逐次的な場合に適しています。

より複雑なシナリオについては、実行プランをファイルに保存できます。 後で、または別のマシンからでも、その実行プランを適用することができます。

この機能を使用する場合は、「[自動での Terraform の実行](https://learn.hashicorp.com/terraform/development/running-terraform-in-automation)」の記事を読むことをお勧めします。

次の手順は、この機能を使用するための基本的なパターンを示しています。

1. [terraform init](https://www.terraform.io/docs/commands/init.html) を実行します。

    ```bash
    terraform init
    ```

1. `-out` パラメーターを指定して `terraform plan` を実行します。

    ```bash
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

1. 前の手順のファイルの名前を指定して `terraform apply` を実行します。

    ```bash
    terraform apply QuickstartTerraformTest.tfplan
    ```

    **注**:
    
    - 自動化での使用を有効にするために、`terraform apply <filename>` の実行で確認が求められることはありません。
    - この機能を使用する場合は、[セキュリティ警告セクション](https://www.terraform.io/docs/commands/plan.html#security-warning)を参照してください。
    
## <a name="clean-up-resources"></a>リソースをクリーンアップする

この記事で作成したリソースが不要になったら、削除してください。

1. [terraform plan](https://www.terraform.io/docs/commands/plan.html) を実行して、Terraform 構成ファイルに示されているリソースを破棄する実行プランを作成します。

    ```bash
    terraform plan -destroy -out QuickstartTerraformTest.destroy.tfplan
    ```

    **注**:
    - `terraform plan` コマンドは、実行プランを作成しますが、実行はしません。 代わりに、構成ファイルに指定された構成を作成するために必要なアクションを決定します。 これにより、実際のリソースに変更を加える前に、実行プランが自分の想定と一致しているかどうかを確認できます。
    - `-destroy` パラメーターを指定すると、リソースを破棄するプランが生成されます。
    - 省略可能な `-out` パラメーターを使用すると、プランの出力ファイルを指定できます。 `-out` パラメーターは必ず使用してください。これにより、レビューしたプランが適用内容とまったく同じであることが確実になるためです。
    - 実行プランの永続化とセキュリティの詳細については、[「セキュリティの警告」セクション](https://www.terraform.io/docs/commands/plan.html#security-warning)を参照してください。

1. [terraform apply](https://www.terraform.io/docs/commands/apply.html) を実行して、実行プランを適用します。

    ```bash
    terraform apply QuickstartTerraformTest.destroy.tfplan
    ```

1. [az group show](/cli/azure/group?#az-group-show) を使用して、リソース グループが削除されたことを確認します。

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    **注**:
    - 成功すると、`az group show` により、リソース グループが存在しないという事実が表示されます。

1. ディレクトリを親ディレクトリに変更し、demo ディレクトリを削除します。 `-r` パラメーターを指定すると、ディレクトリが削除される前にディレクトリの内容が削除されます。 ディレクトリの内容には、前に作成した構成ファイルと Terraform の状態ファイルが含まれています。

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Terraform がインストールされている Azure VM を作成する](create-linux-virtual-machine-with-infrastructure.md)