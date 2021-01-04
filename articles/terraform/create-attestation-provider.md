---
title: Terraform を使用して Azure 構成証明プロバイダーを構成する
description: Terraform を使用して Azure で構成証明プロバイダーを作成する方法について説明します。
keywords: Azure DevOps Terraform 構成証明
ms.topic: how-to
ms.date: 11/08/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: c42f2a00886bedf3e9f26566cb619c69bb5073b7
ms.sourcegitcommit: f723980ade4cbc13548a5d8ac3f3fa681b8a2dbd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/16/2020
ms.locfileid: "97609398"
---
# <a name="configure-an-azure-attestation-policy-using-terraform"></a>Terraform を使用して Azure Attestation ポリシーを構成する

この記事では、Azure で[構成証明プロバイダー](https://docs.microsoft.com/azure/attestation/overview)を作成するための Terraform コードの例を示します。

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
