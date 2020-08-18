---
title: クイック スタート - Jenkins の使用を開始する
description: Azure Linux 仮想マシンに Jenkins をインストールし、サンプル Java アプリケーションをビルドする方法について説明します。
keywords: Jenkins, Azure, DevOps, ポータル, Linux, 仮想マシン
ms.topic: quickstart
ms.date: 08/07/2020
ms.openlocfilehash: 06d2365f51df76861a3a154702c4b82f962f7038
ms.sourcegitcommit: f65561589d22b9ba2d69b290daee82eb47b0b20f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "88162091"
---
# <a name="quickstart-get-started-with-jenkins"></a>クイック スタート:Jenkins の使用を開始する

このクイックスタートでは、Azure で動作するよう構成されているプラグインとツールを備えた [Jenkins](https://jenkins.io) を Ubuntu Linux VM にインストールする方法について説明します。

このクイック スタートでは、以下のタスクを完了します。

> [!div class="checklist"]
> * Jenkins をダウンロードしてインストールするセットアップ ファイルを作成する
> * リソース グループを作成する
> * セットアップ ファイルを使用して仮想マシンを作成する
> * 仮想マシンに SSH 接続できるように ポート 8080 を開く
> * SSH 経由で仮想マシンに接続する
> * GitHub のサンプル Java アプリに基づいてサンプル Jenkins ジョブを構成する
> * サンプル Jenkins ジョブをビルドする

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="troubleshooting"></a>トラブルシューティング

Jenkins の構成で問題が発生した場合は、[Cloudbees Jenkins のインストールのページ](https://www.jenkins.io/doc/book/installing/)を参照して、最新の手順と既知の問題を確認してください。

## <a name="create-a-virtual-machine"></a>仮想マシンの作成

1. [Azure portal](https://portal.azure.com) にサインインします。

1. [Azure Cloud Shell](/azure/cloud-shell/overview) を開き、**Bash**に切り替えます (まだ行っていない場合)。

1. `cloud-init-jenkins.txt` という名前でファイルを作成します。

    ```bash
    code cloud-init-jenkins.txt
    ```

1. 次のコードを新しいファイルに貼り付けます。

    ```json
    #cloud-config
    package_upgrade: true
    runcmd:
      - apt install openjdk-8-jdk -y
      - wget -qO - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
      - sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
      - apt-get update && apt-get install jenkins -y
      - service jenkins restart
    ```

1. ファイルを保存し ( **&lt;Ctrl> + S** キー)、エディターを終了します ( **&lt;Ctrl> + Q** キー)。

1. [az group create](/cli/azure/group#az-group-create) を使用してリソース グループを作成します。 場合によっては、`--location` パラメーターをお使いの環境に適した値に置き換える必要があります。

    ```azurecli
    az group create --name QuickstartJenkins-rg --location eastus
    ```

1. [az vm create](/cli/azure/vm#az-vm-create) を使用して仮想マシンを作成します。

    ```azurecli
    az vm create --resource-group QuickstartJenkins-rg --name QuickstartJenkins-vm --image UbuntuLTS --admin-username "azureuser" --generate-ssh-keys --custom-data cloud-init-jenkins.txt
    ```

1. [az vm open](/cli/azure/vm#az-vm-open-port) を使用して、新しい仮想マシンでポート 8080 を開きます。

    ```azurecli
    az vm open-port --resource-group QuickstartJenkins-rg --name QuickstartJenkins-vm  --port 8080 --priority 1010
    ```

## <a name="configure-jenkins"></a>Jenkins を構成する

1. [az vm show](/cli/azure/vm#az-vm-show) を使用して、サンプル仮想マシンのパブリック IP アドレスを取得します。

    ```azurecli
    az vm show --resource-group QuickstartJenkins-rg --name QuickstartJenkins-vm -d --query [publicIps] --output tsv
    ```

    **注**:

    - `--query` パラメーターにより、出力が仮想マシンのパブリック IP アドレスに限定されます。

1. 前の手順で取得した IP アドレスを使用して、仮想マシンに SSH 接続します。 接続要求を確認する必要があります。

    ```azurecli
    ssh azureuser@<ip_address>
    ```

    **注**:

    - 接続が成功すると、Cloud Shell プロンプトにユーザー名と仮想マシン名 (`azureuser@QuickstartJenkins-vm`) が含まれます。

1. Jenkins サービスの状態を取得して、Jenkins が実行されていることを確認します。

    ```bash
    service jenkins status
    ```

1. 自動生成された Jenkins パスワードを取得します。

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

1. IP アドレスを使用して、ブラウザーで URL `http://<ip_address>:8080` を開きます。

1. 先ほど取得したパスワードを入力し、 **[Continue]\(続行\)** を選択します。

    ![Jenkins のロックを解除するための初期ページ](./media/configure-on-linux-vm/unlock-jenkins.png)

1. **[Select plugins to install]\(インストールするプラグインの選択\)** を選択します。

    ![選択したプラグインをインストールするオプションを選択する](./media/configure-on-linux-vm/select-plugins.png)

1. ページの上部にあるフィルター ボックスに「`github`」と入力します。 GitHub プラグインを選択し、 **[Install]\(インストール\)** を選択します。

1. 最初の管理者ユーザーの情報を入力し、 **[Save and Continue]\(保存して続行\)** を選択します。

    ![最初の管理者ユーザーの情報を入力する](./media/configure-on-linux-vm/create-first-user.png)

1. **[Instance Configuration]\(インスタンスの構成\)** ページで、 **[Save and Finish]\(保存して終了\)** を選択します。

    ![インスタンスの構成の確認ページ](./media/configure-on-linux-vm/instance-configuration.png)

1. **[Start using Jenkins]\(Jenkins の使用を開始\)** を選択します。

    ![Jenkins の準備ができました。](./media/configure-on-linux-vm/start-using-jenkins.png)

## <a name="create-your-first-job"></a>最初のジョブの作成

1. Jenkins ホーム ページで、 **[Create a job]\(ジョブの作成\)** を選択します。

    ![Jenkins コンソールのホーム ページ](./media/configure-on-linux-vm/jenkins-home-page.png)

1. ジョブ名として「`mySampleApp`」を入力し、 **[Freestyle project]\(フリースタイル プロジェクト\)** を選択して、 **[OK]** を選択します。

    ![新しいジョブの作成](./media/configure-on-linux-vm/new-job.png)

1. **[Source Code Management]\(ソース コード管理\)** タブを選択します。 **[Git]** を有効にし、 **[Repository URL]\(リポジトリ URL\)** の値として URL「`https://github.com/spring-guides/gs-spring-boot.git`」を入力します。

    ![Git リポジトリの定義](./media/configure-on-linux-vm/source-code-management.png)

1. **[Build]\(ビルド\)** タブを選択し、 **[Add build step]\(ビルド ステップの追加\)** を選択します

    ![新しいビルド ステップを追加する](./media/configure-on-linux-vm/add-build-step.png)

1. ドロップダウン メニューから **[Invoke Gradle script]\(Gradle スクリプトの呼び出し\)** を選択します。

    ![Gradle スクリプト オプションを選択する](./media/configure-on-linux-vm/invoke-gradle-script-option.png)

1. **[Use Gradle Wrapper]\(Gradle ラッパーの使用\)** を選択して、**[Wrapper location]\(ラッパーの場所\)** に「`complete`」、**[タスク]** に「`build`」と入力します。

    ![Gradle スクリプトのオプション](./media/configure-on-linux-vm/gradle-script-options.png)

1. **[Advanced]\(詳細設定\)** を選択し、 **[Root Build script]\(ルート ビルド スクリプト\)** フィールドに「`complete`」と入力します。

    ![Gradle スクリプトの詳細設定オプション](./media/configure-on-linux-vm/root-build-script.png)

1. ページの下部までスクロールし、 **[Save]\(保存\)** を選択します。

## <a name="build-the-sample-java-app"></a>サンプル Java アプリをビルドする

1. 自分のプロジェクトのホーム ページが表示されたら、 **[Build now]\(今すぐビルド\)** を選択し、コードをコンパイルしてサンプル アプリをパッケージ化します。

    ![プロジェクトのホーム ページ](./media/configure-on-linux-vm/project-home-page.png)

1. **[Build History]\(ビルド履歴\)** 見出しの下のグラフィックは、ジョブがビルド中であることを示しています。

    ![進行中のジョブのビルド](./media/configure-on-linux-vm/job-currently-building.png)

1. ビルドが完了したら、 **[Workspace]\(ワークスペース\)** リンクを選択します。

    ![ワークスペース リンクを選択する](./media/configure-on-linux-vm/job-workspace.png)

1. `complete/build/libs` に移動し、`.jar` ファイルが正常にビルドされたことを確認します。

    ![ターゲット ライブラリによってビルドが成功したことが確認される](./media/configure-on-linux-vm/successful-build.png)

1. Azure 上の Jenkins サーバーで独自のプロジェクトをビルドする準備が整いました。

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure 上の Jenkins](/azure/developer/jenkins)