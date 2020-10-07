---
title: Terraform を使用して Azure VM クラスターを構成する
description: Terraform モジュールを使用して Azure で Windows 仮想マシン クラスターを作成する方法について説明します。
keywords: Azure DevOps Terraform VM 仮想マシン クラスター モジュール レジストリ
ms.topic: how-to
ms.date: 09/27/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 73f375090a2178b38b0fc7e0afd4eb8c6b514672
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401588"
---
# <a name="configure-an-azure-vm-cluster-using-terraform"></a>Terraform を使用して Azure VM クラスターを構成する

この記事では、Azure で VM クラスターを作成するための Terraform コード例を示します。

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

[!INCLUDE [terraform-configure-environment.md](includes/terraform-configure-environment.md)]

## <a name="configure-an-azure-vm-cluster"></a>Azure VM クラスターを構成する

```hcl
module "windowsservers" {
  source              = "Azure/compute/azurerm"
  resource_group_name = azurerm_resource_group.rg.name
  is_windows_image    = true
  vm_hostname         = "mywinvm"                         // Line can be removed if only one VM module per resource group
  admin_password      = "ComplxP@ssw0rd!"                 // See note following code about storing passwords in config files
  vm_os_simple        = "WindowsServer"
  public_ip_dns       = ["winsimplevmips"]                // Change to a unique name per data center region
  vnet_subnet_id      = module.network.vnet_subnets[0]
    
  depends_on = [azurerm_resource_group.rg]
}

module "network" {
  source              = "Azure/network/azurerm"
  resource_group_name = azurerm_resource_group.rg.name
  subnet_prefixes     = ["10.0.1.0/24"]
  subnet_names        = ["subnet1"]

  depends_on = [azurerm_resource_group.rg]
}

output "windows_vm_public_name" {
  value = module.windowsservers.public_ip_dns_name
}

output "vm_public_ip" {
  value = module.windowsservers.public_ip_address
}

output "vm_private_ips" {
  value = module.windowsservers.network_interface_private_ip
}
```

**注**:

- 前のコード例では、わかりやすくするために、変数 `admin_password` にリテラル値が割り当てられています。 パスワードなどの機密データを格納するには、さまざまな方法があります。 データを保護する方法の決定は、ご使用の特定の環境や、このデータが漏えいする不安度に関する個別の選択に影響されることになります。 リスクの例として、このようなファイルをソース管理に格納すると、他のユーザーにパスワードを見られる可能性があります。 このトピックの詳細について、HashiCorp では[入力変数を宣言する](https://www.terraform.io/docs/configuration/variables.html)ためのさまざまな方法および[機密データ (パスワードなど) を管理する](https://www.terraform.io/docs/state/sensitive-data.html)ための手法を文書化しています。

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"] 
> [Azure での Terraform の使用について詳細を参照](/azure/terraform)
