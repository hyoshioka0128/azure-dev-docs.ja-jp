---
title: インクルード ファイル
description: インクルード ファイル
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 9fcde450a19515c38eb6653d75e644ca23d08964
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401559"
---
## <a name="create-a-base-terraform-configuration-file"></a>基本の Terraform 構成ファイルを作成する

Terraform 構成ファイルはプロバイダーの指定から開始します。 Azure を使用する場合、`provider` ブロックで [Azure プロバイダー (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html) を指定します。

```terraform
provider "azurerm" {
  version = "~>2.0"
  features {}
}

resource "azurerm_resource_group" "rg" {
  name = "<your_resource_group_name>"
  location = "<your_resource_group_location>"
}

# Your Terraform code goes here...

```

**注**:

- `version` 属性は省略可能ですが、HashiCorp では特定のバージョンのプロバイダーに固定することが推奨されています。 
- Azure プロバイダー 1.x を使用する場合、`features` ブロックは許可されません。
- Azure プロバイダー 2.x を使用する場合、`features` ブロックは必須です。
- [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html) の [リソース宣言](https://www.terraform.io/docs/configuration/resources.html) には、`name` と `location` という 2 つの引数があります。 プレースホルダーは、ご使用の環境の適切な値に設定してください。
- 操作方法とチュートリアルの記事全体で、リソース グループを参照するときに、リソース グループの[ローカルの名前付きの値](https://www.terraform.io/docs/configuration/expressions.html#references-to-named-values)である `rg` が使用されます。 これはリソース グループ名に依存せず、コード内の変数名のみを参照します。 リソース グループ定義内でこの値を変更した場合、それを参照するコードでもこれを変更する必要があります。
