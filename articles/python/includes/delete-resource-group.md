---
ms.openlocfilehash: c45bda8b08ec963febbc6497136cda71086423a3
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035494"
---
[Azure portal](https://portal.azure.com) または Azure CLI を使用して、リソース グループを削除できます。

- [Azure portal](https://portal.azure.com) で、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルのプロセスで作成されたリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用します。

- 次の Azure CLI コマンドを (ローカルでまたは [Cloud Shell](/azure/cloud-shell/overview) を使用して) 実行します。`<resource_group>` はこのチュートリアルで使用したグループ名に置き換えます。

    ```azurecli
    az group delete --no-wait --name <resource_group>
    ```
