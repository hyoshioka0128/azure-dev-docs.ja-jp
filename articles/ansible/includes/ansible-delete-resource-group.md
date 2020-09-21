---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 09/15/2020
ms.author: tarcher
ms.openlocfilehash: 4c4bb046496f80a7280aed429a8cd251e533a8db
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2020
ms.locfileid: "90681655"
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

1. [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) を実行して、リソース グループを削除します。 リソース グループ内のすべてのリソースが削除されます。

    ```azurecli
    az group delete --name <resource_group>
    ```

1. [az group show](https://docs.microsoft.com/cli/azure/group#az_group_show) を使用して、リソース グループが削除されたことを確認します。

    ```azurecli
    az group show --name <resource_group>
    ```

#### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

1. [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzResourceGroup) を実行して、リソース グループを削除します。 リソース グループ内のすべてのリソースが削除されます。

    ```azurepowershell
    Remove-AzResourceGroup -Name <resource_group>
    ```

1. [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Get-AzResourceGroup) を使用して、リソース グループが削除されたことを確認します。

    ```azurepowershell
    Get-AzResourceGroup -Name <resource_group>
    ```

---
