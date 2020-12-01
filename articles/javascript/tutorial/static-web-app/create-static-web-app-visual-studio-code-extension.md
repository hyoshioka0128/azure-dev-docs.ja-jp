---
title: 静的 Web アプリ リソースを作成する
description: 静的 Web アプリ リソースを作成します。その際、このサービスのための Visual Studio Code 拡張機能を使用します。
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: c6daa3f2a7ceb7f981cdaf57bdba61a722edbaa3
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993588"
---
# <a name="4-create-azure-static-web-app-resource"></a>4.Azure Static Web アプリ リソースを作成する

チュートリアルのこのセクションでは、静的 Web アプリ リソースを作成します。その際、このサービスのための Visual Studio Code 拡張機能を使用し、ローカル コードをリモート リポジトリにプッシュしてビルドしてから、アプリを Azure にデプロイします。

## <a name="create-a-new-branch-dedicated-to-deployment"></a>デプロイ専用の新しいブランチを作成する

Azure Static Web アプリは、GitHub リポジトリの特定のブランチからビルドを受信します。 現在、このチュートリアルでは `main` ブランチを使用しています。 Visual Studio Code の新しいターミナルで、アプリのビルドとデプロイにのみ使用される `live` ブランチを作成します。

```bash
git checkout -b live
```

## <a name="push-the-live-branch-to-github"></a>live ブランチを GitHub にプッシュする

Visual Studio Code ターミナルで、ローカル ブランチ `live` をリモート リポジトリにプッシュます。

```bash
git push origin live
```

## <a name="create-a-static-web-app-resource"></a>静的 Web アプリ リソースを作成する

1. **[Azure]** アイコンを選択し、 **[Static Web Apps]** サービスを右クリックして、 **[Create Static web app]\(静的 Web アプリを作成する\)** を選択します。 

    :::image type="content" source="../../media/static-web-app/visualstudiocode-storage-extension-create-static-web-resource.png" alt-text="Visual Studio の拡張機能を示す Visual Studio Code のスクリーンショット":::

1. 次の情報を、一度に 1 つずつ表示される後続のフィールドに入力します。 

    |フィールド名| value|
    |--|--|
    |[A name for your static web app.]\(静的 Web アプリの名前。\)|`Demo-ComputerVisionAnalyzer`|
    |[Choose branch for repository]\(リポジトリのブランチの選択\)|`live`| 
    |[Select the location of your application code.]\(アプリケーション コードの場所を選択してください。\)|`/`|
    |[Select the location of your Azure Functions code.]\(Azure Functions コードの場所を選択してください。\)|**[後で確認する]** を選択します|
    |[Enter the path of your build output relative to your app's location.]\(アプリの場所を基準とした、ビルド出力のパスを入力します。\)|`build`|
    |Select a location for new resources (新しいリソースの場所を選択する)|近くの Azure の場所を選択します。|

## <a name="update-the-github-action-with-secret-environment-variables"></a>シークレット環境変数を使用して GitHub アクションを更新する

Computer Vision のキーとエンドポイントは、リポジトリのシークレット コレクションに含まれていますが、まだ GitHub アクションにはありません。 この手順では、キーとエンドポイントをアクションに追加します。

1. Azure リソースの作成から行った変更をプルし、GitHub アクション ファイルを取得します。

    ```bash
    git pull origin live
    ```

1. Visual Studio Code エディターで、`./.github/workflows/` にある GitHub アクション ファイルを編集して、シークレットを追加します。 

    :::code language="yml" source="~/../js-e2e-client-cognitive-services/.github/workflows/sample-github-workflow.yml" highlight="34-36" :::

    
1. ローカルの `live` ブランチに変更を追加してコミットします。

    ```bash
    git add . && git commit -m "add secrets to action"
    ```

1. 変更をリモート リポジトリにプッシュし、Azure Static Web アプリに対する新しい build and deploy アクションを開始します。

    ```bash
    git push origin live
    ```

## <a name="view-the-github-action-build-process"></a>GitHub アクションのビルド プロセスを表示する

1. Web ブラウザーで、このチュートリアル用に GitHub リポジトリを開き、 **[Actions]\(アクション\)** を選択します。 

1. 一覧の一番上のビルドを選択し、左側のメニューの **[Build and Deploy Job]\(ビルドとデプロイ ジョブ\)** を選択してビルド プロセスを監視します。 **[Build And Deploy]\(ビルドとデプロイ\)** が正常に終了するまで待ちます。

    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-action-build-react-computer-vision-app.png" alt-text=" 一覧の一番上のビルドを選択し、左側のメニューの [Build and Deploy Job]\(ビルドとデプロイ ジョブ\) を選択してビルド プロセスを監視します。ビルドが正常に終了するまで待ちます。":::

## <a name="view-azure-static-web-site-in-browser"></a>ブラウザーで Azure 静的 Web サイトを表示する

1. Visual Studio Code で、右端のメニューの **[Azure]** アイコンを選択し、静的 Web アプリを選択します。次に **[Browse site]\(サイトの参照\)** を右クリックし、 **[Open]\(開く\)** を選択して、パブリックな静的 Web サイトを表示します。 

:::image type="content" source="../../media/static-web-app/visualstudiocode-browse-static-web-app.png" alt-text="[Browse site]\(サイトの参照\) を選択し、[Open]\(開く\) を選択して、パブリックな静的 Web サイトを表示します。 ":::

サイトの URL は次の場所で見つけることもできます。
* リソースの Azure portal ( **[Overview]\(概要\)** ページ)。
* GitHub アクションの build and deploy 出力には、スクリプトの最後にサイトの URL が含まれています 

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [React コードと Cognitive Services Computer Vision 分析を確認する](add-computer-vision-react-app.md)