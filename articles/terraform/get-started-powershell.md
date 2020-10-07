---
title: クイック スタート - Azure PowerShell を使用して Terraform を構成する
description: このクイックスタートでは、Azure PowerShell を使用して Terraform をインストールして構成する方法について説明します。
keywords: Azure DevOps Terraform インストール 構成 Windows 初期化 プラン 適用 実行 ログイン RBAC サービス プリンシパル 自動スクリプト PowerShell
ms.topic: quickstart
ms.date: 09/27/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 8f95d0bb09d7e9e7ea789b90a27178cdf5426d74
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401556"
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
> * 基本の Terraform 構成ファイルを作成する
> * Terraform 実行プランを作成して適用する
> * 実行プランを破棄する

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>環境を構成する

1. Azure リソースの操作を可能にする最新の PowerShell モジュールは、[Azure PowerShell Az モジュール](/powershell/azure/new-azureps-module-az)と呼ばれています。 Azure PowerShell Az モジュールを使用する場合、すべてのプラットフォームで推奨されるバージョンは PowerShell 7 (またはそれ以降) です。 PowerShell がインストールされている場合は、PowerShell プロンプトで次のコマンドを入力して、バージョンを確認できます。

    ```powershell
    $PSVersionTable.PSVersion
    ```

1. [PowerShell をインストールします](/powershell/scripting/install/installing-powershell-core-on-windows)。 このデモは、Windows 10 で PowerShell v7.0.2 を使用してテストされました。

1. [Terraform で Azure に対して認証を行う](https://www.terraform.io/docs/providers/azurerm/guides/azure_cli.html)には、[Azure CLI をインストールする](/cli/azure/install-azure-cli-windows)必要があります。 このデモは、Azure CLI バージョン 2.9.1 を使用してテストされました。

1. [Terraform をダウンロードします](https://www.terraform.io/downloads.html)。

1. ダウンロードから、任意のディレクトリに実行可能ファイルを抽出します。

1. [システムのグローバル パスを実行可能ファイルに更新](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)します。

1. `terraform` コマンドでグローバル パス構成を確認します。

    ```powershell
    terraform
    ```

    **注**:
    - Terraform 実行可能ファイルが見つかった場合は、構文と使用可能なコマンドが一覧表示されます。

## <a name="authenticate-to-azure"></a>Azure に対して認証します

PowerShell と Terraform を使用する場合は、サービス プリンシパルを使用してログインする必要があります。 次の 2 つのセクションでは、次のタスクについて説明します。

- [Azure サービス プリンシパルを作成する](#create-an-azure-service-principal)
- [サービス プリンシパルを使用して Azure にログインする](#log-in-to-azure-using-a-service-principal)


### <a name="span-idcreate-an-azure-service-principalcreate-an-azure-service-principal"></a><span id="create-an-azure-service-principal"/>Azure サービス プリンシパルを作成する

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

### <a name="span-idlog-in-to-azure-using-a-service-principallog-in-to-azure-using-a-service-principal"></a><span id="log-in-to-azure-using-a-service-principal"/>サービス プリンシパルを使用して Azure にログインする

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

[!INCLUDE [terraform-create-base-config-file.md](includes/terraform-create-base-config-file.md)]

[!INCLUDE [terraform-create-and-apply-execution-plan.md](includes/terraform-create-and-apply-execution-plan.md)]

[!INCLUDE [terraform-reverse-execution-plan.md](includes/terraform-reverse-execution-plan.md)]

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Terraform を使用して Linux VM を作成する](create-linux-virtual-machine-with-infrastructure.md)
