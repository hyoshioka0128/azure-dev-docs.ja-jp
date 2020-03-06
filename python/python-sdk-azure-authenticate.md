---
title: Python 用 Azure 管理ライブラリを使用してアプリを認証する
description: Azure 管理 SDK ライブラリを使用して、Azure サービスを使って Python アプリを認証します
ms.date: 01/16/2020
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: abf1b0b37d782addfd2b772fb7d382ce2815e891
ms.sourcegitcommit: aa2c66b0fecce51862cc9115f68d39c770f0b2ae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77709819"
---
# <a name="authenticate-by-using-the-azure-management-libraries-for-python"></a>Python 用 Azure 管理ライブラリを使用した認証

この記事では、Python SDK 管理ライブラリを使用して、サービス プリンシパルを使って Azure Active Directory (Azure AD) でアプリケーションを認証する方法について説明します。 サービス プリンシパルは Azure AD に登録されているアプリケーションの ID であり、これにより、アプリケーションはそのアクセス許可に従ってリソースにアクセスしたり、リソースを変更したりできます。

アプリケーションを登録するには、まず、組織の適切なテナントを使用して Active Directory を作成する必要があります。 これを行うには、「[Azure Active Directory で新しいテナントを作成する](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)」の手順に従ってください。 Active Directory の準備ができたら、「[方法: リソースにアクセスできる Azure AD アプリケーションとサービス プリンシパルをポータルで作成する](/azure/active-directory/develop/howto-create-service-principal-portal)」に従ってアプリケーションを登録し、[サービス プリンシパル用のテナントとアプリケーション (クライアント) ID を取得](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)し、Python コードからの認証に使用する[アプリケーション シークレット](/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret)を設定します。

これらの値を設定したら、その資格情報を使用して、Python SDK ライブラリを使ってさまざまな方法で認証を行えます。 各メソッドの結果は、コードから他のリソースにアクセスするときに使用する SDK クライアント オブジェクトです。

これらの値がシステムまたはソース管理のどこにも存在しないようにするために、テナント ID、クライアント ID、シークレットを [Azure KeyVault](/azure/key-vault/) に格納することを強くお勧めします。 必要に応じて、いつでも値を簡単に取得できます。

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="mgmt-auth-file"></a>JSON ファイルを使用した認証

このメソッドでは、サービス プリンシパルに必要な資格情報を含む JSON ファイルを作成し、ファイル内の情報を使用して SDK クライアント オブジェクトを作成します。

1. 次の形式で JSON ファイルを作成します (*app_credentials.json* など、任意の名前を付けます)。 4 つのプレースホルダーを、ご使用の Azure サブスクリプション ID、Azure AD テナント ID、アプリケーション (クライアント) ID、シークレットに置き換えます。

    ```json
    {
        "subscriptionId": "<azure_aubscription_id>",
        "tenantId": "<tenant_id>",
        "clientId": "<application_id>",
        "clientSecret": "<application_secret>",
        "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
        "resourceManagerEndpointUrl": "https://management.azure.com/",
        "activeDirectoryGraphResourceId": "https://graph.windows.net/",
        "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
        "galleryEndpointUrl": "https://gallery.azure.com/",
        "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

    > [!TIP]
    > Azure にサインインすることで、サブスクリプション ID が既に設定されている資格情報ファイルを取得できます。その際には、[az login](/cli/azure/reference-index#az-login) コマンドの後に [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) コマンドを使用します。
    >
    > ```azurecli
    > az login
    > az ad sp create-for-rbac --sdk-auth > credentials.json
    > ```
    >
    > その後、汎用の値を使用するのではなく、特定のアプリケーションの `tenantId`、`clientId`、`clientSecret` 値を置き換えることができます。

1. このファイルは、コードがアクセスできる安全な場所に保存してください。

1. [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-auth-file-client-class--auth-path-none----kwargs-) メソッドを使用してクライアント オブジェクトを作成します。`<path_to_file>` を JSON ファイルへのパスに置き換えます。

    ```python
    from azure.common.client_factory import get_client_from_auth_file
    from azure.mgmt.compute import ComputeManagementClient

    client = get_client_from_auth_file(ComputeManagementClient, auth_path=<path_to_file>)
    ```

1. また、ファイルへのパスを `AZURE_AUTH_LOCATION` という名前の環境変数に格納し、`auth_path` 引数を省略することもできます。

## <a name="authenticate-with-a-json-dictionary"></a>JSON ディクショナリを使用した認証

前のセクションで説明したように、ファイルを使用する代わりに、必要な JSON を変数に作成し、代わりに [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-json-dict-client-class--config-dict----kwargs-) を呼び出すことができます。 この場合常に、テナント ID、クライアント ID、シークレットを、[Azure KeyVault](/azure/key-vault/) のように安全な場所に保存する必要があります。

```python
   from azure.common.client_factory import get_client_from_auth_file
   from azure.mgmt.compute import ComputeManagementClient

    # Retrieve tenant_id, client_id, and client_secret from Azure KeyVault

   config_dict = {
       "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
        "tenantId": tenant_id,
       "clientId": client_id,
       "clientSecret": client_secret,
       "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
       "resourceManagerEndpointUrl": "https://management.azure.com/",
       "activeDirectoryGraphResourceId": "https://graph.windows.net/",
       "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
       "galleryEndpointUrl": "https://gallery.azure.com/",
       "managementEndpointUrl": "https://management.core.windows.net/"
   }
   client = get_client_from_json_dict(ComputeManagementClient, config_dict)
```

## <a name="mgmt-auth-token"></a>トークン資格情報による認証

[Azure KeyVault](/azure/key-vault/) など、セキュリティで保護されたストレージから資格情報を取得する場合は、まず [ServicePrincipalCredentials] オブジェクトを作成し、次にこれらの資格情報とサブスクリプション ID を使用してクライアントのインスタンスを作成します。

```python
from azure.mgmt.compute import ComputeManagementClient
from azure.common.credentials import ServicePrincipalCredentials

# Retrieve credentials from secure storage. Never hard-code credentials into code.

credentials = ServicePrincipalCredentials(tenant = <tenant_id>,
    client_id = <client_id>, secret = <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

さらに制御する必要がある場合は、[Azure Active Directory Authentication Library (ADAL) for Python](https://github.com/AzureAD/azure-activedirectory-library-for-python) と SDK ADAL ラッパーを使用します。

```python
from azure.mgmt.compute import ComputeManagementClient
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

# Retrieve credentials from secure storage. Never hard-code credentials into code.

LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)

credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    RESOURCE, <client_id>, <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

> [!NOTE]
> Azure ソブリン クラウドを使用する場合は、管理クライアントを作成するときに (`msrestazure.azure_cloud` の定数を使用して) 適切なベース URL を指定します。
>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```

### <a name="mgmt-auth-legacy"></a>トークン資格情報による認証 (非推奨)

[Azure Active Directory Authentication Library (ADAL) for Python](https://github.com/AzureAD/azure-activedirectory-library-for-python) を利用できるようになる前に、`UserPassCredentials` クラスを使用しました。 これは 2 要素認証をサポートしないため、このクラスの使用は非推奨とされています。今後は使用しないでください。

```python
from azure.common.credentials import UserPassCredentials

# DEPRECATED - legacy purposes only - use ADAL instead
credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```

## <a name="mgmt-auth-msi"></a>Azure マネージド ID を使用して認証する

Azure マネージド ID により、Azure のリソースで、特定の資格情報を使用せずに、簡単に認証できます。

マネージド ID を使用するには、別の Azure リソース (Azure 関数、仮想マシンなど) から Azure に接続している必要があります。 リソースにマネージド ID を構成する方法については、[Azure リソースのマネージド ID の構成](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)に関するページ、および[Azure リソースのマネージド ID を使用する方法](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)に関するページをご覧ください。

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

# Create MSI Authentication
credentials = MSIAuthentication()

# Create a Subscription Client
subscription_client = SubscriptionClient(credentials)
subscription = next(subscription_client.subscriptions.list())
subscription_id = subscription.subscription_id

# Create a Resource Management client
client = ResourceManagementClient(credentials, subscription_id)

# List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
for resource_group in resource_client.resource_groups.list():
    print(resource_group.name)
```

## <a name="mgmt-auth-cli"></a>CLI ベースの認証 (開発目的のみ)

`az login` を実行した後、SDK では、Azure CLI のアクティブなサブスクリプションを使用してクライアントを作成できます。 SDK では既定のサブスクリプション ID がを使用されます。または、[az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) を使用してサブスクリプションを設定することもできます。

このオプションは、開発目的でのみ使用してください。

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
