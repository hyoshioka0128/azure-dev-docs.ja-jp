---
title: Visual Studio Code から Node.js アプリのコンテナー イメージを作成する
description: チュートリアル パート 3、Node.js アプリケーション イメージを作成する
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 47e9440852f8a3443147e3d03b5c833d8da0c36b
ms.sourcegitcommit: a65fa8dbb168bd39e225a293d9ee73d18ece1864
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/27/2020
ms.locfileid: "80362787"
---
# <a name="create-your-nodejs-application-image"></a>Node.js アプリケーション イメージを作成する

[前の手順:コンテナー レジストリを使用する](tutorial-vscode-docker-node-02.md)

この手順では、Visual Studio Code の Docker 拡張機能を使用して、アプリのイメージを作成するために必要なファイルを追加し、イメージをビルドして、レジストリにプッシュします。

このチュートリアル用のアプリがまだない場合は、[Visual Studio Code Node.js チュートリアル](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial#_an-express-application)のアプリを使用してください。

## <a name="add-docker-files"></a>Docker ファイルを追加する

1. Visual Studio Code で**コマンド パレット**を開き (**F1**)、「`add docker files to workspace`」と入力してから、**Docker: Add Docker files to workspace** コマンドを選択します。

1. メッセージが表示されたら、アプリケーションの種類として **Node.js** を選択し、Docker Compose ファイルについては **[いいえ]** と答えて、アプリケーションがリッスンするポートを選択します。

1. このコマンドで、`Dockerfile` と、Docker Compose のいくつかの構成ファイル、および `.dockerignore` が作成されます。

    > [!TIP]
    > VS Code には、Docker ファイルのサポートが豊富に用意されています。 スマートな提案、入力候補、エラー検出などの豊富な言語機能については、VS Code ドキュメントの「[Docker の使用](https://code.visualstudio.com/docs/azure/docker)」を参照してください。

## <a name="build-a-docker-image"></a>Docker イメージをビルドする

`Dockerfile` には、ソース ファイルの場所を含むアプリの環境と、コンテナー内でアプリを起動するコマンドを記述します。

> [!TIP]
> コンテナーとイメージの比較:コンテナーは、イメージのインスタンスです。

1. **コマンド パレット**を開き (**F1**)、**Docker Images: Build Image** を実行してイメージをビルドします。 VS Code は、現在のフォルダーにある Dockerfile を使用して、現在のフォルダーと同じ名前をイメージに付けます。

1. 完了すると、Visual Studio Code の **[Terminal]\(ターミナル\)** パネルが開いて、`docker build` コマンドが実行されます。 出力には、アプリ環境を構成する各ステップ (レイヤー) も表示されます。

1. ビルドされると、イメージは、**DOCKER** エクスプローラーで **[Images]\(イメージ\)** の下に表示されます。

    ![Visual Studio Code 内の Docker イメージの一覧](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>イメージをレジストリにプッシュする

1. レジストリにイメージをプッシュするには、最初にレジストリ名を使用してそれにタグを付ける必要があります。 **DOCKER** エクスプローラーで、 **[latest]\(最新\)** イメージを右クリックします。

    ![Visual Studio Code のイメージ タグ付けコマンド](media/deploy-containers/tag-command.png)

1. 後続のプロンプトで、タグを入力し、**Enter** キーを押します。

    慣例により、タグ付けでは次の形式を使用します。

    `[registry or username]/[image name]:[tag]`

    Azure Container Registry を使用している場合、イメージ名は次のようになります。

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    Docker Hub を使用している場合は、Docker Hub のユーザー名を使用します。 次に例を示します。

    `fiveisprime/myexpressapp:latest`

1. これで、新しくタグ付けされたイメージが、 **[Images]\(イメージ\)** の下のレジストリ名を含むノードに表示されます。 そのノードを展開し、 **[latest]\(最新\)** を右クリックして、 **[Push]\(プッシュ\)** を選択します。

    ![Visual Studio Code のイメージのプッシュ コマンド](media/deploy-containers/push-command.png)

1. **[Terminal]\(ターミナル\)** パネルに、この操作に使用される `docker push` コマンドが表示されます。 ターゲット レジストリは、イメージ名に指定されたレジストリによって決まります。

1. 完了したら、Docker 拡張機能エクスプローラーの **[Registries]\(レジストリ\)** ノードを展開して、レジストリ内のイメージを確認します。

    ![プッシュされたイメージが Azure Container Registry に表示されている](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [アプリケーションのイメージを作成しました](tutorial-vscode-docker-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
