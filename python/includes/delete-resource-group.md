---
ms.openlocfilehash: 2f54e918e7126123ada68b696ea96f4d5f3dbb83
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125241"
---
[Azure portal](https://portal.azure.com) または Azure CLI を使用して、リソース グループを削除できます。

- ポータルで、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用します。

- 次の Azure CLI コマンドを (ローカルでまたは [Cloud Shell](/cloud-shell/overview) を使用して) 実行します。`<resource_group>` はこのチュートリアルで使用したグループ名に置き換えます。

    ```azurecli
    az group delete --name <resource_group>
    ```
