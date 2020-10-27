---
title: インクルード ファイル tutorial-azure-web-app-mongodb-05.md
description: インクルード ファイル tutorial-azure-web-app-mongodb-05.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 9b8a718ec921050237f911ee4b5c3a47b6bbb45e
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183939"
---
チュートリアルのこのセクションでは、クラウドベースのデータベースを作成し、リモート アプリを接続して、そのクラウド データベースを使用します。 

## <a name="create-a-cosmos-database"></a>Cosmos データベースを作成する

クラウドベースの MongoDB データベースをホストする Cosmos リソースを作成します。 

1. Visual Studio Code で、左端のメニューから **Azure** アイコンを選択し、 **[データベース]** セクションを選択します。 

    **[データベース]** セクションが表示されていない場合は、上部にある Azure の **[...]** メニューでセクションをオンにしていることを確認してください。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-azure-extension-select-database-section.png" alt-text="Visual Studio Code のリモート コンテナー アイコンの部分的なスクリーンショット"::: 

1. Azure エクスプローラーの **[データベース]** セクションで、右クリックでサブスクリプションを選択し、 **[サーバーの作成]** を選択します。
1. **[Create new Azure Database Server]\(新しい Azure データベース サーバーを作成する\)** コマンド パレットで、 **[Azure Cosmos DB for MongoDB API]\(MongDB 用 Azure Cosmos DB API\)** を選択します。 
1. 値がどのように使用されるかを理解するために次の表を使用しながら、プロンプトに従います。 データベースの作成には、最大 15 分かかる場合があります。

    |プロパティ|値|
    |--|--|
    |新しいリソースに対するグローバルに一意の **アカウント名** を入力します。| リソースに対して、`cosmos-mongodb-YOUR-NAME` などの値を入力します。 `YOUR-NAME` を自分の名前または一意の ID に置き換えます。 この一意の名前は、ブラウザーでリソースにアクセスするための URL の一部としても使用されます。|
    |リソース グループを選択または作成します。|リソース グループを作成する必要がある場合は、所有者、目的、およびリージョン (`westus-cosmostutorial-joesmith` など) を識別する名前付け規則を使用します。|
    |Location|リソースの場所。 このチュートリアルでは、自分の近くにあるリージョンの場所を選択します。|

    > [!CAUTION]
    > リソースの作成には、最大で 15 分かかる場合があります。     

1. エクスプローラーで、新しく作成した Cosmos リソースを表示します。 まだデータベースはありません。 
1. 引き続き Azure データベース エクスプローラーでリソース名を右クリックし、 **[接続文字列のコピー]** を選択して接続文字列をコピーします。 この情報は後で必要になります。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-databases-extenion-copy-connection-string.png" alt-text="Visual Studio Code のリモート コンテナー アイコンの部分的なスクリーンショット":::

## <a name="optional-use-new-cloud-database-in-local-environment"></a>省略可能:新しいクラウド データベースをローカル環境で使用する

新しいクラウド データベースを使用するには、新しい接続文字列を使用するようにローカル アプリケーションを変更する必要があります。 

1. Visual Studio Code で、`.env` ファイルを開き、 **DATABASE_URL** 値を新しい接続文字列に追加します。 
1. 接続文字列の末尾に `&retrywrites=false` を追加して、Web アプリを初めて実行するときにデータベースを作成できるようにします。 
1. 開発コンテナーを使用せずに Web アプリをローカルで実行して、アプリがクラウド データベースに接続されることを確認します。 

## <a name="use-new-cloud-database-in-remote-web-app"></a>新しいクラウド データベースをリモート Web アプリで使用する

データベースへの接続は、`DATABASE_URL` という名前の環境変数を使用して設定されます。 その環境変数が使用されるようにリモート Web アプリを構成するには、リモート Web アプリでその変数を作成する必要があります。 

1. Visual Studio Code の Azure App Service エクスプローラーで、Web アプリのサービス ノードを選択して展開します。
1.  **[アプリケーションの設定]** ノードを右クリックして、MongoDB 用の Azure Cosmos DB の接続文字列を含む `DATABASE_URL` プロパティを追加します。 接続文字列の末尾に `&retrywrites=false` を追加して、Web アプリを初めて実行するときにデータベースを作成できるようにします。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-remote-web-app-application-settings.png" alt-text="Visual Studio Code のリモート コンテナー アイコンの部分的なスクリーンショット"::: 

1. リモート Web サイトをブラウザーで開き、フォームを使用してデータの追加や削除を行います。 

## <a name="want-to-know-more"></a>詳細について 

### <a name="mongodb-connection-strings"></a>MongoDB の接続文字列
コードを初めて実行する際のデータベースの作成で、再試行が必要になることがあります。したがって、接続文字列には `&retrywrites=false` が設定されている必要があります。 この問題の詳細を調べるには、GitHub にあるこちらの[公開されている問題 #1296](https://github.com/microsoft/vscode-cosmosdb/issues/1296) を最初に参照してください。 