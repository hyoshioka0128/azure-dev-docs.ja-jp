---
title: チュートリアル - Azure Terraform Visual Studio Code 拡張機能の構成
description: Visual Studio Code で Azure Terraform 拡張機能をインストールして使用する方法を説明します。
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 9fd6d6b93537795109117198aa20063b556b8368
ms.sourcegitcommit: db56786f046a3bde1bd9b0169b4f62f0c1970899
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/03/2020
ms.locfileid: "84329570"
---
# <a name="tutorial-configure-the-azure-terraform-visual-studio-code-extension"></a>チュートリアル:Azure Terraform Visual Studio Code 拡張機能の構成

Azure Terraform Visual Studio Code 拡張機能を使用すると、エディターから Terraform を操作できます。 この拡張機能を使用して、Terraform 構成の作成、テスト、実行を行うことができます。 また、この拡張機能は、グラフを使ったリソースの視覚化もサポートしています。

この記事では、次のことについて説明します。
> [!div class="checklist"]
> * Terraform を使用して Azure サービスのプロビジョニングを自動化する。
> * Azure サービス用の Terraform Visual Studio Code 拡張機能をインストールして使用する。
> * Visual Studio Code を使用して、Terraform プランを記述、計画、実行する。

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Terraform**:[Terraform をインストールして構成します](getting-started-cloud-shell.md)。
- **Visual Studio Code**:ご利用の環境に適したバージョンの [Visual Studio Code](https://code.visualstudio.com/download) をインストールしてください。

## <a name="prepare-your-dev-environment"></a>開発環境の準備

### <a name="install-git"></a>Git のインストール

この記事の演習を完了するには、[Git をインストール](https://git-scm.com/)する必要があります。

### <a name="install-hashicorp-terraform"></a>HashiCorp Terraform のインストール

HashiCorp の「[Install Terraform (Terraform のインストール)](https://www.terraform.io/intro/getting-started/install.html)」Web ページの手順に従います。このページでは、以下について説明しています。

- Terraform のダウンロード
- Terraform のインストール
- Terraform が正しくインストールされていることの確認

>[!Tip]
>ご自身の PATH システム変数の設定に関する手順に従ってください。

### <a name="install-nodejs"></a>Node.js のインストール

Cloud Shell で Terraform を使用するには、[Node.js をインストール](https://nodejs.org/)する必要があります (Node.js 6.0 以上)。

>[!NOTE]
>Node.js がインストールされているかどうかを確認するには、ターミナル ウィンドウを開いて、「`node -v`」と入力します。

### <a name="install-graphviz"></a>GraphViz のインストール

Terraform 視覚化機能を使用するには、[GraphViz をインストール](https://graphviz.org/)する必要があります。

>[!NOTE]
>GraphViz がインストールされているかどうかを確認するには、ターミナル ウィンドウを開いて、「`dot -V`」と入力します。

### <a name="install-the-azure-terraform-visual-studio-code-extension"></a>Azure Terraform Visual Studio Code 拡張機能のインストール

1. Visual Studio Code を起動します。

1. **[拡張機能]** を選択します。

    ![拡張機能ボタン](media/configure-vs-code-extension-for-terraform/tf-vscode-extensions-button.png)

1. **Marketplace の拡張機能の検索**テキスト ボックスを使用して、Azure Terraform 拡張機能を検索します。

    ![Marketplace での Visual Studio Code 拡張機能の検索](media/configure-vs-code-extension-for-terraform/tf-search-extensions.png)

1. **[インストール]** を選択します。

    >[!NOTE]
    >Azure Terraform 拡張機能の **[インストール]** を選択すると、Visual Studio Code によって Azure アカウントの拡張機能が自動的にインストールされます。 Azure アカウントは Azure Terraform 拡張機能の依存関係ファイルであり、Azure サブスクリプション認証および Azure 関連のコード拡張機能の実行に使用されます。

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Visual Studio Code に Terraform 拡張機能がインストールされていることの確認

1. **[拡張機能]** を選択します。

1. 検索テキスト ボックスに「`@installed`」と入力します。

    ![インストール済み拡張機能](media/configure-vs-code-extension-for-terraform/tf-installed-extensions.png)

Azure Terraform 拡張機能が、インストール済み拡張機能の一覧に表示されます。

![インストール済み Terraform 拡張機能](media/configure-vs-code-extension-for-terraform/tf-installed-terraform-extension-button.png)

お使いの Cloud Shell 環境でサポートされているすべての Terraform コマンドを、Visual Studio Code 内から実行できるようになりました。

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>演習 1:基本的な Terraform コマンドのチュートリアル

この演習では、新しい Azure リソース グループをプロビジョニングする基本的な Terraform 構成ファイルを作成して実行します。

### <a name="prepare-a-test-plan-file"></a>テスト計画ファイルの準備

1. Visual Studio Code で、メニュー バーから **[ファイル] > [新しいファイル]** を選択します。

1. ブラウザーで [Terraform azurerm_resource_group ページ](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#)に移動し、**使用例**コード ブロックのコードをコピーします。

    ![使用例](media/configure-vs-code-extension-for-terraform/tf-azurerm-resource-group-example-usage.png)

1. コピーしたコードを、Visual Studio Code で作成した新しいファイルに貼り付けます。

    ![使用例コードの貼り付け](media/configure-vs-code-extension-for-terraform/tf-paste-example-usage-code.png)

    >[!NOTE]
    >リソース グループの **name** 値は変更できますが、この値はご自身の Azure サブスクリプションに対して一意である必要があります。

1. メニュー バーから **[ファイル] > [名前を付けて保存]** を選択します。

1. **[名前を付けて保存]** ダイアログで、任意の場所に移動し、 **[新しいフォルダー]** を選択します  (新しいフォルダーの名前を、 *[新しいフォルダー]* 以外のわかりやすい名前に変更してください)。

    >[!NOTE]
    >この例では、フォルダーに TERRAFORM-TEST-PLAN という名前を付けます。

1. 新しいフォルダーが強調表示 (選択) されていることを確認し、 **[開く]** を選択します。

1. **[名前を付けて保存]** ダイアログで、ファイルの既定の名前を *main.tf* に変更します。

    ![main.tf として保存](media/configure-vs-code-extension-for-terraform/tf-save-as-main.png)

1. **[保存]** を選択します。
1. メニュー バーで、 **[ファイル] > [フォルダーを開く]** を選択します。 作成した新しいフォルダーに移動し、選択します。

### <a name="run-terraform-init-command"></a>Terraform *init* コマンドの実行

1. Visual Studio Code を起動します。

1. Visual Studio Code のメニュー バーから、 **[ファイル] > [フォルダーを開く]** を選択し、*main.tf* ファイルを見つけて選択します。

    ![main.tf ファイル](media/configure-vs-code-extension-for-terraform/tf-main-tf.png)

1. メニュー バーで、 **[表示] > [コマンド パレット] > [Azure Terraform:Init]** の順に選択します。

1. 確認のメッセージが表示されたら **[OK]** を選択します。

    ![Cloud Shell を開きますか、と尋ねるメッセージ](media/configure-vs-code-extension-for-terraform/tf-do-you-want-to-open-cloud-shell.png)

1. 新しいフォルダーから初めて Cloud Shell を起動すると、Web アプリケーションを作成するよう求められます。 **[Open (開く)]** を選択します。

    ![初めての Cloud Shell の起動](media/configure-vs-code-extension-for-terraform/tf-first-launch-of-cloud-shell.png)

1. [Welcome to Azure Cloud Shell]\(Azure Cloud Shell へようこそ\) ページが開きます。 Bash または PowerShell を選択します。

    ![Azure Cloud Shell へようこそ](media/configure-vs-code-extension-for-terraform/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >この例では、Bash (Linux) が選択されています。

1. Azure ストレージ アカウントをまだ設定していない場合は、次の画面が表示されます。 **[Create storage]\(ストレージの作成\)** を選択します。

    ![ストレージがマウントされていません](media/configure-vs-code-extension-for-terraform/tf-you-have-no-storage-mounted.png)

1. Azure Cloud Shell によって、前に選択したシェルが起動され、先ほど作成されたクラウド ドライブに関する情報が表示されます。

    ![クラウド ドライブが作成されました](media/configure-vs-code-extension-for-terraform/tf-your-cloud-drive-has-been-created-in.png)

1. ここで Cloud Shell を終了できます

1. メニュー バーから、 **[表示]**  >  **[コマンド パレット]**  >  **[Azure Terraform: init]** を選択します。

    ![Terraform が正常に初期化されました](media/configure-vs-code-extension-for-terraform/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>プランの視覚化

先ほどこのチュートリアルの中で GraphViz をインストールしました。 Terraform では、構成または実行プランを視覚化するために GraphViz を使用できます。 この機能は、Azure Terraform Visual Studio Code 拡張機能の *visualize* コマンドによって実装されます。

- メニュー バーで、 **[表示] > [コマンド パレット] > [Azure Terraform:Visualize]** の順に選択します。

    ![プランの視覚化](media/configure-vs-code-extension-for-terraform/tf-graph.png)

### <a name="run-terraform-plan-command"></a>Terraform *plan* コマンドの実行

Terraform *plan* コマンドは、一連の変更に対する実行プランが意図したとおりに動作するかどうかの確認に使用されます。

>[!NOTE]
>Terraform *plan* によって、実際の Azure リソースに変更が行われることはありません。 ご自身のプランに含まれる変更を実際に行うには、Terraform *apply* コマンドを使用します。

- メニュー バーから、 **[表示]**  >  **[コマンド パレット]**  >  **[Azure Terraform: plan]** を選択します。

    ![Terraform プラン](media/configure-vs-code-extension-for-terraform/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>Terraform *apply* コマンドの実行

Terraform *plan* の結果に満足したら、*apply* コマンドを実行できます。

1. メニュー バーから、 **[表示]**  >  **[コマンド パレット]**  >  **[Azure Terraform: apply]** を選択します。

    ![terraform apply](media/configure-vs-code-extension-for-terraform/tf-terraform-apply.png)

1. 「`yes`」と入力します。

    ![Terraform apply、yes](media/configure-vs-code-extension-for-terraform/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>目的の Terraform プランが実行されていることの確認

目的の新しい Azure リソース グループが正常に作成されたかどうかを確認するには:

1. Azure portal を開きます。

1. 左側のナビゲーション ウィンドウで、 **[リソース グループ]** を選択します。

    ![新しいリソースの確認](media/configure-vs-code-extension-for-terraform/tf-verify-resource-group-created.png)

新しいリソース グループが **[名前]** 列に表示されています。

>[!NOTE]
>この Azure portal ウィンドウは開いておいてください。次の手順で使用します。

### <a name="run-terraform-destroy-command"></a>Terraform *destroy* コマンドの実行

1. メニュー バーから、 **[表示]**  >  **[コマンド パレット]**  >  **[Azure Terraform: destroy]** を選択します。

    ![Terraform destroy](media/configure-vs-code-extension-for-terraform/tf-terraform-destroy.png)

1. 「*yes*」と入力します。

    ![Terraform destroy、yes](media/configure-vs-code-extension-for-terraform/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>目的のリソース グループが破棄されていることの確認

新しいリソース グループが Terraform によって正常に破棄されていることを確認するには:

1. Azure portal の **[リソース グループ]** ページで、 **[更新]** を選択します。

1. 目的のリソース グループは表示されません。

    ![リソース グループが破棄されていることの確認](media/configure-vs-code-extension-for-terraform/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>演習 2:Terraform *compute* モジュール

この演習では、Terraform *compute* モジュールを Visual Studio Code 環境に読み込む方法について説明します。

### <a name="clone-the-terraform-azurerm-compute-module"></a>terraform-azurerm-compute モジュールのクローン

1. [こちらのリンク](https://github.com/Azure/terraform-azurerm-compute)を使用して、GitHub 上の Terraform Azure Rm Compute モジュールにアクセスします。

1. **[Clone or download]\(クローンまたはダウンロード\)** を選択します。

    ![複製またはダウンロード](media/configure-vs-code-extension-for-terraform/tf-clone-with-https.png)

    >[!NOTE]
    >この例では、使用するフォルダーに *terraform azurerm-compute* という名前が付けられています。

### <a name="open-the-folder-in-visual-studio-code"></a>このフォルダーを Visual Studio Code で開きます。

1. Visual Studio Code を起動します。

1. メニュー バーから、 **[ファイル] > [フォルダーを開く]** を選択し、前の手順で作成したフォルダーに移動して選択します。

    ![terraform-azurerm-compute フォルダー](media/configure-vs-code-extension-for-terraform/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Terraform を初期化する

Visual Studio Code 内からの Terraform コマンドの使用を開始するには、2 つの Azure プロバイダー用のプラグイン (random および azurerm) をダウンロードする必要があります。

1. Visual Studio Code IDE のターミナル ウィンドウで、「`terraform init`」と入力します

    ![terraform init コマンド](media/configure-vs-code-extension-for-terraform/tf-terraform-init-command.png)

1. 「`az login`」と入力し、**Enter** キーを押して、画面の指示に従います。

### <a name="module-test-lint"></a>モジュール テスト: *lint*

1. メニュー バーで、 **[表示] > [コマンド パレット] > [Azure Terraform:Execute Test]** の順に選択します。

1. テストの種類のオプション一覧から **[lint]** を選択します。

    ![テストの種類の選択](media/configure-vs-code-extension-for-terraform/tf-select-type-of-test-lint.png)

1. 確認のメッセージが表示されたら **[OK]** を選択し、画面の指示に従います。

    ![CloudShell を開くかどうかを尋ねるメッセージ](media/configure-vs-code-extension-for-terraform/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>**lint** または **end to end** のテストを実行すると、Azure では、コンテナー サービスを使用して、実際のテストを実行するテスト マシンがプロビジョニングされます。 このため、通常、テスト結果が返されるまでに数分かかる場合があります。

しばらくすると、次の例のようなリストがターミナル ウィンドウに表示されます。

![lint テストの結果](media/configure-vs-code-extension-for-terraform/tf-lint-test-results.png)

### <a name="test-the-module"></a>モジュールのテスト

1. メニュー バーで、 **[表示] > [コマンド パレット] > [Azure Terraform:Execute Test]** の順に選択します。

1. テストの種類のオプション一覧から **[end to end]** を選択します。

    ![テストの種類の選択](media/configure-vs-code-extension-for-terraform/tf-select-type-of-test-end-to-end.png)

1. 確認のメッセージが表示されたら **[OK]** を選択し、画面の指示に従います。

    ![CloudShell を開くかどうかを尋ねるメッセージ](media/configure-vs-code-extension-for-terraform/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>**lint** または **end to end** のテストを実行すると、Azure では、コンテナー サービスを使用して、実際のテストを実行するテスト マシンがプロビジョニングされます。 このため、通常、テスト結果が返されるまでに数分かかる場合があります。

しばらくすると、次の例のようなリストがターミナル ウィンドウに表示されます。

![Test results](media/configure-vs-code-extension-for-terraform/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>次のステップ
> [!div class="nextstepaction"]
> [Azure およびサポートされているその他のプロバイダーで使用できる Terraform モジュールの一覧](https://registry.terraform.io/)
