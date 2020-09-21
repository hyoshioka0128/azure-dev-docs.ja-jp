---
title: クイック スタート - Azure CLI を使用して Ansible を構成する
description: このクイックスタートでは、Ubuntu、CentOS、SLES で Azure リソースを管理するため、Ansible をインストールして構成する方法を説明します
keywords: Ansible, Azure, DevOps, Bash, CloudShell, プレイブック, Azure CLI
ms.topic: quickstart
ms.date: 08/13/2020
ms.custom: devx-track-ansible,devx-track-cli
ms.openlocfilehash: 50fbcb4d086679265d728f14061a5c4c649fa48d
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2020
ms.locfileid: "90682025"
---
# <a name="quickstart-configure-ansible-using-azure-cli"></a>クイック スタート:Azure CLI を使用した Ansible の構成

このクイック スタートでは、Azure CLI を使用して [Ansible](https://docs.ansible.com/) をインストールする方法について説明します。

このクイック スタートでは、以下のタスクを完了します。

> [!div class="checklist"]
> * SSH キー ペアの作成
> * リソース グループを作成する
> * CentOS 仮想マシンを作成する 
> * Ansible を仮想マシンにインストールする
> * SSH 経由で仮想マシンに接続する
> * 仮想マシンで Ansible を構成する

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-sp.md](../includes/open-source-devops-prereqs-create-service-principal.md)]
- **Linux または Linux 仮想マシンへのアクセス** - Linux マシンを所有していない場合は、[Linux 仮想マシン](/azure/virtual-network/quick-create-cli)を作成してください。

## <a name="create-an-ssh-key-pair"></a>SSH キー ペアの作成

Linux VM に接続する場合、パスワード認証またはキーベースの認証を使用できます。 キーベースの認証は、パスワードを使用するよりも安全です。 そのため、この記事ではキーベースの認証を使用します。

キーベースの認証では、次の 2 つのキーがあります。

- **公開キー**: 公開キーは、(この記事のように) VM 上などのホストに格納されます。
- **秘密キー**: 秘密キーを使用すると、ホストに安全に接続できます。 秘密キーは事実上パスワードであり、そのように保護する必要があります。
        
次の手順では、SSH キーの組を作成する方法について説明します。

1. [Azure portal](https://portal.azure.com) にサインインします。

1. [Azure Cloud Shell](/azure/cloud-shell/overview) を開き、**Bash**に切り替えます (まだ行っていない場合)。

1. [ssh-keygen](https://www.ssh.com/ssh/keygen/) を使用して SSH キーを作成します。

    ```bash
    ssh-keygen -m PEM -t rsa -b 2048 -C "azureuser@azure" -f ~/.ssh/ansible_rsa -N ""
    ```

    **注**:

    - `ssh-keygen` コマンドは、生成されたキー ファイルの場所を表示します。 このディレクトリ名は、仮想マシンを作成するときに必要になります。
    - 公開キーは `ansible_rsa.pub` に格納され、秘密キーは `ansible_rsa` に格納されます。

## <a name="create-a-virtual-machine"></a>仮想マシンの作成

1. [az group create](/cli/azure/group#az-group-create) を使用してリソース グループを作成します。 場合によっては、`--location` パラメーターをお使いの環境に適した値に置き換える必要があります。

    ```azurecli
    az group create --name QuickstartAnsible-rg --location eastus
    ```

1. [az vm create](/cli/azure/vm#az-vm-create) を使用して仮想マシンを作成します。

    ```azurecli
    az vm create \
    --resource-group QuickstartAnsible-rg \
    --name QuickstartAnsible-vm \
    --image OpenLogic:CentOS:7.7:latest \
    --admin-username azureuser \
    --ssh-key-values <ssh_public_key_filename>
    ```

1. [az vm list](/cli/azure/vm#az-vm-list) を使用して、新しい仮想マシンの作成 (および状態) を確認します。

    ```azurecli
    az vm list -d -o table --query "[?name=='QuickstartAnsible-vm']"
    ```

    **注**:

    - `az vm list` コマンドからの出力には、SSH 経由で仮想マシンに接続するために使用されるパブリック IP アドレスが含まれます。

## <a name="install-ansible-on-the-virtual-machine"></a>Ansible を仮想マシンにインストールする

[az vm extension set](/cli/azure/vm/extension?#az-vm-extension-set) を使用して Ansible インストール スクリプトを実行します。

```azurecli
az vm extension set \
 --resource-group QuickstartAnsible-rg \
 --vm-name QuickstartAnsible-vm \
 --name customScript \
 --publisher Microsoft.Azure.Extensions \
 --version 2.1 \
 --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-ansible-control-machine/master/configure-ansible-centos.sh"]}' \
 --protected-settings '{"commandToExecute": "./configure-ansible-centos.sh"}'
```

**注:**

- 完了すると、`az vm extension` コマンドにより、インストール スクリプトの実行結果が表示されます。

## <a name="connect-to-your-virtual-machine-via-ssh"></a>SSH 経由で仮想マシンに接続する

SSH コマンドを使用して仮想マシンに接続します。

```azurecli
ssh -i <ssh_private_key_filename> azureuser@<vm_ip_address>
```

## <a name="create-azure-credentials"></a>Azure 資格情報の作成

Ansible の資格情報を構成するには、次の情報が必要です。

* Azure サブスクリプション ID
* サービス プリンシパルの値

Ansible Tower または Jenkins を使っている場合は、サービス プリンシパルの値を環境変数として宣言します。

次のいずれかの方法を使って、Ansible の資格情報を構成します。

- [Ansible の資格情報ファイルの作成](#file-credentials)
- [Ansible 環境変数の使用](#env-credentials)

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Ansible の資格情報ファイルの作成

このセクションでは、Ansible に資格情報を提供するためのローカル資格情報ファイルを作成します。

Ansible の資格情報の定義について詳しくは、「[Providing Credentials to Azure Modules (Azure モジュールに資格情報を提供する)](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules)」をご覧ください。

1. 開発環境の場合は、ホストの仮想マシンで `credentials` という名前のファイルを作成します。

    ```bash
    mkdir ~/.azure
    vi ~/.azure/credentials
    ```

1. そのファイルに次の行を挿入します。 プレースホルダーをサービス プリンシパルの値に置き換えます。

    ```bash
    [default]
    subscription_id=<your-subscription_id>
    client_id=<security-principal-appid>
    secret=<security-principal-password>
    tenant=<security-principal-tenant>
    ```

1. ファイルを保存して閉じます。

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>Ansible 環境変数の使用

このセクションでは、Ansible の資格情報を構成するためにサービス プリンシパルの値をエクスポートします。

1. ターミナル ウィンドウを開きます。

1. サービス プリンシパルの値をエクスポートします。

    ```bash
    export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
    export AZURE_CLIENT_ID=<security-principal-appid>
    export AZURE_SECRET=<security-principal-password>
    export AZURE_TENANT=<security-principal-tenant>
    ```

## <a name="test-ansible-installation"></a>Ansible インストールをテストする

これで、仮想マシンに Ansible がインストールされて構成されました。

[!INCLUDE [ansible-test-configuration.md](includes/ansible-test-configuration.md)]

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure 上の Ansible](/azure/developer/Ansible)