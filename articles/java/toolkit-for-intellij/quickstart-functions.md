---
title: IntelliJ IDEA を使用して Azure で初めての関数を作成する
description: Azure Toolkit for IntelliJ を使用して、HTTP によってトリガーされる単純な関数を作成し、Azure に発行します。
ms.topic: quickstart
ms.date: 03/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: b27655657d763e295a1817acf200a67c090ff67d
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831408"
---
# <a name="quickstart-create-an-azure-functions-project-using-intellij-idea"></a>クイック スタート:IntelliJ IDEA を使用して Azure Functions プロジェクトを作成する

この記事では、IntelliJ IDEA を使用して、HTTP 要求に応答する関数を作成します。 コードをローカルでテストした後、Azure Functions のサーバーレス環境にデプロイします。 このクイックスタートを完了すると、ご利用の Azure アカウントでわずかな (数セント未満の) コストが発生します。

## <a name="configure-your-environment"></a>環境を構成する

作業を開始する前に、次の要件が満たされていることを確認します。

+ アクティブなサブスクリプションが含まれる Azure アカウント。 [無料でアカウントを作成できます](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
+ [Azure でサポートされている Java 8 用の Java Development Kit (JDK)](../fundamentals/java-jdk-long-term-support.md)
+ [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition または Community Edition がインストールされていること
+ [Maven 3.5.0 以降](https://maven.apache.org/download.cgi)
+ 最新の [Function Core Tools](https://github.com/Azure/azure-functions-core-tools)

## <a name="installation-and-sign-in"></a>インストールとサインイン

1. IntelliJ IDEA の [設定/環境設定] ダイアログ (Ctrl+Alt+S) で、 **[プラグイン]** を選択します。 次に、**Marketplace** で **Azure Toolkit for IntelliJ** を見つけ、 **[インストール]** をクリックします。 インストール後、 **[再起動]** をクリックしてプラグインをアクティブにします。 

   ![Marketplace の Azure Toolkit for IntelliJ プラグイン][marketplace]

2. Azure アカウントにサインインするには、サイドバーの **Azure Explorer** を開き、上部のバーにある **[Azure サインイン]** アイコンをクリックします (または [IDEA] メニューから **[ツール]/[Azure]/ [Azure サインイン]** をクリックします)。

   ![IntelliJ Azure サインイン コマンド][I01]

3. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、次に **[サインイン]** をクリックします ([他のサインイン オプション](sign-in-instructions.md))。

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

4. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

5. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

   ![デバイスのログイン ブラウザー][I04]

6. **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="create-your-local-project"></a>ローカル プロジェクトを作成する

このセクションでは、Azure Toolkit for IntelliJ を使用して、ローカル Azure Functions プロジェクトを作成します。 後からこの記事の中で、関数コードを Azure に発行します。 

1. IntelliJ のウェルカム ダイアログを開き、 *[Create New Project]\(新規プロジェクトの作成\)* を選択して新規プロジェクト ウィザードを開き、 *[Azure Functions]* を選択します。

    ![関数プロジェクトを作成する](media/quickstart-functions/create-functions-project.png)

1. *[Http Trigger]\(HTTP トリガー\)* を選択し、 *[Next]\(次へ\)* をクリックして、以降のページですべての構成を行います。プロジェクトの場所を確認して *[Finish]\(終了\)* をクリックすると、Intellj IDEA で新しいプロジェクトが開きます。

    ![関数プロジェクトの作成の終了](media/quickstart-functions/create-functions-project-finish.png)

## <a name="run-the-function-app-locally"></a>関数アプリをローカルで実行する

1. `src/main/java/org/example/functions/HttpTriggerFunction.java`に移動して生成されたコードを確認します。 行 *17*の横に、緑色の *[Run]\(実行\)* ボタンが表示されています。それをクリックして *[Run 'azure-function-exam...']\('azure-function-exam...' の実行\)* を選択すると、関数アプリが実行されていくつかのログが出力されます。

    ![関数プロジェクトのローカル実行](media/quickstart-functions/local-run-functions-project.png)

    ![関数プロジェクトのローカル実行の出力](media/quickstart-functions/local-run-functions-output.png)

1. 出力されたエンドポイントにブラウザーからアクセスし、関数を試すことができます (例: `http://localhost:7071/api/HttpTrigger-Java?name=Azure`)。

    ![関数のローカル実行のテスト結果](media/quickstart-functions/local-run-functions-test.png)

1. ログは IDEA にも出力されます。ここで、"*停止*" ボタンをクリックして関数を停止します。

    ![関数のローカル実行のテスト ログ](media/quickstart-functions/local-run-functions-log.png)

## <a name="debug-the-function-app-locally"></a>関数アプリをローカルでデバッグする

1. 関数アプリをローカルでデバッグしてみましょう。ツールバーの *[Debug]\(デバッグ\)* ボタンをクリックします (表示されていない場合は、 *[View]\(表示\) -> [Appearance]\(外観\) -> [Toolbar]\(ツールバー\)* をクリックしてツールバーを有効にします)。

    ![関数をローカルでデバッグするボタン](media/quickstart-functions/local-debug-functions-button.png)

1. ファイル `src/main/java/org/example/functions/HttpTriggerFunction.java` の行 *20* をクリックしてブレークポイントを追加し、エンドポイント `http://localhost:7071/api/HttpTrigger-Java?name=Azure` に再度アクセスすると、ブレークポイントにヒットします。これにより、"*ステップ実行*"、"*ウォッチ*"、"*評価*"など、より多くのデバッグ機能を試すことができます。 停止ボタンをクリックしてデバッグ セッションを停止します。

    ![関数のローカル デバッグの中断](media/quickstart-functions/local-debug-functions-break.png)

## <a name="deploy-your-function-app-to-azure"></a>Azure に関数アプリをデプロイする

1. IntelliJ の [Project]\(プロジェクト\) エクスプローラーで、 *[Azure] -> [Deploy to Azure Functions]\(Azure Functionsにデプロイ\)* を選択します。

    ![Azure に関数をデプロイする](media/quickstart-functions/deploy-functions-to-azure.png)

1. まだ関数アプリがない場合は、 *[No available function, click to create a new one]\(使用可能な関数がありません。クリックして新規作成します\)* をクリックします。

    ![Azure への関数のデプロイでのアプリの作成](media/quickstart-functions/deploy-functions-create-app.png)

1. 関数アプリの名前を入力して適切なサブスクリプション、プラットフォーム、リソース グループ、App Service プランを選択します。ここで、リソース グループや App Service プランを作成することもできます。 次に、アプリの設定を変更せずに *[OK]* をクリックして新しい関数が作成されるまでしばらく待ちます。 *[Creating New Function App]\(新しい関数アプリを作成しています\)* 進行状況バーが消えます。

    ![Azure への関数のデプロイでのアプリの作成ウィザード](media/quickstart-functions/deploy-functions-create-app-wizard.png)

1. デプロイする関数アプリを選択します (作成した新しい関数アプリが自動的に選択されます)。 *[Run]\(実行\)* をクリックして関数をデプロイします。

    ![Azure への関数のデプロイの実行](media/quickstart-functions/deploy-functions-run.png)

    ![Azure への関数のデプロイのログ](media/quickstart-functions/deploy-functions-log.png)

## <a name="manage-azure-functions-from-idea"></a>IDEA から Azure Functions を管理する

1. IDEA の "*Azure 用エクスプローラー*" を使用して関数を管理できます。 *[Function App]\(関数アプリ\)* をクリックすると、ここにすべての関数が表示されます。

    ![エクスプローラーでの関数の表示](media/quickstart-functions/explorer-view-functions.png)

1. いずれかの関数をクリックして選択し、右クリックして *[Show Properties]\(プロパティの表示\)* を選択して詳細ページを開きます。 

    ![関数プロパティの表示](media/quickstart-functions/explorer-functions-show-properties.png)

1. 関数 *HttpTrigger-Java* を右クリックし、 *[Trigger Function]\(関数のトリガー\)* を選択します。トリガー URL でブラウザーが開きます。

    ![Azure への関数のデプロイの実行](media/quickstart-functions/explorer-trigger-functions.png)

## <a name="add-more-functions-to-the-project"></a>プロジェクトに関数を追加する

1. パッケージ *org.example.functions* を右クリックして *[New]\(新規\) -> [Azure Function Class]\(Azure 関数クラス\)* を選択します。 

    ![プロジェクト エントリへの関数の追加](media/quickstart-functions/add-functions-entry.png)

1. 関数クラスの作成ウィザードで、クラス名に「*HttpTest*」を入力し、 *[HttpTrigger]* を選択し、 *[OK]* をクリックして作成します。この方法で、新しい関数を必要なだけ作成できます。

    ![プロジェクトへの関数の追加でのトリガーの選択](media/quickstart-functions/add-functions-trigger.png)
    
    ![プロジェクトへの関数の追加での出力](media/quickstart-functions/add-functions-output.png)

## <a name="cleaning-up-functions"></a>関数のクリーンアップ

1. Azure Explorer での Azure Functions の削除
      
      ![プロジェクトへの関数の追加でのトリガーの選択](media/quickstart-functions/delete-function.png)
      

## <a name="next-steps"></a>次のステップ

HTTP でトリガーされる関数を含む Java 関数プロジェクトを作成し、ローカル コンピューターでそれを実行し、Azure にデプロイしました。 次は以下の方法で関数を拡張します。

> [!div class="nextstepaction"]
> [Azure Storage キュー出力バインドを追加する](/azure/azure-functions/functions-add-output-binding-storage-queue-java)


[marketplace]:./media/create-hello-world-web-app/marketplace.png
[I01]: media/sign-in-instructions/I01.png
[I02]: media/sign-in-instructions/I02.png
[I03]: media/sign-in-instructions/I03.png
[I04]: media/sign-in-instructions/I04.png
[I05]: media/sign-in-instructions/I05.png