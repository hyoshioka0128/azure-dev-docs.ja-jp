---
author: yevster
ms.author: yebronsh
ms.date: 5/19/2020
ms.openlocfilehash: 51ed106fde71e37467571baab2b327b092dddc15
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507636"
---
### <a name="migrate-and-enable-the-identity-provider"></a>ID プロバイダーを移行して有効にする

アプリケーションで認証または認可が必要な場合、次のガイダンスを使用し、それらを確実に ID プロバイダーにアクセスするように構成します。

* ID プロバイダーが Azure Active Directory である場合は、変更は必要ありません。
* ID プロバイダーがオンプレミスの Active Directory フォレストである場合は、Azure Active Directory を使用したハイブリッド ID ソリューションの実装を検討します。 詳細については、[ハイブリッド ID のドキュメント](/azure/active-directory/hybrid/)を参照してください。
* ID プロバイダーが別のオンプレミス ソリューション (PingFederate など) である場合は、「[Azure AD Connect のカスタム インストール](/azure/active-directory/hybrid/how-to-connect-install-custom)」トピックを参照して、Azure Active Directory とのフェデレーションを構成します。 または、Spring Security を使用して [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) または [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 経由で ID プロバイダーを使用することを検討します。
