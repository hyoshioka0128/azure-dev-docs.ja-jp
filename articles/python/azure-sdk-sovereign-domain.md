---
title: Python 用 Azure ライブラリを使用してすべてのリージョンに接続する - マルチクラウド
description: msrestazure の azure_cloud モジュールを使用して、さまざまなソブリン リージョンで Azure に接続する方法
ms.date: 07/13/2020
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: 25e8851a8812782712ff65ec4627a0d2ead848ae
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86377906"
---
# <a name="multi-cloud-connect-to-all-regions-with-the-azure-libraries-for-python"></a>マルチクラウド:Python 用 Azure ライブラリを使用してすべてのリージョンに接続する

Python 用 Azure ライブラリを使用して、Azure が[利用可能な](https://azure.microsoft.com/regions/services)すべてのリージョンに接続できます。

既定では、Azure ライブラリはグローバル Azure に接続するように構成されています。

## <a name="using-pre-defined-sovereign-cloud-constants"></a>ソブリン クラウドの定義済み定数の使用

ソブリン クラウドの定義済み定数は、`msrestazure` (0.4.11+) の `azure_cloud` モジュールによって提供されます。

- `AZURE_PUBLIC_CLOUD`
- `AZURE_CHINA_CLOUD`
- `AZURE_US_GOV_CLOUD`
- `AZURE_GERMAN_CLOUD`

定数をコード全体に適用するには、上記の一覧のいずれかの値を使用して `AZURE_CLOUD` という名前の環境変数を定義します (`AZURE_PUBLIC_CLOUD` が既定値です)。

特定の操作内に定数を適用するには、`msrest.azure_cloud` から目的の定数をインポートし、資格情報とクライアント オブジェクトを作成するときにそれを使用します。

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

credentials = UserPassCredentials(login, password,
    cloud_environment=AZURE_CHINA_CLOUD)

client = ResourceManagementClient(credentials,
    subscription_id, base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
```
  
## <a name="using-your-own-cloud-definition"></a>独自のクラウド定義の使用

次のコードでは、プライベート クラウド (たとえば、Azure Stack 上に構築されたもの) の Azure Resource Manager エンドポイントで `get_cloud_from_metadata_endpoint` を使用します。

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

別のリージョンに接続する場合は、次の事項を考慮してください。

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
