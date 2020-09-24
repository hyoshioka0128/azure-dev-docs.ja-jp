---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 9fa87bf3d0de64271a9ffb0e7e8fbff3cd1afd12
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738500"
---
### <a name="determine-whether-your-application-uses-a-resource-adapter"></a>アプリケーションでリソース アダプターが使用されるかどうかを判断する

アプリケーションでリソース アダプター (RA) が必要な場合、アプリケーションは WildFly と互換性がある必要があります。 WildFly のスタンドアロン インスタンスで RA が正常に機能するかどうかを確認するには、それをサーバーにデプロイし、適切に構成します。 RA が適切に機能する場合、JAR を Docker イメージのサーバー クラスパスに追加し、必要な構成ファイルを WildFly サーバー ディレクトリ内の適切な場所に配置して使用できるようにする必要があります。
