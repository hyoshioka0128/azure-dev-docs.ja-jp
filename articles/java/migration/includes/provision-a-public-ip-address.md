---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 5527a38151afd531cbd256428bd729b7aba8fe45
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673168"
---
### <a name="provision-a-public-ip-address"></a>パブリック IP アドレスをプロビジョニングする

内部ネットワークまたは仮想ネットワークの外部からアプリケーションにアクセスできるようにする場合は、パブリック静的 IP アドレスが必要になります。 この IP アドレスは、次の例に示すように、クラスターのノード リソース グループ内にプロビジョニングする必要があります。

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```
