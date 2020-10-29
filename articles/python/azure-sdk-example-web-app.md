---
title: Azure SDK ライブラリを使用して Web アプリをプロビジョニングしてデプロイする
description: Python 用 Azure SDK ライブラリ内の管理ライブラリを使用して Web アプリをプロビジョニングし、GitHub リポジトリからアプリ コードをデプロイします。
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: ac678bef07fb660ca3ad094d56455e3b85db9f19
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92688988"
---
# <a name="example-use-the-azure-libraries-to-provision-and-deploy-a-web-app"></a>例:Azure ライブラリを使用して Web アプリをプロビジョニングしてデプロイする

この例では、Python スクリプトで Azure SDK の管理ライブラリを使用し、Azure App Service に Web アプリをプロビジョニングし、GitHub リポジトリからアプリ コードをデプロイする方法について説明します。 ([同等の Azure CLI コマンド](#for-reference-equivalent-azure-cli-commands)については、この記事の後半で説明します。)

特に記載のない限り、この記事で使用されているコマンドはいずれも、Linux と macOS の bash および Windows のコマンド シェルで同じように動作します。

## <a name="1-set-up-your-local-development-environment"></a>1:ローカルの開発環境を設定する

「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」の手順をすべて実行します (まだ済んでいない場合)。

必ず、ローカル開発用のサービス プリンシパルを作成し、このプロジェクト用の仮想環境を作成してアクティブ化してください。

## <a name="2-install-the-needed-azure-library-packages"></a>2:必要な Azure ライブラリ パッケージをインストールする

*requirements.txt* という名前のファイルを作成し、内容を次のようにします。

```text
azure-mgmt-resource==10.2.0
azure-mgmt-web
azure-cli-core
```

azure-mgmt-resource の特定のバージョン要件により、azure-mgmt-web の現在のバージョンと互換性のあるバージョンを使用する必要があります。 これらのバージョンは、azure.core に基づいていないため、以前の認証方法が使用されます。

仮想環境をアクティブ化し、ターミナルまたはコマンド プロンプトで要件をインストールします。

```cmd
pip install -r requirements.txt
```

## <a name="3-fork-the-sample-repository"></a>3:サンプル リポジトリをフォークする

[https://github.com/Azure-Samples/python-docs-hello-world](https://github.com/Azure-Samples/python-docs-hello-world) にアクセスし、ご自身の GitHub アカウントにリポジトリをフォークします。 フォークを使用して、リポジトリを Azure にデプロイするためのアクセス許可があることを確認します。

![GitHub 上のサンプル リポジトリをフォークする](media/azure-sdk-example-web-app/fork-github-repository.png)

次に、フォークの URL を使用して、`REPO_URL` という名前の環境変数を作成します。 次のセクションのコード例は、この環境変数に依存しています。

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
set REPO_URL=<url_of_your_fork>
```

# <a name="bash"></a>[bash](#tab/bash)

```bash
REPO_URL=<url_of_your_fork>
```

---

## <a name="4-write-code-to-provision-and-deploy-a-web-app"></a>4:Web アプリのプロビジョニングとデプロイを行うコードを作成する

次のコードを使用して、 *provision_deploy_webapp.py* という名前の Python ファイルを作成します。 詳細はコメントで説明しています。

```python
import random, os
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.web import WebSiteManagementClient

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = 'PythonAzureExample-WebApp-rg'
LOCATION = "centralus"

# Step 1: Provision the resource group.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


#Step 2: Provision the App Service plan, which defines the underlying VM for the web app.

# Names for the App Service plan and App Service. We use a random number with the
# latter to create a reasonably unique name. If you've already provisioned a
# web app and need to re-run the script, set the WEB_APP_NAME environment 
# variable to that name instead.
SERVICE_PLAN_NAME = 'PythonAzureExample-WebApp-plan'
WEB_APP_NAME = os.environ.get("WEB_APP_NAME", f"PythonAzureExample-WebApp-{random.randint(1,100000):05}")

# Obtain the client object
app_service_client = get_client_from_cli_profile(WebSiteManagementClient)

# Provision the plan; Linux is the default
poller = app_service_client.app_service_plans.create_or_update(RESOURCE_GROUP_NAME,
    SERVICE_PLAN_NAME,
    {
        "location": LOCATION,
        "reserved": True,
        "sku" : {"name" : "B1"}
    }
)

plan_result = poller.result()

print(f"Provisioned App Service plan {plan_result.name}")


# Step 3: With the plan in place, provision the web app itself, which is the process that can host
# whatever code we want to deploy to it.

poller = app_service_client.web_apps.create_or_update(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": LOCATION,
        "server_farm_id": plan_result.id,
        "site_config": {
            "linux_fx_version": "python|3.8"
        }
    }
)

web_app_result = poller.result()

print(f"Provisioned web app {web_app_result.name} at {web_app_result.default_host_name}")

# Step 4: deploy code from a GitHub repository. For Python code, App Service on Linux runs
# the code inside a container that makes certain assumptions about the structure of the code.
# For more information, see How to configure Python apps,
# https://docs.microsoft.com/azure/app-service/configure-language-python.
#
# The create_or_update_source_control method doesn't provision a web app. It only sets the
# source control configuration for the app. In this case we're simply pointing to
# a GitHub repository.
#
# You can call this method again to change the repo.

REPO_URL = os.environ[REPO_URL]

poller = app_service_client.web_apps.create_or_update_source_control(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": "GitHub",
        "repo_url": REPO_URL,
        "branch": "master"
    }
)

sc_result = poller.result()

print(f"Set source control on web app to {sc_result.branch} branch of {sc_result.repo_url}")
```

このコードは、Azure CLI で直接認証を行う場合の操作をデモンストレーションしているため、CLI ベースの認証方法 (`get_client_from_cli_profile`) を使用しています。 どちらの場合も、同じ ID を認証に使用することになります。

そのようなコードを本番スクリプトで使用する場合は、代わりに `DefaultAzureCredential` (推奨) またはサービス プリンシパル ベースの方法を使用してください (「[Azure サービスを使用して Python アプリを認証する方法](azure-sdk-authenticate.md)」を参照)。

### <a name="reference-links-for-classes-used-in-the-code"></a>コードで使用されているクラスの参照リンク

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)
- [WebSiteManagementClient (azure.mgmt.web import)](/python/api/azure-mgmt-web/azure.mgmt.web.websitemanagementclient)

## <a name="5-run-the-script"></a>5:スクリプトを実行する

```cmd
python provision_deploy_web_app.py
```

## <a name="6-verify-the-web-app-deployment"></a>6: Web アプリのデプロイを確認する

1. 次のコマンドを実行して、デプロイ済みの Web サイトにアクセスします。

    ```azurecli
    az webapp browse -n PythonAzureExample-WebApp-12345
    ```

    "PythonAzureExample-WebApp-12345" は、実際の Web アプリの名前に置き換えてください。

    "Hello World!" が ブラウザーに表示されます。

1. [Azure portal](https://portal.azure.com) にアクセスし、 **[リソース グループ]** を選択して、一覧に "PythonAzureExample-WebApp-rg" が表示されていることを確認します。 次に、そのリストに移動して、適切なリソース、つまり App Service プランと App Service が存在することを確認します。

## <a name="7-clean-up-resources"></a>7:リソースをクリーンアップする

```azurecli
az group delete -n PythonAzureExample-WebApp-rg --no-wait
```

この例でプロビジョニングしたリソースを残す必要がなければ、今後サブスクリプションに課金されないようにするために、このコマンドを実行してください。

コードから [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) メソッドを使用してリソース グループを削除することもできます。

### <a name="for-reference-equivalent-azure-cli-commands"></a>参考: 同等の Azure CLI コマンド

次の Azure CLI コマンドは、Python スクリプトと同じプロビジョニング手順を実行するものです。

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az group create -l centralus -n PythonAzureExample-WebApp-rg

az appservice plan create -n PythonAzureExample-WebApp-plan --is-linux --sku F1

az webapp create -g PythonAzureExample-WebApp-rg -n PythonAzureExample-WebApp-12345 ^
    --plan PythonAzureExample-WebApp-plan --runtime "python|3.8"

rem You can use --deployment-source-url with the first create command. It's shown here
rem to match the sequence of the Python code.

az webapp create -n PythonAzureExample-WebApp-12345 --plan PythonAzureExample-WebApp-plan ^
    --deployment-source-url %REPO_URL% --runtime "python|3.8"

rem Replace <your_fork> with the specific URL of your forked repository.
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az group create -l centralus -n PythonAzureExample-WebApp-rg

az appservice plan create -n PythonAzureExample-WebApp-plan --is-linux --sku F1

az webapp create -g PythonAzureExample-WebApp-rg -n PythonAzureExample-WebApp-12345 \
    --plan PythonAzureExample-WebApp-plan --runtime "python|3.8"

# You can use --deployment-source-url with the first create command. It's shown here
# to match the sequence of the Python code.

az webapp create -n PythonAzureExample-WebApp-12345 --plan PythonAzureExample-WebApp-plan \
    --deployment-source-url https://github.com/<your_fork>/python-docs-hello-world

# Replace <your_fork> with the specific URL of your forked repository.
```

---

## <a name="see-also"></a>関連項目

- [例:リソース グループをプロビジョニングする](azure-sdk-example-resource-group.md)
- [例:サブスクリプション内のリソース グループを一覧表示する](azure-sdk-example-list-resource-groups.md)
- [例:Azure Storage をプロビジョニングする](azure-sdk-example-storage.md)
- [例:Azure Storage を使用する](azure-sdk-example-storage-use.md)
- [例:MySQL データベースをプロビジョニングして使用する](azure-sdk-example-database.md)
- [例:仮想マシンをプロビジョニングする](azure-sdk-example-virtual-machines.md)
