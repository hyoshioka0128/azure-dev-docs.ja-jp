---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: 430f4fef9c512f91da4fd95f04320ddd127ea784
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990254"
---
### <a name="inventory-configuration-sources-and-secrets"></a>構成のソースとシークレットをインベントリする

#### <a name="inventory-passwords-and-secure-strings"></a>パスワードとセキュリティで保護された文字列をインベントリする

すべてのシークレット文字列とパスワードについて、実稼働デプロイ上のすべてのプロパティおよび構成ファイルとすべての環境変数を確認します。 Spring Boot アプリケーションでは、多くの場合、このような文字列は *application.properties* または *application.yml* ファイルで見つかります。

[!INCLUDE [inventory-certificates-h4](inventory-certificates-h4.md)]
