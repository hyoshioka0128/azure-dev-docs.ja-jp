---
title: Azure リソースを削除する
description: Azure CLI コマンドを使用してリソース グループを削除し、課金対象のリソースをクリーンアップします。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 132ccc26a4ddf17eb38be1573f462492fe1f7f0c
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687498"
---
# <a name="7-clean-up-resources-for-static-web-app"></a>7.静的 Web アプリのリソースをクリーンアップする

このチュートリアルを完了したら、Computer Vision リソースおよび静的 Web アプリが含まれているリソース グループを削除し、さらなる使用によって課金されないようにする必要があります。 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>リソース グループを削除してすべてのリソースを削除する

同じターミナルで、[Azure CLI コマンド](/cli/azure/group?view=azure-cli-latest#az_group_delete)を使用してリソース グループを削除します。

```azurecli
az group delete --name rg-demo  -y
```

このコマンドには数分かかる場合があります。 
