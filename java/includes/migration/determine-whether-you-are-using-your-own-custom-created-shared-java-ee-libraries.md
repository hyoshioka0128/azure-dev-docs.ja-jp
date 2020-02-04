---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 2dccfba5cfe61ac862aeabe7b928e121502093b7
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830880"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>独自に作成したカスタム共有 Java EE ライブラリを使用しているかどうか確認する

共有 Java EE ライブラリ機能を使用している場合は、次の 2 つの選択肢があります。

1. アプリケーション コードをリファクターして、ライブラリへの依存関係をすべて削除し、代わりにその機能をアプリケーションに直接組み込みます。
2. サーバーのクラスパスにそれらのライブラリを追加します。
