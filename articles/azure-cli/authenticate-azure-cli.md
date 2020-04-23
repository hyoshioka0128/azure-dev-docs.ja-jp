---
title: Azure CLI を使用してサインインする
description: 対話形式またはローカル資格情報で Azure CLI を使用してサインインします
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 26112c9a7f338a7f178dc627d89f6e1f33d97714
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030864"
---
# <a name="sign-in-with-azure-cli"></a>Azure CLI を使用してサインインする 

Azure CLI には、いくつかの認証の種類があります。 [Azure Cloud Shell](/azure/cloud-shell/overview) を使用すると自動的にログインできるため、最も簡単に作業を開始できます。
ローカルでは、ご使用のブラウザーで [az login](/cli/azure/reference-index#az-login) コマンドを使用して、対話形式でサインインできます。 スクリプトを記述するときは、サービス プリンシパルを使用することをお勧めします。 サービス プリンシパルに必要とされる適切なアクセス許可だけを付与することによって、自動化をより安全に維持できます。

CLI によってサインイン情報が保存されることは、まったくありません。 代わりに、[認証更新トークン](https://docs.microsoft.com/azure/active-directory/develop/v1-id-and-access-tokens#refresh-tokens)が Azure によって生成され、保存されます。 2018 年 8 月の時点で、このトークンは非アクティブな状態が 90 日続くと取り消されますが、この値は、Microsoft またはテナント管理者が変更できます。 トークンが取り消されると、もう一度サインインする必要があることが CLI からメッセージで通知されます。

サインインすると、CLI コマンドが既定のサブスクリプションに対して実行されます。 複数のサブスクリプションがある場合は、[既定のサブスクリプションを変更](manage-azure-subscriptions-azure-cli.md)できます。

## <a name="sign-in-interactively"></a>対話操作でサインインする

Azure CLI の既定の認証方法では、サインインに Web ブラウザーとアクセス トークンを使用します。

[!INCLUDE [interactive_login](includes/interactive-login.md)]

## <a name="sign-in-with-credentials-on-the-command-line"></a>コマンド ラインで資格情報を使用してサインインする

コマンド ラインで、Azure ユーザー資格情報を指定します。

> [!Note]
> この方法は、Microsoft アカウント、または 2 要素認証が有効になっているアカウントでは機能しません。

```azurecli-interactive
az login -u <username> -p <password>
```

> [!IMPORTANT]
> `az login` を対話形式で使用しているときに、コンソールにパスワードが表示されないようにするには、`bash` では `read -s` コマンドを使用します。
>
> ```bash
> read -sp "Azure password: " AZ_PASS && echo && az login -u <username> -p $AZ_PASS
> ```
>
> PowerShell では、`Get-Credential` コマンドレットを使用します。
>
> ```powershell
> $AzCred = Get-Credential -UserName <username>
> az login -u $AzCred.UserName -p $AzCred.GetNetworkCredential().Password
> ```

## <a name="sign-in-with-a-service-principal"></a>サービス プリンシパルを使ってサインインする

サービス プリンシパルは、特定のユーザーに関連付けられていないアカウントであり、定義済みのロールによってアクセス許可を割り当てることができます。 サービス プリンシパルを使用した認証は、セキュリティで保護されたスクリプトやプログラムを記述するのに最適な方法で、アクセス許可の制限と、ローカルに保存された静的な資格情報の両方を適用できます。 サービス プリンシパルの詳細については、[Azure CLI を使用した Azure サービス プリンシパルの作成](/cli/azure/create-an-azure-service-principal-azure-cli#sign-in-using-a-service-principal)に関するページをご覧ください。

サービス プリンシパルを使ってサインインするには、以下が必要です。

* サービス プリンシパルに関連付けられている URL または名前
* サービス プリンシパルのパスワード、またはサービス プリンシパルを PEM 形式で作成するために使用する X509 証明書
* `.onmicrosoft.com` ドメインまたは Azure オブジェクト ID として、サービス プリンシパルに関連付けられているテナント

> [!NOTE]
> PEM ファイル内で **PRIVATE KEY** (秘密キー) に **CERTIFICATE** (証明書) を追加する必要があります。  PEM ファイルの形式の例については、「[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli#sign-in-using-a-service-principal)」を参照してください。 
>

> [!IMPORTANT]
>
> お使いのサービス プリンシパルでは、Key Vault に格納されている証明書が使用されている場合、その証明書の秘密キーは Azure にサインインしなくても使用できる必要があります。 オフラインで使用する秘密キーを取得するには、[az keyvault secret show](/cli/azure/keyvault/secret) を使用します。

```azurecli-interactive
az login --service-principal -u <app-url> -p <password-or-cert> --tenant <tenant>
```

> [!IMPORTANT]
> `az login` を対話形式で使用しているときに、コンソールにパスワードが表示されないようにするには、`bash` では `read -s` コマンドを使用します。
>
> ```bash
> read -sp "Azure password: " AZ_PASS && echo && az login --service-principal -u <app-url> -p $AZ_PASS --tenant <tenant>
> ```
>
> PowerShell では、`Get-Credential` コマンドレットを使用します。
>
> ```powershell
> $AzCred = Get-Credential -UserName <app-url>
> az login --service-principal -u $AzCred.UserName -p $AzCred.GetNetworkCredential().Password --tenant <tenant>
> ```

## <a name="sign-in-with-a-different-tenant"></a>別のテナントでサインインする

`--tenant` 引数を使用すると、サインインするテナントを選択できます。 この引数の値として、`.onmicrosoft.com` ドメインまたはテナントの Azure オブジェクト ID を指定できます。 対話形式のサインイン方法とコマンドラインによるサインイン方法の両方で、`--tenant` を使用できます。

```azurecli-interactive
az login --tenant <tenant>
```

## <a name="sign-in-with-a-managed-identity"></a>マネージド ID を使用したサインイン

Azure リソースのマネージド ID 用に構成されたリソースでは、マネージド ID を使用してサインインできます。 リソースの ID を使用したサインインは、`--identity` フラグを介して行われます。

```azurecli-interactive
az login --identity
```

Azure リソースのマネージド ID の詳細については、[Azure リソースのマネージド ID の構成](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)に関するページ、および[Azure リソースのマネージド ID を使用したサインイン](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)に関するページをご覧ください。
