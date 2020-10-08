---
title: 'チュートリアル、パート 5: Azure サービスで Python アプリを認証する'
description: メイン アプリの依存関係 (主に Azure SDK ライブラリ)、必要な import ステートメント、および設定する必要がある環境変数について説明します。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 9c6204afd17d86cd8677022a59641e5343c6a543
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764732"
---
# <a name="part-5-main-app-dependencies-import-statements-and-environment-variables"></a>パート 5: メイン アプリの依存関係、import ステートメント、および環境変数

[前のパート:メイン アプリの実装例](walkthrough-tutorial-authentication-04.md)

このパートでは、メイン アプリに取り込まれた Python ライブラリと、コードに必要な環境変数について説明します。 Azure にデプロイした場合は、Azure App Service のアプリケーション設定を使用して環境変数を指定します。

## <a name="dependencies-and-import-statements"></a>依存関係と import ステートメント

アプリ コードには、次のようないくつかのライブラリが必要です。Flask、標準の HTTP 要求ライブラリ、および Active Directory ([azure.identity](/python/api/overview/azure/identity-readme))、Key Vault ([azure.keyvault.secrets](/python/api/overview/azure/keyvault-secrets-readme))、Queue Storage ([azure.storage.queue](/python/api/overview/azure/storage-queue-readme)) 用の Azure ライブラリ。 これらのライブラリは、アプリの *requirements.txt* ファイルに含まれています。

```txt
flask
requests
azure.identity
azure.keyvault.secrets
azure.storage.queue
```

アプリを Azure App Service にデプロイすると、Azure によってこれらの要件がホスト サーバーに自動的にインストールされます。 ローカルで実行する場合は、`pip install -r requirements.txt` を使用してそれらを環境にインストールします。

コードの先頭には、ライブラリから使用するパーツに必要な import ステートメントがあります。

```python
from flask import Flask, request, jsonify
import requests, random, string, os
from datetime import datetime
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient
from azure.storage.queue import QueueClient
```

## <a name="environment-variables"></a>環境変数

アプリ コードは、次の 4 つの環境変数に依存します。

| 変数 | 値 |
| --- | --- |
| THIRD_PARTY_API_ENDPOINT | サードパーティ API の URL。[パート 3](walkthrough-tutorial-authentication-03.md) で説明されている `https://msdocs-api-example.azurewebsites.net/api/RandomNumber` など。 |
| KEY_VAULT_URL | サードパーティ API のアクセス キーを格納した Azure Key Vault の URL。 |
| THIRD_PARTY_API_SECRET_NAME | サードパーティ API のアクセス キーを含む Key Vault 内のシークレットの名前。 |
| STORAGE_QUEUE_URL | Azure に構成されている Azure Storage Queue の URL。`https://msdocsmainappexample.queue.core.windows.net/code-requests` など ([パート 4](walkthrough-tutorial-authentication-04.md) を参照)。 キュー名は URL の末尾に含まれているので、コード内のどこにも名前は表示されません。 |

ローカルで実行する場合は、使用している任意のコマンド シェル内でこれらの変数を作成します。 アプリを仮想マシンにデプロイする場合は、同様のサーバー側変数を作成します。

ただし、Azure App Service にデプロイする場合、サーバー自体にはアクセスできません。 この場合は、同じ名前で "*アプリケーション設定*" を作成すると、環境変数としてアプリに表示されます。 

プロビジョニング スクリプトは、Azure CLI コマンド [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) を使用してこれらの設定を作成します。 4 つの変数はすべて 1 つのコマンドで設定されます。

Azure portal を使用して設定を作成するには、「[Azure portal で App Service アプリを構成する](/azure/app-service/configure-common)」を参照してください。

> [!div class="nextstepaction"]
> [パート 6 - メイン アプリのスタートアップ コード >>>](walkthrough-tutorial-authentication-06.md)
