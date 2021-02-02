---
title: Python 用 Azure ライブラリを使用した Azure Managed Disks の使用
description: Azure SDK を使用して、マネージド ディスクの作成、サイズ変更、更新を行います。
ms.topic: conceptual
ms.date: 11/18/2020
ms.custom: devx-track-python
ms.openlocfilehash: b8d45f3d4b5ccd2c8a1c2850d496b9f68625ef46
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759323"
---
# <a name="use-azure-managed-disks-with-the-azure-libraries-sdk-for-python"></a>Python 用 Azure ライブラリ (SDK) を使用した Azure Managed Disks の使用

Azure Managed Disks は、ストレージ アカウントを直接操作しなくても、ディスク管理の簡素化、スケーラビリティの向上、セキュリティの強化、スケーリングの向上を実現します。

マネージド ディスクを管理するには、[`azure-mgmt-compute`](/python/api/overview/azure/virtualmachines) ライブラリを使用します。 (`azure-mgmt-compute` ライブラリを使用した仮想マシンのプロビジョニングの例については、「[例 - 仮想マシンのプロビジョニング](azure-sdk-example-virtual-machines.md)」をご覧ください。)

## <a name="standalone-managed-disks"></a>スタンドアロンの管理ディスク

スタンドアロンのマネージド ディスクは、次のセクションに示すように、さまざまな方法で作成できます。

### <a name="create-an-empty-managed-disk"></a>空のマネージド ディスクを作成する

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = poller.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a>Blob Storage からマネージド ディスクを作成する

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = poller.result()
```

### <a name="create-a-managed-disk-image-from-blob-storage"></a>Blob Storage からマネージド ディスク イメージを作成する

```python
from azure.mgmt.compute.models import DiskCreateOption

poller = compute_client.images.begin_create_or_update(
    'my_resource_group',
    'my_image_name',
    {
        'location': 'eastus',
        'storage_profile': {
           'os_disk': {
              'os_type': 'Linux',
              'os_state': "Generalized",
              'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
              'caching': "ReadWrite",
           }
        }
    }
)
image_resource = poller.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a>独自のイメージからマネージド ディスクを作成する

```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')

poller = compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = poller.result()
```

## <a name="virtual-machine-with-managed-disks"></a>Managed Disks を使用した仮想マシン

特定のディスク イメージの暗黙的なマネージド ディスクを使用して仮想マシンを作成できるため、ユーザーがすべての詳細を指定する必要がなくなります。

管理ディスクは、Azure の OS イメージから VM を作成するときに暗黙的に作成されます。 `storage_profile` パラメーターでは、`os_disk` は省略可能になっており、仮想マシン作成の必須の前提条件としてストレージ アカウントを作成する必要はありません。

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
```

Python 用 Azure 管理ライブラリを使用して仮想マシンを作成する方法の例全体については、「[例 - 仮想マシンのプロビジョニング](azure-sdk-example-virtual-machines.md)」を参照してください。

また、独自のイメージを使用して `storage_profile` を作成することもできます。

```python
# If you don't know the id, do a 'get' like this to obtain it
image = compute_client.images.get(self.group_name, 'myImageDisk')

storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        id = image.id
    )
)
```

以前にプロビジョニングしたマネージド ディスクを簡単にアタッチできます。

```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')

vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOptionTypes.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})

async_update = compute_client.virtual_machines.begin_create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>Managed Disks を使用した仮想マシン スケール セット

管理ディスクを使用する前は、スケール セットに含めるすべての VM に対してストレージ アカウントを手動で作成し、その後、リスト パラメーター `vhd_containers` を使用してすべてのストレージ アカウント名をスケール セットの RestAPI に渡す必要がありました。 (マイグレーション ガイドについては、「[スケール セット テンプレートをマネージド ディスク スケール セット テンプレートに変換する](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md)」を参照してください。)

ストレージ アカウントを Azure Managed Disks で管理する必要がないため、`storage_profile` は VM の作成に使用されたものとまったく同じにすることができます。

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

サンプル全体は次のとおりです。

```python
naming_infix = "PyTestInfix"

vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    }
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.begin_create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
```

## <a name="other-operations-with-managed-disks"></a>Managed Disks を使用したその他の操作

### <a name="resizing-a-managed-disk"></a>マネージド ディスクのサイズ変更を行う

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25

async_update = self.compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>マネージド ディスクのストレージ アカウントの種類を更新する

```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs

async_update = self.compute_client.disks.begin_create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-blob-storage"></a>BLOB ストレージからイメージを作成する。

```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>現在仮想マシンにアタッチされているマネージド ディスクのスナップショットを作成する

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')

async_snapshot_creation = self.compute_client.snapshots.begin_create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```

## <a name="see-also"></a>関連項目

- [例:仮想マシンをプロビジョニングする](azure-sdk-example-virtual-machines.md)
- [例:リソース グループをプロビジョニングする](azure-sdk-example-resource-group.md)
- [例:サブスクリプション内のリソース グループを一覧表示する](azure-sdk-example-list-resource-groups.md)
- [例:Azure Storage をプロビジョニングする](azure-sdk-example-storage.md)
- [例:Azure Storage を使用する](azure-sdk-example-storage-use.md)
- [例:MySQL データベースをプロビジョニングして使用する](azure-sdk-example-database.md)
- [Azure SDK for Python に関する簡単なアンケートを完了する](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
