---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: f5995a3c5efc46bc58b446589ce089bf7d86b2ba
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897506"
---
### <a name="determine-whether-your-application-uses-a-resource-adapter"></a>アプリケーションでリソース アダプターが使用されるかどうかを判断する

アプリケーションでリソース アダプター (RA) が必要な場合、アプリケーションは WildFly と互換性がある必要があります。 WildFly のスタンドアロン インスタンスで RA が正常に機能するかどうかを確認するには、それをサーバーにデプロイし、適切に構成します。 RA が適切に機能する場合、JAR を Docker イメージのサーバー クラスパスに追加し、必要な構成ファイルを WildFly サーバー ディレクトリ内の適切な場所に配置して使用できるようにする必要があります。
