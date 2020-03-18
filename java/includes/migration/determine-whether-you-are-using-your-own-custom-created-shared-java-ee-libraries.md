---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 64a4a0e191c70d930ad8c5f9d19cb81dfb9cb851
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78894186"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>独自に作成したカスタム共有 Java EE ライブラリを使用しているかどうか確認する

共有 Java EE ライブラリ機能を使用している場合は、次の 2 つの選択肢があります。

* アプリケーション コードをリファクターして、ライブラリへの依存関係をすべて削除し、代わりにその機能をアプリケーションに直接組み込みます。
* サーバーのクラスパスにそれらのライブラリを追加します。
