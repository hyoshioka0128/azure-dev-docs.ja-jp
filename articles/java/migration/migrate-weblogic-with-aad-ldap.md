---
title: WebLogic Server 上の Java アプリを Azure に移行するための Azure Active Directory によるエンドユーザーの認可と認証
description: このガイドでは、LDAP 経由で Azure Active Directory Domain Services に接続するように Oracle WebLogic Server を構成する方法について説明します
author: edburns
ms.author: edburns
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: 0f3b8f7e2535bc91629f056cf59bc0d2658aba19
ms.sourcegitcommit: 1f78e54deb85c6063b887286a13a967d1d186b50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "87118444"
---
# <a name="end-user-authorization-and-authentication-for-migrating-java-apps-on-weblogic-server-to-azure"></a>WebLogic Server 上の Java アプリを Azure に移行するためのエンドユーザーの認可と認証

このガイドは、Azure Active Directory を使用して、WebLogic Server 上の Java アプリに対するエンタープライズ レベルのエンドユーザーの認証と認可を有効にする際に役立ちます。

Java EE 開発者は、ワークロードを Azure に移行しているときでも、[標準プラットフォームのセキュリティ メカニズム](https://javaee.github.io/tutorial/security-intro.html#BNBWJ)が "引き続き機能する" ことを期待します。  [Oracle WebLogic Server (WLS) Azure アプリケーション](/azure/virtual-machines/workloads/oracle/oracle-weblogic)を使用すると、組み込みのセキュリティ領域に Azure Active Directory Domain Services (Azure AD DS) のユーザーを設定できます。  Azure アプリケーション上の Java EE では、標準の `<security-role>` 要素を使用します。ユーザー情報は Azure AD DS からライトウェイト ディレクトリ アクセス プロトコル (LDAP) を介して送信されます。

このガイドは、2 つの部分に分かれています。  セキュリティで保護された LDAP が公開されている Azure AD DS を既にお持ちの場合は、2 番目のパートに直接進むことができます。

* [Azure Active Directory の構成](#azure-active-directory-configuration)
* [WLS の構成](#wls-configuration)

このガイドでは、以下の方法について説明します。

> [!div class="checklist"]
> * Azure Active Directory Domain Services のマネージド ドメインを作成して構成する
> * Azure AD DS マネージド ドメイン用にセキュリティで保護されたライトウェイト ディレクトリ アクセス プロトコル (LDAP) を構成する
> * WebLogic Server が既定のセキュリティ領域として LDAP にアクセスできるようにする

このガイドを使用して既存の Azure AD デプロイを再構成することはできませんが、このガイドに従って、スキップできる手順を確認することは可能です。

## <a name="prerequisites"></a>前提条件

* 有効な Azure サブスクリプション
  * Azure サブスクリプションをお持ちでない場合は、[アカウントを作成](https://azure.microsoft.com/free/)してください。
* 「[Oracle WebLogic Server Azure アプリケーション](/azure/virtual-machines/workloads/oracle/oracle-weblogic)」に記載されている、いずれかの WLS Azure アプリケーションをデプロイできること。

## <a name="migration-context"></a>移行のコンテキスト

オンプレミスの WLS インストールと Azure AD の移行について考慮すべきことをいくつか次に示します。

* LDAP 経由で Domain Services が公開されていない Azure AD テナントが既にある場合は、LDAP 機能を公開して WLS と統合する方法の説明をこのガイドで確認できます。
* ご自身のシナリオでオンプレミスの Active Directory フォレストを必要とする場合は、Azure AD を使用したハイブリッド ID ソリューションの実装を検討してください。  詳細については、「[ハイブリッド ID のドキュメント](/azure/active-directory/hybrid/)」を参照してください。
* オンプレミスの Active Directory Domain Services (AD DS) デプロイが既にある場合は、「[自己管理型の Active Directory Domain Services、Azure Active Directory、およびマネージド Azure Active Directory Domain Services の比較](/azure/active-directory-domain-services/compare-identity-solutions)」にアクセスして移行パスを確認してください。
* クラウド向けに最適化する場合は、Azure AD DS LDAP と WLS を使用してゼロから始める方法の説明をこのガイドで確認できます。

## <a name="azure-active-directory-configuration"></a>Azure Active Directory の構成

このセクションでは、WLS と統合された Azure AD DS インスタンスを立ち上げるためのすべての手順について説明します。  Azure Active Directory では、ライトウェイト ディレクトリ アクセス プロトコル (LDAP) や Secure LDAP は直接サポートされません。 代わりに、Azure AD テナント内の Azure AD Domain Services (Azure AD DS) インスタンスを介してサポートが有効になります。

>[!NOTE]
> このガイドでは、Azure AD DS の "クラウド専用" ユーザー アカウント機能を使用します。  その他のユーザー アカウントの種類もサポートされていますが、このガイドでは説明しません。

### <a name="create-and-configure-an-azure-active-directory-domain-services-managed-domain"></a>Azure Active Directory Domain Services のマネージド ドメインを作成して構成する

このセクションでは、Azure AD DS マネージド ドメインを立ち上げるために、別のチュートリアルについて説明します。

チュートリアル「[Azure Active Directory Domain Services のマネージド ドメインを作成して構成する](/azure/active-directory-domain-services/tutorial-create-instance)」を、セクション「[Azure AD DS のユーザー アカウントを有効にする](/azure/active-directory-domain-services/tutorial-create-instance#enable-user-accounts-for-azure-ad-ds)」まで (これは含みません) 完了します。  そのセクションには、次のセクションで説明するように、このチュートリアルとの関連で特別な処理が必要です。  DNS アクションを完全かつ正確に完了するようにしてください。

"マネージド ドメインの DNS ドメイン名の入力" 手順を完了するときに指定した値を書き留めます。  この記事の後半で、その値を使用します。

### <a name="create-users-and-reset-passwords"></a>ユーザーを作成してパスワードをリセットする

このセクションには、ユーザーを作成してそのパスワードを変更する手順が記載されています。これは、LDAP を介してユーザーを正常に伝播させるために必要です。  既存の Azure AD DS インストールがある場合、この手順は必要ないことがあります。

1. Azure portal 内で、Azure AD テナントに対応するディレクトリが現在アクティブなディレクトリであることを確認します。  正しいディレクトリを選択する方法については、「[Azure サブスクリプションを Azure Active Directory テナントに関連付けるまたは追加する](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)」を参照してください。  正しくないディレクトリが選択されている場合は、ユーザーを作成できないか、間違ったディレクトリにユーザーが作成されてしまいます。
1. Azure portal の上部にある検索ボックスに、「ユーザー」と入力します。
1. **[ 新規ユーザー]** を選択します。
1. **[ユーザーの作成]** が選択されていることを確認します。
1. [ユーザー名]、[名前]、[名]、および [姓] の値を入力します。  残りのフィールドは既定値のままにします。
1. **［作成］** を選択します
1. テーブル内の新しく作成したユーザーを選択します。
1. **[パスワードのリセット]** を選択します。
1. 表示されるパネルで、 **[パスワードのリセット]** を選択します。
1. 一時パスワードを書き留めます。
1. "incognito" のブラウザー ウィンドウで、[Azure portal](https://portal.azure.com/) にアクセスし、ユーザーの資格情報とパスワードを使用してログインします。
1. パスワードの変更を求められたら、変更します。  新しいパスワードを書き留めます。  これは後で使用します。
1. ログアウトして、"incognito" ウィンドウを閉じます。

有効にするユーザーごとに、" **[新規ユーザー]** を選択します" から "ログアウトして、閉じます" までの手順を繰り返します。

### <a name="allow-ldap-in-azure-ad-ds"></a>Azure AD DS で LDAP を許可する

このセクションでは、WLS の構成に使用する値を抽出するために、別のチュートリアルについて説明します。

まず、別のブラウザー ウィンドウでチュートリアル「[Azure Active Directory Domain Services のマネージド ドメイン用に Secure LDAP を構成する](/azure/active-directory-domain-services/tutorial-configure-ldaps)」を開きます。これで、そのチュートリアルに目を通しながら、次の相違点について確認できます。  

「[クライアント コンピューター用の証明書をエクスポートする](/azure/active-directory-domain-services/tutorial-configure-ldaps#export-a-certificate-for-client-computers)」セクションに達したら、末尾が *.cer* の証明書ファイルを保存した場所を書き留めておきます。  この証明書は、WLS の構成への入力として使用します。

「[インターネット経由での Secure LDAP アクセスをロック ダウンする](/azure/active-directory-domain-services/tutorial-configure-ldaps#lock-down-secure-ldap-access-over-the-internet)」セクションに達したら、ソースとして **[すべて]** を指定します。  このガイドの後の方で、特定の IP アドレスを指定してセキュリティ規則を強化します。

「[マネージド ドメインに対するクエリをテストする](/azure/active-directory-domain-services/tutorial-configure-ldaps#test-queries-to-the-managed-domain)」の手順を実行する前に、次の手順を実行してテストが成功するようにします。

   1. ポータルで、Azure AD Domain Services インスタンスの概要ページにアクセスします。
   1. [設定] 領域で **[プロパティ]** を選択します。
   1. ページの右側で、 **[管理者グループ]** が表示されるまで下にスクロールします。  この見出しの下に **AAD DC Administrators** のリンクがあるはずです。  そのリンクを選択します。
   1. **[管理]** セクションで、 **[メンバー]** を選択します。
   1. **[メンバーの追加]** を選択します。
   1. **[検索]** テキスト フィールドに、前の手順で作成したいずれかのユーザーを検索するための文字を入力します。
   1. そのユーザーを選択し、 **[選択]** ボタンをアクティブにします。
   1. このユーザーは、「**マネージド ドメインに対するクエリをテストする**」セクションの手順を実行するときに使用する必要があります。
   >[!NOTE]
   >
   > WLS の構成に必要な値を収集するために行う必要がある LDAP データのクエリに関するヒントをいくつか次に示します。
   >
   > * このチュートリアルでは、Windows プログラム *LDP.exe* の使用を推奨しています。  同じ目的で [Apache Directory Studio](https://directory.apache.org/studio/downloads.html) を使用することもできます。
   > * *LDP.exe* を使用して LDAP にログインする場合、ユーザー名は @ の前の部分だけです。  たとえば、ユーザーが `alice@contoso.onmicrosoft.com` である場合、*LDP.exe* バインド アクションのユーザー名は `alice` となります。  また、後続の手順で使用するために、*LDP.exe* は実行およびログインしたままにしておいてください。
   >
「[外部アクセスのための DNS ゾーンを構成する](/azure/active-directory-domain-services/tutorial-configure-ldaps#configure-dns-zone-for-external-access)」セクションで、 **[Secure LDAP 外部 IP アドレス]** の値を書き留めます。  これは後で使用します。

このガイドで指示されるまで、「[リソースをクリーンアップする](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources)」の手順を実行しないでください。

上記の相違点を念頭に置いて、「[Azure Active Directory Domain Services のマネージド ドメイン用に Secure LDAP を構成する](/azure/active-directory-domain-services/tutorial-configure-ldaps)」を完了します。  これで、WLS の構成に提供するために必要な値を収集できるようになりました。

## <a name="wls-configuration"></a>WLS の構成

このセクションは、前にデプロイした Azure AD DS からパラメーター値を収集するのに役立ちます。

「[Oracle WebLogic Server Azure アプリケーション](/azure/virtual-machines/workloads/oracle/oracle-weblogic)」に記載されているいずれかの Azure アプリケーションをデプロイするときに、既存の LDAP サーバーにデプロイを自動的に接続させることを選択できます。  あるいは、Active Directory 統合サブテンプレートを呼び出して、後で LDAP 接続を構成することもできます。  この方法は、[公式ドキュメント](https://wls-eng.github.io/arm-oraclelinux-wls/)の付録 A に記載されています。  どちらの方法でも、ARM テンプレートに渡すために必要なパラメーター値を用意する必要があります。

| パラメーター名 | 説明   | 詳細 | 
|----------------|---------------|---------|
| `aadsServerHost` | サーバー ホスト | この値は、「[Azure Active Directory Domain Services のマネージド ドメインを作成して構成する](/azure/active-directory-domain-services/tutorial-create-instance)」の完了時に保存したパブリック DNS 名です。 |
| `aadsPublicIP` | Secure LDAP 外部 IP アドレス | この値は、「**外部アクセスのための DNS ゾーンを構成する**」セクションで保存した [[Secure LDAP 外部 IP アドレス]](/azure/active-directory-domain-services/tutorial-configure-ldaps#configure-dns-zone-for-external-access) です。|
| `wlsLDAPPrincipal` | プリンシパル   | *LDP.exe* に戻ります。  `wlsLDAPPrincipal` の追加の値を取得するには、次の手順を実行します。 <ol><li>**[表示]** メニューの **[ツリー]** を選択します。</li><li>**[ツリー ビュー]** ダイアログで、 **[ベース DN]** を空白のままにして、 **[OK]** を選択します。</li><li>右側のペインを右クリックし、 **[出力のクリア]** を選択します。</li><li>左側のツリー ビューを展開し、"OU=AADDC Users" で始まるエントリを選択します。</li><li>**[参照]** メニューで、 **[検索]** を選択します。</li><li>表示されるダイアログで、既定値をそのまま使用し、 **[実行]** を選択します。</li><li>右側のペインに出力が表示されたら、 **[実行]** の横にある **[閉じる]** を選択します。</li><li>出力を調べて、"AAD DC Administrators" グループに追加したユーザーに対応する **Dn** エントリを探します。  これは、**Dn:CN=&lt;ユーザー名&gt;OU=AADDC Users"** で始まります。</li></ol> |
| `wlsLDAPGroupBaseDN` および `wlsLDAPUserBaseDN` | ユーザー ベース DN とグループ ベース DN | このチュートリアルでは、これらの両方のプロパティの値は同じです。最初のコンマの後の **wlsLDAPPrincipal** の部分です。|
| `wlsLDAPPrincipalPassword` | プリンシパルのパスワード | この値は、**AAD DC Administrators** グループに追加されたユーザーのパスワードです。 |
| `wlsLDAPProviderName` | プロバイダー名 | この値は、既定値のままにしておくことができます。  これは、WLS で認証プロバイダーの名前として使用されます。 |
| `wlsLDAPSSLCertificate` | SSL 構成のトラスト キーストア | この値は、「[クライアント コンピューター用の証明書をエクスポートする](/azure/active-directory-domain-services/tutorial-configure-ldaps#export-a-certificate-for-client-computers)」手順の完了時に別に保存するように求められた、Base 64 でエンコードされた *.cer* ファイルです。  この値は、次の UNIX または PowerShell コマンドを使用して取得できます。 <br /> bash: <br /> `base64 your-certificate.cer -w 0 >temp.txt` <br /> PowerShell: <br /> `$Content = Get-Content -Path .\your-certificate.cer -Encoding Byte`<br /> `$Base64 = [System.Convert]::ToBase64String($Content)` <br /> `$Base64 | Out-File .\temp.txt`

### <a name="integrating-azure-ad-ds-ldap-with-wls"></a>Azure AD DS LDAP と WLS の統合

上記の構成値を入手し、Azure AD DS LDAP がデプロイされて、構成を開始できるようになりました。  このプロセスを完了する方法は、2 つあります。

#### <a name="during-wls-deployment"></a>WLS のデプロイ時

「[Oracle WebLogic Server Azure アプリケーション](/azure/virtual-machines/workloads/oracle/oracle-weblogic)」にアクセスし、管理またはいずれかのクラスターのオファーを選択します。  オファーのデプロイ中に、デプロイ プロセスのいずれかのタブが **[Azure Active Directory]** になります。  **[Connect to Azure Active Directory]\(Azure Active Directory に接続する\)** を **[はい]** に切り替えます。  前のセクションで収集した情報を使用して、値を入力します。  証明書については、`.cer` ファイルを直接アップロードする必要があります。

#### <a name="after-wls-deployment"></a>WLS のデプロイ後

デプロイ時に **[Connect to Azure Active Directory]\(Azure Active Directory に接続する\)** を **[はい]** に切り替えなかった場合は、前のセクションで収集した値を使用して後で構成を行うことができます。  詳細については[公式ドキュメント](https://wls-eng.github.io/arm-oraclelinux-wls/)を参照してください。

### <a name="validate-the-deployment"></a>デプロイの検証

上記の 2 つの方法のいずれかを使用して、WLS のデプロイと LDAP の構成を行った後、これらの手順に従って、統合が成功したことを確認します。

1. WLS の管理コンソールにアクセスします。
1. 左側のナビゲーターで、ツリーを展開して **[Security Realms]\(セキュリティ領域\)**  ->  **[myrealm]**  ->  **[Providers]\(プロバイダー\)** の順に選択します。
1. 統合が成功した場合は、`AzureActiveDirectoryProvider` などの AAD プロバイダーが見つかります。
1. 左側のナビゲーターで、ツリーを展開して **[Security Realms]\(セキュリティ領域\)**  ->  **[myrealm]**  ->  **[Users and Groups]\(ユーザーとグループ\)** の順に選択します。
1. 統合が成功した場合は、AAD プロバイダーのユーザーが見つかります。

### <a name="lock-down-and-secure-ldap-access-over-the-internet"></a>インターネット経由での LDAP アクセスのロック ダウンとセキュリティ保護

前の手順でセキュリティで保護された LDAP を立ち上げているときに、ネットワーク セキュリティ グループの `AllowLDAPS` 規則においてソースを **[すべて]** として設定しました。  これで WLS 管理サーバーがデプロイされて LDAP に接続されたので、Azure portal を使用してパブリック IP アドレスを取得します。  「[インターネット経由での Secure LDAP アクセスをロック ダウンする](/azure/active-directory-domain-services/tutorial-configure-ldaps?branch=pr-en-us-778#lock-down-secure-ldap-access-over-the-internet)」に再度アクセスして、 **[すべて]** を、WLS 管理サーバーの特定の IP アドレスに変更します。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

ここでは、「[Azure Active Directory Domain Services のマネージド ドメイン用に Secure LDAP を構成する](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources)」の「[リソースをクリーンアップする](/azure/active-directory-domain-services/tutorial-configure-ldaps#clean-up-resources)」セクションの手順に従います。

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [WebLogic アプリケーションを Azure Virtual Machines に移行する](/azure/developer/java/migration/migrate-weblogic-to-virtual-machines)
