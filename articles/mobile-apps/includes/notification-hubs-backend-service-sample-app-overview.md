---
author: mikeparker104
ms.author: miparker
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: 510218bcbed53dfb4383b6a906911790f7865975
ms.sourcegitcommit: cf23d382eee2431a3958b1c87c897b270587bde0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/29/2020
ms.locfileid: "87401590"
---
これにより、作成したバックエンド サービスを使用して、通知ハブから登録と登録解除を行うことができます。 

アクションが指定され、アプリがフォアグラウンドにある場合は、アラートが表示されます。 それ以外の場合は、通知センターに通知が表示されます。

> [!NOTE]
> 通常、明示的なユーザー登録/登録解除の入力を行わずに、アプリケーション ライフサイクルの適切な時点で (または初回起動時に)、登録 (および登録解除) アクションを実行します。 ただし、この例では、この機能をより簡単に調査およびテストできるように、明示的なユーザー入力が必要です。
