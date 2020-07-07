---
title: クイックスタート - Windows を使用して Terraform の使用を開始する
description: このクイックスタートでは、Azure リソースを作成するために Terraform をインストールして構成する方法について説明します。
keywords: Azure DevOps Terraform インストール 構成 Windows 初期化 プラン 適用 実行 ログイン RBAC サービス プリンシパル 自動スクリプト CLI PowerShell
ms.topic: quickstart
ms.date: 06/12/2020
ms.openlocfilehash: d28a79af9a59551153f297cebfb0c51ed2e72838
ms.sourcegitcommit: 2d6c9687b39e33a6b5e980d9a375c9f8f1f2cab7
ms.contentlocale: ja-JP
ms.lasthandoff: 06/15/2020
ms.locfileid: "84783804"
---
# <a name="quickstart-getting-started-with-terraform-using-windows"></a>クイック スタート:Windows を使用して Terraform の使用を開始する
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>環境を構成する

1. PowerShell 7 (以降) が、すべてのプラットフォーム (Windows を含む) で Azure PowerShell に使用できる PowerShell の推奨バージョンです。 PowerShell がインストールされている場合は、PowerShell プロンプトで次のように入力することで、バージョンを確認できます。

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
1. 「[Windows への PowerShell のインストール](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7)」の記事の手順に従って、PowerShell の最新バージョンをインストールします。

1. この記事では、[Azure PowerShell Az モジュール](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)を使用します。 「[Windows での Azure CLI のインストール](/cli/azure/install-azure-cli-windows?view=azure-cli-latest)」の記事の手順に従います。

1. インストールした PowerShell のバージョンのインスタンスを開きます。

## <a name="log-into-azure"></a>Azure にログインする

Azure サブスクリプションにログインするためのオプションがいくつかあります。

- [Microsoft アカウントにログインする](#log-into-your-microsoft-account)
- [Azure サービス プリンシパルを使用してログインする](#log-into-azure-using-an-azure-service-principal)

### <a name="log-into-your-microsoft-account"></a>Microsoft アカウントにログインする

パラメーターを指定せずに [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount) を呼び出すと、URL とコードが表示されます。 URL を参照してコードを入力し、指示に従って、ご自身の Microsoft アカウントを使用して Azure にログインします。 ログインしたら、ポータルに戻ります。

```powershell
Connect-AzAccount
```

注:
- ログインが成功すると、`Connect-AzAccount` により、ログインした Microsoft アカウントに関連付けられている既定の Azure サブスクリプションが表示されます。 別の Azure サブスクリプションに切り替える方法については、「[現在の Azure サブスクリプションを指定する](#specify-the-current-azure-subscription)」セクションを参照してください。

### <a name="log-into-azure-using-an-azure-service-principal"></a>Azure サービス プリンシパルを使用して Azure にログインする

**Azure サービス プリンシパルを作成する**:サービス プリンシパルを使用して Azure サブスクリプションにログインするには、まずサービス プリンシパルへのアクセスが必要です。 サービス プリンシパルを既にお持ちの場合は、セクションのこの部分を省略できます。

Azure サービスをデプロイまたは使用する自動化ツール (Terraform など) のアクセス許可は、常に制限されている必要があります。 完全な特権を持つユーザーとしてアプリケーションをログインさせる代わりに、Azure にはサービス プリンシパルが用意されています。 しかし、ログインに使用するサービス プリンシパルがない場合はどうすればよいでしょうか。 そのようなシナリオの場合、ユーザー資格情報を使用してログインしてから、サービス プリンシパルを作成できます。 サービス プリンシパルが作成されると、その情報を今後のログイン試行に使用できるようになります。

[PowerShell でサービス プリンシパルを作成](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps)する場合、数多くのオプションがあります。 この記事では、**共同作成者**ロールを使用してサービス プリンシパルを作成します。 **共同作成者**ロール (既定のロール) には、Azure アカウントに対して読み取りと書き込みを行うための完全なアクセス許可があります。 ロールベースのアクセス制御 (RBAC) とロールの詳細については、[RBAC の組み込みのロール](/azure/active-directory/role-based-access-built-in-roles)に関するページをご覧ください。

[New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADServicePrincipal) を呼び出すと、指定されたサブスクリプションのサービス プリンシパルが作成されます。 正常に完了すると、サービス プリンシパルの情報 (サービス プリンシパル名、表示名など) が表示されます。 認証資格情報を指定せずに `New-AzADServicePrincipal` を呼び出すと、パスワードが自動的に生成されます。 ただし、このパスワードは `SecureString` 型で返されるため、表示されません。 そのため、結果を変数に渡して `New-AzADServicePrincipal` を呼び出す必要があります。 その後、パスワードの変数を照会できます。

1. 次のコマンドを入力します。`<subscription_id>` は、使用するサブスクリプション アカウントの ID に置き換えます。

    ```powershell
    $sp = New-AzADServicePrincipal -Scope /subscriptions/<subscription_id>
    ```

1. 次を入力して、サービス プリンシパルの名前を表示します。

    ```powershell
    $sp.ServicePrincipalNames
    ```

1. `ConvertFrom-SecureString` を呼び出して、パスワードをテキストとして表示します。

    ```powershell
    $UnsecureSecret = ConvertFrom-SecureString -SecureString $sp.Secret -AsPlainText
    ```

注:
- この時点で、サービス プリンシパル名とパスワードがわかっています。 これらの値は、サービス プリンシパルを使用してサブスクリプションにログインするために必要です。
- このパスワードは、紛失した場合、取得できません。 したがって、パスワードは安全な場所に保管してください。 パスワードを忘れた場合は、[サービス プリンシパルの資格情報をリセット](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps#reset-credentials)する必要があります。

**Azure サービス プリンシパルを使用してログインする**:サービス プリンシパルを使用して Azure サブスクリプションにログインするには、`Connect-AzAccount` を呼び出し、[PsCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential) 型のオブジェクトを渡します。 インタラクティブとスクリプトの 2 つのオプションがあります。

- **インタラクティブ パターン**:[Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential) を呼び出し、要求されたら資格情報を入力します。 `Get-Credential` を呼び出すと、`Connect-AzAccount` に渡す `PsCredential` オブジェクトが返されます。

    1. `Get-Credential` を呼び出し、サービス プリンシパル名とパスワードを手動で入力します。

        ```powershell
        $Credential = Get-Credential
        ```

    2. `Connect-AzAccount` を呼び出し、`PsCredential` オブジェクトを渡します。 (`<azureSubscriptionTenantId>` プレースホルダーは、Azure サブスクリプションのテナント ID に置き換えてください)。

        ```powershell
        Connect-AzAccount -Credential $Credential -Tenant <azureSubscriptionTenantId> -ServicePrincipal
        ```

- **スクリプト パターン**:`PsCredential` オブジェクトを構築し、それを `Connect-AzConnect` に渡します。

    1. `Get-Credential` を構築します。 (プレースホルダーは、ご自分の Azure サブスクリプションとサービス プリンシパルの適切な値に置き換えてください)。

        ```powershell
        $spName = "<servicePrincipalName>"
        $spPassword = ConvertTo-SecureString "<servicePrincipalPassword>" -AsPlainText -Force
        $psCredential = New-Object System.Management.Automation.PSCredential($spName , $spPassword)
        ```

    1. `Connect-AzAccount` を呼び出し、構築された `PsCredential` オブジェクトを渡します。

        ```powershell
        Connect-AzAccount -Credential $psCredential -TenantId "<azureSubscriptionTenantId>"  -ServicePrincipal
        ```

## <a name="specify-the-current-azure-subscription"></a>現在の Azure サブスクリプションを指定する

前のセクションで説明したように、Azure にログインする 2 つの方法としては、次のシナリオがあります。

- **Microsoft アカウントを使用してログインする**: Microsoft アカウントは複数の Azure サブスクリプションに関連付けることができます。そのうちの 1 つは、既定のサブスクリプションです。 既定のサブスクリプションとは、ログインし、別のサブスクリプションに切り替えない場合に使用するものです。
- **Azure サービス プリンシパルを使用してログインする**: サービス プリンシパルは、Azure サブスクリプションに固有です。 ログインするとサブスクリプション情報が表示されることに留意してください。

以下の手順は最初のシナリオに対応しています。ここでは、次のタスクを実行します。

- 現在の Azure サブスクリプションを表示する
- 現在の Microsoft アカウントで使用可能なすべての Azure サブスクリプションを一覧表示する
- 別の Azure サブスクリプションに切り替える

1. 現在の Azure サブスクリプションを表示するには、[Get-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext) を使用します。

    ```powershell
    Get-AzContext
    ```

1. 複数の利用可能な Azure サブスクリプションにアクセスできる場合は、`Get-AzContext -ListAvailable` を使用します。

    ```powershell
    Get-AzContext -ListAvailable | Select Name
    ```

1. 自動生成されたコンテキスト名は、扱いにくい場合があります。 コンテキスト名を読みやすくし、パラメーターとして使用しやすくするために、コンテキストの名前を変更できます。 コンテキスト名の変更は、[Rename-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/rename-azcontext) を使用して行われます。

    ```powershell
    Rename-AzContext -SourceName <current_context_name> -TargetName <new_context_Name>
    ```

1. 現在の PowerShell セッションで特定の Azure サブスクリプションを使用するには、[Select-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/select-azcontext) を使用します。

    ```powershell
    Select-AzContext <context_name>
    ```

## <a name="configure-terraform"></a>Terraform を構成する

1. [Terraform をダウンロードします](https://www.terraform.io/downloads.html)。

1. ダウンロードから、任意のディレクトリに実行可能ファイルを抽出します。

1. [システムのグローバル パスを実行可能ファイルに更新](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)します。

1. `terraform` コマンドでグローバル パス構成を確認します。 Terraform 実行可能ファイルが見つかった場合は、使用可能な Terraform オプションのリストが表示されます。

    ```powershell
    terraform
    ```

    Terraform 実行可能ファイルが見つかった場合は、構文と使用可能なコマンドのリストが表示されます。

    ```output
    Usage: terraform [-version] [-help] <command> [args]

    The available commands for execution are listed below.
    The most common, useful commands are shown first, followed by
    less common or more advanced commands. If you're just getting
    started with Terraform, stick with the common commands. For the
    other commands, please read the help and docs before usage.
    ...
    ```

## <a name="create-a-terraform-configuration-file"></a>Terraform 構成ファイルを作成する

このセクションでは、Azure リソース グループを作成する Terraform 構成ファイルを作成する方法について説明します。

1. このデモ用の Terraform ファイルを格納するディレクトリを作成します。

1. ディレクトリを demo ディレクトリに変更します。

1. 好みのエディターを使用して、`QuickstartTerraformTest.tf` という名前の Terraform 構成ファイルを作成します。

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

    注:
    - プロバイダー ブロックは、[Azure プロバイダー (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html) が使用されることを指定しています。
    - azurerm プロバイダー ブロック内で、バージョンと機能の属性が設定されます。 コメントに記載されているように、その使用方法はバージョン固有です。 ご使用環境に合わせてこれらの属性を設定する方法の詳細については、[AzureRM プロバイダーの v2.0](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html) に関するページを参照してください。
    - 唯一の[リソース宣言](https://www.terraform.io/docs/configuration/resources.html)は、[azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html) のリソースの種類に対するものです。 azure_resource_group の 2 つの必須引数は、name と location です。

## <a name="create-and-apply-a-terraform-execution-plan"></a>Terraform 実行プランを作成して適用する

構成ファイルを作成したら、このセクションでは、"*実行プラン*" を作成し、それをクラウド インフラストラクチャに適用する方法について説明します。

1. [terraform init](https://www.terraform.io/docs/commands/init.html) を使用して Terraform のデプロイを初期化します。 この手順によって、Azure リソース グループを作成するために必要な Azure モジュールがダウンロードされます。

    ```powershell
    terraform init
    ```
    
1. Terraform では、[terraform plan](https://www.terraform.io/docs/commands/plan.html) を使用して、完了する予定のアクションをプレビューできます。

    ```powershell
    terraform plan
    ```

    **注:**
    - `terraform plan` コマンドは、実行プランを作成しますが、実行はしません。 代わりに、構成ファイルに指定された構成を作成するために必要なアクションを決定します。
    - `terraform plan` コマンドを使用すると、実際のリソースに変更を加える前に、実行プランが自分が期待しているものと一致しているかどうかを確認できます。
    - 省略可能な `-out` パラメーターを使用すると、プランの出力ファイルを指定できます。 `-out` パラメーターの使用方法の詳細については、「[後のデプロイのために実行プランを保存する](#persist-an-execution-plan-for-later-deployment)」セクションを参照してください。

1. [terraform apply](https://www.terraform.io/docs/commands/apply.html) を使用して、実行プランを適用します。

    ```powershell
    terraform apply
    ```
    
1. Terraform は、実行プランを適用した場合に何が起こるかを示し、その実行を確認するよう求めます。 `yes` を入力し、**Enter** キーを押して、コマンドを確認します。

1. 再生の実行を確認したら、[az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) を使用して、リソース グループが正常に作成されたことをテストします。

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    成功すると、コマンドにより、新しく作成されたリソース グループのさまざまなプロパティが表示されます。

## <a name="persist-an-execution-plan-for-later-deployment"></a>後のデプロイのために実行プランを保存する

前のセクションでは、[terraform plan](https://www.terraform.io/docs/commands/plan.html) を実行して実行プランを作成する方法を確認しました。 次に、[terraform apply](https://www.terraform.io/docs/commands/apply.html) を使用してそのプランを適用することを確認しました。 このパターンは、手順が対話型で逐次的な場合に適しています。

より複雑なシナリオについては、実行プランをファイルに保存できます。 後で、または別のマシンからでも、その実行プランを適用することができます。

この機能を使用する場合は、[自動での Terraform の実行](https://learn.hashicorp.com/terraform/development/running-terraform-in-automation)に関するページを読むことをお勧めします。

次の手順は、この機能を使用するための基本的なパターンを示しています。

1. [terraform init](https://www.terraform.io/docs/commands/init.html) を実行します。

    ```powershell
    terraform init
    ```

1. `-out` パラメーターを指定して `terraform plan` を実行します。

    ```powershell
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

1. 前の手順のファイルの名前を指定して `terraform apply` を実行します。

    ```powershell
    terraform apply QuickstartTerraformTest.tfplan
    ```

注:
- 自動化での使用を有効にするために、`terraform apply <filename>` の実行で確認が求められることはありません。
- この機能を使用する場合は、[セキュリティ警告セクション](https://www.terraform.io/docs/commands/plan.html#security-warning)を参照してください。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

この記事で作成したリソースが不要になったら、削除してください。

1. 現在の実行プランを破棄する [terraform destroy](https://www.terraform.io/docs/commands/destroy.html) を実行します。

    ```powershell
    terraform destroy
    ```

1. Terraform は、実行プランを破棄すると何が起こるかを示し、確認するよう求めます。 `yes` を入力し、**Enter** キーを押して、コマンドを確認します。

1. 再生の実行を確認すると、出力は次の例のようになります。[az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) を使用して、リソース グループが削除されたことを確認します。

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    注:
    - 成功すると、`Get-AzResourceGroup` により、リソース グループが存在しないという事実が表示されます。

1. ディレクトリを親ディレクトリに変更し、demo ディレクトリを削除します。 `-r` パラメーターを指定すると、ディレクトリが削除される前にディレクトリの内容が削除されます。 ディレクトリの内容には、前に作成した構成ファイルと Terraform の状態ファイルが含まれています。

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Terraform がインストールされている Azure VM を作成する](create-linux-virtual-machine-with-infrastructure.md)