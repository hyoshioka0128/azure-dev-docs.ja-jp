---
title: クイックスタート - Azure に Jenkins サーバーを作成する
description: Jenkins ソリューション テンプレートから Azure Linux 仮想マシンに Jenkins をインストールし、サンプル Java アプリケーションをビルドする方法について説明します。
keywords: jenkins, azure, devops, portal, linux, 仮想マシン, ソリューション テンプレート
ms.topic: quickstart
ms.date: 03/03/2020
ms.openlocfilehash: 2ea038dad2294784804f45026ea26198a9b12d79
ms.sourcegitcommit: 9ff9b51ab21c93bfd61e480c6ff8e39c9d4bf02e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2020
ms.locfileid: "82170338"
---
# <a name="quickstart-create-a-jenkins-server-on-an-azure-linux-vm"></a>クイック スタート:Azure Linux VM に Jenkins サーバーを作成する

このクイックスタートでは、Azure で動作するよう構成されているプラグインとツールを備えた [Jenkins](https://jenkins.io) を Ubuntu Linux VM にインストールする方法について説明します。 最終的には、[GitHub](https://github.com) からのサンプル Java アプリをビルドする Jenkins サーバーが Azure で稼働することになります。

## <a name="prerequisites"></a>前提条件

* コンピューターのコマンド ラインでの SSH アクセス (Bash シェル、[PuTTY](https://www.putty.org/) など)

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>ソリューション テンプレートからの Jenkins VM の作成

Jenkins は、Jenkins サーバーが 1 つ以上のエージェントに作業を委任するモデルをサポートしているので、1 つの Jenkins インストールで多数のプロジェクトをホストすることや、ビルドやテストに必要なさまざまな環境を提供することができます。 このセクションの手順では、Azure に Jenkins サーバーをインストールして構成する方法を説明します。

1. ブラウザーで、[Jenkins 用の Azure Marketplace イメージ](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview)を開きます。

1. **[今すぐ入手する]** を選択します。

    ![[今すぐ入手する] を選択して、Jenkins Marketplace イメージのインストール プロセスを開始する。](./media/install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. 料金の詳細と使用条件についての情報を確認した後、 **[続行]** を選択します。

    ![Jenkins Marketplace イメージの料金と使用条件の情報。](./media/install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. **[作成]** を選択して、Azure ポータルで Jenkins サーバーを構成します。 

    ![Jenkins Marketplace イメージをインストールする。](./media/install-from-azure-marketplace-image/jenkins-install-create.png)

1. **[基本]** タブで、次の値を指定します。

   - **[Name]\(名前\)** - 「`Jenkins`」と入力します。
   - **ユーザー名** - Jenkins が実行される仮想マシンへのサインイン時に使用するユーザー名を入力します。 ユーザー名は、[特定の要件](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm)を満たしている必要があります。
   - **認証の種類** - **[SSH 公開キー]** を選択します。
   - **SSH 公開キー** - RSA 公開キーをコピーして、1 行形式 (`ssh-rsa` で始まります) または複数行の PEM 形式で貼り付けます。 SSH キーは、Linux と macOS では ssh-keygen を、Windows では PuTTYGen を使用して生成できます。 SSH キーと Azure の詳細については、「[Azure 上の Windows で SSH キーを使用する方法](/azure/virtual-machines/linux/ssh-from-windows)」を参照してください。
   - **サブスクリプション**: Jenkins をインストールする Azure サブスクリプションを選択します。
   - **リソース グループ**: **[新規作成]** を選択し、Jenkins のインストールを構成するリソースのコレクションの論理コンテナーとして機能するリソース グループの名前を入力します。
   - **場所**: **[米国東部]** を選択します。

     ![[基本] タブに Jenkins の認証とリソース グループの情報を入力する。](./media/install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. **[OK]** を選択して、 **[追加設定]** タブに進みます。 

1. **[追加設定]** タブで、次の値を指定します。

   - **サイズ**: Jenkins 仮想マシンの適切なサイズ オプションを選択します。
   - **VM ディスクの種類**: Jenkins 仮想マシンで許可されるストレージ ディスクの種類を示すために、[HDD] \(ハード ディスク ドライブ) または [SSD] \(ソリッドステート ドライブ) のどちらかを指定します。
   - **仮想ネットワーク** - (省略可能) **[仮想ネットワーク]** を選択して既定の設定を変更します。
   - **サブネット** - **[サブネット]** を選択し、情報を確認し、 **[OK]** を選択します。
   - **パブリック IP アドレス**: IP アドレス名の既定値は、前のページで指定した Jenkins 名にサフィックス-IP が付いたものになります。 この既定値を変更するオプションを選択できます。
   - **ドメイン名ラベル**: Jenkins 仮想マシンの完全修飾 URL の値を指定します。
   - **Jenkins リリースの種類** - オプションから必要なリリースの種類を選択します (`LTS`、`Weekly build`、または`Azure Verified`)。 `LTS` オプションと `Weekly build` オプションについては、「[Jenkins LTS Release Line](https://jenkins.io/download/lts/)」(Jenkins LTS リリース ライン) に説明があります。 `Azure Verified` オプションは、Azure での実行が検証されている [Jenkins LTS バージョン](https://jenkins.io/download/lts/) を参照します。 
   - **JDK の種類** - インストールする JDK。 既定値は、OpenJDK のテスト済み、認定済みのビルドである Zulu です。

     ![[設定] タブで、Jenkins の仮想マシン設定を入力する。](./media/install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. **[OK]** を選択して、 **[統合設定]** タブに進みます。

1. **[統合設定]** タブで、次の値を指定します。

    - **サービス プリンシパル** - サービス プリンシパルは、Azure で認証されるための資格情報として Jenkins に追加されます。 `Auto` は、プリンシパルが MSI (管理対象サービス ID) によって作成されることを意味します。 `Manual` は、プリンシパルを自分で作成する必要があることを意味します。 
        - **アプリケーション ID** と**シークレット** - **[サービス プリンシパル]** オプションで `Manual` オプションを選択した場合は、サービス プリンシパルの `Application ID` と `Secret` を指定する必要があります。 [サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)ときは、既定のロールが**共同作成者**であることに注意してください。これは Azure リソースを操作するのに十分なロールです。
    - **[Enable Cloud Agents]\(クラウド エージェントの有効化\)** - エージェントの既定のクラウド テンプレートを指定します。`ACI` は Azure コンテナー インスタンスを、`VM` は仮想マシンを参照します。 クラウド エージェントを有効にしない場合は、`No` を指定できます。

1. **[OK]** を選択して、 **[概要]** タブに進みます。

1. **[概要]** タブが表示されれば、入力した情報は検証されています。 「**検証に成功しました** 」というメッセージが (タブの上部に) 表示されたら、 **[OK]** をクリックします。 

     ![[概要] タブが表示され、選択したオプションが検証される。](./media/install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. **[作成]** タブが表示されたら、 **[作成]** を選択して Jenkins 仮想マシンを作成します。 サーバーの準備が完了すると、Azure ポータルに通知が表示されます。

     ![Jenkins の準備が完了したことを示す通知。](./media/install-from-azure-marketplace-image/jenkins-install-notification.png)

## <a name="connect-to-jenkins"></a>Jenkins に接続する

1. Web ブラウザーで仮想マシンに移動します (例: `http://jenkins2517454.eastus.cloudapp.azure.com/`)。 セキュリティで保護されていない HTTP 経由では Jenkins コンソールにアクセスできません。そのためこのページには、ご使用のコンピューターから SSH トンネルを使って安全に Jenkins コンソールにアクセスする手順が掲載されています。

    ![Jenkins のロックを解除する](./media/install-solution-template-steps/jenkins-ssh-instructions.png)

1. このページのコマンド ラインから `ssh` コマンドを使用してトンネルを設定します。`username` は、先ほどソリューション テンプレートから仮想マシンをセットアップするときに選択した仮想マシンの管理者ユーザーの名前に置き換えてください。

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
    ```
    
1. トンネルを開始した後、ローカル コンピューターで `http://localhost:8080/` に移動します。 

1. Jenkins VM に SSH 接続している状態で、コマンド ラインから次のコマンドを実行して初期パスワードを取得します。

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
    
1. 初めて使うときは、この初期パスワードを使って Jenkins ダッシュボードのロックを解除します。

    ![Jenkins のロックを解除する](./media/install-solution-template-steps/jenkins-unlock.png)

1. 次のページの **[Install suggested plugins]\(推奨プラグインのインストール\)** を選択し、Jenkins ダッシュボードへのアクセスに使用した Jenkins 管理者ユーザーを作成します。

    ![Jenkins の準備ができました。](./media/install-solution-template-steps/jenkins-welcome.png)

Jenkins サーバーでコードをビルドする準備が整いました。

## <a name="create-your-first-job"></a>最初のジョブの作成

1. Jenkins コンソールから **[Create new jobs]\(新しいジョブの作成\)** を選択して **mySampleApp** という名前を付け、 **[Freestyle project]\(フリースタイル プロジェクト\)** を選択して **[OK]** を選択します。

    ![新しいジョブを作成する](./media/install-solution-template-steps/jenkins-new-job.png) 

1. **[ソース コード管理]** タブを選択して **[Git]** を有効にし、 **[リポジトリの URL]** フィールドに次の URL を入力します。`https://github.com/spring-guides/gs-spring-boot.git`

    ![Git リポジトリの定義](./media/install-solution-template-steps/jenkins-job-git-configuration.png) 

1. **[ビルド]** タブを選択し、 **[ビルド ステップの追加]** 、 **[Invoke Gradle script]\(Gradle スクリプトの呼び出し\)** の順に選択します。 **[Use Gradle Wrapper]\(Gradle ラッパーの使用\)** を選択して、 **[Wrapper location]\(ラッパーの場所\)** に「`complete`」、 **[タスク]** に「`build`」と入力します。

    ![Gradle ラッパーを使用したビルド](./media/install-solution-template-steps/jenkins-job-gradle-config.png) 

1. **[詳細]** を選択し、 **[Root Build script]\(ルート ビルド スクリプト\)** フィールドに `complete` と入力します。 **[保存]** を選択します。

    ![Gradle ラッパー ビルド ステップの詳細設定](./media/install-solution-template-steps/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>コードのビルド

1. **[Build Now]\(今すぐビルド\)** を選択してコードをコンパイルし、サンプル アプリをパッケージします。 ビルドが完了したら、プロジェクトの **[ワークスペース]** リンクを選択します。

    ![ワークスペースを参照画面で選択し、ビルドから JAR ファイルを取得します。](./media/install-solution-template-steps/jenkins-access-workspace.png) 

1. ビルドが成功したことを確かめるために、`complete/build/libs` に移動し、`gs-spring-boot-0.1.0.jar` が存在することを確認します。 Azure 上の Jenkins サーバーで独自のプロジェクトをビルドする準備が整いました。

## <a name="troubleshooting-the-jenkins-solution-template"></a>Jenkins ソリューション テンプレートのトラブルシューティング

Jenkins ソリューション テンプレートに関するバグを見つけた場合は、[Jenkins GitHub リポジトリ](https://github.com/azure/jenkins/issues)で問題を報告してください。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Azure 上の Jenkins](/azure/developer/jenkins)