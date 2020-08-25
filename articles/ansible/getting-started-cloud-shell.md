---
title: クイック スタート - Azure Cloud Shell を使用して Ansible を構成する
description: このクイックスタートでは、Azure Cloud Shell で Bash を使用してさまざまな Ansible タスクを実行する方法について説明します
keywords: Ansible, Azure, DevOps, Bash, Cloud Shell, プレイブック, Bash
ms.topic: quickstart
ms.date: 08/13/2020
ms.custom: devx-track-ansible
ms.openlocfilehash: fa118213624db21192617b65930041b8712075bd
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "88240314"
---
# <a name="quickstart-configure-ansible-using-azure-cloud-shell"></a>クイック スタート:Azure Cloud Shell を使用した Ansible の構成

[!INCLUDE [annsible-intro.md](includes/ansible-intro.md)]

この記事では、[Azure Cloud Shell](/azure/cloud-shell/overview) 環境からの Ansible の使用を開始する方法について説明します。

## <a name="configure-your-environment"></a>環境を構成する

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Azure Cloud Shell の構成** - Azure Cloud Shell を初めて使用する場合は、「[Azure Cloud Shell の Bash のクイック スタート](https://docs.microsoft.com/azure/cloud-shell/quickstart)」を参照してください。

[!INCLUDE [open-cloud-shell.md](../includes/open-cloud-shell.md)]

## <a name="automatic-credential-configuration"></a>資格情報の自動構成

Cloud Shell にサインインするとき、Ansible は Azure で認証され、追加構成なしでインフラストラクチャを管理できます。 

複数のサブスクリプションを操作する場合は、`AZURE_SUBSCRIPTION_ID` 環境変数をエクスポートして、Ansible で使用されるサブスクリプションを指定します。 

すべての Azure サブスクリプションを一覧表示するには、次のコマンドを実行します。

```azurecli-interactive
az account list
```

Azure サブスクリプション ID を使用して、次のように `AZURE_SUBSCRIPTION_ID` を設定します。

```console
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>構成を確認する
正しく構成されたことを確認するには、Ansible を使用して Azure リソース グループを作成します。

[!INCLUDE [create-resource-group-with-ansible.md](includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"] 
> [クイック スタート: Ansible を使用して Azure で仮想マシンを構成する](./vm-configure.md)
