---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: c8cd0672bfedf640077e9ae3b9272b12d8ce0b61
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738150"
---
### <a name="provision-azure-container-registry-and-azure-kubernetes-service"></a>Azure Container Registry と Azure Kubernetes Service をプロビジョニングする

次のコマンドを使用して、レジストリの閲覧者ロールを持つサービス プリンシパルを使って、コンテナー レジストリと Azure Kubernetes クラスターを作成します。 クラスターのネットワーク要件に応じて、[適切なネットワーク モデル](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)を選択してください。

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```
