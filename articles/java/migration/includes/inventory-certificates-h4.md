---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 975c6ce1011f76a34f33ec6093f70f281eefa6c3
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990214"
---
#### <a name="inventory-certificates"></a>証明書をインベントリする

パブリック SSL エンドポイント、またはバックエンド データベースやその他のシステムとの通信に使用されるすべての証明書を文書化します。 次のコマンドを実行して、運用サーバー上のすべての証明書を表示できます。

```bash
keytool -list -v -keystore <path to keystore>
```
