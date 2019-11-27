---
title: チュートリアル:Visual Studio Code を使用して Python でサーバーレスの Azure Functions を作成してデプロイする
description: チュートリアルの手順 1、概要と前提条件。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 0aa353d741906c887723a012bb6d4abfc719a049
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118208"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>チュートリアル:Visual Studio Code を使用して Python でサーバーレスの Azure Functions を作成してデプロイする

この記事では、Visual Studio Code と Azure Functions 拡張機能を使用して Python でサーバーレス HTTP エンドポイントを作成し、さらに接続 ("バインド") をストレージに追加します。

Azure Functions では、サーバーレス環境でコードが実行されますが、仮想マシンをプロビジョニングしたり、Web アプリを公開したりする必要がありません。 Visual Studio Code 用の Azure Functions 拡張により、さまざまな構成問題が自動的に処理され、Functions の使用プロセスが大幅に簡素化されます。

このチュートリアルのいずれかの手順で問題が発生した場合は、詳細をお知らせください。 フィードバックを送信するには、各記事の最後にある **[問題が発生しました]** ボタンを使用します。

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Azure Functions 拡張機能を搭載した Visual Studio Code](#visual-studio-code-python-and-the-azure-functions-extension)。
- [Azure Functions Core Tools](#azure-functions-core-tools)。

### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、30 日間の無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code、Python、Azure Functions 拡張機能

次のソフトウェアをインストールします。

- Azure Functions で必要な Python 3.7 または Python 3.6。 [Python 3.7.5](https://www.python.org/downloads/release/python-375/) と [Python 3.6.8](https://www.python.org/downloads/release/python-368/) は互換性のある最新バージョンです。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Python 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-python.python)。[Visual Studio Code Python チュートリアルの前提条件](https://code.visualstudio.com/docs/python/python-tutorial)に説明があります。
- [Azure Functions 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。 全般的な情報については、[vscode-azurefunctions GitHub リポジトリ](https://github.com/Microsoft/vscode-azurefunctions)をご覧ください。

> [!NOTE]
> Azure Functions 拡張機能は、[Azure Tools 拡張機能パック](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) に含まれています。

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

ご利用のオペレーティング システムに対する指示は、「[Azure Functions Core Tools のインストール](/azure/azure-functions/functions-run-local#v2)」に従います。 ツール自体は .NET Core で記述されています。Core Tools パッケージは Node.js パッケージ マネージャー (npm) を使用してインストールすることをお勧めします。そのため、Python であっても、現在のところ、.NET Core と Node.js をインストールする必要があります。 ただし、"拡張バンドル" を利用して .NET Core 要件を回避できます。説明は前述のドキュメントにあります。 いずれにせよ、以上のコンポーネントは 1 回だけインストールする必要があります。その後、更新プログラムがあれば、Visual Studio Code からインストールを自動的に求められます。

### <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>前提条件を確認する

すべての Azure Functions ツールがインストールされていることを確認するには、Visual Studio Code コマンド パレット (**F1**) を開き、**Terminal: Create New Integrated Terminal** コマンドを選択し、ターミナルが開いたら、コマンド `func` を実行します。

![Azure Functions Core Tools の前提条件を確認する](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

Azure Functions ロゴで始まる出力は (出力を上方向にスクロールする必要があります)、Azure Functions Core Tools が存在することを示します。

`func` コマンドが認識されない場合は、Azure Functions Core Tools をインストールしたフォルダーが PATH 環境変数に含まれることを確認します。

> [!div class="nextstepaction"]
> [Azure にサインインしました](tutorial-vs-code-serverless-python-02.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
