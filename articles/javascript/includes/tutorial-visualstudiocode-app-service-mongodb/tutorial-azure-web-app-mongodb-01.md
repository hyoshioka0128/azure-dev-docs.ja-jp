---
title: インクルード ファイル tutorial-azure-web-app-mongodb-01.md
description: インクルード ファイル tutorial-azure-web-app-mongodb-01.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 61bb61d147e01061dcf58701132feddd09c53b59
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94340999"
---
チュートリアルのこのセクションでは、Azure サブスクリプションと、このチュートリアルを使用するためのすべてのソフトウェアが必要です。

## <a name="create-or-use-existing-azure-subscription"></a>Azure サブスクリプションを作成するか、既存のものを使用する 

* アクティブなサブスクリプションが含まれる Azure アカウント。 [無料で作成できます](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)。

## <a name="install-software"></a>ソフトウェアをインストールする

- [Node.js 8.x 以降と npm](https://nodejs.org/en/download) (ローカ ル コンピューターにインストールされている Node.js パッケージ マネージャー)。
- [Docker](https://docs.docker.com/get-docker/) - Docker は、MongoDB をインストールすることなく、ローカルの MongoDB データベースを提供するために使用されます。 
    - Docker を使用してローカルの MongoDB データベースを用意する必要がある場合は、次のものも使用する必要があります。
        -  Visual Studio [開発コンテナー](https://code.visualstudio.com/docs/remote/containers)は、JavaScript 開発用の一般的なコンテナーをいくつか提供しています。 
        - [リモート コンテナー](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
    - ローカル MongoDB が既に存在していて、Docker をインストールしたくない場合でも、この手順を実行できます。 開発コンテナーを使用してローカルで実行されている MongoDB にアクセスする手順は、次の MongoDB URL が使用できる場合に限り、独自のローカル MongoDB を使用する目的のために使用できます。 
        - `mongodb://localhost:27017`
- ローカル コンピューターにインストールされている [Visual Studio Code](https://code.visualstudio.com/)。 
- Visual Studio Code 拡張機能:
    - Visual Studio Code 用の [Azure App Service 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) (Visual Studio Code 内からインストールされます)。
    - [Azure のデータベース](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)

## <a name="want-to-know-more"></a>詳細について 

オプションの Visual Studio Code 拡張機能:
* [MongoDB for VS Code](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)
* [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)