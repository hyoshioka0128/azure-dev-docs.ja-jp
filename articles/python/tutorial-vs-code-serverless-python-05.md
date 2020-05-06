---
title: 手順 5:VS Code を使用して Python で Azure Functions をデプロイする
description: チュートリアルの手順 5、Python の関数コードを Azure にデプロイし、ローカル プロジェクトと Azure の間でのログのストリーム配信や設定の同期の方法を学習する。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 425fb745cec74672cfabc6c3c5eab96821a43224
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441167"
---
# <a name="5-deploy-azure-functions-in-python"></a>5:Python で Azure Functions をデプロイする

[前の手順: ローカルでデバッグする](tutorial-vs-code-serverless-python-04.md)

この記事では、Azure Functions 拡張機能を使用し、Azure で関数アプリとその他の必要な Azure リソースを作成します。 Function App を使用すると、リソースの管理、デプロイ、および共有を容易にするためのロジック ユニットとして関数をグループ化できます。

関数アプリでは、データと[ホスティング プラン](/azure/azure-functions/functions-scale#hosting-plan-support)に Azure Storage アカウントが必要になります。 これらのリソースはすべて、1 つのリソース グループ内で整理されます。

1. **[Azure: Functions]** エクスプローラーで、**Deploy to Function App** コマンドを選択するか、コマンド パレット (**F1**) を開き、**Azure Functions: Deploy to Function App** コマンドを選択します。 繰り返しになりますが、関数アプリは Azure で Python プロジェクトが実行される場所です。

    ![Python 関数を Azure 関数アプリにデプロイする](media/tutorial-vs-code-serverless-python/deploy-a-python-fuction-to-azure-function-app.png)

1. プロンプトが表示されたら、 **[Create New Function App in Azure\(Azure で新しい関数アプリを作成する\)]** を選択し、Azure 全体で一意となる名前を指定します (通常、個人の名前か社名を他の一意の識別子と共に使用します。文字、数字、ハイフンを使用できます)。 前に Function App を作成した場合、その名前はこのオプション一覧に表示されます。

1. 拡張機能により次のアクションが実行されますが、それらは Visual Studio Code のポップアップ メッセージと **[出力]** ウィンドウで確認できます (処理には数分かかります)。

    - 与えた名前でリソース グループを作成します (ハイフンを除きます)。
    - そのリソース グループで、ストレージ アカウント、ホスティング プラン、関数アプリを作成します。 既定では、[従量課金プラン](/azure/azure-functions/functions-scale#consumption-plan)が作成されます。 専用プランで関数を実行するには、[拡張作成オプションを使用して発行を有効にする](/azure/azure-functions/functions-develop-vs-code)必要があります。
    - 関数アプリにコードをデプロイします。

    **[Azure:Functions]** エクスプローラーには進捗状況も表示されます。

    ![[Azure:Functions] エクスプローラー内の新しい関数を作成するコマンド](media/tutorial-vs-code-serverless-python/deployment-progress-indicator-in-azure-function-explorer.png)

1. デプロイが完了すると、Azure Functions 拡張機能に 3 つの追加アクションのためのボタンを含むメッセージが表示されます。

    ![デプロイの成功と追加のアクションを示すメッセージ](media/tutorial-vs-code-serverless-python/azure-functions-deployment-success-with-additional-actions.png)

    **[Stream logs]\(ログをストリーミング\)** と **[Upload settings]\(設定をアップロード\)** については、次のセクションを参照してください。 **[View output]\(出力を表示\)** については、後続の手順 5 をご覧ください。

1. デプロイ後、 **[出力]** ウィンドウには Azure のパブリック エンドポイントも表示されます (特定のエンドポイントの URL は、関数アプリに指定した名前と一致します)。

    <pre>
    HTTP Trigger Urls:

          HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    </pre>

    このエンドポイントを使用し、ローカルで実行したものと同じテストを実行します。要求本文には、URL パラメーターと JSON データを含む要求の一方または両方を使用します。 パブリック エンドポイントの結果は、前に[パート 4](tutorial-vs-code-serverless-python-04.md) でテストしたローカル エンドポイントの結果と一致するはずです。

## <a name="stream-logs"></a>ログのストリーミング

ログのストリーミングのサポートは現在開発中です。詳細は、Azure Functions 拡張機能の[問題 589](https://github.com/microsoft/vscode-azurefunctions/issues/589) にあります。 デプロイ メッセージ ポップアップの **[Stream logs]\(ログをストリーミング\)** ボタンで最終的に Azure のログ出力と Visual Studio Code がつながります。 Functions プロジェクトを右クリックし、 **[ストリーミングの開始]** または **[ストリーミングの停止]** を選択することで、**Azure Functions** エクスプローラーでログ ストリームを開始したり、停止したりできるようになります。

ただし、現在のところ、これらのコマンドは動作しません。 ログ ストリーミングは代わりに、次のコマンドを実行することでブラウザーで実行できます。`<app_name>` は、Azure の Functions アプリの名前に変更してください。

```
# Replace <app_name> with the name of your Functions app on Azure
func azure functionapp logstream <app_name> --browser
```

## <a name="sync-local-settings-to-azure"></a>Azure にローカル設定を同期する

デプロイ メッセージ ポップアップの **[Upload settings]\(設定をアップロード\)** ボタンをクリックすると、"*local.settings.json*" ファイルに行った変更が Azure に適用されます。 このコマンドは、**Azure Functions** エクスプローラーで呼び出すこともできます。これには、Functions プロジェクト ノードを展開して、 **[アプリケーション設定]** を右クリックし、 **[Upload local settings]\(ローカル設定のアップロード\)** を選択します。 コマンド パレットを使用し、 **[Azure Functions:ローカル設定のアップロード]** コマンドを選択することもできます。

設定をアップロードすると、既存の設定が更新され、"*local.settings.json*" で定義されている新しい設定が追加されます。 アップロードしても、ローカル ファイルに記載されていない設定は Azure から削除されません。 そのような設定を削除するには、**Azure Functions** エクスプローラーで **[アプリケーション設定]** ノードを展開し、設定を右クリックして、 **[設定の削除]** を選択します。 Azure portal で直接設定を編集することもできます。

ポータルまたは **Azure Explorer** で行った変更を *local.settings.json* ファイルに適用するには、 **[アプリケーション設定]** ノードを右クリックし、 **[Download remote settings]\(リモート設定のダウンロード\)** コマンドを選択します。 コマンド パレットを使用し、 **[Azure Functions:リモート設定のダウンロード]** コマンドを選択することもできます。

> [!div class="nextstepaction"]
> [関数をデプロイしました - 手順 6 に進む >>>](tutorial-vs-code-serverless-python-06.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=05-deploy)
