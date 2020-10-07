---
title: インクルード ファイル
description: インクルード ファイル
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 23a5bbc2e6a7e364b3c6eab38bfd5e98b97348a6
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401560"
---
## <a name="create-and-apply-a-terraform-execution-plan"></a>Terraform 実行プランを作成して適用する

このセクションでは、"*実行プラン*" を作成し、クラウド インフラストラクチャに適用する方法について説明します。

1. Terraform のデプロイを初期化するには、[terraform init](https://www.terraform.io/docs/commands/init.html) を実行します。 このコマンドによって、Azure リソース グループを作成するために必要な Azure モジュールがダウンロードされます。

    ```cmd
    terraform init
    ```

1. 初期化後、[terraform plan](https://www.terraform.io/docs/commands/plan.html) を実行して、実行プランを作成します。

    ```cmd
    terraform plan -out <terraform_plan>.tfplan
    ```

    [!INCLUDE [terraform-plan-notes.md](terraform-plan-notes.md)]

1. クラウド インフラストラクチャに実行プランを適用する準備ができたら、[terraform apply](https://www.terraform.io/docs/commands/apply.html) を実行します。

    ```cmd
    terraform apply <terraform_plan>.tfplan
    ```
