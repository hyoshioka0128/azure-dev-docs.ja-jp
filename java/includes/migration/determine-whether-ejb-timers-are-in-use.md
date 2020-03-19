---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: bda7ba230d8287101fc8f3f298b08777bb6e1c61
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897472"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>EJB タイマーが使用中かどうかを判断する

アプリケーションで EJB タイマーを使用する場合は、EJB タイマー コードが各 WildFly インスタンスによって個別にトリガーできることを確認する必要があります。 この確認が必要な理由は、Azure Kubernetes Service のデプロイ シナリオでは、各 EJB タイマーがその独自の WildFly インスタンス上でトリガーされるためです。
