---
title: インクルード ファイル
description: インクルード ファイル
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 9f3d916bc37cc9d5f86c1f6b2738f419414e5816
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401557"
---
  **注:**

  - `terraform plan` コマンドは、実行プランを作成しますが、実行はしません。 代わりに、構成ファイルに指定された構成を作成するために必要なアクションを決定します。 このパターンを使用すると、実際のリソースに変更を加える前に、実行プランが自分の想定と一致しているかどうかを確認できます。
  - 省略可能な `-out` パラメーターを使用すると、プランの出力ファイルを指定できます。 `-out` パラメーターを使用すると、レビューしたプランが適用内容とまったく同じであることが確実になります。
  - 実行プランの永続化とセキュリティの詳細については、[「セキュリティの警告」セクション](https://www.terraform.io/docs/commands/plan.html#security-warning)を参照してください。