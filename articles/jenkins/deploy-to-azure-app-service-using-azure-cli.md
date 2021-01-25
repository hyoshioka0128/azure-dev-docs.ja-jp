---
title: チュートリアル - Jenkins と Azure CLI を使用して Azure App Service にデプロイする
description: Azure CLI を使用して Java Web アプリを Jenkins パイプラインで Azure にデプロイする方法について説明します
keywords: Jenkins, Azure, 開発, App Service, CLI
ms.topic: tutorial
ms.date: 01/06/2021
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: 1f73da29b6b1bff2abf92383d672afd5af92abe4
ms.sourcegitcommit: 0eb25e1fdafcd64118843748dc061f60e7e48332
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98625999"
---
# <a name="tutorial-deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>チュートリアル:Jenkins と Azure CLI を使用して Azure App Service にデプロイする

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

Java Web アプリを Azure にデプロイするには、[Jenkins パイプライン](https://jenkins.io/doc/book/pipeline/)で Azure CLI を使用します。 このチュートリアルでは、次のタスクを行います。

> [!div class="checklist"]
> * Jenkins VM を作成する
> * Jenkins を構成する
> * Azure で Web アプリを作成する
> * GitHub レポジトリを準備する
> * Jenkins パイプラインを作成する
> * パイプラインを実行し、Web アプリを確認する

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

- **Jenkins** - [Linux VM に Jenkins をインストールします](configure-on-linux-vm.md)
- **Azure CLI**:Azure CLI (バージョン 2.0.67 以上) を Jenkins サーバーにインストールします。

## <a name="configure-jenkins"></a>Jenkins を構成する

次の手順は、必要な Java JDK と Maven を Jenkins コントローラーにインストールする方法を示しています。

1. SSH を使用して Jenkins コントローラーにサインインします。

1. [apt-get リポジトリから Azul Zulu JDK をダウンロードしてインストールする](../java/fundamentals/java-jdk-install.md#download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository):

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
    sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
    sudo apt-get -q update
    sudo apt-get -y install zulu-8-azure-jdk
    ```
    
1. 次のコマンドを実行して Maven をインストールします。

    ```bash
    sudo apt-get install -y maven
    ```
    
## <a name="add-azure-service-principal-to-a-jenkins-credential"></a>Jenkins 資格情報への Azure サービス プリンシパルの追加

次の手順は、Azure 資格情報を指定する方法を示しています。

1. [Credentials プラグイン](https://plugins.jenkins.io/credentials/)がインストールされていることを確認します。

1. Jenkins ダッシュボードで、 **[Credentials]\(資格情報\) -> [System]\(システム\)** を選択します。 

1. **[Global credentials(unrestricted)]\(グローバル資格情報 (制限なし)\)** を選択します。

1. [Microsoft Azure サービス プリンシパル](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%252fazure%252fazure-resource-manager%252ftoc.json)を追加するには、 **[Add Credentials]\(資格情報の追加\)** を選択します。 資格情報の種類が **_[Username with password]\(ユーザー名とパスワード\)_* _ であることを確認し、次の項目を入力します。

    _ **[Username]\(ユーザー名\)** :サービス プリンシパル `appId`
    * **パスワード**:サービス プリンシパル `password`
    * **[ID]** :資格情報の識別子 (`AzureServicePrincipal` など)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Java Web アプリをデプロイするための Azure App Service の作成

[az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) を使用して、**Free** 価格レベルで Azure App Service プランを作成します。

```azurecli
az appservice plan create \
    --name <app_service_plan> \ 
    --resource-group <resource_group> \
    --sku FREE
```

**注**:

- App Service プランは、アプリをホストするために使用される物理リソースを定義します。
- App Service プランに割り当てられたすべてのアプリケーションがこれらのリソースを共有します。
- Appservice プランを使用すると、複数のアプリをホストするときのコストを削減できます。

## <a name="create-an-azure-web-app"></a>Azure Web アプリを作成する

[az webapp create](/cli/azure/webapp#az-webapp-create) を使用して、`myAppServicePlan` App Service プランで Web アプリ定義を作成します。

```azurecli
az webapp create \
    --name <app_name> \ 
    --resource-group <resource_group> \
    --plan <app_service_plan>
```

**注**:

- Web アプリ定義によって、アプリケーションにアクセスするための URL が提供され、Azure にコードをデプロイするためのいくつかのオプションが構成されます。
- `<app_name>` プレースホルダーを一意のアプリ名で置き換えます。
- アプリ名は、Web アプリの既定のドメイン名の一部です。 そのため、この名前は、Azure のすべてのアプリで一意である必要があります。
- Web アプリをユーザーに公開する前に、カスタム ドメイン名エントリを Web アプリにマップできます。


## <a name="configure-java"></a>Java を構成する

[az appservice web config update](/cli/azure/webapp/config) を使用して、アプリの Java ランタイム構成を設定します。

```azurecli
az webapp config set \ 
    --name <app_name> \
    --resource-group <resource_group> \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>GitHub レポジトリを準備する

1. [Azure 用のシンプルな Java Web アプリ](https://github.com/azure-devops/javawebappsample) レポジトリを開きます。

1. 自身の GitHub アカウントにリポジトリをフォークするには、 **[Fork]** ボタンを選択します。

1. ファイル名をクリックして、**Jenkinsfile** ファイルを開きます。

1. 鉛筆アイコンを選択してファイルを編集します。

1. サブスクリプション ID とテナント ID を更新します。
    
    ```groovy
      withEnv(['AZURE_SUBSCRIPTION_ID=<subscription_id>',
            'AZURE_TENANT_ID=<tenant_id>']) 
    ```
    
1. 行 22 と 23 にある、リソース グループと Web アプリの名前をそれぞれ更新します。

    ```groovy
    def resourceGroup = '<resource_group>'
    def webAppName = '<app_name>'
    ```

1. Jenkins インスタンスの資格情報 ID を更新します。

    ```groovy
    withCredentials([usernamePassword(credentialsId: '<service_princial>', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
    ```
    
## <a name="create-jenkins-pipeline"></a>Jenkins パイプラインを作成する

Jenkins パイプラインを作成するには、以下のことを行います。

1. Web ブラウザーで Jenkins を開きます。

1. **[New Item (新しい項目)]** を選択します。

1. ジョブの名前を入力します。

1. **[パイプライン]** を選択します。

1. **[OK]** を選択します。

1. **[パイプライン]** を選択します。

1. **[定義]** で、 **[Pipeline script from SCM]\(SCM からのパイプライン スクリプト\)** を選択します。

1. **[SCM]** で、 **[Git]** を選択します。

1. フォークしたリポジトリの GitHub URL を入力します: `https:\<forked_repo\>.git`

1. **[保存]** を選びます。

## <a name="test-your-pipeline"></a>パイプラインをテストする

1. 作成したパイプラインに移動します

1. **[Build Now]\(今すぐビルド\)** を選択します

1. ビルドが完了したら、 **[コンソール出力]** を選択して、ビルドの詳細を表示します。

## <a name="verify-your-web-app"></a>Web アプリを検証する

Web アプリに WAR ファイルが正常にデプロイされたことを確認するには、以下のことを行います。

1. 次の URL に移動します: `http://&lt;app_name>.azurewebsites.net/api/calculator/ping`

1. 次のようなテキストが表示されます。

    ```output
    Welcome to Java Web App!!! This is updated!
    Today's date
    ```

1. 次の URL に移動します (&lt;x> と &lt;y> は、合計する 2 つの値に置き換えます): http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y>。

    ![加算のデモを実行する例](./media/deploy-to-azure-app-service-using-azure-cli/calculator-add.png)

## <a name="deploy-to-azure-app-service-on-linux"></a>Azure App Service on Linux にデプロイする

App Service では、サポートされているアプリケーション スタック向けに Web アプリを Linux 上でネイティブにホストすることもできます。 また、カスタム Linux コンテナー (Web App for Containers とも呼ばれます) を実行することもできます。

スクリプトを変更して、Azure App Service on Linux にデプロイできます。 App Service on Linux では、Docker がサポートされています。 そのため、Web アプリとサービス ランタイムを Docker イメージにパッケージ化する Dockerfile を用意します。 プラグインによりイメージがビルドされて Docker レジストリにプッシュされ、Web アプリにイメージがデプロイされます。

1. Azure App Service on Linux と Azure Container Registry を作成するには、「[カスタム コンテナーを使用してカスタム ソフトウェアを Azure App Service に移行する](/azure/app-service/tutorial-custom-container?pivots=container-linux#configure-app-service-to-deploy-the-image-from-the-registry)」をご覧ください。

    ```azurecli
        az group create --name myResourceGroup2 --location westus2
        az acr create --name myACRName --resource-group myResourceGroup2 --sku Basic --admin-enabled true
        az appservice plan create --name myAppServicePlan --resource-group  myResourceGroup2 --is-linux
        az webapp create --resource-group myResourceGroup2 --plan myAppServicePlan --name myApp --deployment-container-image-name myACRName.azurecr.io/calculator:latest
    ```

1. [Jenkins に Docker をインストールする](https://docs.docker.com/engine/installation/linux/ubuntu/).

1. [Docker Pipeline プラグイン](https://plugins.jenkins.io/docker-workflow/)がインストールされていることを確認してください。

1. フォークした同じ [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) リポジトリで、**Jenkinsfile2** ファイルを次のように編集します。

    1. サブスクリプション ID とテナント ID を更新します。

        ```groovy
         withEnv(['AZURE_SUBSCRIPTION_ID=<mySubscriptionId>',
                'AZURE_TENANT_ID=<myTenantId>']) {
        ```

    1. リソース グループ、Web アプリ、および ACR の名前に更新します (プレースホルダーを実際の値に置き換えます)。

        ```bash
        def webAppResourceGroup = '<resource_group>'
        def webAppName = '<app_name>'
        def acrName = '<registry>'
        ```

    1. `<azsrvprincipal\>` を資格情報 ID に更新します

        ```bash
        withCredentials([usernamePassword(credentialsId: '<service_principal>', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
        ```

1. Azure Web アプリを Windows にデプロイしたときと同様に、`Jenkinsfile2` を使用して新しい Jenkins パイプラインを作成します。

1. 新しいジョブを実行します。

1. 確認するために、Azure CLI で次のコマンドを実行します。

    ```azurecli
    az acr repository list -n <myRegistry> -o json
    ```

    次のような結果が表示されます。
    
    ```output
    [
    "calculator"
    ]
    ```
    
1. `http://<app_name>.azurewebsites.net/api/calculator/ping` を参照します (プレースホルダーを置き換えます)。 次のような結果が表示されます。 

    ```output
    Welcome to Java Web App!!! This is updated!
    Today's date
    ```

1. `http://<app_name>.azurewebsites.net/api/calculator/add?x=<x>&y=<y>` を参照します (プレースホルダーを置き換えます)。 `x` と `y` に指定する値が合計されて表示されます。
    
## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure 上の Jenkins](/azure/jenkins)