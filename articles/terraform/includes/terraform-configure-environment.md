---
title: インクルード ファイル
description: インクルード ファイル
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: daa57dfc18cae3250c42265ebe8cbf7722e4235b
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401589"
---
## <a name="configure-your-environment"></a>環境を構成する

環境に応じて、Terraform をインストールして構成します。

- [Azure Cloud Shell および Azure CLI を使用して Terraform を構成する](../get-started-cloud-shell.md)
- [Azure PowerShell を使用して Terraform を構成する](../get-started-powershell.md)

構成の記事では、次のタスクを実行する方法についても説明します。

- 基本の Terraform 構成ファイルを作成する。 このファイルには、`provider` ブロックに [Azure プロバイダー (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html) が含まれており、[Azure リソース グループ](/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group)が定義されています。
- Terraform 実行プランを作成して適用し、コードを"実行"　する。
- リソースを使用し終えて削除する必要がある場合に、実行プランを破棄する。
