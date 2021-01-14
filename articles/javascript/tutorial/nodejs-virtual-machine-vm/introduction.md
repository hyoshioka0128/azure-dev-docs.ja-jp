---
title: Azure CLI Linux 仮想マシン
description: GitHub リポジトリの Express.js ベースのアプリのクローンを使用して、Azure Linux 仮想マシンを作成します。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: c68a8442862eb03291f80609bb9767fb1c9b1b6b
ms.sourcegitcommit: ed31f841fa9680335658df6708f107e170d47ff0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2021
ms.locfileid: "97974549"
---
# <a name="1-create-linux-virtual-machine-with-expressjs-app-using-azure-cli"></a>1.Azure CLI を使用して、Express.js アプリ用の Linux 仮想マシンを作成する

このチュートリアルでは、Express.js アプリ用の Linux 仮想マシン (VM) を作成します。 この VM は、cloud-init 構成ファイルで構成され、NGINX と Express.js アプリの GitHub リポジトリが含まれています。 VM が実行されたら、SSH を使用して VM に接続し、トレースのログ記録を含めるように Web アプリを変更して、公開された Express.js サーバー アプリを Web ブラウザーで表示できます。

* [**サンプル コード**](https://github.com/Azure-Samples/js-e2e-vm)

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

- SSH を使用した VM への接続:bash シェル (SSH が含まれています) などの最新のターミナルを使用します。
[!INCLUDE [Azure CLI](../../../includes/azure-cli-prepare-your-environment-no-header.md)]


## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure リソースのリソース グループを作成する](create-azure-monitoring-application-insights-web-resource.md) 