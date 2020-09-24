---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: c77a5ae60807cf25415ab86dab9d9891abab13f9
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738395"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>EJB タイマーが使用中かどうかを判断する

アプリケーションで EJB タイマーを使用する場合は、EJB タイマー コードが各 WildFly インスタンスによって個別にトリガーできることを確認する必要があります。 この確認が必要な理由は、Azure Kubernetes Service のデプロイ シナリオでは、各 EJB タイマーがその独自の WildFly インスタンス上でトリガーされるためです。
