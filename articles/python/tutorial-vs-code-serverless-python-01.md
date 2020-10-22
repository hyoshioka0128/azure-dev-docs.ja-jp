---
title: チュートリアル:VS Code を使用して Python でサーバーレスの Azure Functions を作成してデプロイする
description: チュートリアル ステップ 1、Azure Functions のローカル環境を構成する
ms.topic: conceptual
ms.date: 05/19/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 69f66c51a6e55eff91a7de780ebd0bd6f5500f68
ms.sourcegitcommit: 050c898df76a1af5feffe99e392a073b8ac9c19c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2020
ms.locfileid: "92137221"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>チュートリアル:Visual Studio Code を使用して Python でサーバーレスの Azure Functions を作成してデプロイする

この記事では、Visual Studio Code と Azure Functions 拡張機能を使用して Python でサーバーレス HTTP エンドポイントを作成し、さらに接続 ("バインド") をストレージに追加します。

Azure Functions では、サーバーレス環境でコードが実行されますが、仮想マシンをプロビジョニングしたり、Web アプリを公開したりする必要がありません。 Visual Studio Code 用の Azure Functions 拡張により、さまざまな構成問題が自動的に処理され、Functions の使用プロセスが大幅に簡素化されます。

このチュートリアルのいずれかの手順で問題が発生した場合は、詳細をお知らせください。 各記事の最後にある **[このページ]** フィードバック ボタンを使用してください。

デモ ビデオについては、仮想 PyCon 2020 の <a href="https://www.youtube.com/watch?v=9bMsdBYy-D0&feature=youtu.be&ocid=AID3006292" target="_blank">VS Code での Azure Functions のビルド</a>に関する動画 (youtube.com) をご覧ください。 より長いセッションに関心があれば、<a href="https://www.youtube.com/watch?v=PV7iy6FPjAY&feature=youtu.be&t=13&ocid=AID3006292" target="_blank">Azure Functions での簡単なデータ処理</a>に関する動画 (youtube.com) もご覧ください。 

## <a name="configure-your-environment"></a>環境を構成する

- [Azure サブスクリプション](#azure-subscription)。
- [Azure Functions Core Tools](#azure-functions-core-tools)。
- [Azure Functions 拡張機能を搭載した Visual Studio Code](#visual-studio-code-python-and-the-azure-functions-extension)。

### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、30 日間の無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Azure Functions Core Tools をインストールするには、「[Azure Functions Core Tools の操作](/azure/azure-functions/functions-run-local#v2)」で、ご使用のオペレーティング システム向けの指示に従います。 Chocolatey パッケージ マネージャーに関する記事のコメントは無視してください。このチュートリアルを完了するためには必要ありません。

Node.js をインストールする場合は、既定のオプションを使用し、必要なツールを自動的にインストールするオプションは選択 "*しない*" でください。  また、`npm install` コマンドで `-g` オプションを必ず使用して、Core Tools を以降のコマンドで使用できるようにしてください。

> [!TIP]
> Core Tools は .NET Core で記述されています。Core Tools パッケージは Node.js パッケージ マネージャー (npm) を使用してインストールすることをお勧めします。そのため、Python で Azure Functions を処理する場合であっても、現在のところ、.NET Core と Node.js をインストールする必要があります。 ただし、"拡張バンドル" を利用して .NET Core 要件を回避できます。説明は前述のドキュメントにあります。 いずれにせよ、以上のコンポーネントは 1 回だけインストールする必要があります。その後、更新プログラムがあれば、Visual Studio Code からインストールを自動的に求められます。

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code、Python、Azure Functions 拡張機能

次のソフトウェアをインストールします。

- Azure Functions で必要な 64 ビット版の Python 3.6、3.7、または 3.8。 [python.org](https://www.python.org/downloads) から Python をインストールします。インストール時に、 **[Add Python 3.x to PATH]\(Python 3.x を PATH に追加\)** を選択し、 **[今すぐインストール]** オプションを選択して既定のオプションを使用します。 Windows では、プロセスの終了時に **[Disable Path length limit]\(パスの長さの制限を無効にする\)** も選択します。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Python 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-python.python)。[Visual Studio Code Python チュートリアルの前提条件](https://code.visualstudio.com/docs/python/python-tutorial)に説明があります。
- [Azure Functions 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。 全般的な情報については、[vscode-azurefunctions GitHub リポジトリ](https://github.com/Microsoft/vscode-azurefunctions)をご覧ください。

    > [!NOTE]
    > Azure Functions 拡張機能は、[Azure Tools 拡張機能パック](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) に含まれています。

### <a name="sign-in-to-azure"></a>Azure へのサインイン

Azure 拡張機能をインストールしたら、お使いの Azure アカウントにサインインします。それには、**Azure** エクスプローラーに移動し、 **[Functions]** の下の **[Azure にサインイン]** を選択して、ブラウザーの指示に従います。

![VS Code から Azure にサインインする](media/tutorial-vs-code-serverless-python/azure-sign-in.png)

サインインしたら、ステータス バーに **[Azure: サインイン]** と表示され、サブスクリプションが **Azure** エクスプローラーに表示されていることを確認します。

![Visual Studio Code で Azure アカウントが表示されているステータス バー](media/tutorial-vs-code-serverless-python/azure-account-status-bar.png)

![Visual Studio Code のサブスクリプションが表示されている Azure App Service エクスプローラー](media/tutorial-vs-code-serverless-python/azure-subscription-view.png)

[!INCLUDE [proxy-config](includes/proxy-config.md)]

### <a name="verify-your-environment"></a>環境の確認

すべての Azure Functions ツールがインストールされていることを確認するには、Visual Studio Code コマンド パレット (**F1**) を開き、**Terminal: Create New Integrated Terminal** コマンドを選択し、ターミナルが開いたら、コマンド `func` を実行します。

![Azure Functions Core Tools の前提条件を確認する](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

Azure Functions ロゴで始まる出力は (出力を上方向にスクロールする必要があります)、Azure Functions Core Tools が存在することを示します。

`func` コマンドが認識されない場合は、`npm install -g azure-functions-core-tools` を再度実行して、インストールが成功したことを確認します。 `-g` スイッチを install コマンドと共に使用することも確認してください。そうしない場合、npm は現在のフォルダーにのみパッケージをインストールします。

`func` コマンドは、Node.js グローバル フォルダーにインストールされている *func.cmd* ファイルを介して機能します。 このフォルダーの場所を確認するには、`npm -l` を実行し、出力の最後にある場所を確認します。

> [!div class="nextstepaction"]
> [Azure にサインインしました - 手順 2 に進む >>>](tutorial-vs-code-serverless-python-02.md)

[問題がある場合は、お知らせください。](https://aka.ms/python-functions-qs-ms-survey)
