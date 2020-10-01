---
title: Eclipse を使用して Azure App Service 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for Eclipse を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
keywords: java, eclipse, web アプリ, azure app service, hello world, クイック スタート
documentationcenter: java
author: selvasingh
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.reviewer: asirveda
ms.date: 08/25/2020
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: devx-track-java
ms.openlocfilehash: 57cb6f2b731ee6d9522787d70efbaf35e029c632
ms.sourcegitcommit: 717e32b68fc5f4c986f16b2790f4211967c0524b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586150"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-eclipse"></a>Eclipse を使用して Azure App Service 用の Hello World Web アプリを作成する

この記事では、[Azure Toolkit for Eclipse](https://marketplace.eclipse.org/content/azure-toolkit-eclipse) を使用して、基本的な Hello World Web アプリを作成して Azure App Service に発行するために必要な手順について説明します。

> [!NOTE]
>
> IntelliJ IDEA を使用したい場合は、[IntelliJ 用の同様のチュートリアル][intellij-hello-world]をご覧ください。
>
>[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]
>
> このチュートリアルを完了したら、忘れずにリソースをクリーンアップしてください。 その場合、このガイドの実行によって無料アカウントのクォータが超過することはありません。
>

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>インストールとサインイン

次の手順では、Eclipse 開発環境での Azure サインイン プロセスについて説明します。

1. プラグインをインストールしていない場合は、「[Azure Toolkit for Eclipse のインストール](installation.md)」を参照してください。

1. Azure アカウントにサインインするには、 **[Tools]\(ツール\)** 、 **[Azure]** 、 **[Sign In]\(サインイン\)** の順にクリックします。

   :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="Eclipse IDE で Azure にサインインします。":::

1. **[Azure サインイン]** ウィンドウで、**[Device Login]\(デバイスのログイン\)** を選択し、次に **[サインイン]** をクリックします ([他のサインイン オプション](sign-in-instructions.md))。

1. **[Azure Device Login]\(Azure デバイスのログイン\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

1. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

1. Azure アカウントを選択し、サインインするために必要な認証手順を完了します。

1. サインインしたら、ブラウザーを閉じて Eclipse IDE に戻ります。 **[Select Subscriptions]\(サブスクリプションの選択\)** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

### <a name="install-required-software-optional"></a>必要なソフトウェアのインストール " *(省略可能)* "

Web アプリ プロジェクトを操作するために必要なコンポーネントがあることを確認するには、次の手順に従います。

1. **[Help]\(ヘルプ\)** メニューを選択し、 **[Install New Software]\(新しいソフトウェアのインストール\)** をクリックします。

1. **[Available Software]\(使用可能なソフトウェア\)** ダイアログで、 **[Manage]\(管理\)** をクリックし、最新の Eclipse バージョン ( *[2020-06]* など) が選択されていることを確認します。

1. **[Apply and Close]\(適用して閉じる\)** をクリックします。 *[Work with:]\(連携:\)* ドロップダウン メニューを展開し て、推奨サイトを表示します。 最新の Eclipse バージョンのサイトを選択して、使用可能なソフトウェアを照会します。

1. 一覧を下にスクロールし、 **[Web, XML, Java EE and OSGi Enterprise Development]\(Web、XML、Java EE、および OSGi Enterprise Development\)** 項目を選択します。 **[次へ]** をクリックします。

1. [Install Details]\(インストールの詳細\) ウィンドウで、 **[Next]\(次へ\)** をクリックします。

1. [Review Licenses] ダイアログ ボックスで、使用許諾契約の条項を確認します。 ライセンス契約の条項に同意する場合は、 **[I accept the terms of the license agreements (ライセンス契約の条項に同意する)]** をクリックし、 **[Finish (完了)]** をクリックします。 

   > [!NOTE]
   > Eclipse ワークスペースの右下隅でインストールの進行状況を確認できます。

1. インストールを完了するために Eclipse を再起動するよう求めるメッセージが表示されたら、**[Restart Now]** をクリックします。

## <a name="creating-a-web-app-project"></a>Web アプリ プロジェクトの作成

1. **[File]\(ファイル\)** をクリックし、 **[New]\(新規\)** を展開して、 **[...Project]\(...プロジェクト\)** をクリックします。 [New Project]\(新しいプロジェクト\) ダイアログ ウィンドウ内で、 **[Web]** を展開し、 **[Dynamic Web Project]\(動的 Web プロジェクト\)** を選択し、 **[Next]\(次へ\)** をクリックします。

   > [!TIP]
   > 使用可能なプロジェクトとして **[Web]** が表示されない場合は、[こちらのセクション](#install-required-software-optional)を参照して、必要な Eclipse ソフトウェアがあることを確認してください。

1. このチュートリアルでは、プロジェクトに **MyWebApp**という名前を付けます。 画面は次のようになります。
   
   ![新しい動的 Web プロジェクトのプロパティ][dynamic-web-project-properties]

1. **[完了]** をクリックします。

1. 左側の [Package Explorer]\(パッケージ エクスプローラー\) ペインで、 **[MyWebApp]** を展開します。 **[WebContent]** を右クリックし、 **[New]\(新規\)** にマウス ポインターを置いて、 **[Other...]\(その他...\)** をクリックします。

1. **[Web]** を展開して、 **[JSP File]\(JSP ファイル\)** オプションを見つけます。 **[次へ]** をクリックします。

1. **[New JSP File (新しい JSP ファイル)]** ダイアログ ボックスで **index.jsp** ファイルに名前を付け、親フォルダーは **MyWebApp/WebContent** のままにして **[次へ]** をクリックします。

   ![[New JSP File\(新しい JSP ファイル\)] ダイアログ ボックス][new-jsp-file-dialog]

1. **[Select JSP Template]\(JSP テンプレートの選択\)** ダイアログ ボックスで、このチュートリアルのために **[New JSP File (html 5)]\(新しい JSP ファイル (html 5)\)** を選択し、 **[Finish]\(完了\)** をクリックします。

1. index.jsp ファイルが Eclipse で開いたら、"**Hello World!** " を動的に表示するためのテキストを 既存の `<body>` 要素に追加します。 更新された `<body>` コンテンツは、次のようになります。
   
   ```jsp
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. index.jsp を保存します。

## <a name="deploying-the-web-app-to-azure"></a>Azure への Web アプリのデプロイ

1. 左側の [Package Explorer]\(パッケージ エクスプローラー\) ペインでプロジェクトを右クリックし、 **[Azure]** 、 **[Publish as Azure Web App]\(Azure Web アプリとして発行\)** の順に選択します。
   
   ![[Publish as Azure Web App (Azure Web アプリとして発行)]][publish-as-azure-web-app]

1. **[Deploy Web App]\(Web アプリのデプロイ\)** ダイアログ ボックスが表示されたら、次のいずれかのオプションを選択できます。

   * 既存の Web アプリを選択します (存在する場合)。

   * 既存の Web アプリがない場合は、 **[Create]\(作成\)** をクリックします。

      ここで、ランタイム環境、アプリ サービス プラン リソース グループ、アプリ設定を構成できます。 必要に応じて、新しいリソースを作成してください。

      **[App Service の作成]** ダイアログ ボックスで Web アプリに必要な情報を指定し、**[作成]** をクリックします。

1. Web アプリを選択し、**[デプロイ]** をクリックします。

1. ツールキットにより Web アプリが正常にデプロイされると、**[Azure の活動ログ]** タブに **[発行済み]** 状態として表示され、デプロイされた Web アプリの URL へのハイパーリンクが設定されます。

1. ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。

   ![Web アプリの参照][browse-web-app]

## <a name="cleaning-up-resources"></a>リソースの後処理

1. Web アプリを Azure に発行した後、それを管理するには、Azure Explorer で右クリックし、コンテキスト メニューからオプションのいずれかを選択します。 たとえば、ここで Web アプリを **[削除]** して、このチュートリアルのリソースをクリーンアップできます。

   ![アプリ サービスの管理][manage-app-service]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="next-steps"></a>次のステップ

[!INCLUDE [additional-resources](includes/additional-resources.md)]

Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。

<!-- URL List -->

[Azure Toolkit for Eclipse]: /azure/developer/java/tookit-for-eclipse
[Azure Toolkit for IntelliJ]: ../toolkit-for-intellij
[intellij-hello-world]: ../toolkit-for-intellij/create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[browse-web-app]: media/create-hello-world-web-app/browse-web-app.png
[dynamic-web-project-properties]: media/create-hello-world-web-app/dynamic-web-project-properties.png
[new-jsp-file-dialog]: media/create-hello-world-web-app/new-jsp-file-dialog.png
[publish-as-azure-web-app]: media/create-hello-world-web-app/publish-as-azure-web-app.png
[publish-status]: media/create-hello-world-web-app/publish-status.png
[manage-app-service]: media/create-hello-world-web-app/manage-app-service.png