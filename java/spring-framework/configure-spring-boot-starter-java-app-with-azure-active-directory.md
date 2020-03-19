---
title: Azure Active Directory 用の Spring Boot Starter の使用方法
description: Azure Active Directory スターターを使用して、Spring Boot Initializer アプリを構成する方法について説明します。
services: active-directory
documentationcenter: java
ms.date: 03/05/2020
ms.service: active-directory
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 9b2915f6f754a62d53ecebb9251778c76fccce9a
ms.sourcegitcommit: 9f9f5c51472dbdd7b9304b02364ed136dcf81f1c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79139557"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory"></a>チュートリアル:Azure Active Directory 用の Spring Boot Starter を使用して Java Web アプリをセキュリティで保護する

## <a name="overview"></a>概要

この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した **[Spring Initializr]** で Java アプリを作成する方法について説明します。

このチュートリアルでは、以下の内容を学習します。

 * Spring Initializr を使用して Java アプリケーションを作成する
 * Azure Active Directory を構成する
 * Spring Boot クラスと注釈を使用してアプリケーションをセキュリティで保護する
 * Java アプリケーションをビルドしてテストする

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

## <a name="prerequisites"></a>前提条件

この記事の手順を実行するには、次の前提条件を満たす必要があります。

* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-an-app-using-spring-initializr"></a>Spring Initializr を使用したアプリの作成

1. <https://start.spring.io/> を参照します。

1. **Java** で **Maven** プロジェクトを生成することを指定し、アプリケーションの **[グループ]** と **[アーティファクト]** に名前を入力します。

   ![グループとアーティファクトの名前を指定する][create-spring-app-01]

1. 下にスクロールして、**Spring Web**、**Azure Active Directory**、および **Spring Security** の**依存関係**を追加します。

1. ページの下部にある **[生成]** ボタンをクリックします。

   ![セキュリティ、Web、および Azure Active Directory スターターを選択する][create-spring-app-02]

1. メッセージが表示されたら、ローカル コンピューター上のパスにプロジェクトをダウンロードします。

## <a name="create-azure-active-directory-instance"></a>Azure Active Directory インスタンスの作成

### <a name="create-the-active-directory-instance"></a>Active Directory インスタンスを作成する

1. <https://portal.azure.com> にログインします。

1. **[+リソースの作成]** 、 **[ID]** 、 **[Azure Active Directory]** の順にクリックします。

   ![新しい Azure Active Directory インスタンスを作成する][create-directory-01]

1. **組織名**と**初期ドメイン名**を入力します。 ディレクトリの完全な URL をコピーします。このチュートリアルでは後ほど、これを使用してユーザー アカウントを追加します (例: `wingtiptoysdirectory.onmicrosoft.com`)。 

ディレクトリの完全な URL をコピーします。このチュートリアルでは後ほど、これを使用してユーザー アカウントを追加します (例: wingtiptoysdirectory.onmicrosoft.com)。

操作が完了したら、 **[作成]** をクリックします。 新しいリソースの作成には数分かかります。

   ![Azure Active Directory の名前を指定する][create-directory-02]

1. 完了したら、クリックして新しいディレクトリにアクセスします。

   ![Azure アカウント名を選択する][create-directory-03]

1. **テナント ID** をコピーします。この値を使用して、このチュートリアルの後の方で *application.properties* ファイルを構成します。

   ![テナント ID のコピー][create-directory-05]

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Spring Boot アプリのアプリケーション登録を追加する

1. ポータル メニューで **[アプリの登録]** をクリックしてから、 **[アプリケーションの登録]** をクリックします。

   ![新しいアプリ登録を追加する][create-app-registration-01]

1. アプリケーションを指定し、 **[登録]** をクリックします。

   ![新しいアプリ登録を作成する][create-app-registration-02]

1. アプリ登録のページが表示されたら、**アプリケーション ID** と**テナント ID** をコピーします。このチュートリアルでは後ほど、これらの値を使用して *application.properties* ファイルを構成します。

   ![アプリの登録キーを作成する][create-app-registration-03]

1. 左側のナビゲーション ウィンドウで、 **[証明書とシークレット]** をクリックします。  次に、 **[新しいクライアント シークレット]** をクリックします。

   ![アプリの登録キーを作成する][create-app-registration-03.5]

1. **説明**を追加し、 **[有効期限]** 一覧で期間を選択します。  **[追加]** をクリックします。 キーの値は自動的に入力されます。

   ![アプリの登録キーのパラメーターを指定する][create-app-registration-04]

1. このチュートリアルの後の方で *application.properties* ファイルを構成するために、クライアント シークレットの値をコピーして保存します。 (この値は後で取得することはできません)。

   ![アプリの登録キーのパラメーターを指定する][create-app-registration-04.5]

1. 左側のナビゲーション ウィンドウで **[API のアクセス許可]** をクリックします。 

1. **[API のアクセス許可]** ページで、 **[管理者の同意を与える]** をクリックし、メッセージが表示されたら **[はい]** をクリックします。

   ![アクセス許可を付与する][create-app-registration-08]

1. アプリ登録のメイン ページで、 **[認証]** をクリックし、 **[プラットフォームを追加]** をクリックします。  次に、 **[Web アプリケーション]** をクリックします。

    ![応答 URL を編集する][create-app-registration-09]

1. <http:<span></span>//localhost:8080/login/oauth2/code/azure> を新しい**リダイレクト URI** として入力し、 **[構成]** をクリックします。

    ![新しい応答 URL を追加する][create-app-registration-10]

1. アプリ登録のメイン ページで、 **[マニフェスト]** をクリックして `oauth2AllowImplicitFlow` パラメーターの値を `true` に設定し、 **[保存]** をクリックします。

    ![アプリケーション マニフェストの構成][create-app-registration-11]

    > [!NOTE]
    > 
    > `oauth2AllowImplicitFlow` パラメーターとその他のアプリケーション設定の詳細については、「[Azure Active Directory アプリケーション マニフェスト][AAD app manifest]」を参照してください。 
    >

### <a name="add-a-user-account-to-your-directory-and-add-that-account-to-a-group"></a>ディレクトリにユーザー アカウントを追加し、そのアカウントをグループに追加する

1. Active Directory の **[概要]** ページで、 **[すべてのユーザー]** をクリックし、次に **[新しいユーザー]** をクリックします。

   ![新しいユーザー アカウントを追加する][create-user-01]

1. **[ユーザー]** パネルが表示されたら、 **[ユーザー名]** と **[名前]** を入力します。  **[Create]** をクリックします。

   ![ユーザー アカウント情報を入力する][create-user-02]

   > [!NOTE]
   > 
   > ユーザー名を入力するときに、以下のように、このチュートリアルで先に出てきたディレクトリの URL を指定する必要があります。
   >
   > `wingtipuser@wingtiptoysdirectory.onmicrosoft.com`
   > 

1. **[グループ]** をクリックして、アプリケーションの承認に使用する**新しいグループを作成**します。

1. **[メンバーが選択されていません]** をクリックします (このチュートリアルでは、*users* という名前のグループを作成します)。前の手順で作成されたユーザーを検索します。  **[選択]** をクリックして、ユーザーをグループに追加します。  **[作成]** をクリックして、新しいグループを作成します。

   ![グループのユーザーの選択][create-user-03]

1. **[ユーザー]** パネルに戻り、 **[パスワードのリセット]** をクリックしてパスワードをコピーします。このチュートリアルで後ほどアプリケーションにログインするときに、これを使用します。 

   ![パスワードを表示する][create-user-04]

## <a name="configure-and-compile-your-app"></a>アプリの構成およびコンパイル

1. このチュートリアルで先ほど作成しダウンロードしたプロジェクト アーカイブからディレクトリにファイルを抽出します。

1. プロジェクトの親フォルダーに移動し、テキスト エディターで `pom.xml` Maven プロジェクト ファイルを開きます。

1. Spring OAuth2 セキュリティの依存関係を `pom.xml` に追加します。

   ```xml
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-client</artifactId>
   </dependency>
   <dependency>
      <groupId>org.springframework.security</groupId>
      <artifactId>spring-security-oauth2-jose</artifactId>
   </dependency>
   ```

1. *pom.xml* ファイルを保存して閉じます。

1. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.properties* ファイルを開きます。

1. 前に作成した値を使用して、アプリ登録の設定を指定します。たとえば、以下のとおりです。

   ```yaml
   # Specifies your Active Directory ID:
   azure.activedirectory.tenant-id=22222222-2222-2222-2222-222222222222

   # Specifies your App Registration's Application ID:
   spring.security.oauth2.client.registration.azure.client-id=11111111-1111-1111-1111-1111111111111111

   # Specifies your App Registration's secret key:
   spring.security.oauth2.client.registration.azure.client-secret=AbCdEfGhIjKlMnOpQrStUvWxYz==

   # Specifies the list of Active Directory groups to use for authorization:
   azure.activedirectory.active-directory-groups=Users
   ```
   各値の説明:

   | パラメーター | 説明 |
   |---|---|
   | `azure.activedirectory.tenant-id` | 前の Active Directory の **ディレクトリ ID** を指定します。 |
   | `spring.security.oauth2.client.registration.azure.client-id` | 以前に完了したアプリ登録の**アプリケーション ID** を指定します。 |
   | `spring.security.oauth2.client.registration.azure.client-secret` | 以前に完了したアプリ登録キーの**値** を指定します。 |
   | `azure.activedirectory.active-directory-groups` | 承認に使用する Active Directory グループの一覧を指定します。 |

   > [!NOTE]
   > 
   > *application.properties* ファイルで使用できる値の完全な一覧については、GitHub にある [Azure Active Directory Spring Boot のサンプル][AAD Spring Boot Sample]を参照してください。
   >

1. *application.properties* ファイルを保存して閉じます。

1. アプリケーションの Java ソース フォルダーに、"*controller*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/controller*)。

1. *controller* フォルダーに "*HelloController.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

1. 次のコードを入力し、ファイルを保存して閉じます。

   ```java
   package com.wingtiptoys.security;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.security.access.prepost.PreAuthorize;
   import org.springframework.security.oauth2.client.OAuth2AuthorizedClient;
   import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
   import org.springframework.ui.Model;

   @RestController
   public class HelloController {
      @Autowired
      @PreAuthorize("hasRole('Users')")
      @RequestMapping("/")
      public String helloWorld() {
         return "Hello World!";
      }
   }
   ```
   > [!NOTE]
   > 
   > `@PreAuthorize("hasRole('')")` メソッドに指定するグループ名には、*application.properties* ファイルの `azure.activedirectory.active-directory-groups` フィールドに指定したグループのいずれかが含まれている必要があります。
   > 
   > 異なる要求マッピングには異なる承認設定を指定することもできます。以下に例を示します。
   >
   > ``` java
   > public class HelloController {
   >    @Autowired
   >    @PreAuthorize("hasRole('Users')")
   >    @RequestMapping("/")
   >    public String helloWorld() {
   >       return "Hello Users!";
   >    }
   >    @PreAuthorize("hasRole('Group1')")
   >    @RequestMapping("/Group1")
   >    public String groupOne() {
   >       return "Hello Group 1 Users!";
   >    }
   >    @PreAuthorize("hasRole('Group2')")
   >    @RequestMapping("/Group2")
   >    public String groupTwo() {
   >       return "Hello Group 2 Users!";
   >    }
   > }
   > ```
   >    

1. アプリケーションの Java ソース フォルダーに、"*security*" という名前のフォルダーを作成します (例: *src/main/java/com/wingtiptoys/security/security*)。

1. *security* フォルダーに "*WebSecurityConfig.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

1. 次のコードを入力し、ファイルを保存して閉じます。

    ```java
    package com.wingtiptoys.security;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.oauth2.client.oidc.userinfo.OidcUserRequest;
    import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
    import org.springframework.security.oauth2.core.oidc.user.OidcUser;

    @EnableWebSecurity
    @EnableGlobalMethodSecurity(prePostEnabled = true)
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Autowired
        private OAuth2UserService<OidcUserRequest, OidcUser> oidcUserService;

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .oauth2Login()
                .userInfoEndpoint()
                .oidcUserService(oidcUserService);
        }
    }
    ```

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、ディレクトリをアプリの *pom.xml* ファイルがあるフォルダーに変更します。

1. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

   ![アプリをビルドする][build-application]

1. Maven によってアプリケーションがビルドされ、起動したら、Web ブラウザーでhttp:<span></span>//localhost:8080 を開きます。ユーザー名とパスワードを入力するように求めるメッセージが表示されます。

   ![アプリケーションへのログイン][application-login]

   > [!NOTE]
   > 
   > 新しいユーザー アカウントに初めてログインする場合、パスワードの変更を求められることがあります。
   > 
   > ![パスワードの変更][update-password]
   > 

1. 正常にログインしたら、コントローラーにサンプルの "Hello World" テキストが表示されます。

   ![正常なログイン][hello-world]

   > [!NOTE]
   > 
   > 承認されていないユーザー アカウントには、**HTTP 403 Unauthorized** メッセージが表示されます。
   >

## <a name="summary"></a>まとめ

このチュートリアルでは、Azure Active Directory スターターを使用した新しい Java Web アプリケーションの作成、新しい Azure AD テナントの構成とそのテナントへの新しいアプリケーションの登録を行いました。また、Spring の注釈とクラスを使用して Web を保護するようにアプリケーションを構成しました。

## <a name="see-also"></a>関連項目
* 新しい UI オプションについて詳しくは、「[新しい Azure portal アプリの登録トレーニング ガイド](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)」をご覧ください。

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](/azure/java/spring-framework)

<!-- URL List -->

[Azure Active Directory Documentation]: /azure/active-directory/
[AAD app manifest]: /azure/active-directory/develop/active-directory-application-manifest
[Get started with Azure AD]: /azure/active-directory/get-started-azure-ad
[Azure for Java Developers]: /azure/java/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[AAD Spring Boot Sample]: https://github.com/Microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-active-directory-spring-boot-backend-sample

<!-- IMG List -->

[create-spring-app-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-01.png
[create-spring-app-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-02.png
[create-spring-app-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-spring-app-03.png

[create-directory-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-01.png
[create-directory-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-02.png
[create-directory-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-03.png
[create-directory-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-04.png
[create-directory-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-directory-05.png

[create-app-registration-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-01.png
[create-app-registration-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-02.png
[create-app-registration-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.png
[create-app-registration-03.5]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-03.5.png
[create-app-registration-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.png
[create-app-registration-04.5]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-04.5.png
[create-app-registration-05]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-05.png
[create-app-registration-06]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-06.png
[create-app-registration-07]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-07.png
[create-app-registration-08]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-08.png
[create-app-registration-09]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-09.png
[create-app-registration-10]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-10.png
[create-app-registration-11]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-app-registration-11.png

[create-user-01]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-01.png
[create-user-02]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-02.png
[create-user-03]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-03.png
[create-user-04]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/create-user-04.png

[application-login]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/application-login.png
[build-application]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/build-application.png
[hello-world]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/hello-world.png
[update-password]: media/configure-spring-boot-starter-java-app-with-azure-active-directory/update-password.png
