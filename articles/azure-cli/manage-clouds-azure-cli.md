---
title: Azure CLI でクラウドを選択する
description: Azure CLI で複数のクラウドの作成、サインイン、管理を行います。
author: dbradish-microsoft
manager: barbkess
ms.author: dbradish
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 8e24a4740d97ddf67f81e60fef9217a4e72daab0
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031224"
---
# <a name="select-clouds-with-the-azure-cli"></a>Azure CLI でクラウドを選択する

異なるリージョンにわたって作業したり、[Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/) を使用したりする場合、複数のクラウドの使用が必要になることがあります。 Microsoft はお客様が使用可能な、リージョンの法規に従ったクラウドを提供します。 この記事では、クラウドに関する情報の取得、現行クラウドの変更、新規クラウドの登録または登録解除の方法について説明します。

## <a name="list-available-clouds"></a>使用可能なクラウドを一覧表示する

使用可能なクラウドを [az cloud list](/cli/azure/cloud#az-cloud-list) コマンドで一覧表示することができます。 このコマンドにより、現在アクティブなクラウド、その現在のプロファイル、およびリージョン サフィックスとホスト名に関する情報が得られます。

アクティブなクラウドと、すべての使用可能クラウドの一覧を取得するには、次のように入力します。

```azurecli-interactive
az cloud list --output table
```

```output
IsActive    Name               Profile
----------  -----------------  ---------
True        AzureCloud         latest
            AzureChinaCloud    latest
            AzureUSGovernment  latest
            AzureGermanCloud   latest
```

現在アクティブなクラウドには `IsActive` 列に `True` と表示されます。 一度にアクティブにできるクラウドは 1 つのみです。 Azure サービスに使用するエンドポイントなど、クラウドに関するより詳細な情報を得るには、`cloud show` コマンドを使用します。

```azurecli-interactive
az cloud show --name AzureChinaCloud --output json
```

```json
{
  "endpoints": {
    "activeDirectory": "https://login.chinacloudapi.cn",
    "activeDirectoryDataLakeResourceId": null,
    "activeDirectoryGraphResourceId": "https://graph.chinacloudapi.cn/",
    "activeDirectoryResourceId": "https://management.core.chinacloudapi.cn/",
    "batchResourceId": "https://batch.chinacloudapi.cn/",
    "gallery": "https://gallery.chinacloudapi.cn/",
    "management": "https://management.core.chinacloudapi.cn/",
    "resourceManager": "https://management.chinacloudapi.cn",
    "sqlManagement": "https://management.core.chinacloudapi.cn:8443/",
    "vmImageAliasDoc": "https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json"
  },
  "isActive": false,
  "name": "AzureChinaCloud",
  "profile": "latest",
  "suffixes": {
    "azureDatalakeAnalyticsCatalogAndJobEndpoint": null,
    "azureDatalakeStoreFileSystemEndpoint": null,
    "keyvaultDns": ".vault.azure.cn",
    "sqlServerHostname": ".database.chinacloudapi.cn",
    "storageEndpoint": "core.chinacloudapi.cn"
  }
}
```

## <a name="switch-the-active-cloud"></a>アクティブなクラウドを切り替える

構成ファイルを使用して既定のクラウドを設定する方法については、「[CLI 構成値と環境変数](/cli/azure/azure-cli-configuration?view=azure-cli-latest#cli-configuration-values-and-environment-variables)」を参照してください。  アクティブなクラウドを切り替えるには、[az cloud set](/cli/azure/cloud#az-cloud-set) コマンドを実行します。 このコマンドは、1 つの必須の引数として、クラウドの名前を受け取ります。

```azurecli-interactive
az cloud set --name AzureChinaCloud
```

> [!IMPORTANT]
> アクティブ化されたクラウドの認証が期限切れになった場合は、他の CLI タスクを実行する前に、再認証を行う必要があります。 新しいクラウドに初めて切り替える場合は、アクティブなサブスクリプションの設定も行う必要があります。
> 認証の手順については、「[Azure CLI を使用してサインインする](authenticate-azure-cli.md)」を参照してください。 サブスクリプションの管理については、[Azure CLI での Azure サブスクリプションの管理](manage-azure-subscriptions-azure-cli.md)に関するページをご覧ください

## <a name="register-a-new-cloud"></a>新しいクラウドを登録する

Azure Stack 用の独自のエンドポイントがある場合は、新しいクラウドを登録します。 [az cloud register](/cli/azure/cloud#az-cloud-register) コマンドを実行すると、クラウドが作成されます。 このコマンドには、名前と一連のサービス エンドポイントが必要です。 Azure Stack で使用するクラウドの登録方法については、[Azure Stack での Azure CLI による API バージョンのプロファイルの使用](/azure/azure-stack/user/azure-stack-version-profiles-azurecli2#connect-to-azure-stack)に関するページをご覧ください。

中国、米国政府、またはドイツのリージョンについては、情報を登録する必要はありません。 これらのクラウドは Microsoft が管理しているため、既定値で使用可能です。  使用可能なエンドポイント設定に関する詳細については、[`az cloud register` のドキュメント](/cli/azure/cloud#az-cloud-register)を参照してください。

クラウドを登録しても、自動的にそのクラウドに切り替わるわけではありません。 `az cloud set` コマンドを使用して、新規作成したクラウドを選択してください。

## <a name="update-an-existing-cloud"></a>既存のクラウドを更新する

権限がある場合は、既存のクラウドを更新することもできます。 クラウドを更新すると、別の Azure サービス プロファイルに切り替わるか、接続エンドポイントが変更されます。
クラウドの更新は [az cloud update](/cli/azure/cloud#az-cloud-update) コマンドを使用して実行します。このコマンドでは `az cloud register` と同じ引数を使用します。

## <a name="unregister-a-cloud"></a>クラウドを登録解除する

作成したクラウドが不要になった場合は、次のように、[az cloud unregister](/cli/azure/cloud#az-cloud-unregister) コマンドで登録解除することができます。

```azurecli-interactive
az cloud unregister --name MyCloud
```
