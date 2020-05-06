---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: bda7ba230d8287101fc8f3f298b08777bb6e1c61
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672908"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>EJB タイマーが使用中かどうかを判断する

アプリケーションで EJB タイマーを使用する場合は、EJB タイマー コードが各 WildFly インスタンスによって個別にトリガーできることを確認する必要があります。 この確認が必要な理由は、Azure Kubernetes Service のデプロイ シナリオでは、各 EJB タイマーがその独自の WildFly インスタンス上でトリガーされるためです。
