---
title: IntelliJ を使用して Azure App Service 用の Hello World Web アプリを作成する
description: このチュートリアルでは、Azure Toolkit for IntelliJ を使用して、Azure 用の Hello World Web アプリを作成する方法について説明します。
services: app-service
keywords: java, intellij, web アプリ, azure app service, hello world, クイック スタート
documentationcenter: java
author: selvasingh
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.reviewer: asirveda
ms.date: 09/09/2020
ms.service: app-service
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: web
ms.custom: devx-track-java
ms.openlocfilehash: b340802d861ad1dbe1b4f118734e527cdefeec21
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831612"
---
# <a name="create-a-hello-world-web-app-for-azure-app-service-using-intellij"></a>IntelliJ を使用して Azure App Service 用の Hello World Web アプリを作成する

この記事では、[Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) を使用して、基本的な Hello World Web アプリを作成して Azure App Service に発行するために必要な手順について説明します。

> [!NOTE]
>
> Eclipse を使用したい場合は、[Eclipse 用の同様のチュートリアル][eclipse-hello-world]をご覧ください。
>
>[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]
>
> このチュートリアルを完了したら、忘れずにリソースをクリーンアップしてください。 その場合、このガイドの実行によって無料アカウントのクォータが超過することはありません。
>

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="installation-and-sign-in"></a>インストールとサインイン

次の手順では、IntelliJ 開発環境での Azure サインイン プロセスについて説明します。

1. プラグインをインストールしていない場合は、「[Azure Toolkit for IntelliJ のインストール](installation.md)」を参照してください。

1. Azure アカウントにサインインするには、左側の **[Azure Explorer]** サイドバーに移動し、 **[Azure サインイン]** アイコンをクリックします。 または、 **[ツール]** に移動し、 **[Azure]** を展開して、 **[Azure サインイン]** をクリックします。

   :::image type="content" source="media/sign-in-instructions/I01.png" alt-text="IntelliJ で Azure にサインインします。"::: 

1. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、次に **[サインイン]** をクリックします ([他のサインイン オプション](sign-in-instructions.md))。

1. **[Azure Device Login]\(Azure デバイスのログイン\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

1. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

1. Azure アカウントを選択し、サインインするために必要な認証手順を完了します。

1. サインインしたら、ブラウザーを閉じて IntelliJ IDE に戻ります。 **[Select Subscriptions]\(サブスクリプションの選択\)** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

## <a name="creating-a-new-web-app-project"></a>新しい Web アプリ プロジェクトの作成

1. **[File]\(ファイル\)** をクリックし、 **[New]\(新規\)** を展開して、 **[Project]\(プロジェクト\)** をクリックします。

1. **[New Project]\(新しいプロジェクト\)** ダイアログ ボックスで、 **[Maven]** を選択し、 **[Create from Archetype]\(アーキタイプから作成\)** オプションがオンになっていることを確認します。 一覧から **[maven-archetype-webapp]** を選択し、 **[Next]\(次へ\)** をクリックします。

   :::image type="content" source="media/create-hello-world-web-app/maven-archetype-webapp.png" alt-text="[maven-archetype-webapp] オプションを選択します。"::: 

1. **[Artifact Coordinates]\(成果物の調整\)** ドロップダウンを展開してすべての入力フィールドを表示し、新しい Web アプリに次の情報を指定して、 **[Next]\(次へ\)** をクリックします。

   * **Name**:Web アプリの名前です。 これは、Web アプリの **ArtifactId** フィールドに自動的に入力されます。
   * **GroupId**:成果物グループの名前です (通常は会社のドメイン)。 (例: *com.microsoft.azure*)
   * **バージョン**:既定のバージョン *1.0-SNAPSHOT* のままにします。

1. Maven 設定をカスタマイズするか、既定の設定をそのまま使用し、 **[Finish]\(完了\)** をクリックします。

1. 左側の **[Project]\(プロジェクト\)** タブでプロジェクトに移動し、ファイル **src/main/webapp/WEB-INF/index.jsp** を開きます。 コードを次の内容に置き換え、**変更を保存します**。

   ```html
   <html>
    <body>
      <b><% out.println("Hello World!"); %></b>
    </body>
   </html>
   ```
   :::image type="content" source="media/create-hello-world-web-app/open-index-page.png" alt-text="index.jsp ファイルを開きます。":::

## <a name="deploying-web-app-to-azure"></a>Azure への Web アプリのデプロイ

1. [Project Explorer] ビューでプロジェクトを右クリックし、 **[Azure]** を展開して、次に **[Deploy to Azure Web Apps]\(Azure Web Apps へのデプロイ\)** をクリックします。

1. [Deploy to Azure]\(Azure へのデプロイ\) ダイアログ ボックスでは、既存の Tomcat webapp にアプリケーションをデプロイすることも、新たに作成することもできます。

   a. **[No available webapp, click to create a new one]\(使用可能な webapp がありません。クリックして新規作成します\)** をクリックし、新しい webapp を作成します。 あるいは、サブスクリプションに既存の webapp がある場合は、WebApp ドロップダウンから **[Create New WebApp]\(新しい WebApp を作成\)** を選択します。

      :::image type="content" source="media/create-hello-world-web-app/deploy-to-azure-webapps.png" alt-text="[Deploy to Azure]\(Azure へのデプロイ\) ダイアログ ウィンドウ。":::

   ポップアップされる **[Create WebApp]\(WebApp の作成\)** ダイアログ ボックスで、次の情報を指定し、 **[OK]** をクリックします。 

      * **Name**:WebApp のドメイン名の文字列です。
      * **サブスクリプション**:新しい WebApp に使用する Azure サブスクリプションを指定します。
      * **Platform**: *[Linux]* を選択します。
      * **Web Container (Web コンテナー)** :*TOMCAT 9.0-jre8* または適宜選択します。
      * **リソース グループ**:自分の WebApp 用のリソース グループを指定します。 Azure アカウントに関連付けられている既存のリソース グループを選択することも、新規作成することもできます。
      * **[App Service プラン]** :WebApp の App Service プランを指定します。 Azure アカウントに関連付けられている既存のプランを選択することも、新規作成することもできます。

   b. 既存の webapp にデプロイするには、WebApp ドロップ ダウンから Web アプリを選択し、 **[Run]\(実行\)** をクリックします。

1. ツールキットにより Web アプリが正常にデプロイされた場合、状態メッセージと、デプロイされた Web アプリの URL (成功した場合) が表示されます。

1. ステータス メッセージに表示されたリンクを使用して、Web アプリを参照できます。

   :::image type="content" source="media/create-hello-world-web-app/browse-web-app.png" alt-text="Web アプリの参照。":::

## <a name="managing-deploy-configurations"></a>デプロイ構成の管理

> [!TIP]
> Web アプリを発行したら、ツール バーの緑色の矢印アイコンをクリックしてデプロイを実行できます。

1. WebApp のデプロイを実行する前に、Web アプリのドロップダウン メニューをクリックし、 **[Edit Configurations]\(構成の編集\)** を選択して、既定の設定を変更でき ます。

   :::image type="content" source="media/create-hello-world-web-app/edit-configuration-menu.png" alt-text="[Edit Configurations]\(構成の編集\) メニュー。":::

1. **[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスで、既定の設定をどれでも変更できます。 **[OK]** をクリックして設定を保存します。

## <a name="cleaning-up-resources"></a>リソースの後処理

1. Web アプリを削除するには、左側の **[Azure Explorer]** サイドバーに移動し、 **[Web Apps]\(Web アプリ\)** 項目を見つけます。 

   > [!NOTE]
   > Web Apps\(Web アプリ\) メニュー項目が展開されない場合は、Azure Explorer ツール バーの  **Refresh\(更新\)** アイコンをクリックするか、Web Apps\(Web アプリ\) メニュー項目を右クリックして **Refresh\(更新\)** を選択して、一覧を手動で最新の情報に更新してください。

1. 削除する Web アプリを右クリックし、 **[Delete]\(削除\)** をクリックします。

1. App Service プランまたはリソース グループを削除するには、 [Azure portal](https://portal.azure.com) にアクセスし、お使いのサブスクリプションのリソースを手動で削除してください。

## <a name="next-steps"></a>次のステップ

[!INCLUDE [additional-resources](includes/additional-resources.md)]

Azure Web Apps の作成の詳細については、「 [Web Apps の概要]」を参照してください。

<!-- URL List -->

[Azure Toolkit for IntelliJ]: /azure/developer/java/tookit-for-intellij
[Azure Toolkit for Eclipse]: /azure/developer/java/tookit-for-eclipse
[eclipse-hello-world]: ../toolkit-for-eclipse/create-hello-world-web-app.md
[Web Apps の概要]: /azure/app-service/app-service-web-overview
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/
[Legacy Version]: create-hello-world-web-app-legacy-version.md
[intelliJ-sign-in-instructions]: sign-in-instructions.md

<!-- IMG List -->
[marketplace]:media/create-hello-world-web-app/marketplace.png
[file-new-project]: media/create-hello-world-web-app/file-new-project.png
[maven-archetype-webapp]: media/create-hello-world-web-app/maven-archetype-webapp.png
[groupid-and-artifactid]: media/create-hello-world-web-app/groupid-and-artifactid.png
[maven-options]: media/create-hello-world-web-app/maven-options.png
[project-name]: media/create-hello-world-web-app/project-name.png
[open-index-page]: media/create-hello-world-web-app/open-index-page.png
[edit-index-page]: media/create-hello-world-web-app/edit-index-page.png
[deploy-to-azure-menu]: media/create-hello-world-web-app/run-on-web-app-menu.png
[deploy-to-azure-dialog]: media/create-hello-world-web-app/run-on-web-app-dialog.png
[deploy-to-existing-webapp]: media/create-hello-world-web-app/deploy-to-existing-webapp.png
[create-new-web-app-dialog]: media/create-hello-world-web-app/create-new-web-app-dialog.png
[successfully-deployed]: media/create-hello-world-web-app/successfully-deployed.png
[browse-web-app]: media/create-hello-world-web-app/browse-web-app.png
[edit-configuration-menu]: media/create-hello-world-web-app/edit-configuration-menu.png
[edit-configuration-dialog]: media/create-hello-world-web-app/edit-configuration-dialog.png
[clean-resources]: media/create-hello-world-web-app/clean-resource.png