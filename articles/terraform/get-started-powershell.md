---
title: クイック スタート - Azure PowerShell を使用して Terraform を構成する
description: このクイックスタートでは、Azure リソースを作成するために Terraform をインストールして構成する方法について説明します。
keywords: Azure DevOps Terraform インストール 構成 Windows 初期化 プラン 適用 実行 ログイン RBAC サービス プリンシパル 自動スクリプト PowerShell
ms.topic: quickstart
ms.date: 08/18/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 401a6c4cc8827e48858a936a10c9c7f62af15aab
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90830058"
---
# <a name="quickstart-configure-terraform-using-azure-powershell"></a>クイック スタート:Azure PowerShell を使用して Terraform を構成する
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

この記事では、PowerShell を使用して、[Azure で Terraform](https://www.terraform.io/docs/providers/azurerm/index.html) の使用を開始する方法について説明します。

この記事では、次のことについて説明します。
> [!div class="checklist"]
> * 最新バージョンの PowerShell をインストールする
> * 新しい PowerShell Az モジュールをインストールする
> * Azure CLI のインストール
> * Terraform のインストール
> * 認証の目的で Azure サービス プリンシパルを作成する
> * サービス プリンシパルを使用して Azure にログインする 
> * Terraform が Azure サブスクリプションに対して正しく認証を行うように環境変数を設定する
> * Azure リソース グループを作成するための Terraform スクリプトを記述する
> * Terraform 実行プランを作成して適用する
> * `terraform plan -destroy` フラグを使用して実行プランを破棄する

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>環境を構成する

1. Azure リソースの操作を可能にする最新の PowerShell モジュールは、[Azure PowerShell Az モジュール](/powershell/azure/new-azureps-module-az)と呼ばれています。 Azure PowerShell Az モジュールを使用する場合、すべてのプラットフォームで推奨されるバージョンは PowerShell 7 (またはそれ以降) です。 PowerShell がインストールされている場合は、PowerShell プロンプトで次のコマンドを入力して、バージョンを確認できます。

    ```powershell
    $PSVersionTable.PSVersion
    ```

1. [PowerShell をインストールします](/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7)。 このデモは、Windows 10 で PowerShell v7.0.2 を使用してテストされました。

1. [Terraform で Azure に対して認証を行う](https://www.terraform.io/docs/providers/azurerm/guides/azure_cli.html)には、[Azure CLI をインストールする](/cli/azure/install-azure-cli-windows?view=azure-cli-latest)必要があります。 このデモは、Azure CLI バージョン 2.9.1 を使用してテストされました。

1. [Terraform をダウンロードします](https://www.terraform.io/downloads.html)。

1. ダウンロードから、任意のディレクトリに実行可能ファイルを抽出します。

1. [システムのグローバル パスを実行可能ファイルに更新](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)します。

1. `terraform` コマンドでグローバル パス構成を確認します。

    ```powershell
    terraform
    ```

    **注**:
    - Terraform 実行可能ファイルが見つかった場合は、構文と使用可能なコマンドが一覧表示されます。

## <a name="create-an-azure-service-principal"></a>Azure サービス プリンシパルを作成する

PowerShell と Terraform を使用する場合は、サービス プリンシパルを使用してログインする必要があります。

サービス プリンシパルを使用して Azure サブスクリプションにログインするには、まずサービス プリンシパルへのアクセスが必要です。 サービス プリンシパルが既にある場合は、このセクションを省略できます。

[PowerShell でサービス プリンシパルを作成](/powershell/azure/create-azure-service-principal-azureps)する場合、数多くのオプションがあります。 この記事では、**共同作成者**ロールを使用してサービス プリンシパルを作成します。 **共同作成者**ロール (既定のロール) には、Azure アカウントに対して読み取りと書き込みを行うための完全なアクセス許可があります。 ロールベースのアクセス制御 (RBAC) とロールの詳細については、[RBAC の組み込みのロール](/azure/active-directory/role-based-access-built-in-roles)に関するページをご覧ください。

[New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal) を呼び出すと、指定されたサブスクリプションのサービス プリンシパルが作成されます。 正常に完了すると、サービス プリンシパルの情報 (サービス プリンシパル名、表示名など) が表示されます。 認証資格情報を指定せずに `New-AzADServicePrincipal` を呼び出すと、パスワードが自動的に生成されます。 ただし、このパスワードは `SecureString` 型で返されるため、表示されません。 そのため、結果が変数に入るようにして `New-AzADServicePrincipal` を呼び出す必要があります。 その後、変数をプレーンテキストに変換して表示できます。

1. 使用する Azure サブスクリプションのサブスクリプション ID を取得します。 サブスクリプション ID がわからない場合は、[Azure portal](https://portal.azure.com/) から値を取得できます。

    1. [Azure Portal](https://portal.azure.com/) にログインします。
    1. **[Azure サービス]** で、 **[サブスクリプション]** を選択します。
    1. サブスクリプションの一覧表には、各サブスクリプションの ID を含む列があります。

1. PowerShell を開始します。

1. [New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal) を使用して新しいサービス プリンシパルを作成します。 `<azure_subscription_id>` は、使用する Azure サブスクリプションの ID に置き換えてください。

    ```powershell
    $sp = New-AzADServicePrincipal -Scope /subscriptions/<azure_subscription_id>
    ```

1. サービス プリンシパルの名前を表示します。

    ```powershell
    $sp.ServicePrincipalNames
    ```

1. 自動生成されたパスワードをテキストとして表示します ([ConvertFrom-SecureString](/powershell/module/microsoft.powershell.security/convertfrom-securestring))。

    ```powershell
    $UnsecureSecret = ConvertFrom-SecureString -SecureString $sp.Secret -AsPlainText
    ```

**注**:

- サービス プリンシパル名とパスワードの値は、サービス プリンシパルを使用してサブスクリプションにログインするために必要です。
- このパスワードは、紛失した場合、取得できません。 したがって、パスワードは安全な場所に保管してください。 パスワードを忘れた場合は、[サービス プリンシパルの資格情報をリセット](/powershell/azure/create-azure-service-principal-azureps#reset-credentials)する必要があります。

## <a name="log-in-to-azure-using-a-service-principal"></a>サービス プリンシパルを使用して Azure にログインする

サービス プリンシパルを使用して Azure サブスクリプションにログインするには、[PsCredential](/dotnet/api/system.management.automation.pscredential) 型のオブジェクトを指定して [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) を呼び出します。

1. PowerShell を開始します。

1. 次のいずれかの方法を使用して、[PsCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトを取得します。

    1. [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential) を呼び出し、要求されたら、サービス プリンシパル名とパスワードを入力します。

        ```powershell
        $spCredential = Get-Credential
        ```

    1. メモリ内に `PsCredential` オブジェクトを作成します。 プレースホルダーは、ご使用のサービス プリンシパルの適切な値に置き換えてください。 このパターンは、スクリプトからログインする方法です。

        ```powershell
        $spName = "<service_principal_name>"
        $spPassword = ConvertTo-SecureString "<service_principal_password>" -AsPlainText -Force
        $spCredential = New-Object System.Management.Automation.PSCredential($spName , $spPassword)
        ```

1. `Connect-AzAccount` を呼び出し、`PsCredential` オブジェクトを渡します。 `<azure_subscription_tenant_id>` プレースホルダーは、Azure サブスクリプションのテナント ID に置き換えてください。

    ```powershell
    Connect-AzAccount -Credential $spCredential -Tenant "<azure_subscription_tenant_id>" -ServicePrincipal
    ```

## <a name="set-environment-variables"></a>環境変数の設定

Terraform で目的の Azure サブスクリプションを使用するために、環境変数を設定します。 環境変数は、Windows システム レベルまたは特定の PowerShell セッション内で設定できます。 特定のセッションに環境変数を設定する場合は、次のコードを使用します。 プレースホルダーは、ご使用の環境の適切な値に置き換えてください。

```powershell
$env:ARM_CLIENT_ID="<service_principal_app_id>"
$env:ARM_SUBSCRIPTION_ID="<azure_subscription_id>"
$env:ARM_TENANT_ID="<azure_subscription_tenant_id>"
```

## <a name="create-a-terraform-configuration-file"></a>Terraform 構成ファイルを作成する

このセクションでは、Azure リソース グループを作成する Terraform 構成ファイルをコーディングします。

1. このデモ用の Terraform ファイルを格納するディレクトリを作成します。

    ```powershell
    mkdir QuickstartTerraformTest
    ```

1. ディレクトリを demo ディレクトリに変更します。

    ```powershell
    cd QuickstartTerraformTest
    ```

1. 任意のエディターを使用して、Terraform 構成ファイルを作成します。 この記事では、[Visual Studio Code](https://code.visualstudio.com/Download) を使用します。

    ```powershell
    code QuickstartTerraformTest.tf
    ```

1. 次の HCL コードを新しいファイルに貼り付けます。 詳細については、コード リストの後の注を参照してください。

    ```hcl
    provider "azurerm" {
      # The "feature" block is required for AzureRM provider 2.x.
      # If you're using version 1.x, the "features" block isn't allowed.
      version = "~>2.0"
      features {}
    }

    resource "azurerm_resource_group" "rg" {
      name     = "QuickstartTerraformTest-rg"
      location = "eastus"
    }
    ```

    **注**:
    - プロバイダー ブロックは、[Azure プロバイダー (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html) が使用されることを指定しています。
    - `azurerm` プロバイダー ブロック内には、`version` と `features` 属性が設定されています。 コメントに記載されているように、その使用方法はバージョン固有です。 これらの属性の設定に関する詳細については、「[AzureRM プロバイダーの v2.0](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html)」を参照してください。
    - 唯一の[リソース宣言](https://www.terraform.io/docs/configuration/resources.html)は、[azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html) のリソースの種類に対するものです。 azure_resource_group の 2 つの必須引数は、name と location です。

## <a name="create-and-apply-a-terraform-execution-plan"></a>Terraform 実行プランを作成して適用する

このセクションでは、"*実行プラン*" を作成し、クラウド インフラストラクチャに適用します。

1. [terraform init](https://www.terraform.io/docs/commands/init.html) を使用して Terraform のデプロイを初期化します。 この手順によって、Azure リソース グループを作成するために必要な Azure モジュールがダウンロードされます。

    ```powershell
    terraform init
    ```

1. [terraform plan](https://www.terraform.io/docs/commands/plan.html) を 実行して、Terraform 構成ファイルから実行プランを作成します。

    ```powershell
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

    **注:**
    - `terraform plan` コマンドは、実行プランを作成しますが、実行はしません。 代わりに、構成ファイルに指定された構成を作成するために必要なアクションを決定します。 このパターンを使用すると、実際のリソースに変更を加える前に、実行プランが自分の想定と一致しているかどうかを確認できます。
    - 省略可能な `-out` パラメーターを使用すると、プランの出力ファイルを指定できます。 `-out` パラメーターを使用すると、レビューしたプランが適用内容とまったく同じであることが確実になります。
    - 実行プランの永続化とセキュリティの詳細については、[「セキュリティの警告」セクション](https://www.terraform.io/docs/commands/plan.html#security-warning)を参照してください。

1. [terraform apply](https://www.terraform.io/docs/commands/apply.html) を実行して、実行プランを適用します。

    ```powershell
    terraform apply QuickstartTerraformTest.tfplan
    ```

1. 実行プランが適用されたら、[Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup) を使用して、リソース グループが正常に作成されたことをテストできます。

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    **注**:

    - 成功すると、コマンドにより、新しく作成されたリソース グループのさまざまなプロパティが表示されます。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

この記事で作成したリソースが不要になったら、削除してください。

1. [terraform plan](https://www.terraform.io/docs/commands/plan.html) を実行して、Terraform 構成ファイルに示されているリソースを破棄する実行プランを作成します。

    ```powershell
    terraform plan -destroy -out QuickstartTerraformTest.destroy.tfplan
    ```

    **注:**
    - `terraform plan` コマンドは、実行プランを作成しますが、実行はしません。 代わりに、構成ファイルに指定された構成を作成するために必要なアクションを決定します。 このパターンを使用すると、実際のリソースに変更を加える前に、実行プランが自分の想定と一致しているかどうかを確認できます。
    - `-destroy` パラメーターを指定すると、リソースを破棄するプランが生成されます。
    - 省略可能な `-out` パラメーターを使用すると、プランの出力ファイルを指定できます。 `-out` パラメーターを使用すると、レビューしたプランが適用内容とまったく同じであることが確実になります。
    - 実行プランの永続化とセキュリティの詳細については、[「セキュリティの警告」セクション](https://www.terraform.io/docs/commands/plan.html#security-warning)を参照してください。

1. [terraform apply](https://www.terraform.io/docs/commands/apply.html) を実行して、実行プランを適用します。

    ```powershell
    terraform apply QuickstartTerraformTest.destroy.tfplan
    ```

1. [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup) を使用して、リソース グループが削除されたことを確認します。

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    **注**:
    - 成功すると、`Get-AzResourceGroup` により、リソース グループが存在しないという事実が表示されます。

1. ディレクトリを親ディレクトリに変更し、demo ディレクトリを削除します。 `-r` パラメーターを指定すると、ディレクトリが削除される前にディレクトリの内容が削除されます。 ディレクトリの内容には、前に作成した構成ファイルと Terraform の状態ファイルが含まれています。

    ```powershell
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Terraform がインストールされている Azure VM を作成する](create-linux-virtual-machine-with-infrastructure.md)