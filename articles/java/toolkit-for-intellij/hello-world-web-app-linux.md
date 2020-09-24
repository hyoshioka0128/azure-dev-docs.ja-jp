---
title: Linux コンテナーに Hello World Web アプリをデプロイする
titleSuffix: Azure Toolkit for IntelliJ
description: Linux コンテナーで基本的な Hello World Web アプリを実行し、Azure Toolkit for IntelliJ を使用してクラウドにデプロイします。
services: app-service\web
documentationcenter: java
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: df71e27850a467e03ab58edf6beaf085b195803b
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831978"
---
# <a name="deploy-java-app-to-azure-web-apps-for-containers-using-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ を使用して Azure Web Apps for Containers に Java アプリをデプロイする

[Docker] コンテナーは、Web アプリケーションをデプロイするために広く使用されている方法です。 Docker コンテナーを使用すると、開発者は、すべてのプロジェクト ファイルと依存関係を、サーバーにデプロイするために 1 つのパッケージに統合できます。 Azure Toolkit for IntelliJ には、Microsoft Azure にコンテナーをデプロイする機能が追加されているため、Java 開発者はこのプロセスを簡略化できます。

この記事では、基本的な Hello World Web アプリを作成し、Azure Toolkit for IntelliJ を使用して Azure 用の Linux コンテナーに Web アプリを発行するために必要な手順について説明します。

[!INCLUDE [prerequisites](includes/prerequisites.md)]
* [Docker] クライアント。

> [!NOTE]
>
> このチュートリアルの手順を実行するには、TLS を使用せず、ポート 2375 でデーモンを公開する [Docker] を構成する必要があります。 この設定は、Docker のインストール時、または、[Docker の設定] メニューから構成できます。
>
> ![[Docker の設定] メニュー][docker-settings-menu]
>

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

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Azure Container Registry をプライベート Docker レジストリとして使用するために作成する

Azure Portal を使用して Azure Container Registry を作成する手順を説明します。

> [!NOTE]
>
> Azure Portal ではなく Azure CLI を使用する場合は、「[Azure CLI 2.0 を使用したプライベート Docker コンテナー レジストリの作成][Create Docker Registry using Azure CLI]」の手順に従います。
>

1. [Azure Portal]を参照して、サインインします。

   Azure Portal のアカウントにサインインしたら、「[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]」の記事の手順に従います。便宜上、この手順を改めて以下で説明します。

1. **[+ リソースの作成]** のメニュー アイコンをクリックし、 **[コンテナー]** カテゴリをクリックし、 **[コンテナー レジストリ]** をクリックします。

1. **[コンテナー レジストリの作成]** ページが表示されたら、次の情報を指定します。

   * **サブスクリプション**:新しいコンテナー レジストリに使用する Azure サブスクリプションを指定します。

   * **リソース グループ**:コンテナー レジストリ用のリソース グループを指定します。 以下のオプションの 1 つを選択します。
      * **[新規作成]** : 新しいリソース グループを作成することを指定します。
      * **[Use Existing]\(既存の使用\)** :Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。

   * **レジストリ名**:新しいコンテナー レジストリの名前を指定します。

   * **[場所]** :コンテナー レジストリを作成するリージョン ("米国西部" など) を指定します。

   * **SKU**:コンテナー レジストリのサービス階層を指定します。 このチュートリアルでは、 *[Basic]\(ベーシック\)* を選択します。 詳細については、「[Azure Container Registry サービス階層](/azure/container-registry/container-registry-skus)」を参照してください。

1. **[確認および作成]** をクリックし、情報が正しいことを確認します。 **[作成]** をクリックして完了します。

## <a name="deploy-your-web-app-in-a-docker-container"></a>Docker コンテナーに Web アプリを配置する

次の手順では、Web アプリの Docker サポートを構成し、Web アプリをにデプロイする手順について説明します。

1. 左側の **[プロジェクト]** タブでプロジェクトに移動し、対象のプロジェクトを右クリックします。 **[Azure]** を展開し、 **[Add Docker Support]\(Docker サポートの追加\)** をクリックします。

   既定の構成の Docker ファイルが自動的に作成されます。

   :::image type="content" source="media/hello-world-web-app-linux/docker-support-file.png" alt-text="Docker サポート ファイル。":::

1. Docker のサポートを追加したら、プロジェクト エクスプローラーでプロジェクトを右クリックし、 **[Azure]** を展開し、 **[Run on Web App for Containers]\(Web App for Containers で実行\)** をクリックします。

1. **[Run on Web App for Containers]\(Web App for Containers で実行\)** ダイアログ ボックスで、次の情報を入力します。

   * **Name**:Azure Toolkit に表示されるフレンドリ名を指定します。 

   * **[コンテナー レジストリ]** :この記事の前のセクションで作成したコンテナー レジストリをドロップダウン メニューから選択します。 **[サーバーの URL]** 、 **[ユーザー名]** 、 **[パスワード]** の各フィールドには、値が自動的に入力されます。

   * **[Image and tag]\(イメージとタグ\)** :コンテナー イメージ名を指定します。通常は、"*registry*.azurecr.io/*appname*: latest" の構文が使用されます。 
      * *registry* は、この記事の前のセクションで説明されているコンテナー レジストリを指します。 
      * *appname* は、Web アプリの名前です。 

   * **[Use Existing Web App]\(既存の Web アプリを使用\)** または **[Create New Web App]\(新しい Web アプリの作成\)** :コンテナーを既存の Web アプリに展開するか、新しい Web アプリを作成するかを指定します。 指定した**アプリ名**を使用して Web アプリの URL が作成されます (例: *wingtiptoys.azurewebsites.net*)。

   * **リソース グループ**:既存のリソース グループを使用するか、新しいリソース グループを作成するかを指定します。 

   * **[App Service プラン]** :既存の App Service プランを使用するか、新しい App Service プランを作成するかを指定します。 

1. 上記の設定の構成が完了したら、 **[実行]** をクリックします。 Web アプリが正常にデプロイされると、 **[実行]** ウィンドウに状態が表示されます。

1. Web アプリが発行されたら、以前に指定した Web アプリの URL (例: *wingtiptoys.azurewebsites.net*) を参照できます。

   ![Web アプリの参照][browsing-to-web-app]

## <a name="optional-modify-your-web-app-publish-settings"></a>省略可能:Web アプリの発行設定を変更する

1. Web アプリを発行すると、使用した設定が既定の設定として保存されます。ツール バーの緑色の矢印アイコンをクリックすると、Azure でアプリケーションを実行できます。 設定を変更するには、Web アプリのドロップダウン メニューをクリックし、 **[構成の編集]** をクリックします。

    :::image type="content" source="media/create-hello-world-web-app/edit-configuration-menu.png" alt-text="[構成の編集] メニュー。":::

1. **[Run/Debug Configurations]\(構成の実行/デバッグ\)** ダイアログ ボックスが表示されたら、既定の設定を変更し、 **[OK]** をクリックします。

## <a name="next-steps"></a>次のステップ

Docker の他のリソースについては、公式の [Docker の Web サイト][Docker]を参照してください。

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Azure Portal]: https://portal.azure.com/
[Azure Portal を使用したプライベート Docker コンテナー レジストリの作成]: /azure/container-registry/container-registry-get-started-portal
[Azure for Java Developers]: ../index.yml
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Create Docker Registry using Azure CLI]: /azure/container-registry/container-registry-get-started-azure-cli

[Docker]: https://www.docker.com/
[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[browsing-to-web-app]:  media/hello-world-web-app-linux/browsing-to-web-app.png
[docker-settings-menu]: media/hello-world-web-app-linux/docker-settings-menu.png