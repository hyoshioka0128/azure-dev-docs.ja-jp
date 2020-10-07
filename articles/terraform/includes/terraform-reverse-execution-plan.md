---
title: インクルード ファイル
description: インクルード ファイル
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 718ac8c480c5d366e985f9488d81ab626a82b586
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401558"
---
## <a name="reverse-a-terraform-execution-plan"></a>Terraform 実行プランを破棄する

1. 実行プランを破棄する (元に戻す) には、[terraform plan](https://www.terraform.io/docs/commands/plan.html) を実行し、次のように `destroy` フラグを指定します。

    ```cmd
    terraform plan -destroy -out <terraform_plan>.destroy.tfplan
    ```

    [!INCLUDE [terraform-plan-notes.md](terraform-plan-notes.md)]

1. [terraform apply](https://www.terraform.io/docs/commands/apply.html) を実行して、実行プランを適用します。

    ```cmd
    terraform apply <terraform_plan>.destroy.tfplan
    ```
