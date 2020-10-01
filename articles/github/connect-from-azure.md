---
title: GitHub と Azure を接続する
description: Azure およびその他のサービスから GitHub に接続するためのリソース
author: N-Usha
ms.author: ushan
ms.topic: reference
ms.service: azure
ms.date: 08/31/2020
ms.custom: github-actions-azure
ms.openlocfilehash: 4900097ea8d3635c2030f73eab1c63fb46323bc9
ms.sourcegitcommit: e97cb81a245ce7dcabeac3260abc3db7c30edd79
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91493123"
---
# <a name="use-github-actions-to-connect-to-azure"></a>GitHub Actions を使用して Azure に接続する

[Azure ログイン](https://github.com/Azure/login)を [Azure PowerShell](https://github.com/Azure/PowerShell) または [Azure CLI](https://github.com/Azure/CLI) とともに使用して Azure リソースを操作する方法について説明します。

Azure PowerShell または Azure CLI を使用するには、最初に [Azure ログイン](https://github.com/marketplace/actions/azure-login)でログインする必要があります。 Azure ログイン アクションは、サービス プリンシパルを使用して Azure サブスクリプションを GitHub に接続します。

ログイン アクションを設定したら、Azure CLI または Azure PowerShell を使用できます。  
Azure CLI は、Azure CLI の GitHub アクション ランナー環境を設定します。 Azure PowerShell は、Azure PowerShell モジュールを使用して GitHub アクション ランナー環境を設定します。


## <a name="create-a-service-principal-and-add-it-to-github-secret"></a>サービス プリンシパルを作成して GitHub シークレットに追加する

[Azure ログイン](https://github.com/marketplace/actions/azure-login)を使用するには、まず、Azure サービス プリンシパルをシークレットとして GitHub リポジトリに追加する必要があります。

この例では、Azure での認証に使用できる `AZURE_CREDENTIALS` という名前のシークレットを作成します。  

1. 既存のアプリケーションがない場合は、サービス プリンシパルで使用する[新しい Active Directory アプリケーション](/azure/active-directory/develop/howto-create-service-principal-portal#register-an-application-with-azure-ad-and-create-a-service-principal&preserve-view=true)を登録します。

    ```azurecli-interactive
        appName="myApp"

        az ad app create \
        --display-name $appName \
        --homepage "http://localhost/$appName" \
        --identifier-uris http://localhost/$appName
    ```

1. Azure portal でアプリ用の[新しいサービス プリンシパルを作成します](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)。 

    ```azurecli-interactive
        az ad sp create-for-rbac --name "myApp" --role contributor \
                                    --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                                    --sdk-auth
    ```

1. サービス プリンシパルの JSON オブジェクトをコピーします。

    ```json
    {
        "clientId": "<GUID>",
        "clientSecret": "<GUID>",
        "subscriptionId": "<GUID>",
        "tenantId": "<GUID>",
        (...)
    }
    ```

1. GitHub リポジトリを開き、 **[Settings]\(設定\)** に移動します。

    :::image type="content" source="media/github-repo-settings.png" alt-text="ナビゲーションで [Settings]\(設定\) を選択する":::

1. **[Settings]\(設定\)** 、 **[New Secret]\([新しいシークレット\)]** の順に選択します。

    :::image type="content" source="media/select-secrets.png" alt-text="ナビゲーションで [Settings]\(設定\) を選択する":::

1. `AZURE_CREDENTIALS` という名前のサービス プリンシパルの JSON オブジェクトを貼り付けます。 

    :::image type="content" source="media/azure-secret-add.png" alt-text="ナビゲーションで [Settings]\(設定\) を選択する":::

1. **[Add secret]\(シークレットの追加\)** を選択して保存します。

## <a name="use-the-azure-login-action"></a>Azure ログイン アクションを使用する

Azure で認証するには、サービス プリンシパル シークレットと [Azure ログイン アクション](https://github.com/Azure/login)を使用します。

このワークフローでは、`secrets.AZURE_CREDENTIALS` を使用して認証し、Azure CLI アクションを実行します。

動作する Azure ログインを作成したら、Azure PowerShell または Azure CLI アクションを使用できます。 [Azure Webapp のデプロイ](https://github.com/Azure/webapps-deploy)や [Azure Functions](https://github.com/Azure/functions-action) など、その他の Azure アクションを使用することもできます。

```yaml
on: [push]

name: AzureLoginSample

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Log in with Azure
        uses: azure/login@v1
        with:
          creds: '${{ secrets.AZURE_CREDENTIALS }}'
```

## <a name="use-the-azure-powershell-action"></a>Azure PowerShell アクションを使用する

この例では、[Azure ログイン アクション](https://github.com/Azure/login)を使用してログインし、[Azure CLI アクション](https://github.com/azure/powershell)を使用してリソース グループを取得します。

```yaml
on: [push]

name: AzureLoginSample

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Log in with Azure
        uses: azure/login@v1
        with:
          creds: '${{ secrets.AZURE_CREDENTIALS }}'
          enable-AzPSSession: true
      - name: Azure PowerShell Action
        uses: Azure/powershell@v1
        with:
          inlineScript: Get-AzVM -ResourceGroupName "< YOUR RESOURCE GROUP >"
          azPSVersion: 3.1.0
```

## <a name="use-the-azure-cli-action"></a>Azure CLI アクションを使用する

この例では、[Azure ログイン アクション](https://github.com/Azure/login)を使用してログインし、[Azure CLI アクション](https://github.com/Azure/CLI)を使用してリソース グループを取得します。


```yaml
on: [push]

name: AzureLoginSample

jobs:
build-and-deploy:
    runs-on: ubuntu-latest
    steps:

    - name: Log in with Azure
        uses: azure/login@v1
        with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Azure CLI script
        uses: azure/CLI@v1
        with:
        azcliversion: 2.0.72
        inlineScript: |
            az account show
            az storage -h
```

## <a name="connect-with-other-azure-services"></a>他の Azure サービスと接続する

次の記事では、Azure およびその他のサービスから GitHub に接続する方法について詳しく説明しています。  

### <a name="azure-active-directory"></a>Azure Active Directory 

- [Azure AD で GitHub Enterprise にサインインする (シングル サインオン)](/azure/active-directory/saas-apps/github-tutorial)   

### <a name="power-bi"></a>Power BI

- [Power BI を GitHub に接続する](/power-bi/service-connect-to-github)   

### <a name="connectors"></a>Connectors

- [Azure Logic Apps、Power Automate、および Power Apps 用の GitHub コネクタ](/connectors/github/)   

### <a name="azure-databricks"></a>Azure Databricks

- [GitHub をノートブックのバージョン管理として使用する](/azure/databricks/notebooks/github-version-control) 

> [!div class="nextstepaction"]
> [GitHub から Azure にアプリをデプロイする](deploy-to-azure.md)