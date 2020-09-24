---
ms.openlocfilehash: 4215099ae39963448b7a94d389ded0c9096b1c67
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2020
ms.locfileid: "90772681"
---
[Azure portal](https://portal.azure.com) または Azure CLI を使用して、リソース グループを削除できます。

- ポータルで、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、**[リソース グループの削除]** コマンドを使用します。

- 次の Azure CLI コマンドを (ローカルでまたは [Cloud Shell](/azure/cloud-shell/overview) を使用して) 実行します。`<resource_group>` はこのチュートリアルで使用したグループ名に置き換えます。

    ```azurecli
    az group delete --no-wait --name <resource_group>
    ```
