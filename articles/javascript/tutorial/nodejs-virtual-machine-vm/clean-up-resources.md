---
title: Linux 仮想マシン リソースを削除する
description: Azure CLI コマンドを使用してリソース グループを削除し、Azure リソースをクリーンアップします。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5c8c0bb8a1413da72cb2c32d9ce541bee1c36cac
ms.sourcegitcommit: dc74b60217abce66fe6cc93923e869e63ac86a8f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/18/2020
ms.locfileid: "94872823"
---
# <a name="7-clean-up-resources"></a>7.リソースをクリーンアップする

このチュートリアルを完了したら、リソース グループを削除する必要があります。これにはすべてのリソースが含まれているので、さらなる使用によって課金されないようにすることができます。 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>リソース グループを削除してすべてのリソースを削除する

同じターミナルで、[Azure CLI コマンド](/cli/azure/group?view=azure-cli-latest#az_group_delete)を使用してリソース グループを削除します。

```azurecli
az group delete --name rg-demo-vm-eastus -y
```

このコマンドには数分かかります。 

## <a name="next-step"></a>次のステップ

* [Azure Linux VM](/azure/virtual-machines) に関する詳細情報
