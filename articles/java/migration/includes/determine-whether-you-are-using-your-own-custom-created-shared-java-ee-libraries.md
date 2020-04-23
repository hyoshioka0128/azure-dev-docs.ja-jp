---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 64a4a0e191c70d930ad8c5f9d19cb81dfb9cb851
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673418"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>独自に作成したカスタム共有 Java EE ライブラリを使用しているかどうか確認する

共有 Java EE ライブラリ機能を使用している場合は、次の 2 つの選択肢があります。

* アプリケーション コードをリファクターして、ライブラリへの依存関係をすべて削除し、代わりにその機能をアプリケーションに直接組み込みます。
* サーバーのクラスパスにそれらのライブラリを追加します。
