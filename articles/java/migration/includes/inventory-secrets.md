---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: e37d0337361ce742ce7f7994ab634e63bc4b2ead
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81671648"
---
### <a name="inventory-secrets"></a>シークレットをインベントリする

#### <a name="passwords-and-secure-strings"></a>パスワードとセキュリティで保護された文字列

すべてのシークレット文字列とパスワードについて、運用サーバー上のすべてのプロパティと構成ファイルを確認します。 *$CATALINA_BASE/conf* にある *server.xml* と *context.xml* を必ず確認してください。 また、アプリケーション内にパスワードや資格情報を含むファイルが存在する場合もあります。 これらには、*META-INF/context.xml*や、Spring Boot アプリケーションの場合は *application.properties* または *application.yml* ファイルなどがあります。
