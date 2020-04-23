---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 3427cc445333c9851a760a607c402e689c7b6ba4
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673158"
---
### <a name="inventory-all-certificates"></a>すべての証明書をインベントリする

パブリック SSL エンドポイントで使用されるすべての証明書を文書化します。 次のコマンドを実行して、運用サーバー上のすべての証明書を表示できます。

```bash
keytool -list -v -keystore <path to keystore>
```
