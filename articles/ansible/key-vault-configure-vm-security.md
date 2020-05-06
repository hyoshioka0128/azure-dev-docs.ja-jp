---
title: チュートリアル - Ansible で Linux 仮想マシンと共に Azure Key Vault を使用する
description: Azure Key Vault を使って VM セキュリティを構成するために Ansible を使用する方法について説明します
keywords: ansible、azure、devops、キー コンテナー、セキュリティ、資格情報、シークレット、キー、証明書、azure 用 ansible モジュール、リソース グループ、azure_rm_resourcegroup、
ms.topic: tutorial
ms.date: 04/20/2020
ms.openlocfilehash: ce9adb7ea121425d410665e1f4cc225cfdb82bd8
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81755235"
---
# <a name="tutorial-use-azure-key-vault-with-a-linux-virtual-machine-in-ansible"></a>チュートリアル:Ansible で Linux 仮想マシンと共に Azure Key Vault を使用する

[!INCLUDE [ansible-29-note.md](includes/ansible-29-note.md)]

このチュートリアルでは、[Azure Key Vault](/azure/key-vault/general/overview) を使用して Azure モジュール用の Ansible コレクションを使用する方法について説明します。 Azure Key Vault を使用すると、アプリケーション シークレット、キー、証明書などの資格情報の保管を一元化できます。 資格情報とアプリケーション コードを分離することにより、システムの安全性が高まります。 また、自動有効期限を使用したローテーション付きの資格情報管理パターンを実装すると、管理がはるかに容易になります。

> [!div class="checklist"]
>
> * Azure CLI を使用して、Azure サブスクリプションとサービス プリンシパルの値を取得する
> * キー値を Linux 環境変数として保存する
> * Ansible プレイブックから Linux 環境変数を取得する
> * Key Vault を作成します
> * キー コンテナーのアクセス ポリシーを設定する
> * Azure portal を使用して、アクセス ポリシーをキー コンテナーに追加する
> * キー コンテナー シークレットを作成する
> * Ansible シェル モジュールを使用して、キー コンテナーのシークレットを取得する
> * 仮想マシンをそのすべての構成コンポーネントと共に作成する

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
[!INCLUDE [ansible-configure-azure-collection.md](includes/ansible-configure-azure-collection.md)]
    
## <a name="get-azure-subscription-info"></a>Azure サブスクリプション情報を取得する

Azure CLI を使用して、Azure 用の Ansible モジュールを使用するときに必要な Azure サブスクリプション情報を取得します。 

1. `az account show` コマンドを使用して、Azure サブスクリプション ID と Azure サブスクリプション テナント ID を取得します。 `<Subscription>` プレースホルダーには、Azure サブスクリプション名または Azure サブスクリプション ID を指定します。 このコマンドを実行すると、既定の Azure サブスクリプションに関連付けられている多くのキー値が表示されます。 複数のサブスクリプションがある場合は、[az account set](/cli/azure/account?view=azure-cli-latest#az-account-set) コマンドを使用して、現在のサブスクリプションを設定する必要がある場合があります。 コマンドの出力から、**ID** と **tenantID** の両方の値をメモします。

    ```azurecli
    az account show --subscription "<Subscription>" --query tenantId
    ```

1. Azure サブスクリプションのサービス プリンシパルがない場合、 [Azure CLI で Azure サービス プリンシパルを作成します](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)。 コマンドの出力から、**appId** の値をメモします。

1. `az ad sp show` コマンドを使用してサービス プリンシパルのオブジェクト ID を取得します。 `<ApplicationID>` プレースホルダーに、サービス プリンシパルの appId を指定します。 `--query` パラメーターでは、*stdout* に出力する値を指定します。 この場合、サービス プリンシパルのオブジェクト ID です。

    ```azurecli
    az ad sp show --id <ApplicationID> --query objectId
    ```
    
1. 次の値を環境変数として保存します。Azure サブスクリプション ID、Azure サブスクリプション テナント ID、およびサービス プリンシパル オブジェクト ID。 プレイブックの実行方法、サブスクリプションと資格情報の値の保存場所、およびそれらの値の取得方法は、ご使用の特定の環境に基づきます。 このデモの目的のために、Azure Cloud Shell を使用し、次の行をファイルの最後に追加することで、必要な Azure の値を `~/.bashrc` に保存しました。

    ```bash
    export AZ_SUBSCRIPTION_ID="<subscriptionID>"
    export AZ_TENANT_ID="<tenantID>"
    export AZ_OBJECT_ID="<objectID>"
    export AZ_CLIENT_ID="<appID>"
    ```

## <a name="declare-the-azure-collection"></a>Azure コレクションを宣言する

[最新の Azure コレクションをダウンロード](#prerequisites)した後、`collections` キーを介してその使用を指定します。

```yml
- hosts: all
  collections:
    - azure.azcollection
```

## <a name="create-azure-resource-group-for-the-key-vault"></a>キー コンテナー用の Azure リソース グループを作成する

次のプレイブックのスニペットにより、キー コンテナーの作成先となる、一意の名前を持つリソース グループを作成します。 

```yml
---
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 10000 | random }}"
      run_once: yes
      
- hosts: localhost
  vars:
    kv_rg: kv_rg_{{ rpfx }}
    kv_rg_loc: eastus

  tasks:
    - name: Set facts
      set_fact:
        az_sub_id: "{{ lookup('env', 'AZ_SUBSCRIPTION_ID') }}"

    - name: Create a resource group to hold the key vault
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ kv_rg }}"
        location: "{{ kv_rg_loc }}"

    - debug:
        msg: "New resource group = {{ kv_rg }}"
```

**注:**

- このデモでは、キー コンテナーはリソース グループ内の唯一のリソースとして作成します。 キー コンテナーは、それを使用するリソースから分離するのが一般的です。 このパターンによって、他のリソースを削除するときに、キー コンテナーが誤って削除されないようにすることができます。
- キー コンテナー名は Azure 内で一意である必要があるため、このデモでは、ランダムな "*ポストフィックス*" 値を作成します。 この値は、(次のセクションで作成される) キー コンテナー リソース グループとキー コンテナーの名前に付加されます。 タスク `Prepare random postfix` のコードによって、`rpfx` 変数に割り当てられるランダムなポストフィックス値が生成されます。
- タスク `Set facts` では、`lookup` コマンドを使用して、環境変数として保存されている Azure サブスクリプション ID を取得します。
- 新しいリソース グループを作成するために、[azure_rm_resourcegroup モジュール](https://docs.ansible.com/ansible/latest/modules/azure_rm_resourcegroup_module.html)を使用します。
- プレイブックの最後の `debug` タスクによって、新しいリソース グループの名前が表示されます。

## <a name="create-a-key-vault"></a>Key Vault を作成します

前のセクションで説明したように、キー コンテナーの名前は Azure 全体で一意である必要があります。 そのため、次のプレイブックのスニペットにより、リテラルの `kv` と `rpfx` 値を連結したものを `kv` 変数に代入します。

```yml
vars:
  kv: "kv{{ rpfx }}"
  kv_rg: "kv_rg_{{ rpfx }}"

tasks:
  - name: Set facts
    set_fact:
      az_object_id: "{{ lookup('env', 'AZ_OBJECT_ID') }}"
      az_tenant_id: "{{ lookup('env', 'AZ_TENANT_ID') }}"

  - name: Create a key vault
    azure_rm_keyvault:
      subscription_id: "{{ az_sub_id }}"
      resource_group: "{{ kv_rg }}"
      vault_name: "{{ kv }}"
      vault_tenant: "{{ az_tenant_id }}"
      enabled_for_deployment: yes
      sku:
        name: standard
        family: A
      access_policies:
        - object_id: "{{ az_object_id }}"
          tenant_id: "{{ az_tenant_id }}"
          secrets:
            - get
            - list
            - set
  - debug:
      msg: "New key vault name = {{ kv }} within the {{ kv_rg }} resource group"

```

**注:**

- キー コンテナーを作成するために、[azure_rm_keyvault モジュール](https://docs.ansible.com/ansible/latest/modules/azure_rm_keyvault_module.html)を使用します。
- キー コンテナーの一部としてアクセス ポリシーを作成するときに、オブジェクト ID とテナント ID が提供されました。 これらの値によって、(Azure サブスクリプションに関連付けられている) 特定のサービス プリンシパルのアクセス ポリシーが定義されます。 ただし、Azure portal にアクセスし、キー コンテナーのシークレットを表示しようとすると、エラー メッセージが表示されることがあります。 これらのメッセージは、"操作 "List" はこのキー コンテナーのアクセス ポリシーでは有効になっていません。" や、 "このコンテンツを表示する権限がありません。" のようなものです。 これらのメッセージを受信した場合、Active Directory ユーザーとしてアクセス権がないということになります。 次のセクションでは、自分用のアクセス ポリシーをキー コンテナーに追加する方法について説明します。
- プレイブックの最後の `debug` タスクによって、新しいキー コンテナーの名前が表示されます。

## <a name="add-yourself-to-key-vault-access-policy"></a>キー コンテナーのアクセス ポリシーに自分自身を追加する

キー コンテナーのシークレットを表示する場合、またはデモの手順に従って進めている場合は、Azure Active Directory ID のアクセス ポリシーを追加する必要があります。 次の手順では、ユーザーとして自分用のアクセス ポリシーをキー コンテナーに追加する方法について説明します。

1. [Azure ポータル](https://portal.azure.com)にアクセスします。

1. ページのメインの検索ボックスに「`key vaults`」と入力し、 **[サービス]** で **[キー コンテナー]** を選択します。

1. 前のセクションで作成したキー コンテナーを選択します。 (この名前はプレイブックから stdout に出力されました。)

1. **[アクセス ポリシー]** を選択します。

1. 1 つのアクセス ポリシーが、指定されたサービス プリンシパルを表す Azure Active Directory ID と共に表示されます。

1. **[アクセス ポリシーの追加]** を選択します。

1. **[プリンシパルの選択]** を選択します。

1. **[プリンシパル]** タブの検索ボックスに、お使いのメール アドレスを入力します。

1. フィルターを適用した一覧から、適切なエントリを選択します。

1. 選択したユーザーの情報が **[選択したメンバー]** 一覧にコピーされます。 **[選択]** を選択します。

1. **[キーのアクセス許可]** 、 **[シークレットのアクセス許可]** 、および **[証明書のアクセス許可]** で適切なオプションを選択します。 このデモでは、 **[シークレットのアクセス許可]** を選択し、その後 **[Get]\(取得\)** 、 **[List]\(一覧\)** 、 **[Set]\(設定\)** を選択するだけで十分です。

1. **[追加]** を選択します。

1. 選択したユーザーの新しいアクセス ポリシーが **[アクセス ポリシー]** ページに表示されるようになります。

1. **[保存]** を選択します。

1. ポータルの右上隅にある **[通知]** を選択します。 アクセス ポリシーが更新されるまで待機し、次の手順に進みます。

## <a name="create-a-key-vault-secret"></a>キー コンテナー シークレットを作成する

次の Ansible プレイブックのスニペットは、キー コンテナーのシークレットを作成する方法を示しています。

```yml
  vars:
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

  tasks:
    - name: Set facts
      set_fact:
        az_client_id: "{{ lookup('env', 'AZ_CLIENT_ID') }}"

    - name: Create a secret
      azure_rm_keyvaultsecret:
        subscription_id: "{{ az_sub_id }}"
        client_id: "{{ az_client_id }}"
        keyvault_uri: "{{ kv_uri }}"
        secret_name: "{{ kv_secret_name }}"
        secret_value: "{{ kv_secret_value }}"
```

**注:**

- キー コンテナーのシークレットを作成するために、[azure_rm_keyvaultsecret モジュール](https://docs.ansible.com/ansible/latest/modules/azure_rm_keyvaultsecret_module.html)を使用します。
- わかりやすくするために、このデモには `secret_name` と `secret_value` が含まれています。 ただし、プレイブックは、プロジェクトのソース コードと同様に、コードとしてのインフラストラクチャ (AiC) ファイルです。 そのため、このような値は、運用環境で使用する場合には、プレーンテキスト ファイルに保存しないでください。
- このコードを実行すると、キー コンテナーの **[シークレット]** タブに、`testsecret` という名前の新しく追加されたシークレットが一覧表示されます。 これを表示するには、シークレットを選択し、現在のバージョンを選択し、 **[シークレット値を表示する]** を選択します。

## <a name="get-a-key-vault-secret"></a>キー コンテナーのシークレットを取得する

次の Ansible プレイブックのスニペットは、キー コンテナーのシークレットの最新バージョンを取得する方法を示しています。

```yml
  vars:
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

tasks:
    - name: Get latest version of a secret
      azure_rm_keyvaultsecret_info:
        vault_uri: "{{ kv_uri }}"
        name: "{{ kv_secret_name }}"
      register: output
    - debug:
        var: output['secrets'][0]['secret']
```

**注:**

- キー コンテナーのシークレットを取得するために、**azure_rm_keyvaultsecret_info モジュール**を使用します。 このモジュールは、Azure モジュールの Ansible コレクションを使用する場合にのみ使用できます。 
- このスニペットの実行中にエラーが発生した場合は、[「前提条件」セクション](#prerequisites)のすべての説明に従っていることを確認してください。
- わかりやすくするために、このデモには `secret_name` と `secret_value` が含まれています。 ただし、プレイブックは、プロジェクトのソース コードと同様に、コードとしてのインフラストラクチャ (AiC) ファイルです。 そのため、このような値は、運用環境で使用する場合には、プレーンテキスト ファイルに保存しないでください。

## <a name="run-the-complete-playbook"></a>完全なプレイブックを実行する

キー コンテナーとそのシークレットを確立すると、仮想マシンなどの Azure リソースを保護するときに、その情報を使用できます。 次の Ansible プレイブックは、このチュートリアルを通して示されているタスクを実行し、完全な仮想マシンを作成します。 

```yml
---
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 10000 | random }}"
      run_once: yes
      
- hosts: localhost
  collections:
    - azure.azcollection
  vars:
    kv_rg: kv_rg_{{ rpfx }}
    kv_rg_loc: eastus
    kv: "kv{{ rpfx }}"
    kv_uri: "https://{{ kv }}.vault.azure.net"
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

    # Test VM vars
    test_vm_rg: kv_test_vm_rg
    test_vm_rg_loc: eastus
    test_vm: kvtestvm
    test_vm_vnet: "kv_test_vm_vnet"
    test_vm_subnet: kv_test_vm_subnet
    test_vm_public_ip: kv_test_vm_public_ip
    test_vm_nsg: kv_test_vm_nsg
    test_vm_nsg_list: 
      - name: Allow-SSH
        access: Allow
        protocol: Tcp
        direction: Inbound
        priority: 300
        port: 22 
        source_address_prefix: Internet
      - name: Allow-HTTP
        access: Allow
        protocol: Tcp
        direction: Inbound
        priority: 100
        port: 80
        source_address_prefix: Internet 
    test_vm_nic: kv_test_vnic
    admin_username: testadmin

  tasks:
    - name: Set facts
      set_fact:
        az_sub_id: "{{ lookup('env', 'AZ_SUBSCRIPTION_ID') }}"
        az_object_id: "{{ lookup('env', 'AZ_OBJECT_ID') }}"
        az_tenant_id: "{{ lookup('env', 'AZ_TENANT_ID') }}"
        az_client_id: "{{ lookup('env', 'AZ_CLIENT_ID') }}"

    - name: Create a resource group to hold the Key Vault instance
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ kv_rg }}"
        location: "{{ kv_rg_loc }}"

    - debug:
        msg: "New resource group = {{ kv_rg }}"

    - name: Create instance of Key Vault
      azure_rm_keyvault:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ kv_rg }}"
        vault_name: "{{ kv }}"
        vault_tenant: "{{ az_tenant_id }}"
        enabled_for_deployment: yes
        sku:
          name: standard
          family: A
        access_policies:
          - object_id: "{{ az_object_id }}"
            tenant_id: "{{ az_tenant_id }}"
            secrets:
              - get
              - list
              - set

    - debug:
        msg: "New Key Vault instance name = {{ kv }} within the {{ kv_rg }} resource group"

    - name: Create a secret
      azure_rm_keyvaultsecret:
        subscription_id: "{{ az_sub_id }}"
        client_id: "{{ az_client_id }}"
        keyvault_uri: "{{ kv_uri }}"
        secret_name: "{{ kv_secret_name }}"
        secret_value: "{{ kv_secret_value }}"

    - name: Register Key Vault provider.
      shell:
        az provider register -n Microsoft.KeyVault

    - name: Get latest version of a secret
      azure_rm_keyvaultsecret_info:
        vault_uri: "{{ kv_uri }}"
        name: "{{ kv_secret_name }}"
      register: output
    - debug:
        var: output['secrets'][0]['secret']

    - name: Create resource group for test VM.
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ test_vm_rg }}"
        location: "{{ test_vm_rg_loc }}"

    - name: Create virtual network.
      azure_rm_virtualnetwork:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_vnet }}"
        address_prefixes: "172.16.0.0/16"

    - name: Create subset within virtual network.
      azure_rm_subnet:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        virtual_network_name: "{{ test_vm_vnet }}"
        name: "{{ test_vm_subnet }}"
        address_prefix_cidr:  "172.16.10.0/24"

    - name: Create public IP address.
      azure_rm_publicipaddress:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        allocation_method: Static
        name: "{{ test_vm_public_ip }}"

    - name: Create Network Security Group and rules.
      azure_rm_securitygroup:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_nsg}}"
        rules:
          - name: "{{ item.name }}"
            access: "{{ item.access }}"
            protocol: "{{ item.protocol }}"
            direction: "{{ item.direction }}"
            destination_port_range: "{{ item.port }}"
            priority: "{{ item.priority }}"
            source_address_prefix: "{{ item.source_address_prefix }}"
      loop: "{{ test_vm_nsg_list }}"

    - name: Create virtual network interface card (NIC).
      azure_rm_networkinterface:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_nic }}"
        virtual_network: "{{ test_vm_vnet }}"
        subnet: "{{ test_vm_subnet }}"
        ip_configurations:
          - name: ipconfig
            public_ip_address_name: "{{ test_vm_public_ip }}"
            primary: yes
        security_group: "{{ test_vm_nsg }}"

    - name: Create virtual machine.
      azure_rm_virtualmachine:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm }}"
        admin_username: " {{ admin_username }} "
        admin_password: " {{ output['secrets'][0]['secret'] }}"
        vm_size: Standard_B1ms
        network_interfaces: "{{ test_vm_nic }}"
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          version: latest

```

**注:**

- 仮想マシンの *admin* パスワードは、キー コンテナーのシークレットに設定されます。
- プレイブック全体を一度に実行できるかどうかは、テスト環境によって異なります。 キーを作成する前に、キー コンテナーのアクセス ポリシーに自分自身を手動で追加する必要がある場合があります。 このタスクについては、「[キー コンテナーを作成する](#create-a-key-vault)」および「[キー コンテナーのアクセス ポリシーに自分自身を追加する](#add-yourself-to-key-vault-access-policy)」セクションで説明されています。
- ご覧のように、Azure 仮想マシンとそのすべての構成コンポーネントを作成するために、多種多様な Ansible モジュールが使用されます。 仮想マシンの作成に使用される各種の Ansible モジュールの詳細については、次の一覧を参照してください。
    - [Azure リソース グループ モジュール (azure_rm_resourcegroup)](https://docs.ansible.com/ansible/latest/modules/azure_rm_resourcegroup_module.html)
    - [Azure 仮想ネットワーク モジュール (azure_rm_virtualnetwork)](https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualnetwork_module.html)
    - [Azure 仮想ネットワーク サブネット モジュール (azure_rm_subnet)](https://docs.ansible.com/ansible/latest/modules/azure_rm_subnet_module.html)
    - [Azure パブリック IP モジュール (azure_rm_publicipaddress)](https://docs.ansible.com/ansible/latest/modules/azure_rm_publicipaddress_module.html)
    - [Azure ネットワーク セキュリティ グループ モジュール (azure_rm_securitygroup)](https://docs.ansible.com/ansible/latest/modules/azure_rm_securitygroup_module.html)
    - [Azure ネットワーク インターフェイス (azure_rm_networkinterface)](https://docs.ansible.com/ansible/latest/modules/azure_rm_networkinterface_module.html)
    - [Azure 仮想マシン (azure_rm_virtualmachine)](https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualmachine_module.html)
    
## <a name="clean-up-resources"></a>リソースをクリーンアップする

この記事で作成したリソースが不要になったら、削除してください。 `<kv_rg>` プレースホルダーを、デモのキー コンテナーの保持に使用されたリソース グループに置き換えます。

```yml
- hosts: localhost
  vars:
    kv_rg: <kv_rg>
    test_vm_rg: kv_test_vm_rg
  tasks:
    - name: Delete the key vault resource group
      azure_rm_resourcegroup:
        name: "{{ kv_rg }}"
        force_delete_nonempty: yes
        state: absent
    - name: Delete the test vm resource group
      azure_rm_resourcegroup:
        name: "{{ test_vm_rg }}"
        force_delete_nonempty: yes
        state: absent
```

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"] 
> [Azure Key Vault のセキュリティの概要](/azure/key-vault/general/overview-security)