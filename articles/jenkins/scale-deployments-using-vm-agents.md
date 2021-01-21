---
title: チュートリアル - Azure で実行されている VM を使用した Jenkins のデプロイのスケーリング
description: Azure 仮想マシンを使用して、お使いの Jenkins パイプラインの容量をさらに追加する方法について説明します。
keywords: Jenkins, Azure, 開発,仮想マシン, エージェント
ms.topic: tutorial
ms.date: 01/08/2021
ms.custom: devx-track-jenkins,devx-track-jenkins
ms.openlocfilehash: 599fe9875d1f9ffd107cca80f9445bba18100f42
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561038"
---
# <a name="tutorial-scale-jenkins-deployments-with-vm-running-in-azure"></a>チュートリアル:Azure で実行されている VM を使用した Jenkins のデプロイのスケーリング

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

このチュートリアルでは、Azure で Linux Virtual Machines を作成し、その VM を作業ノードとして Jenkins に追加する方法について説明します。

このチュートリアルでは、次のことについて説明します。

> [!div class="checklist"]
> * エージェント マシンを作成する
> * エージェントを Jenkins に追加する
> * Jenkins の新しいフリースタイル ジョブを作成する
> * Azure VM エージェントでジョブを実行する

## <a name="prerequisites"></a>前提条件

- **Jenkins のインストール**: Jenkins のインストールにアクセスできない場合は、[Azure CLI を使用して Jenkins を構成します](configure-on-linux-vm.md)。

## <a name="configure-agent-virtual-machine"></a>エージェント仮想マシンの構成

1. [az group create](/cli/azure/group?#az_group_create) コマンドを使用して Azure リソース グループを作成します。

    ```azurecli
    az group create --name <resource_group> --location <location>
    ```

1. [az vm create](/cli/azure/vm#az_vm_create) を使用して仮想マシンを作成します。

    ```azurecli
    az vm create --resource-group <resource-group> --name <vm_name> --image UbuntuLTS --admin-username azureuser --admin-password "<password>"
    ```

    **注**:

    - コマンド `--ssh-key-value <ssh_path>` を使用して、お使いの SSH キーをアップロードすることもできます。

1. JDK をインストールします。  

    #### <a name="linux"></a>[Linux](#tab/linux)
    
    1. SSH ツールを使用して、仮想マシンにログインします。
    
        ```bash
        ssh username@123.123.123.123
        ```
        
    1. apt を使用して JDK をインストールします。 yum や pacman などの他のパッケージ マネージャー ツールを使用してインストールすることもできます。
    
        ```bash
        sudo apt-get install -y default-jdk
        ```
    
    1. インストールが完了したら、`java -version` を実行して Java 環境を確認します。 出力には、JDK のさまざまなパーツに関連付けられているバージョン番号が含まれます。
    
    #### <a name="windows"></a>[Windows](#tab/windows)
    
    1. SSH ツールまたはリモート デスクトップ接続を使用して、仮想マシンにログインします。
    
    1. お使いの環境に適した [JDK をダウンロード](https://www.oracle.com/java/technologies/javase-downloads.html)します。
    
    1. JDK をインストールします。
    
## <a name="configure-jenkins-url"></a>Jenkins の URL を構成する

JNLP を使用する場合は、Jenkins の URL を構成する必要があります。

1. メニューから、 **[Manage Jenkins]\(Jenkins の管理\)** を選択します。

1. **[System Configuration]\(システム構成\)** の **[Configure System]\(システムの構成\)** を選択します。

1. **Jenkins URL** が、Jenkins インストール環境の HTTP アドレス (`http://<your_host>.<your_domain>:8080/`) に設定されていることを確認します。

1. **[保存]** を選択します。

## <a name="add-agent-to-jenkins"></a>エージェントを Jenkins に追加する

1. メニューから、 **[Manage Jenkins]\(Jenkins の管理\)** を選択します。

1. **[System Configuration]\(システム構成\)** で、 **[Manage Nodes and Clouds]\(ノードとクラウドの管理\)** を選択します。

1. メニューから **[New Node]\(新しいノード\)** を選択します。

1. **[Node Name]\(ノード名\)** の値を入力します。

1. **[Permanent Agent]\(永続的なエージェント\)** を選択します。

1. **[OK]** を選択します。

1. 次の各フィールドに値を指定します。

    - **[名前]** : 新しい Jenkins インストール環境内のエージェントを識別する一意の名前を指定します。 この値は、エージェントのホスト名とは異なっていても構いません。 ただし、この 2 つの値を同じにしておくと便利です。 名前の値には、次の一覧の特殊文字を使用できます: `?*/\%!@#$^&|<>[]:;`。

    - **[リモート ルート ディレクトリ]** :エージェントには、Jenkins 専用のディレクトリが必要です。 エージェント上のこのディレクトリへのパスを指定します。 `/home/azureuser/work` や `c:\jenkins` などの絶対パスを使用することをお勧めします。 これは、エージェント マシンのローカル パスである必要があります。 このパスがマスターから見えるものである必要はありません。 ./jenkins-agent などの相対パスを使用する場合、パスは起動方法で指定される作業ディレクトリからの相対パスになります。

    - **ラベル**:ラベルは、意味的に関連するエージェントを 1 つの論理グループとしてグループ化するために使用されます。 たとえば、Linux の Ubuntu ディストリビューションを実行しているすべてのエージェントに対して、`UBUNTU` というラベルを定義できます。

    - **[Launch method\(起動方法\)]** :リモート Jenkins ノードを開始するには、次の2つのオプションがあります。 **[Launch agents via SSH]\(SSH 経由でエージェントを起動する\)** と **[Launch agent via execution of command on the master]\(マスター上でのコマンドの実行によりエージェントを起動する\)** 。

        - **[Launch agents via SSH]\(SSH 経由でエージェントを起動する\)** :次の各フィールドに値を指定します。

            - **[Host]\(ホスト\)** : VM のパブリック IP アドレスまたはドメイン名。 たとえば、`123.123.123.123` や `example.com` のようにします。

            - **資格情報**:リモート ホストへのログインに使用する資格情報を選択します。 **[追加]** ボタンを選択して新しい資格情報を定義し、作成後にその新しい資格情報を選択することもできます。

            - **[Host Key Verification Strategy]\(ホスト キーの検証方法\)** :接続中にリモート ホストによって提示される SSH キーを検証する方法を制御します。

            ![SSH 経由でエージェントを起動する起動方法を指定するノード構成の例。](./media/scale-deployments-using-vm-agents/ssh2.png)

        - **[Launch agent via execution of command on the master]\(マスター上でのコマンドの実行によりエージェントを起動する\)** :

            - `https://<your_jenkins_host_name>/jnlpJars/agent.jar` から `agent.jar` をダウンロードします。 たとえば、`https://localhost:8443/jnlpJars/agent.jar` のようにします。

            - 仮想マシンに `agent.jar` をアップロードする

            - コマンド `ssh <node_host> java -jar <remote_agentjar_path>` を使用して Jenkins を起動します。 たとえば、`ssh azureuser@99.99.999.9 java -jar /home/azureuser/agent.jar` のようにします。

            ![マスター上でのコマンドの実行によりエージェントを起動する起動方法を指定するノード構成の例。](./media/scale-deployments-using-vm-agents/config.png)

1. **[保存]** を選択します。

構成を定義すると、仮想マシンが新しい作業ノードとして追加されます。

![新しい作業ノードとしての仮想マシンの例](./media/scale-deployments-using-vm-agents/commandstart.png)

## <a name="create-a-job-in-jenkins"></a>Jenkins でジョブを作成する

1. メニューから **[New Item]\(新しい項目\)** を選択します。

1. 名前に「`demoproject1`」と入力します。

1. **[Freestyle project] \(フリースタイル プロジェクト)** を選択します。

1. **[OK]** を選択します。

1. **[全般]** タブで、**[Restrict where project can be run]\(プロジェクトを実行できる場所を制限する\)** をオンにして、**[ラベル式]** に `ubuntu` と入力します。 前の手順で作成したクラウドの構成によってラベルが提供されていることを確認するメッセージが表示されます。

   ![新しい Jenkins ジョブの設定](./media/scale-deployments-using-vm-agents/job-config.png)

1. **[ソース コード管理]** タブで **[Git]** を選択し、**[リポジトリの URL]** フィールドに `https://github.com/spring-projects/spring-petclinic.git` のURL を追加します。

1. **[ビルド]** タブで **[Add build step]\(ビルド ステップの追加\)** を選択し、**Maven の最上位のターゲットを呼び出し** ます。 **[Goals]\(目標\)** フィールドに `package` を入力します。

1. **[保存]** を選択します。

## <a name="build-the-new-job-on-an-azure-vm-agent"></a>Azure VM エージェントで新しいジョブを作成する

1. 前の手順で作成したジョブを選択します。

1. **[Build now]\(今すぐビルド\)** を選択します。 新しいビルドがキューに追加されますが、Azure サブスクリプションにエージェント VM が作成されるまでは開始されません。

1. ビルドが完了したら、**[Console output\(コンソール出力\)]** に移動します。 ビルドが Azure エージェントでリモートに実行されたことがわかります。

    ![コンソール出力](./media/scale-deployments-using-vm-agents/console-output.png)

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure App Service への CI/CD](./deploy-to-azure-app-service-using-azure-cli.md)