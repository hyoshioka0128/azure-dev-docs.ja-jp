---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 09/15/2020
ms.author: tarcher
ms.openlocfilehash: 885764615beed7a623db03499b4ff6a6ab705ba7
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831174"
---
#### <a name="ansible"></a>[Ansible](#tab/ansible)

1. `delete_rg.yml` として次のコードを保存します。

    ```yml
    ---
    - hosts: localhost
      tasks:
        - name: Deleting resource group - "{{ name }}"
          azure_rm_resourcegroup:
            name: "{{ name }}"
            state: absent
          register: rg
        - debug:
            var: rg
    ```

1. [ansible-playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html) コマンドを使用してプレイブックを実行します。 プレースホルダーは、削除するリソース グループの名前に置き換えます。 リソース グループ内のすべてのリソースが削除されます。

    ```bash
    ansible-playbook delete_rg.yml --extra-vars "name=<resource_group>"
    ```

    **注**:

    - プレイブックの `register` 変数と `debug` セクションにより、コマンドの完了時に結果が表示されます。
    
#### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [az group delete](/cli/azure/group#az_group_delete) を実行して、リソース グループを削除します。 リソース グループ内のすべてのリソースが削除されます。

    ```azurecli
    az group delete --name <resource_group>
    ```

1. [az group show](/cli/azure/group#az_group_show) を使用して、リソース グループが削除されたことを確認します。

    ```azurecli
    az group show --name <resource_group>
    ```

#### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

1. [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup) を実行して、リソース グループを削除します。 リソース グループ内のすべてのリソースが削除されます。

    ```azurepowershell
    Remove-AzResourceGroup -Name <resource_group>
    ```

1. [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup) を使用して、リソース グループが削除されたことを確認します。

    ```azurepowershell
    Get-AzResourceGroup -Name <resource_group>
    ```

---