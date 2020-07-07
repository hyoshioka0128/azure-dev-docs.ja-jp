---
title: Visual Studio App Center と Azure サービスを使用してモバイル アプリに認証を追加する
description: ユーザー認証を設定したり、モバイル アプリケーションがソーシャル アカウント、Azure Active Directory、およびカスタム認証を使用して認証を受けられるようにしたりするのに役立つ Visual Studio App Center などのサービスについて説明します。
author: codemillmatt
ms.assetid: 34a8a070-2222-4faf-9090-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 782123b4b567010a43ce72c622f7db5a65679db9
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632584"
---
# <a name="add-authentication-and-manage-user-identities-in-your-mobile-apps"></a>ご利用のモバイル アプリに認証を追加し、ユーザー ID を管理する

ご利用のアプリケーション全体でのユーザーとその行動を把握することにより、開発者はユーザーに合わせたエクスペリエンスを作成してユーザーの関心を高めることができます。 組織内のユーザー向けにコラボレーション アプリケーションを構築しているアプリケーション開発者であっても、次のソーシャル ネットワーク プラットフォームを作成している場合であっても、ユーザーを認証し、ユーザー ID を管理する方法が必要になります。 ID 管理サービスは、モバイル バックエンド サービスの最も重要な機能の 1 つです。

次のサービスを使用することで、ご利用のモバイル アプリ内でユーザー認証を有効にすることができます。

## <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/) は企業-消費者間 (B2C) の ID 管理サービスです。開発者はこれを使用して、自分の顧客を認証することができます。 このホワイトラベル サービスを使用することで、開発者はユーザーが自分の Web、デスクトップ、モバイル、またはシングルページ アプリケーションと安全に作用する方法をカスタマイズしたり制御したりできます。 Azure AD B2C を使用して、ユーザーは、サインアップ、サインイン、パスワードのリセット、およびプロファイルの編集を行うことができます。 Azure AD B2C では、OpenID Connect プロトコルと OAuth 2.0 プロトコルの一形式が実装されます。 

### <a name="azure-active-directory-b2c-features"></a>Azure Active Directory B2C の機能

- 顧客の好みの ID プロバイダーを使用して顧客を安全に認証します。
- 顧客の ID とアクセスの管理。
- Facebook、GitHub、Google、LinkedIn、Twitter、WeChat、Weibo などのソーシャル メディアのサインインがサポートされています。
- OpenID Connect、SAML などの業界標準のプロトコルを使用してユーザー アカウントにアクセスできるため、多様なプラットフォーム上で ID の管理が可能になります。
- ブランド化された登録とサインインのエクスペリエンスを提供します。
- CRM データベース、マーケティング分析ツール、アカウント検証システムと容易に統合することが可能です。
- 顧客のサインイン、ユーザー設定、および変換データをキャプチャします。

### <a name="azure-active-directory-b2c-references"></a>Azure Active Directory B2C の参考資料

- [Azure Portal](https://portal.azure.com/)
- [Azure AD B2C ドキュメント](/azure/active-directory-b2c/)
- [クイック スタート](/azure/active-directory-b2c/active-directory-b2c-quickstarts-web-app)
- [サンプル](/azure/active-directory-b2c/code-samples)

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) は Microsoft が提供するクラウドベースの ID およびアクセス管理サービスであり、従業員による以下のリソースへのサインインとアクセスを支援します。

- Microsoft Office 365、Azure portal、およびその他何千という サービスとしてのソフトウェア (SaaS) アプリケーションなどの外部リソース。
- 企業ネットワークとイントラネット上のアプリや、自分の組織で開発したクラウド アプリなどの内部リソース。

### <a name="azure-active-directory-features"></a>Azure Active Directory の機能

- ユーザーが必要とするアプリケーションにユーザーを接続することによるシームレスかつ安全性の高いアクセス。
- 包括的な ID 保護と、ユーザー、位置情報、デバイス、データ、アプリ コンテキストに基づく ID およびアクセスの強化されたセキュリティ。
- 何千もの事前統合されたアプリ。Office 365、Salesforce.com、Box などの、商用アプリケーションとカスタム アプリケーションの両方があります。
- 大規模なアクセス管理機能。

### <a name="azure-active-directory-references"></a>Azure Active Directory の参考資料

- [Azure Portal](https://portal.azure.com/)
- [Azure AD とは](/azure/active-directory/fundamentals/active-directory-whatis)
- [Azure Active Directory の概要](/azure/active-directory/fundamentals/active-directory-whatis)
- [クイック スタート](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)