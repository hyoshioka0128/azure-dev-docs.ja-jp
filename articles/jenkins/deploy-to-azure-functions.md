---
title: チュートリアル - Jenkins を使用して Azure Functions にデプロイする
description: Jenkins Azure Functions プラグインを使用して Azure Functions にデプロイする方法を説明します
keywords: Jenkins, Azure, DevOps, Java, Azure Functions
ms.topic: tutorial
ms.date: 01/11/2021
ms.custom: devx-track-jenkins,devx-track-cli
ms.openlocfilehash: 5e246b21f44de9ccb9b876827b44d01f456e60fd
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699950"
---
# <a name="tutorial-deploy-to-azure-functions-using-jenkins"></a>チュートリアル:Jenkins を使用して Azure Functions にデプロイする

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

[Azure Functions](/azure/azure-functions/) はサーバーレス コンピューティング サービスです。 Azure Functions を使用すると、インフラストラクチャをプロビジョニングまたは管理することなく、オンデマンドでコードを実行できます。 このチュートリアルでは、Azure Functions プラグインを使用して Java 関数を Azure Functions にデプロイする方法を説明します。

## <a name="prerequisites"></a>前提条件

- **Azure サブスクリプション**:Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) を作成してください。
- **Jenkins サーバー**: Jenkins サーバーがインストールされていない場合は、[Azure での Jenkins サーバーの作成](./configure-on-linux-vm.md)に関する記事を参照してください。

## <a name="view-the-source-code"></a>ソース コードを表示する

このチュートリアルに使用されているソース コードは [Visual Studio China GitHub リポジトリ](https://github.com/VSChina/odd-or-even-function/blob/master/src/main/java/com/microsoft/azure/Function.java)にあります。

## <a name="create-a-java-function"></a>Java 関数の作成

Java ランタイム スタックを備えた Java 関数を作成するには、[Azure portal](https://portal.azure.com) または [Azure CLI](/cli/azure/) を使用します。

次の手順では、Azure CLI を使用して Java 関数を作成する方法を説明します。

1. **&lt;resource_group>** プレースホルダーを実際のリソース グループ名に置き換えて、リソース グループを作成します。

    ```azurecli
    az group create --name <resource_group> --location eastus
    ```

1. プレースホルダーを適切な値に置き換えて、Azure ストレージ アカウントを作成します。
 
    ```azurecli
    az storage account create --name <storage_account> --location eastus --resource-group <resource_group> --sku Standard_LRS    
    ```

1. プレースホルダーを適切な値に置き換えて、テスト用の関数アプリを作成します。

    ```azurecli
    az functionapp create --resource-group <resource_group> --runtime java --consumption-plan-location eastus --name <function_app> --storage-account <storage_account> --functions-version 2
    ```

## <a name="prepare-jenkins-server"></a>Jenkins サーバーの準備

次の手順では、Jenkins サーバーを準備する方法について説明します。

1. Azure に [Jenkins サーバー](https://azuremarketplace.microsoft.com/marketplace/apps/bitnami.production-jenkins)をデプロイします。 Jenkins サーバーのインスタンスをまだインストールしていない場合は、[Azure での Jenkins サーバーの作成](./configure-on-linux-vm.md)に関する記事でそのプロセスが説明されています。

1. SSH を使用して Jenkins インスタンスにサインインします。

1. Jenkins インスタンスに Az CLI バージョン 2.0.67 以上をインストールします。

1. 次のコマンドを使用して Maven をインストールします。

    ```bash
    sudo apt install -y maven
    ```

1. Jenkins インスタンスのターミナルのプロンプトで、次のコマンドを実行して [Azure Functions Core Tools](/azure/azure-functions/functions-run-local) をインストールします。

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    cat /etc/apt/sources.list.d/dotnetdev.list
    sudo apt-get update
    sudo apt-get install azure-functions-core-tools-3
    ```

1. Jenkins で Azure リソースに対する認証とアクセスを行うには、Azure サービス プリンシパルが必要です。 詳細な手順については、[Azure App Service へのデプロイ](./deploy-to-azure-app-service-using-azure-cli.md)に関する記事を参照してください。

1. [資格情報プラグイン](https://plugins.jenkins.io/credentials/)がインストールされていることを確認します。

    1. メニューから、 **[Manage Jenkins]\(Jenkins の管理\)** を選択します。

    1. **[System Configuration]\(システム構成\)** で、 **[Manage plug-in]\(プラグインの管理\)** を選択します。

    1. **[インストール済み]** タブを選択します。

    1. **フィルター** フィールドに `credentials` と入力します。
    
    1. **資格情報プラグイン** がインストールされていることを確認します。 インストールされていない場合は、 **[Available]\(使用可能\)** タブからインストールする必要があります。

    ![資格情報プラグインをインストールする必要があります。](./media/deploy-to-azure-functions/credentials-plugin.png)

1. メニューから、 **[Manage Jenkins]\(Jenkins の管理\)** を選択します。

1. **[セキュリティ]** で **[資格情報の管理]** を選択します。

1. **[Credentials]\(資格情報\)** で、 **[(global)]\(\(グローバル\)\)** を選択します。

1. メニューで、 **[Add Credentials]\(資格情報の追加\)** を選択します。

1. [Microsoft Azure サービス プリンシパル](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%252fazure%252fazure-resource-manager%252ftoc.json)の次の値を入力します。

    - **種類**:種類が **_[Username with password]\(ユーザー名とパスワード\)_* _ であることを確認します。
    - _*ユーザー名**: 作成されたサービス プリンシパルの **_appId_*_。
    - _*パスワード**: 作成されたサービス プリンシパルの "**_パスワード_*_"。
    - _*ID**:`as azuresp` など、資格情報の識別子。

1. **[OK]** を選択します。

## <a name="fork-the-sample-github-repo"></a>サンプル GitHub リポジトリのフォーク

1. [奇数または偶数のサンプル アプリ用の GitHub リポジトリにサインインします](https://github.com/VSChina/odd-or-even-function.git)。

1. GitHub の右上隅にある **[Fork]\(フォーク\)** を選択します。

1. プロンプトに従ってお客様の GitHub アカウントを選択し、フォークを完了します。

## <a name="create-a-jenkins-pipeline"></a>Jenkins パイプラインの作成

このセクションでは、[Jenkins パイプライン](https://jenkins.io/doc/book/pipeline/)を作成します。

1. Jenkins ダッシュボードで、パイプラインを作成します。

1. **[Prepare an environment for the run]\(実行環境を準備する\)** を有効にします。

1. **[Pipeline]\(パイプライン\)、[Definition]\(定義\)** セクションの順に移動し、**[Pipeline script from SCM]\(SCM からのパイプライン スクリプト\)** を選択します。

1. GitHub フォークの URL とスクリプト パス ("doc/resources/jenkins/JenkinsFile") を入力し、[JenkinsFile の例](https://github.com/VSChina/odd-or-even-function/blob/master/doc/resources/jenkins/JenkinsFile)で使用します。

   ```nodejs
    node {
    withEnv(['AZURE_SUBSCRIPTION_ID=99999999-9999-9999-9999-999999999999',
            'AZURE_TENANT_ID=99999999-9999-9999-9999-999999999999']) {
        stage('Init') {
            cleanWs()
            checkout scm
        }

        stage('Build') {
            sh 'mvn clean package'
        }

        stage('Publish') {
            def RESOURCE_GROUP = '<resource_group>' 
            def FUNC_NAME = '<function_app>'
            // login Azure
            withCredentials([usernamePassword(credentialsId: 'azuresp', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
            sh '''
                az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                az account set -s $AZURE_SUBSCRIPTION_ID
            '''
            }
            sh 'cd $PWD/target/azure-functions/odd-or-even-function-sample && zip -r ../../../archive.zip ./* && cd -'
            sh "az functionapp deployment source config-zip -g $RESOURCE_GROUP -n $FUNC_NAME --src archive.zip"
            sh 'az logout'
            }
        }
    }
    ```

## <a name="build-and-deploy"></a>ビルドとデプロイ

ここで、Jenkins ジョブを実行します。

1. 最初に、記事「[Azure Functions のトリガーとバインド](/azure/azure-functions/functions-bindings-http-webhook-trigger#authorization-keys)」の手順を使用して、承認キーを取得します。

1. ブラウザーでアプリの URL を入力します。 プレースホルダーを適切な値に置き換え、Java 関数に対する入力として **&lt;input_number>** の数値を指定します。

    ```
    https://<function_app>.azurewebsites.net/api/HttpTrigger-Java?code=<authorization_key>&number=<input_number>
    ```
1. 次の出力例のような結果が表示されます (この場合、テストとして 365 という奇数が使用されました)。

    ```output
    The number 365 is Odd.
    ```

## <a name="clean-up-resources"></a>リソースをクリーンアップする

このアプリケーションを引き続き使用しない場合は、作成したリソースを次の手順で削除します。

```azurecli
az group delete -y --no-wait -n <resource_group>
```

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Azure Functions](/azure/azure-functions/)