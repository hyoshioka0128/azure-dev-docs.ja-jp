---
author: edburns
ms.author: edburns
ms.date: 08/09/2020
ms.openlocfilehash: e006f2ec9d6f0d3b2d83a6653d65da19489dc221
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/10/2020
ms.locfileid: "88052262"
---
### <a name="account-for-authentication-and-authorization"></a>認証と認可の考慮

ほとんどのアプリケーションには、ある種の認証および認可が備わっています。  認証に LDAP を使用する場合、Azure 上の WebLogic Server では自動統合がサポートされます。 Marketplace のオファーでは、セキュリティで保護された LDAP を利用して Azure Active Directory Domain Services (Azure AD DS) が使用されます。  このオファーにより、Azure AD DS のデータから WebLogic Server の既定の領域が作成されます。  詳細については、「[WebLogic Server 上の Java アプリを Azure に移行するためのエンドユーザーの認可と認証](../migrate-weblogic-with-aad-ldap.md)」を参照してください。
