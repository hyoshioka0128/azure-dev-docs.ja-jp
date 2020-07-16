---
title: Azure Pipelines を使用した MicroProfile アプリの CI/CD
description: MicroProfile アプリを Azure Web App for Containers インスタンスにデプロイするための CI/CD リリース サイクルを、Azure Pipelines を使用して設定する方法について説明します。
services: devops
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
ms.author: ruyakubu
ms.date: 07/31/2019
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.custom: devx-track-java
ms.openlocfilehash: 7ce992ca063daf0430ad3fc2d6882b820c2a2315
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379916"
---
# <a name="cicd-for-microprofile-apps-using-azure-pipelines"></a>Azure Pipelines を使用した MicroProfile アプリの CI/CD

このチュートリアルでは、[MicroProfile](http://microprofile.io) Java EE アプリケーションを Azure Web App for Containers にデプロイするための Azure Pipelines 継続的インテグレーションと継続的デプロイ (CI/CD) リリース サイクルのセットアップが簡単にできることを示します。 このチュートリアルの MicroProfile アプリでは、[Payara Micro](https://www.payara.fish/payara_micro) のベース イメージを使用して WAR ファイルを作成します。 

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
Azure Pipelines コンテナー化プロセスでは初めに、Docker イメージをビルドし、コンテナー イメージを Azure Container Registry (ACR) のインスタンスにプッシュします。 このプロセスの締めくくりとして、Azure Pipelines リリース パイプラインを作成し、コンテナー イメージを Web アプリにデプロイします。

## <a name="prerequisites"></a>前提条件

1. [Azure portal](https://portal.azure.com) で [Azure Container Registry](https://azure.microsoft.com/services/container-registry) のインスタンスを作成します。
   
1. Azure portal で [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/) のインスタンスを作成します。 **OS** には **Linux** を選択し、**[コンテナーの構成]** の **[イメージのソース]** で **[クイック スタート]** を選択します。  
   
1. クローン URL をサンプル GitHub リポジトリ ([https://github.com/Azure-Samples/microprofile-hello-azure](https://github.com/Azure-Samples/microprofile-hello-azure)) からコピーして保存します。
   
1. [Azure DevOps](https://dev.azure.com) 組織を新規登録するか既存の組織にログインし、新しい[プロジェクト](/vsts/organizations/projects/create-project)を作成します。 
   
1. サンプルの GitHub リポジトリを Azure Repos にインポートします。
   
   1. Azure DevOps プロジェクト ページの左側のナビゲーションにある **[リポジトリ]** を選択します。
   1. **[またはリポジトリをインポートする]** の下の **[インポート]** を選択します。 
   1. **[クローン URL]** の下で、保存した Git クローン URL を入力して **[インポート]** を選択します。
  
## <a name="create-a-build-pipeline"></a>ビルド パイプラインを作成する

Azure Pipelines の継続的インテグレーション ビルド パイプラインは、Java EE ソース アプリでコミットが行われるたびに、すべてのビルド タスクを自動的に実行します。 この例では、Azure Pipelines で Maven を使用して Java MicroProfile プロジェクトをビルドします。

1. Azure DevOps プロジェクト ページの左側のナビゲーションで **[パイプライン]**  >  **[ビルド]** を選択します。 
   
1. **[新しいパイプライン]** を選択します。
   
1. **[YAML なしでパイプラインを作成するには、従来のエディターを使用する]** を選択します。 
   
1. プロジェクト名とインポートした GitHub リポジトリが各フィールドに表示されていることを確認して、 **[続行]** を選択します。
   
1. テンプレートの一覧から **[Maven]** を選択して **[適用]** を選択します。
   
1. 右側のウィンドウの **[エージェント プール]** ドロップダウンに **[Hosted Ubuntu 1604]\(ホストされている Ubuntu 1604\)** があることを確認します。
   
   > [!NOTE]
   > この設定で、どのビルド サーバーを使用するかを Azure Pipelines に対して指定します。  プライベートの、カスタマイズされたビルド サーバーを使用することもできます。
   
1. パイプラインを継続的インテグレーション用に構成するために、左側のウィンドウで **[トリガー]** タブを選択し、 **[継続的インテグレーションを有効にする]** チェック ボックスをオンにします。  
   
1. ページの一番上にある **[保存してキューに登録]** ドロップダウンを選択し、 **[保存]** を選択します。 

   ![継続的インテグレーションの有効化](media/cicd-microprofile/continuous-integration.png)

## <a name="create-a-docker-build-image"></a>Docker ビルド イメージを作成する

Azure Pipelines での Docker イメージ作成に使用される Dockerfile では、Payara Micro からのベース イメージが指定されています。  

1. **[タスク]** タブを選択し、**[エージェント ジョブ 1]** の横にあるプラス記号 (**+**) を選択してタスクを追加します。
   
   ![新しいタスクを追加する](media/cicd-microprofile/add-task.png)
   
1. 右側のウィンドウのテンプレートの一覧から **[Docker]** を選択し、 **[追加]** を選択します。 
   
1. 左側のウィンドウで **[buildAndPush]** を選択し、右側のウィンドウの **[表示名]** フィールドに説明を入力します。
   
1. **[コンテナー リポジトリ]** の下の **[コンテナー レジストリ]** フィールドの横にある **[新規]** を選択します。 
   
1. **[Docker レジストリ サービス接続の追加]** ダイアログ ボックスに次のとおりに入力します。
   
   |フィールド|値|
   |---|---|
   |**レジストリの種類**|**[Azure Container Registry]** を選択します。|
   |**Connection Name**|接続名を入力します。|
   |**Azure サブスクリプション**|自分の Azure サブスクリプションをドロップダウンで選択し、必要に応じて **[承認]** を選択します。|
   |**Azure コンテナー レジストリ**|自分の Azure Container Registry の名前をドロップダウンで選択します。| 
   
1. **[OK]** を選択します。
   
   ![Docker レジストリ サービス接続を追加する](media/cicd-microprofile/dockerconnection.png)
   
   > [!NOTE]
   > Docker Hub または別のレジストリを使用する場合は、**[レジストリの種類]** で **[Azure Container Registry]** の代わりに **[Docker Hub]** または **[その他]** を選択します。 使用するコンテナー レジストリの資格情報と接続情報を入力してください。
   
1. **[コマンド]** の下の **[コマンド]** ドロップダウンで **[build]** を選択します。
   
1. **[Dockerfile]** フィールドの横の省略記号 (**...**) を選択し、GitHub リポジトリの内容の一覧から **[Dockerfile]** を選択して **[OK]** をクリックします。 
   
   ![Dockerfile を選択する](media/cicd-microprofile/selectdockerfile.png)
   
1. **[タグ]** の下で、新しい行に「*latest*」と入力します。 
   
1. ページの一番上にある **[保存してキューに登録]** ドロップダウンを選択し、 **[保存]** を選択します。 

## <a name="push-the-docker-image-to-acr"></a>Docker イメージを ACR にプッシュする

Azure Pipelines によって Docker イメージが Azure Container Registry にプッシュされ、これを使用して MicroProfile API アプリがコンテナー化 Java Web アプリとして実行されます。

1. Docker を Azure Pipelines で使用するので、「[Docker ビルド イメージを作成する](#create-a-docker-build-image)」の手順をもう一度実行して Docker テンプレートをもう 1 つ作成します。 今回は、**[コマンド]** ドロップダウンで **[push]** を選択します。
   
1. **[保存してキューに登録]** のドロップダウンを選択し、 **[保存してキューに登録]** を選択します。 
   
1. **[パイプラインの実行]** ポップアップの **[エージェント プール]** で **[Hosted Ubuntu 1604]\(ホストされている Ubuntu 1604\)** が選択されていることを確認して **[保存して実行]** を選択します。 
   
1. ビルドが完了したら、 **[ビルド]** ページにあるハイパーリンクを選択すると、ビルドの成功とその他の詳細情報を確認できます。
   
   ![ビルドのハイパーリンクを選択する](media/cicd-microprofile/checkbuild.png)

## <a name="create-a-release-pipeline"></a>リリース パイプラインを作成する

Azure Pipelines の継続的リリース パイプラインでは、ビルドが成功するとすぐに、ターゲット環境 (たとえば Azure) へのデプロイが自動的にトリガーされます。 リリース パイプラインを環境ごとに、たとえば開発、テスト、ステージング、本番運用のそれぞれについて作成することができます。

1. Azure DevOps プロジェクト ページの左側のナビゲーションで **[パイプライン]**  >  **[リリース]** を選択します。 
   
1. **[新しいパイプライン]** を選択します。
   
1. テンプレートの一覧で **[Java アプリの Azure App Service への配置]** を選択し、 **[適用]** を選択します。 
   
   ![[Java アプリの Azure App Service への配置] テンプレートを選択する](media/cicd-microprofile/selectreleasetemplate.png)
   
1. ポップアップ ウィンドウで **[ステージ 1]** をステージ名 (たとえば *[開発]* 、 *[テスト]* 、 *[ステージング]* または *[運用]* ) に変更し、ウィンドウを閉じます。 
   
1. 左側のウィンドウの **[成果物]** の下で、 **[追加]** を選択して成果物をビルド パイプラインからリリース パイプラインにリンクします。 
   
1. 右側のウィンドウの **[ソース (ビルド パイプライン)]** の下のドロップダウンでビルド パイプラインを選択して **[追加]** を選択します。
   
   ![ビルド成果物を追加する](media/cicd-microprofile/addbuildartifact.png)
   
1. **[運用]** ステージのハイパーリンクを選択して**ステージ タスクを表示します**。
   
   ![ステージ名を選択する](media/cicd-microprofile/viewstagetasks.png)
   
1. 右側のウィンドウのフォームに、次のとおりに入力します。
   
   |フィールド|値|
   |---|---|
   |**Azure サブスクリプション**|自分の Azure サブスクリプションをドロップダウンから選択します。|
   |**アプリの種類**|**[Web App for Containers (Linux)]** をドロップダウンから選択します。|
   |**App Service の名前**|自分の ACR インスタンスをドロップダウンから選択します。|
   |**レジストリまたは名前空間**|ACR の名前をフィールドに入力します。 たとえば、「*mymicroprofileregistry.azure.io*」と入力します。
   |**リポジトリ**|使用する Docker イメージが格納されているリポジトリを入力します。| 
   
   ![ステージのタスクを構成する](media/cicd-microprofile/configurestage.png)
   
1. 左側のウィンドウで **[Azure App Service に War を配置]** を選択し、右側のウィンドウで **[タグ]** フィールドに「*latest*」タグを入力します。 
   
1. 左側のウィンドウで **[エージェントで実行]** を選択し、右側のウィンドウの **[エージェント プール]** ドロップダウンで **[Hosted Ubuntu 1604]\(ホストされている Ubuntu 1604\)** を選択します。 

## <a name="set-up-environment-variables"></a>環境変数を設定する

デプロイ時にコンテナー レジストリに接続するための環境変数を追加して定義します。

1. **[変数]** タブを選択し、 **[追加]** を選択して次の変数を追加します。これらは、コンテナー レジストリの URL、ユーザー名、パスワードを表します。 
   
   |名前|値|
   |---|---|
   |*registry.url*|自分のコンテナー レジストリ URL を入力します。 例: *https:\//mymicroprofileregistry.azure.io*|
   |*registry.username*|レジストリのユーザー名を入力します。|
   |*registry.password*|レジストリのパスワードを入力します。 セキュリティを確保するために、ロック アイコンを選択してパスワード値を非表示にします。|
   
   ![変数を追加する](media/cicd-microprofile/addvariables.png)
   
1. **[タスク]** タブで、左側のウィンドウの **[Azure App Service に War を配置]** を選択します。 
   
1. 右側のウィンドウで、**[アプリケーションと構成の設定]** を展開し、**[アプリ設定]** フィールドの横にある省略記号 (**...**) を選択します。
   
1. **[アプリ設定]** ポップアップで、 **[追加]** を選択してアプリ設定の変数を定義し、割り当てます。
   
   |名前|値|
   |---|---|
   |*DOCKER_REGISTRY_SERVER_URL*|*$(registry.url)*|
   |*DOCKER_REGISTRY_SERVER_USERNAME*|*$(registry.username)*|
   |*DOCKER_REGISTRY_SERVER_PASSWORD*|*$(registry.password)*|
   
1. **[OK]** を選択します。
   
   ![変数の追加と設定](media/cicd-microprofile/appsettings.png)
   
## <a name="set-up-continuous-deployment"></a>Azure App Service での GIT による継続的なデプロイ 

継続的デプロイを有効にするには 

1. **[パイプライン]** タブの **[成果物]** の下で、ビルド成果物の中の稲妻アイコンを選択します。 
   
1. 右側のウィンドウで、 **[継続的配置トリガー]** を **[有効]** に設定します。
   
1. 右上の **[保存]** を選択し、もう一度 **[保存]** を選択します。 
   
   ![継続的デプロイ トリガーを有効にする](media/cicd-microprofile/setcontinuousdeployment.png)
   
## <a name="deploy-the-java-app"></a>Java アプリをデプロイする

CI/CD を有効化したので、ソース コードに変更を加えるとビルドとリリースの作成と実行が自動的に行われるようになりました。 次の方法で、手動でリリースを作成して実行することもできます。

1. リリース パイプラインのページの右上にある **[リリースを作成]** を選択します。
   
1. **[新しいリリースの作成]** ページの **[トリガーのステージを自動から手動に変更します]** の下でステージ名を選択します。 
   
1. **［作成］** を選択します 
   
1. リリース名を選択し、ステージにマウス カーソルを合わせるかステージを選択してから **[デプロイ]** を選択します。 
   
## <a name="test-the-java-web-app"></a>Java Web アプリをテストする

デプロイが正常に完了したら、Web アプリをテストします。 

1. Web アプリの URL を Azure portal からコピーします。
   
   ![Azure portal の中の App Service アプリ](media/cicd-microprofile/portalurl.png)
   
1. この URL を Web ブラウザーに入力してアプリを実行します。 Web ページに "**Hello Azure!** " と表示されます。
   
   ![Java Web アプリのページ](media/cicd-microprofile/webapp.png)

