---
title: チュートリアル - Jenkins と Azure CLI を使用して Azure App Service にデプロイする
description: Azure CLI を使用して Java Web アプリを Jenkins パイプラインで Azure にデプロイする方法について説明します
keywords: Jenkins, Azure, 開発, App Service, CLI
ms.topic: tutorial
ms.date: 08/08/2020
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: 9c5f3b30507cda764dd29f0c133d4997dbc2bb53
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "88240954"
---
# <a name="tutorial-deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>チュートリアル:Jenkins と Azure CLI を使用して Azure App Service にデプロイする

Java Web アプリを Azure にデプロイするには、[Jenkins パイプライン](https://jenkins.io/doc/book/pipeline/)で Azure CLI を使用します。 このチュートリアルでは、次のタスクを行います。

> [!div class="checklist"]
> * Jenkins VM を作成する
> * Jenkins を構成する
> * Azure で Web アプリを作成する
> * GitHub レポジトリを準備する
> * Jenkins パイプラインを作成する
> * パイプラインを実行し、Web アプリを確認する

## <a name="create-and-configure-jenkins-instance"></a>Jenkins インスタンスの作成と構成

Jenkins マスターがまだない場合は、[Linux VM に Jenkins をインストールします](configure-on-linux-vm.md)。

Azure Credentials プラグインでは、Jenkins に Microsoft Azure サービス プリンシパルの資格情報を格納できます。 バージョン 1.2 では、Jenkins パイプラインで Azure 資格情報を取得できるようにサポートを強化しました。 

バージョン 1.2 以降を使用していることを確認します。

* Jenkins ダッシュボード内で、**[Manage Jenkins]\(Jenkins の管理\) -> [プラグイン マネージャー] ->** の順にクリックして **[Azure 資格情報]** を検索します。 
* バージョンが 1.2 以前である場合は、プラグインを更新します。

Java JDK と Maven も Jenkins マスターで必要です。 インストールするには、SSH を使用して Jenkins マスターにサインインし、次のコマンドを実行します。

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-a-jenkins-credential"></a>Jenkins 資格情報への Azure サービス プリンシパルの追加

Azure CLI を実行するには、Azure の資格情報が必要です。

* Jenkins ダッシュボードで、**[資格情報] -> [システム] ->** の順にクリックします。 **[Global credentials(unrestricted)]\(グローバル資格情報 (制限なし)\)** をクリックします。
* **[資格情報の追加]** をクリックして、サブスクリプション ID、クライアント ID、クライアント シークレット、OAuth 2.0 トークン エンドポイントなどの値を入力し、[Microsoft Azure サービス プリンシパル](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)を追加します。 後の手順で使用する ID を指定します。

![資格情報の追加](./media/deploy-to-azure-app-service-using-azure-cli/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Java Web アプリをデプロイするための Azure App Service の作成

[az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) CLI コマンドを使用して、**Free** 価格レベルで Azure App Service プランを作成します。 App Service プランは、アプリをホストするために使用される物理リソースを定義します。 App Service プランに割り当てられたすべてのアプリケーションは、これらのリソースを共有します。これにより、複数のアプリをホストする際にコストを節約できます。 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

プランの準備が完了すると、Azure CLI で次の例のような出力が表示されます。

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Azure Web アプリを作成する

 [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) CLI コマンドを使用して、`myAppServicePlan` App Service プランで Web アプリ定義を作成します。 Web アプリ定義によって、アプリケーションにアクセスするための URL が提供され、Azure にコードをデプロイするためのいくつかのオプションが構成されます。 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

`<app_name>` プレースホルダーを独自の一意のアプリ名で置き換えます。 この一意の名前は、Web アプリの既定のドメイン名の一部です。そのため、この名前は Azure のすべてのアプリで一意である必要があります。 Web アプリをユーザーに公開する前に、カスタム ドメイン名エントリを Web アプリにマップできます。

Web アプリ定義の準備が完了すると、Azure CLI によって次の例のような情報が表示されます。 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Java を構成する

[az appservice web config update](/cli/azure/webapp/config) コマンドを使用して、アプリで必要な Java ランタイム構成を設定します。

次のコマンドでは、最新の Java 8 JDK および [Apache Tomcat](https://tomcat.apache.org/) 8.0 で動作するように Web アプリを構成します。

```azurecli
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>GitHub レポジトリを準備する

1. [Azure 用のシンプルな Java Web アプリ](https://github.com/azure-devops/javawebappsample) レポジトリを開きます。 自身の GitHub アカウントにリポジトリをフォークするには､右上隅の [**Fork**] ボタンをクリックします｡

1. GitHub Web UI で、**Jenkinsfile** ファイルを開きます。 鉛筆アイコンをクリックしてこのファイルを編集します。20 行目と 21 行目にある、リソース グループと Web アプリの名前をそれぞれ更新します。

    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<app_name>'
    ```
    
1. 23 行目を変更して Jenkins インスタンスの資格情報 ID を更新します。

    ```java
    withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
    ```
    
## <a name="create-jenkins-pipeline"></a>Jenkins パイプラインを作成する

Web ブラウザーで Jenkins を開き、**[新しい項目]** をクリックします。

1. ジョブの名前を入力します。
1. **[パイプライン]** を選択します。 
1. **[OK]** を選択します。
1. **[パイプライン]** を選択します。
1. **[定義]** で、 **[Pipeline script from SCM]\(SCM からのパイプライン スクリプト\)** を選択します。
1. **[SCM]** で、 **[Git]** を選択します。
1. フォークしたリポジトリの GitHub URL を入力します: `https:\<your forked repo\>.git`
1. **[保存]** を選びます。

## <a name="test-your-pipeline"></a>パイプラインをテストする

1. 作成したパイプラインに移動します
1. **[Build now]\(今すぐビルド\)** をクリックします。
1. ビルドが完了したら、 **[コンソール出力]** を選択して、ビルドの詳細を表示します。

## <a name="verify-your-web-app"></a>Web アプリを検証する

Web アプリに WAR ファイルが正常にデプロイされたことを確認するには、以下のようにします。 

1. Web ブラウザーを開いて以下を行います。

1. `http://&lt;app_name>.azurewebsites.net/api/calculator/ping` に移動します

1. 次のようなテキストが表示されます。

    ```output
    Welcome to Java Web App!!! This is updated!
    Today's date
    ```

1. http://&lt;app_name>.azurewebsites.net/api/calculator/add?x=&lt;x>&y=&lt;y> (&lt;x> と &lt;y> は任意の数字に置き換える) に移動して、x と y の合計を取得します

    ![Calculator: add](./media/deploy-to-azure-app-service-using-azure-cli/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a>Azure Web App on Linux へのデプロイ

Jenkins パイプライン内で Azure CLI を使用したら、スクリプトを変更して Linux 上の Azure Web アプリにデプロイします。 Linux の Web Apps では、Docker がサポートされています。 そのため、Web アプリとサービス ランタイムを Docker イメージにパッケージ化する Dockerfile を用意します。 プラグインによりイメージがビルドされて Docker レジストリにプッシュされ、Web アプリにイメージがデプロイされます。

1. [Linux で実行される Azure Web アプリの作成](/azure/app-service/containers/quickstart-nodejs)

1. [Jenkins に Docker をインストールする](https://docs.docker.com/engine/installation/linux/ubuntu/).

1. [Azure portal で Container Registry を作成する](/azure/container-registry/container-registry-get-started-azure-cli)

1. フォークした同じ [Simple Java Web App for Azure](https://github.com/azure-devops/javawebappsample) リポジトリで、**Jenkinsfile2** ファイルを次のように編集します。

    1. リソース グループ、Web アプリ、および ACR の名前に更新します (プレースホルダーを実際の値に置き換えます)。

        ```bash
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    1. `<azsrvprincipal\>` を資格情報 ID に更新します

        ```bash
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
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
