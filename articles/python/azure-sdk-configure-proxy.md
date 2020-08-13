---
title: Azure ライブラリを使用する場合のプロキシの構成
description: HTTP[S]_PROXY 環境変数を使用して、スクリプトまたはアプリ全体のプロキシを定義したり、クライアント コンストラクターまたは操作メソッドに対して省略可能な名前付き引数を使用したりします。
ms.date: 06/16/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 86eaa5b8dc0ddfb529643a5c015938344582e62b
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87982704"
---
# <a name="how-to-configure-proxies-for-the-azure-libraries"></a>Azure ライブラリ用にプロキシを構成する方法

プロキシ サーバーの URL の形式は `http[s]://[username:password@]<ip_address_or_domain>:<port>/` です。この username:password の組み合わせは省略可能です。

その後、環境変数を使用して、プロキシをグローバルに構成できます。あるいは、`proxies` という名前の引数を、個々のクライアント コンストラクターまたは操作メソッドに渡すことによって、プロキシを指定することができます。

## <a name="global-configuration"></a>グローバル構成

スクリプトまたはアプリ用にプロキシをグローバルに構成するには、サーバーの URL を使用して `HTTP_PROXY` または `HTTPS_PROXY` 環境変数を定義します。 これらの変数は、すべてのバージョンの Azure ライブラリで機能します。

パラメーター `use_env_settings=False` をクライアント オブジェクト コンストラクターまたは操作メソッドに渡すと、これらの環境変数は無視されます。

### <a name="from-python-code"></a>Python コードから

```python
import os
os.environ["HTTP_PROXY"] = "http://10.10.1.10:1180"

# Alternate URL and variable forms:
# os.environ["HTTP_PROXY"] = "http://username:password@10.10.1.10:1180"
# os.environ["HTTPS_PROXY"] = "http://10.10.1.10:1180"
# os.environ["HTTPS_PROXY"] = "http://username:password@10.10.1.10:1180"
```

### <a name="from-the-cli"></a>CLI から

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
rem Non-authenticated HTTP server:
set HTTP_PROXY=http://10.10.1.10:1180

rem Authenticated HTTP server:
set HTTP_PROXY=http://username:password@10.10.1.10:1180

rem Non-authenticated HTTPS server:
set HTTPS_PROXY=http://10.10.1.10:1180

rem Authenticated HTTPS server:
set HTTPS_PROXY=http://username:password@10.10.1.10:1180
```

# <a name="bash"></a>[bash](#tab/bash)

```bash
# Non-authenticated HTTP server:
HTTP_PROXY=http://10.10.1.10:1180

# Authenticated HTTP server:
HTTP_PROXY=http://username:password@10.10.1.10:1180

# Non-authenticated HTTPS server:
HTTPS_PROXY=http://10.10.1.10:1180

# Authenticated HTTPS server:
HTTPS_PROXY=http://username:password@10.10.1.10:1180
```

---

## <a name="per-client-or-per-method-configuration"></a>クライアントごとまたはメソッドごとの構成

特定のクライアント オブジェクトまたは操作メソッド用にプロキシを構成するには、`proxies` という名前の引数を使用してプロキシ サーバーを指定します。

たとえば、[例: Azure storage の使用](azure-sdk-example-storage.md)に関する記事の以下のコードでは、`BlobClient` コンストラクターとユーザー資格情報を使用して HTTPS プロキシを指定します。 この場合、オブジェクトは、azure. core に基づく、azure.storage.blob ライブラリから取得されます。

```python
# Earlier code omitted for brevity

blob_client = BlobClient(storage_url, container_name="blob-container-01",
    blob_name="sample-blob.txt", credential=credential,
    proxies={ "https": "https://username:password@10.10.1.10:1180" }
)

# Other forms that the proxy URL might take:
# proxies={ "http": "http://10.10.1.10:1180" }
# proxies={ "http": "http://username:password@10.10.1.10:1180" }
# proxies={ "https": "https://10.10.1.10:1180" }
```
