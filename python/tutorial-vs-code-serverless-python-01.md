---
title: チュートリアル:VS Code を使用して Python でサーバーレスの Azure Functions を作成してデプロイする
description: チュートリアルの手順 1、概要と前提条件。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 3056ccaa6d0cc8e5f2e15c6b7511172f59644936
ms.sourcegitcommit: 44d1abfb836f90b8731d7ea5d5a5af09245b2b89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2020
ms.locfileid: "77422187"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>チュートリアル:Visual Studio Code を使用して Python でサーバーレスの Azure Functions を作成してデプロイする

この記事では、Visual Studio Code と Azure Functions 拡張機能を使用して Python でサーバーレス HTTP エンドポイントを作成し、さらに接続 ("バインド") をストレージに追加します。

Azure Functions では、サーバーレス環境でコードが実行されますが、仮想マシンをプロビジョニングしたり、Web アプリを公開したりする必要がありません。 Visual Studio Code 用の Azure Functions 拡張により、さまざまな構成問題が自動的に処理され、Functions の使用プロセスが大幅に簡素化されます。

このチュートリアルのいずれかの手順で問題が発生した場合は、詳細をお知らせください。 フィードバックを送信するには、各記事の最後にある **[問題が発生しました]** ボタンを使用します。

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Azure Functions Core Tools](#azure-functions-core-tools)。
- [Azure Functions 拡張機能を搭載した Visual Studio Code](#visual-studio-code-python-and-the-azure-functions-extension)。

### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、30 日間の無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Azure Functions Core Tools をインストールするには、「[Azure Functions Core Tools の操作](/azure/azure-functions/functions-run-local#v2)」で、ご使用のオペレーティング システム向けの指示に従います。 Chocolatey パッケージ マネージャーに関する記事のコメントは無視してください。このチュートリアルを完了するためには必要ありません。

Node.js をインストールする場合は、既定のオプションを使用し、必要なツールを自動的にインストールするオプションは選択 "*しない*" でください。  また、`npm install` コマンドで `-g` オプションを必ず使用して、Core Tools を以降のコマンドで使用できるようにしてください。

    > [!TIP]
    > The Core Tools are written in .NET Core, and the Core Tools package is best installed using the Node.js package manager, npm, which is why you need to install .NET Core and Node.js at present, even for working with Azure Functions in Python. You can, however bypass the .NET Core requirement using "extension bundles" as described in the aforementioned documentation. Whatever the case, you need install these components only once, after which Visual Studio Code automatically prompts you to install any updates.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code、Python、Azure Functions 拡張機能

次のソフトウェアをインストールします。

- Azure Functions で必要な Python 3.7 または Python 3.6。 [Python 3.7.5](https://www.python.org/downloads/release/python-375/) と [Python 3.6.8](https://www.python.org/downloads/release/python-368/) は互換性のある最新バージョンです。 これらのページを下にスクロールして、インストーラーを見つけます。 インストール時に、 **[Add Python 3.x to PATH]\(Python 3.x を PATH に追加\)** を選択し、 **[今すぐインストール]** オプションを選択して既定のオプションを使用します。 Windows では、プロセスの終了時に **[Disable Path length limit]\(パスの長さの制限を無効にする\)** も選択します。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Python 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-python.python)。[Visual Studio Code Python チュートリアルの前提条件](https://code.visualstudio.com/docs/python/python-tutorial)に説明があります。
- [Azure Functions 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。 全般的な情報については、[vscode-azurefunctions GitHub リポジトリ](https://github.com/Microsoft/vscode-azurefunctions)をご覧ください。

    > [!NOTE]
    > Azure Functions 拡張機能は、[Azure Tools 拡張機能パック](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) に含まれています。

### <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>前提条件を確認する

すべての Azure Functions ツールがインストールされていることを確認するには、Visual Studio Code コマンド パレット (**F1**) を開き、**Terminal: Create New Integrated Terminal** コマンドを選択し、ターミナルが開いたら、コマンド `func` を実行します。

![Azure Functions Core Tools の前提条件を確認する](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

Azure Functions ロゴで始まる出力は (出力を上方向にスクロールする必要があります)、Azure Functions Core Tools が存在することを示します。

`func` コマンドが認識されない場合は、`npm install -g azure-functions-core-tools` を再度実行して、インストールが成功したことを確認します。 `-g` スイッチを install コマンドと共に使用することも確認してください。そうしない場合、npm は現在のフォルダーにのみパッケージをインストールします。

`func` コマンドは、Node.js グローバル フォルダーにインストールされている *func.cmd* ファイルを介して機能します。 このフォルダーの場所を確認するには、`npm -l` を実行し、出力の最後にある場所を確認します。

> [!div class="nextstepaction"]
> [Azure にサインインしました - 手順 2 に進む >>>](tutorial-vs-code-serverless-python-02.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
