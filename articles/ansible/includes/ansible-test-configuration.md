---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 09/15/2020
ms.author: tarcher
ms.openlocfilehash: 5351445b63618d072ecf4cf8beeffc9a071bd84d
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2020
ms.locfileid: "90682036"
---
このセクションでは、新しい Ansible 構成内でテスト リソース グループを作成する方法について説明します。 これを行う必要がない場合、このセクションはスキップしてかまいません。

### <a name="create-an-azure-resource-group"></a>Azure リソース グループの作成

1. `create_rg.yml` として次のコードを保存します。

    ```yaml
    ---
    - hosts: localhost
      connection: local
      tasks:
        - name: Creating resource group - "{{ name }}"
          azure_rm_resourcegroup:
            name: "{{ name }}"
            location: "{{ location }}"
          register: rg
        - debug:
            var: rg
    ```

1. [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html) を使用してプレイブックを実行します。 プレースホルダーは、削除するリソース グループの名前と場所に置き換えます。

    ```bash
    ansible-playbook create_rg.yml --extra-vars "name=<resource_group_name> location=<resource_group_location>"
    ```

    **注**:

    - プレイブックの `register` 変数と `debug` セクションにより、コマンドの完了時に結果が表示されます。

### <a name="delete-an-azure-resource-group"></a>Azure リソース グループの削除

[!INCLUDE [ansible-delete-resource-group.md](ansible-delete-resource-group.md)]
