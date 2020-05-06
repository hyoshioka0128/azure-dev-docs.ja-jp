---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: e72cefaab3ccdbbaae01992cc11285944fb09a36
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673208"
---
### <a name="provision-azure-container-registry-and-azure-kubernetes-service"></a>Azure Container Registry と Azure Kubernetes Service をプロビジョニングする

次のコマンドを使用して、レジストリの閲覧者ロールを持つサービス プリンシパルを使って、コンテナー レジストリと Azure Kubernetes クラスターを作成します。 クラスターのネットワーク要件に応じて、[適切なネットワーク モデル](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)を選択してください。

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```
