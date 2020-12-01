---
title: Azure Active Directory B2C 用の Spring Boot Starter を使用する
description: Azure Active Directory B2C スターターを使用して、Spring Boot Initializr アプリを構成する方法を学習します。
services: active-directory-b2c
documentationcenter: java
author: panli
manager: kevinzha
ms.author: edburns
ms.date: 10/23/2020
ms.service: active-directory-b2c
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.custom: devx-track-java
ms.openlocfilehash: aa2d60f969895acbcb56f74e909993ceb9b0d7d5
ms.sourcegitcommit: 5c7f5fef798413b1a304cc9ee31c8518b73f27eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2020
ms.locfileid: "93066303"
---
# <a name="tutorial-secure-a-java-web-app-using-the-spring-boot-starter-for-azure-active-directory-b2c"></a>チュートリアル:Azure Active Directory B2C 用の Spring Boot Starter を使用して Java Web アプリをセキュリティで保護する

この記事では、Azure Active Directory (Azure AD) 用 Spring Boot Starter を使用した [Spring Initializr](https://start.spring.io/) で Java アプリを作成する方法について説明します。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> * Spring Initializr を使用して Java アプリケーションを作成する
> * Azure Active Directory B2C を構成する
> * Spring Boot クラスと注釈を使用してアプリケーションをセキュリティで保護する
> * Java アプリケーションをビルドしてテストする

[Azure Active Directory](https://azure.microsoft.com/services/active-directory) は、Microsoft のクラウド規模のエンタープライズ ID ソリューションです。 [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory/external-identities/b2c/) では、Azure Active Directory の機能セットを補完することにより、顧客、消費者、および市民が企業-消費者間 (B2C) アプリケーションへのアクセスを管理できるようにします。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。 まだお持ちでない場合は、開始する前に[無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)を作成してください。
* サポートされている Java Development Kit (JDK)。 Azure での開発時に使用可能な JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
* [Apache Maven](http://maven.apache.org/) バージョン 3.0 以降。

## <a name="create-an-app-using-spring-initializr"></a>Spring Initializr を使用したアプリの作成

1. <https://start.spring.io/> を参照します。

2. このガイダンスに従って、値を入力します。 ラベルとレイアウトは、ここに示されている画像とは異なる場合があることに注意してください。

    * **[プロジェクト]** で、 **[Maven プロジェクト]** を選択します。
    * **[言語]** で、 **[Java]** を選択します。
    * **[Spring Boot]** で、 **[2.3.4]** を選択します。
    * **[グループ]** 、 **[アーティファクト]** および **[名前]** で、短い説明の文字列を使用して同じ値を入力します。 入力時に、UI によってこれらの一部が自動的に入力される場合があります。
    * **[依存関係]** ペインで、 **[依存関係の追加]** を選択します。 UI を使用して **Spring Web** と、**Spring Security** に対する依存関係を追加します。

   ![プロジェクトを生成するための値を入力する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/fill-in-the-values-to-generate-the-project.png)

    > [!NOTE]
    > Spring Initializr では、既定のバージョンとして Java 11 が使用されます。 このトピックで説明されている Spring Boot Starter を使用するには、代わりに Java 8 を選択する必要があります。


3. **[プロジェクトの生成]** を選択してから、ローカル コンピューター上のパスにプロジェクトをダウンロードします。 ダウンロードしたファイルを、プロジェクトに基づいて名前が付けられたディレクトリに移動し、そのファイルを解凍します。 ファイルのレイアウトは以下のようになるはずです。`yourProject` の代わりに、 **[グループ]** に入力した値が示されます。

    ```
    .
    ├── HELP.md
    ├── mvnw
    ├── mvnw.cmd
    ├── pom.xml
    └── src
        ├── main
        │   ├── java
        │   │   └── yourProject
        │   │       └── yourProject
        │   │           └── YourProjectApplication.java
        │   └── resources
        │       ├── application.properties
        │       ├── static
        │       └── templates
        └── test
            └── java
                └── yourProject
                    └── yourProject
                        └── YourProjectApplicationTests.java
    ```

## <a name="create-and-initialize-an-azure-active-directory-instance"></a>Azure Active Directory インスタンスを作成して初期化する

### <a name="create-the-active-directory-instance"></a>Active Directory インスタンスを作成する

1. <https://portal.azure.com> にログインします。

2. **[リソースの作成]** 、 **[ID]** 、 **[すべて表示]** の順に選択します。 **Azure Active Directory B2C** を検索します。

    ![新しい Azure Active Directory B2C インスタンスを作成する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-1-n.png)

3. **［作成］** を選択します

    ![B2C テナント名の取得](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-5-n.png)

4. **[Create a new Azure AD B2C Tenant]\(新しい Azure AD B2C テナントの作成\)** を選択します。

    ![新しい Azure Active Directory を作成する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-2-n.png)

5. **[組織名]** と **[初期ドメイン名]** では、適切な値を指定してから **[作成]** を選択します。

    ![Azure Active Directory を選択する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-3-n.png)

6. Active Directory の作成が完了したら、新しいディレクトリに移動します。 または `b2c` を検索し、 **[Azure AD B2C]** を選択します。

    ![Azure Active Directory B2C インスタンスの検索](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/az-4-n.ng.png)

### <a name="add-an-application-registration-for-your-spring-boot-app"></a>Spring Boot アプリのアプリケーション登録を追加する

1. 左の **[管理]** ペインで、 **[アプリの登録]** を選択し、次に **[新しい登録]** を選択します。

   ![Azure AD B2C [アプリの登録] 画面を示す Azure portal のスクリーンショット](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c1-n.png)

2. **[名前]** フィールドに上記の **[グループ]** の値を入力し、 **[リダイレクト URI (推奨)]** を「 *http://localhost:8080/home* 」に設定して、 **[登録]** を選択します。

   ![新しいアプリ登録を構成する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c4-n.png)

3. **[管理]** ペインに戻り、 **[Applications (Legacy)]\(アプリケーション (レガシ)\)** を選択し、作成したアプリケーション名を選択します。

   ![アプリケーションを更新する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c5-n.png)

4. **[プロパティ]** を選択し、 **[暗黙的フローを許可する]** コントロールを **[はい]** に設定します。
   
5. 他のフィールドは既定値のままにします。
    
6. **[保存]** を選択します。 アプリケーションの準備ができるまでしばらく時間がかかることがあります。
    
   ![アプリ キーを更新する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c6-n.png)

7. **[全般]** ペインで **[キー]** を選択し、 **[キーの生成]** を選択します。

8. **[アプリ キー]** を、上で **[グループ]** に入力した値に設定します。

9. **[保存]** を選択します。 キーがアプリ キー セクションに表示されるのを待ってから、この記事の後半で使用するためにコピーします。

    > [!NOTE]
    > **[キー]** セクションをそのままにして戻ると、キーの値を表示できなくなります。 その場合は、別のキーを作成し、今後の使用のためにコピーする必要があります。
    > 場合によっては、生成されたキーに、*application.yml* ファイルに含める場合に問題となる文字 (円記号やアクサングラーブなど) が含まれていることがあります。 その場合は、そのキーを破棄し、別のものを生成します。

    ![シークレットを作成する](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/b2c3-n.png)

10. **[概要]** を選択します。

11. 左ペインの **[ポリシー]** セクションで、 **[ユーザー フロー]** 、 **[新しいユーザー フロー]** の順に選択します。

12. ここでこのチュートリアルから離れて、別のチュートリアルを実行し、完了したらこのチュートリアルに戻ります。 他のチュートリアルに移動するときに注意すべきいくつかの点を以下に示します。

    * **[新しいユーザー フロー]** を選択するよう求める手順から開始します。
    * このチュートリアルで `webapp1` が示された場合は、代わりに **[グループ]** に入力した値を使用します。
    * フローから返される要求を選択する場合は、確実に **[表示名]** が選択されるようにしてください。 この要求がないと、このチュートリアルでビルドされるアプリは機能しません。
    * ユーザー フローを実行するように求められた場合は、前に指定したリダイレクト URL がまだアクティブになっていません。 フローは引き続き実行できますが、リダイレクトは正常に完了しません。 これは予期されることです。
    * 「次のステップ」に到達したら、このチュートリアルに戻ります。

    「[チュートリアル: Azure Active Directory B2C 内にユーザー フローを作成する](/azure/active-directory-b2c/tutorial-create-user-flows)」のすべての手順に従って、"サインアップとサインイン"、"プロファイルの編集" および "パスワードのリセット" のユーザー フローを作成します。

    Azure AD B2C は、ローカル アカウントとソーシャル ID プロバイダーをサポートしています。 GitHub ID プロバイダーを作成する例については、「[Azure Active Directory B2C を使用して GitHub アカウントでのサインアップおよびサインインを設定する](/azure/active-directory-b2c/identity-provider-github)」を参照してください。

## <a name="configure-and-compile-your-app"></a>アプリの構成およびコンパイル

Azure AD B2C インスタンスといくつかのユーザー フローを作成したので、Spring アプリを Azure AD B2C インスタンスに接続します。

1. コマンド ラインで、Spring Initializr からダウンロードした .zip ファイルを解凍したディレクトリに移動します。

2. プロジェクトの親フォルダーに移動し、テキスト エディターで *pom.xml* Maven プロジェクト ファイルを開きます。

3. Spring OAuth2 セキュリティの依存関係を *pom.xml* に追加します。

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-active-directory-b2c-spring-boot-starter</artifactId>
        <version>See Below</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
        <version>See Below</version>
    </dependency>
    <dependency>
        <groupId>org.thymeleaf.extras</groupId>
        <artifactId>thymeleaf-extras-springsecurity5</artifactId>
        <version>See Below</version>
    </dependency>
    ```

    `azure-active-directory-b2c-spring-boot-starter` には、使用可能な最新バージョンを使用します。 [mvnrepository.com](https://mvnrepository.com/ artifact/com.microsoft.azure/azure-active-directory-spring-boot-starter) を使用して、これを確認できる場合があります。 このドキュメントの更新時点では、最新バージョンは `2.3.5` です。

    `spring-boot-starter-thymeleaf` には、先ほど選択した Spring Boot のバージョン (`2.3.4.RELEASE` など) に対応するバージョンを使用します。

    `thymeleaf-extras-springsecurity5` には、使用可能な最新バージョンを使用します。 [mvnrepository.com](https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity5) を使用して、これを確認できる場合があります。 このドキュメントの執筆時点では、最新バージョンは `3.0.4.RELEASE` です。

4. *pom.xml* ファイルを保存して閉じます。

    * `mvn -DskipTests clean install` を実行して、依存関係が正しいことを確認します。 `BUILD SUCCESS` が表示されない場合は、問題をトラブルシューティングして解決してから続行してください。

5. プロジェクトの *src/main/resources* フォルダーに移動し、テキスト エディターで *application.yml* ファイルを作成します。

6. 前に作成した値を使用して、アプリ登録の設定を指定します。たとえば、以下のとおりです。

    ```yaml
    azure:
      activedirectory:
        b2c:
          tenant: ejb0518domain
          client-id: 11111111-1111-1111-1111-1111111111111111
          client-secret: '<yourAppKey>'
          reply-url: http://localhost:8080/home
          logout-success-url: http://localhost:8080/home
          user-flows:
            sign-up-or-sign-in: B2C_1_signupsignin1
            profile-edit: B2C_1_profileediting1
            password-reset: B2C_1_passwordreset1
    ```

    `client-secret` 値が単一引用符で囲まれていることに注目してください。 これが必要なのは、`<yourAppKey>` の値には、YAML に存在する場合に単一引用符で囲む必要があるいくつかの文字がほぼ確実に含まれるためです。

    > [!NOTE]
    > このドキュメントの執筆時点で、*application.yml* で使用できる Active Directory B2C Spring の統合の値の完全な一覧を以下に示します。
    >
    > ```
    > azure:
    >   activedirectory:
    >     b2c:
    >       tenant:
    >       oidc-enabled:
    >       client-id:
    >       client-secret:
    >       reply-url:  # should be absolute url.
    >       logout-success-url:
    >       user-flows:
    >         sign-up-or-sign-in:
    >         profile-edit: # optional
    >         password-reset: # optional
    > ```
    >
    > *application.yml* ファイルは、GitHub の [Azure Active Directory B2C Spring Boot のサンプル](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-b2c-oidc/src/main/resources/application.yml)で入手できます。

7. *application.yml* ファイルを保存して閉じます。

8. *src/main/java/<yourGroupId>/<yourGroupId>* に *controller* という名前のフォルダーを作成し、`<yourGroupId>` を、 **[グループ]** で入力した値に置き換えます。

9. *controller* フォルダーに "*java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

10. 次のコードを入力し、適宜、`yourGroupId` を変更してから、ファイルを保存して閉じます。

    ```java
    package yourGroupId.yourGroupId.controller;

    import org.springframework.security.oauth2.client.authentication.OAuth2AuthenticationToken;
    import org.springframework.security.oauth2.core.user.OAuth2User;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;

    @Controller
    public class WebController {

        private void initializeModel(Model model, OAuth2AuthenticationToken token) {
            if (token != null) {
                final OAuth2User user = token.getPrincipal();

                model.addAttribute("grant_type", user.getAuthorities());
                model.addAllAttributes(user.getAttributes());
            }
        }

        @GetMapping(value = "/")
        public String index(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "home";
        }

        @GetMapping(value = "/greeting")
        public String greeting(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "greeting";
        }

        @GetMapping(value = "/home")
        public String home(Model model, OAuth2AuthenticationToken token) {
            initializeModel(model, token);

            return "home";
        }
    }
    ```

    コントローラー内のすべてのメソッドで `initializeModel()` が呼び出され、そのメソッドで `model.addAllAttributes(user.getAttributes());` が呼び出されるため、*src/main/resources/templates* 内の任意の HTML ページから、`${name}`、`${grant_type}`、`${auth_time}` など、それらの属性のいずれかにアクセスできます。 `user.getAttributes()` から返される値は、実際には認証用の `id_token` の要求です。 使用可能な要求の完全な一覧については、「[Microsoft ID プラットフォームの ID トークン](/azure/active-directory/develop/id-tokens#payload-claims)」を参照してください。

11. *src/main/java/<yourGroupId>/<yourGroupId>* に *security* という名前のフォルダーを作成し、`yourGroupId` を、 **[グループ]** で入力した値に置き換えます。

12. *security* フォルダーに "*WebSecurityConfiguration.java*" という名前の新しい Java ファイルを作成し、テキスト エディターで開きます。

13. 次のコードを入力し、適宜、`yourGroupId` を変更してから、ファイルを保存して閉じます。

    ```java
    package yourGroupId.yourGroupId.security;

    import com.microsoft.azure.spring.autoconfigure.b2c.AADB2COidcLoginConfigurer;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

    @EnableWebSecurity
    public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

        private final AADB2COidcLoginConfigurer configurer;

        public WebSecurityConfiguration(AADB2COidcLoginConfigurer configurer) {
            this.configurer = configurer;
        }

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                    .authorizeRequests()
                    .anyRequest()
                    .authenticated()
                    .and()
                    .apply(configurer)
            ;
        }
    }
    ```

14. *greeting.html* および *home.html* ファイルを [Azure AD B2C Spring Boot Sample](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-b2c-oidc/src/main/resources/templates) から *src/main/resources/templates* にコピーし、`${your-profile-edit-user-flow}` と `${your-password-reset-user-flow}` を、前に作成したユーザー フローの名前に置き換えます。

## <a name="build-and-test-your-app"></a>アプリのビルドとテスト

1. コマンド プロンプトを開き、ディレクトリをアプリの *pom.xml* ファイルがあるフォルダーに変更します。

2. Spring Boot アプリケーションを Maven でビルドし、実行します。次に例を示します。

    > [!NOTE]
    > ローカルの Spring Boot アプリの実行に使用されるシステム クロックに準じた時刻が正確であることは、非常に重要です。 OAuth 2.0 を使用する場合、クロックのずれの許容範囲はごくわずかです。 誤差が 3 分でもサインインは失敗し、`[invalid_id_token] An error occurred while attempting to decode the Jwt: Jwt used before 2020-05-19T18:52:10Z` のようなエラーが示されることがあります。 このドキュメントの執筆時点では、[time.gov](https://time.gov/) に、クロックが実際の時刻からどれくらいずれているかを示すインジケーターがあります。 アプリは、+0.019 秒のずれで正常に実行されました。

    ```shell
    mvn -DskipTests clean package
    mvn -DskipTests spring-boot:run
    ```

3. Maven によってアプリケーションがビルドされ、起動したら、Web ブラウザーで `http://localhost:8080/` を開きます。ログイン ページにリダイレクトされます。

    ![ログイン ページ](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/lo1-n.png)

4. サインインに関するテキストを含むリンクを選択します。 Azure AD B2C にリダイレクトされ、認証プロセスが開始されるはずです。

5. 正常にログインすると、ブラウザーにサンプルの `home page` が表示されます。

    ![正常なログイン](media/configure-spring-boot-starter-java-app-with-azure-active-directory-b2c-oidc/lo3-n.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、Azure Active Directory B2C スターターを使用した新しい Java Web アプリケーションの作成、新しい Azure AD B2C テナントの構成とそこでの新しいアプリケーションの登録を行ってから、Spring の注釈とクラスを使用して Web アプリを保護するようにアプリケーションを構成しました。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

予想外の課金を防ぐために、この記事で作成したリソースが不要になったら、[Azure portal](https://portal.azure.com/) を使用して削除してください。

## <a name="next-steps"></a>次のステップ

Spring および Azure の詳細については、Azure ドキュメント センターで引き続き Spring に関するドキュメントをご確認ください。

> [!div class="nextstepaction"]
> [Azure の Spring](./index.yml)