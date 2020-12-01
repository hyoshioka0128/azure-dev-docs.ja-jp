---
title: Azure リソースを削除する
description: Azure CLI コマンドを使用してリソース グループを削除し、課金対象のリソースをクリーンアップします。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 0b425ce873c53ca95628cfe8c3f68ea4b010aba3
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993540"
---
# <a name="6-clean-up-resources"></a>6.リソースをクリーンアップする

このチュートリアルを完了したら、Computer Vision リソースおよび静的 Web アプリが含まれているリソース グループを削除し、さらなる使用によって課金されないようにする必要があります。 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>リソース グループを削除してすべてのリソースを削除する

同じターミナルで、[Azure CLI コマンド](/cli/azure/group?view=azure-cli-latest#az_group_delete)を使用してリソース グループを削除します。

```azurecli
az group delete --name rg-demo  -y
```

このコマンドには数分かかる場合があります。 
