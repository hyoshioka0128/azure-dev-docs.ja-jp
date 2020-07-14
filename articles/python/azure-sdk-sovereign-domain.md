---
title: Python 用 Azure ライブラリを使用してすべてのリージョンに接続する - マルチクラウド
description: msrestazure の azure_cloud モジュールを使用して、さまざまなソブリン リージョンで Azure に接続する方法
ms.date: 06/09/2020
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: c8dc34260f4a37090af8c8408f7da70cf1de1f23
ms.sourcegitcommit: b3e506c6f140d91e6fdd9dcadf22ab1aa67f6978
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2020
ms.locfileid: "84947510"
---
# <a name="multi-cloud-connect-to-all-regions-with-the-azure-libraries-for-python"></a>マルチクラウド:Python 用 Azure ライブラリを使用してすべてのリージョンに接続する

Python 用 Azure ライブラリを使用して、Azure が[利用可能な](https://azure.microsoft.com/regions/services)すべてのリージョンに接続できます。

既定では、Azure ライブラリはグローバル Azure に接続するように構成されています。

## <a name="using-pre-declared-cloud-definition"></a>事前に宣言されているクラウド定義の使用

`msrestazure` (0.4.11 以降) の `azure_cloud` モジュールを使用します。

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

credentials = UserPassCredentials(login, password,
    cloud_environment=AZURE_CHINA_CLOUD)

client = ResourceManagementClient(credentials,
    subscription_id, base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
```
  
使用可能なクラウド定義は次のとおりです。

- `AZURE_PUBLIC_CLOUD`
- `AZURE_CHINA_CLOUD`
- `AZURE_US_GOV_CLOUD`
- `AZURE_GERMAN_CLOUD`

## <a name="using-your-own-cloud-definition"></a>独自のクラウド定義の使用

このコードでは、プライベート クラウド (たとえば、Azure Stack 上に構築されたもの) の Azure Resource Manager エンドポイントで `get_cloud_from_metadata_endpoint` を使用します。

```python
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

stack_cloud = get_cloud_from_metadata_endpoint("https://contoso-azurestack-arm-endpoint.com")
credentials = UserPassCredentials(login, password,
    cloud_environment=stack_cloud)

client = ResourceManagementClient(credentials, subscription_id,
    base_url=stack_cloud.endpoints.resource_manager)
```

## <a name="using-adal"></a>ADAL の使用

別のリージョンに接続するには、いくつかの点を考慮する必要があります。

- どのエンドポイントにトークンを要求するか (認証)
- そのトークンをどのエンドポイントで使用するか (使用法)

一般的な例を次に示します。

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Public Azure - default values
authentication_endpoint = 'https://login.microsoftonline.com/'
azure_endpoint = 'https://management.azure.com/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```

### <a name="azure-government"></a>Azure Government

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Government
authentication_endpoint = 'https://login.microsoftonline.us/'
azure_endpoint = 'https://management.usgovcloudapi.net/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```

### <a name="azure-germany"></a>Azure Germany

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure Germany
authentication_endpoint = 'https://login.microsoftonline.de/'
azure_endpoint = 'https://management.microsoftazure.de/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```

### <a name="azure-china-21vianet"></a>Azure China 21Vianet

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure China
authentication_endpoint = 'https://login.chinacloudapi.cn/'
azure_endpoint = 'https://management.chinacloudapi.cn/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```