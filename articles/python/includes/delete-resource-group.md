---
ms.openlocfilehash: 8f757c030849cb89eea36d74b55867dcc2ff98a6
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441357"
---
[Azure portal](https://portal.azure.com) または Azure CLI を使用して、リソース グループを削除できます。

- ポータルで、左側にあるナビゲーション ウィンドウから **[リソース グループ]** を選択し、このチュートリアルで作成したリソース グループを選択し、 **[リソース グループの削除]** コマンドを使用します。

- 次の Azure CLI コマンドを (ローカルでまたは [Cloud Shell](/azure/cloud-shell/overview) を使用して) 実行します。`<resource_group>` はこのチュートリアルで使用したグループ名に置き換えます。

    ```azurecli
    az group delete --name <resource_group>
    ```
