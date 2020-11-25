---
title: Azure CLI Linux 仮想マシン
description: GitHub リポジトリの Express.js ベースのアプリのクローンを使用して、Azure Linux 仮想マシンを作成します。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 835a452e87fed88ac83085c882a5a40461bd2dd0
ms.sourcegitcommit: 6514a061ba5b8003ce29d67c81a9f0795c3e3e09
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94623178"
---
# <a name="1-create-linux-virtual-machine-with-expressjs-app-using-azure-cli"></a>1.Azure CLI を使用して、Express.js アプリ用の Linux 仮想マシンを作成する

このチュートリアルでは、Express.js アプリ用の Linux 仮想マシン (VM) を作成します。 この VM は、cloud-init 構成ファイルで構成され、NGINX と Express.js アプリの GitHub リポジトリが含まれています。 VM が実行されたら、SSH を使用して VM に接続し、トレースのログ記録を含めるように Web アプリを変更して、公開された Express.js サーバー アプリを Web ブラウザーで表示できます。

このチュートリアルに含まれるタスクは次のとおりです。

* Azure CLI を使用して Azure にサインインする
* Azure CLI を使用して Azure Linux VM リソースを作成する
    * パブリック ポート 80 を開く
    * GitHub リポジトリから Express.js Web アプリのデモをインストールする
    * Web アプリの依存関係をインストールする
    * Web アプリを開始する
* Azure CLI を使用して Azure 監視リソースを作成する
    * SSH を使用して VM に接続する
    * npm を使用して Azure SDK クライアント ライブラリをインストールする
    * Application Insights クライアント ライブラリ コードを追加して、カスタム トレースを作成する
* ブラウザーから Web アプリを表示する
    * `/trace` ルートを要求して、Application Insights ログにカスタム トレースを生成する
    * Azure CLI を使用して、ログに収集されたトレースの数を表示する
    * Azure portal でトレースの一覧を表示する
* Azure CLI を使用してリソースを削除する

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="prerequisites"></a>前提条件

* Azure CLI コマンドを実行するには、[Azure CLI](/cli/azure/install-azure-cli) をインストールします
* SSH を使用した VM への接続:bash シェル (SSH が含まれています) などの最新のターミナルを使用します。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure リソースのリソース グループを作成する](create-azure-monitoring-application-insights-web-resource.md) 