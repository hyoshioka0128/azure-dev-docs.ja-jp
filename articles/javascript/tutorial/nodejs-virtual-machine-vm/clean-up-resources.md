---
title: Linux 仮想マシン リソースを削除する
description: Azure CLI コマンドを使用してリソース グループを削除し、Azure リソースをクリーンアップします。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: ac61f1b73e873ee1c1c6cd343792a79a88e76123
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98560988"
---
# <a name="7-clean-up-resources"></a>7.リソースをクリーンアップする

このチュートリアルを完了したら、リソース グループを削除する必要があります。これにはすべてのリソースが含まれているので、さらなる使用によって課金されないようにすることができます。 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>リソース グループを削除してすべてのリソースを削除する

同じターミナルで、[Azure CLI コマンド](/cli/azure/group#az_group_delete)を使用してリソース グループを削除します。

```azurecli
az group delete --name rg-demo-vm-eastus -y
```

このコマンドには数分かかります。 

## <a name="next-step"></a>次のステップ

* [Azure Linux VM](/azure/virtual-machines) に関する詳細情報
