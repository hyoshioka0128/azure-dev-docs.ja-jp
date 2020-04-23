---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a67bdb749231c39e1b64d7b2a982ad4c0946cc25
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673448"
---
### <a name="determine-whether-management-over-rest-is-used"></a>REST 経由の管理が使用されているかどうか確認する

アプリケーションのライフサイクルに REST 経由の管理の使用が含まれている場合は、REST API にアクセスするために使用されているポートを把握し、それらがどのように認証され、公開されているかを確認する必要があります。 移行後、アプリケーションのライフサイクルが移行前と同様の方法で動作できるように、これらの同じポートと認証メカニズムが公開されていることを確認する必要があります。 詳細については、「[RESTful 管理サービスによる Oracle WebLogic Server の管理](https://docs.oracle.com/middleware/12213/wls/WLRUR/title.htm)」を参照してください。
