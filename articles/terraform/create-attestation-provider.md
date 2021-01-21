---
title: Terraform を使用して Azure 構成証明プロバイダーを構成する
description: Terraform を使用して Azure で構成証明プロバイダーを作成する方法について説明します。
keywords: Azure DevOps Terraform 構成証明
ms.topic: how-to
ms.date: 11/08/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: c79c472da4604458475bd230a844e2d308c2bda1
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561548"
---
# <a name="configure-an-azure-attestation-policy-using-terraform"></a>Terraform を使用して Azure Attestation ポリシーを構成する

この記事では、Azure で[構成証明プロバイダー](/azure/attestation/overview)を作成するための Terraform コードの例を示します。

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- *ポリシー署名証明書*: 信頼された署名キーのセットを指定する、*.pem ファイルの形式のファイル。

[!INCLUDE [terraform-configure-environment.md](includes/terraform-configure-environment.md)]

## <a name="configure-an-azure-attestation-provider"></a>Azure 構成証明プロバイダーを構成する

```hcl
resource "azurerm_resource_group" "corpAttestation" {
  name                        = "corp-attestation"
  location                    = "westus"
}

resource "azurerm_attestation_provider" "corpAttestation" {
  name                              = "attestationprovider007"
  resource_group_name               = azurerm_resource_group.corpAttestation.name
  location                          = azurerm_resource_group.corpAttestation.location

  policy_signing_certificate_data   = file("./certs/cert.pem")
}
```

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"] 
> [Azure での Terraform の使用について詳細を参照](/azure/terraform)