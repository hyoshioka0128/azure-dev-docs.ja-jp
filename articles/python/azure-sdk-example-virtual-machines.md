---
title: Python 用 Azure SDK ライブラリを使用して仮想マシンをプロビジョニングする
description: Python と Azure SDK の管理ライブラリを使用して Azure 仮想マシンをプロビジョニングする方法。
ms.date: 05/29/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: a6595b60a70ab3b6ee36bf95b4557cb37c496188
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87983494"
---
# <a name="example-use-the-azure-libraries-to-provision-a-virtual-machine"></a>例:Azure ライブラリを使用して仮想マシンをプロビジョニングする

この例では、Python スクリプトで Azure SDK の管理ライブラリを使用し、Linux 仮想マシンを含んだリソース グループを作成する方法について説明します。 ([同等の Azure CLI コマンド](#for-reference-equivalent-azure-cli-commands)については、この記事の後半で説明します。)

特に記載のない限り、この記事で使用されているコマンドはいずれも、Linux と Mac OS の bash および Windows のコマンド シェルで同じように動作します。

> [!NOTE]
> コードによる仮想マシンのプロビジョニングは、仮想マシンに必要なその他多くのリソースのプロビジョニングを伴う複数のステップから成るプロセスです。 このようなコードをコマンド ラインから実行するだけの場合は、[`az vm create`](/cli/azure/vm?view=azure-cli-latest#az-vm-create) コマンドを使用する方がはるかに簡単です。これにより、これらのセカンダリ リソースは、省略することを選択した設定には既定値を使用して、自動的にプロビジョニングされます。 必須の引数は、リソース グループ、VM 名、イメージ名、ログイン資格情報だけです。 詳細については、「[Azure CLI を使用した仮想マシンの簡易作成](/azure/virtual-machines/scripts/virtual-machines-windows-cli-sample-create-vm-quick-create)」を参照してください。

## <a name="1-set-up-your-local-development-environment"></a>1:ローカルの開発環境を設定する

「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」の手順をすべて実行します (まだ済んでいない場合)。

必ず、ローカル開発用のサービス プリンシパルを作成し、このプロジェクト用の仮想環境を作成してアクティブ化してください。

## <a name="2-install-the-needed-azure-library-packages"></a>2:必要な Azure ライブラリ パッケージをインストールする

1. この例で使用する管理ライブラリを列挙した *requirements.txt* ファイルを作成します。

    ```txt
    azure-mgmt-resource
    azure-mgmt-network
    azure-mgmt-compute
    azure-cli-core
    ```

1. 仮想環境がアクティブになっているターミナルまたはコマンド プロンプトで、*requirements.txt* にリストされている管理ライブラリをインストールします。

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-a-virtual-machine"></a>3:仮想マシンをプロビジョニングするコードを作成する

次のコードを使用して、*provision_vm.py* という名前の Python ファイルを作成します。 詳細はコメントで説明しています。

```python
# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient

print(f"Provisioning a virtual machine...some operations might take a minute or two.")

# Step 1: Provision a resource group

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonAzureExample-VM-rg"
LOCATION = "centralus"

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    {
        "location": LOCATION
    }
)


print(f"Provisioned resource group {rg_result.name} in the {rg_result.location} region")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group

# Step 2: provision a virtual network

# A virtual machine requires a network interface client (NIC). A NIC requires
# a virtual network and subnet along with an IP address. Therefore we must provision
# these downstream components first, then provision the NIC, after which we
# can provision the VM.

# Network and IP address names
VNET_NAME = "python-example-vnet"
SUBNET_NAME = "python-example-subnet"
IP_NAME = "python-example-ip"
IP_CONFIG_NAME = "python-example-ip-config"
NIC_NAME = "python-example-nic"

# Obtain the management object for networks
network_client = get_client_from_cli_profile(NetworkManagementClient)

# Provision the virtual network and wait for completion
poller = network_client.virtual_networks.create_or_update(RESOURCE_GROUP_NAME,
    VNET_NAME,
    {
        "location": LOCATION,
        "address_space": {
            "address_prefixes": ["10.0.0.0/16"]
        }
    }
)

vnet_result = poller.result()

print(f"Provisioned virtual network {vnet_result.name} with address prefixes {vnet_result.address_space.address_prefixes}")

# Step 3: Provision the subnet and wait for completion
poller = network_client.subnets.create_or_update(RESOURCE_GROUP_NAME, 
    VNET_NAME, SUBNET_NAME,
    { "address_prefix": "10.0.0.0/24" }
)
subnet_result = poller.result()

print(f"Provisioned virtual subnet {subnet_result.name} with address prefix {subnet_result.address_prefix}")

# Step 4: Provision an IP address and wait for completion
poller = network_client.public_ip_addresses.create_or_update(RESOURCE_GROUP_NAME,
    IP_NAME,
    {
        "location": LOCATION,
        "sku": { "name": "Standard" },
        "public_ip_allocation_method": "Static",
        "public_ip_address_version" : "IPV4"
    }
)

ip_address_result = poller.result()

print(f"Provisioned public IP address {ip_address_result.name} with address {ip_address_result.ip_address}")

# Step 5: Provision the network interface client
poller = network_client.network_interfaces.create_or_update(RESOURCE_GROUP_NAME,
    NIC_NAME, 
    {
        "location": LOCATION,
        "ip_configurations": [ {
            "name": IP_CONFIG_NAME,
            "subnet": { "id": subnet_result.id },
            "public_ip_address": {"id": ip_address_result.id }
        }]
    }
)

nic_result = poller.result()

print(f"Provisioned network interface client {nic_result.name}")

# Step 6: Provision the virtual machine

# Obtain the management object for virtual machines
compute_client = get_client_from_cli_profile(ComputeManagementClient)

VM_NAME = "ExampleVM"
USERNAME = "azureuser"
PASSWORD = "ChangePa$$w0rd24"

print(f"Provisioning virtual machine {VM_NAME}; this operation might take a few minutes.")

# Provision the VM specifying only minimal arguments, which defaults to an Ubuntu 18.04 VM
# on a Standard DS1 v2 plan with a public IP address and a default virtual network/subnet.

poller = compute_client.virtual_machines.create_or_update(RESOURCE_GROUP_NAME, VM_NAME,
    {
        "location": LOCATION,
        "storage_profile": {
            "image_reference": {
                "publisher": 'Canonical',
                "offer": "UbuntuServer",
                "sku": "16.04.0-LTS",
                "version": "latest"
            }
        },
        "hardware_profile": {
            "vm_size": "Standard_DS1_v2"
        },
        "os_profile": {
            "computer_name": VM_NAME,
            "admin_username": USERNAME,
            "admin_password": PASSWORD
        },
        "network_profile": {
            "network_interfaces": [{
                "id": nic_result.id,
            }]
        }
    }
)

vm_result = poller.result()

print(f"Provisioned virtual machine {vm_result.name}")
```

このコードは、Azure CLI で直接認証を行う場合の操作をデモンストレーションしているため、CLI ベースの認証方法 (`get_client_from_cli_profile`) を使用しています。 どちらの場合も、同じ ID を認証に使用することになります。

VM の管理を自動化するなど、そのようなコードを本番スクリプトで使用する場合は、`DefaultAzureCredential` (推奨) またはサービス プリンシパル ベースの方法を使用してください (「[Azure サービスを使用して Python アプリを認証する方法](azure-sdk-authenticate.md)」を参照)。

### <a name="reference-links-for-classes-used-in-the-code"></a>コードで使用されているクラスの参照リンク

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient?view=azure-python)
- [NetworkManagementClient (azure.mgmt.network)](/python/api/azure-mgmt-network/azure.mgmt.network.networkmanagementclient?view=azure-python)
- [ComputeManagementClient (azure.mgmt.compute)](/python/api/azure-mgmt-compute/azure.mgmt.compute.computemanagementclient?view=azure-python)

## <a name="4-run-the-script"></a>4.スクリプトを実行する

```cmd
python provision_vm.py
```

プロビジョニング プロセスが完了するまでに数分かかります。

## <a name="5-verify-the-resources"></a>5.リソースを確認する

[Azure portal](https://portal.azure.com) を開き、"PythonAzureExample-VM-rg" リソース グループに移動して、仮想マシン、仮想ディスク、ネットワーク セキュリティ グループ、パブリック IP アドレス、ネットワーク インターフェイス、仮想ネットワークを書き留めます。

![新しいリソース グループの Azure portal ページに表示された仮想マシンおよび関連するリソース](media/azure-sdk-example-virtual-machines/portal-vm-resources.png)

### <a name="for-reference-equivalent-azure-cli-commands"></a>参考: 同等の Azure CLI コマンド

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
rem Provision the resource group

az group create -n PythonAzureExample-VM-rg -l centralus

rem Provision a virtual network and subnet

az network vnet create -g PythonAzureExample-VM-rg -n python-example-vnet ^
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet ^
    --subnet-prefix 10.0.0.0/24

rem Provision a public IP address

az network public-ip create -g PythonAzureExample-VM-rg -n python-example-ip ^
    --allocation-method Dynamic --version IPv4

rem Provision a network interface client

az network nic create -g PythonAzureExample-VM-rg --vnet-name python-example-vnet ^
    --subnet python-example-subnet -n python-example-nic ^
    --public-ip-address python-example-ip

rem Provision the virtual machine

az vm create -g PythonAzureExample-VM-rg -n ExampleVM -l "centralus" ^
    --nics python-example-nic --image UbuntuLTS ^
    --admin-username azureuser --admin-password ChangePa$$w0rd24
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonAzureExample-VM-rg -l centralus

# Provision a virtual network and subnet

az network vnet create -g PythonAzureExample-VM-rg -n python-example-vnet \
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet \
    --subnet-prefix 10.0.0.0/24

# Provision a public IP address

az network public-ip create -g PythonAzureExample-VM-rg -n python-example-ip \
    --allocation-method Dynamic --version IPv4

# Provision a network interface client

az network nic create -g PythonAzureExample-VM-rg --vnet-name python-example-vnet \
    --subnet python-example-subnet -n python-example-nic \
    --public-ip-address python-example-ip

# Provision the virtual machine

az vm create -g PythonAzureExample-VM-rg -n ExampleVM -l "centralus" \
    --nics python-example-nic --image UbuntuLTS \
    --admin-username azureuser --admin-password ChangePa$$w0rd24

```

---

## <a name="6-clean-up-resources"></a>6: リソースをクリーンアップする

```azurecli
az group delete -n PythonAzureExample-VM-rg
```

この例で作成したリソースを残す必要がなければ、今後サブスクリプションに課金されないようにするために、このコマンドを実行してください。

## <a name="see-also"></a>関連項目

- [例:リソース グループをプロビジョニングする](azure-sdk-example-resource-group.md)
- [例:Azure Storage をプロビジョニングする](azure-sdk-example-storage.md)
- [例:Azure Storage を使用する](azure-sdk-example-storage-use.md)
- [例:Web アプリをプロビジョニングしてコードをデプロイする](azure-sdk-example-web-app.md)
- [例:データベースをプロビジョニングしてクエリを実行する](azure-sdk-example-database.md)

次のリソースには、Python を使用して仮想マシンを作成する例が、より広範囲にわたって紹介されています。

- [Python を使用して Azure で Windows VM を作成および管理する](/azure/virtual-machines/windows/python) この例を使用すると、`storage_profile` パラメーターを変更することによって Linux VM を作成できます。
- [Azure Virtual Machines 管理のサンプル - Python](https://github.com/Azure-Samples/virtual-machines-python-manage) (GitHub)。 このサンプルでは、その他の管理操作 (VM の起動と再起動、VM の停止と削除、ディスク サイズの増加、データ ディスクの管理など) を紹介しています。
