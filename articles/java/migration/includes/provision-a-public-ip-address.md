---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 92caae7f50ddb0e58f14e8c99a4cd598a72f884f
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738149"
---
### <a name="provision-a-public-ip-address"></a>パブリック IP アドレスをプロビジョニングする

内部ネットワークまたは仮想ネットワークの外部からアプリケーションにアクセスできるようにする場合は、パブリック静的 IP アドレスが必要になります。 この IP アドレスは、次の例に示すように、クラスターのノード リソース グループ内にプロビジョニングする必要があります。

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```
