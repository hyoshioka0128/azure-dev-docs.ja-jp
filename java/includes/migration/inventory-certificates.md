---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 3cf4edcf12d7fe72c0fa5a0216d1a8c97a2f2e59
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825891"
---
### <a name="inventory-certificates"></a>証明書をインベントリする

パブリック SSL エンドポイント、またはバックエンド データベースやその他のシステムとの通信に使用されるすべての証明書を文書化します。 次のコマンドを実行して、運用サーバー上のすべての証明書を表示できます。

```bash
keytool -list -v -keystore <path to keystore>
```
