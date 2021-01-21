---
title: Azure リソースを削除する
description: Azure CLI コマンドを使用してリソース グループを削除し、課金対象のリソースをクリーンアップします。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: bc68f550d0a2c1bc1550eb755e6ef33bae6ae9b7
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561638"
---
# <a name="7-clean-up-resources-for-static-web-app"></a>7.静的 Web アプリのリソースをクリーンアップする

このチュートリアルを完了したら、Computer Vision リソースおよび静的 Web アプリが含まれているリソース グループを削除し、さらなる使用によって課金されないようにする必要があります。 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>リソース グループを削除してすべてのリソースを削除する

同じターミナルで、[Azure CLI コマンド](/cli/azure/group#az_group_delete)を使用してリソース グループを削除します。

```azurecli
az group delete --name rg-demo  -y
```

このコマンドには数分かかる場合があります。 
