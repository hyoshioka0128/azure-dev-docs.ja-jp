---
title: インクルード ファイル azure-sign-in.md
description: インクルード ファイル azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 6f9e5e8f30c5108996f14bef9dc4c90a668c60c1
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405297"
---
1. *python-docs-hello world* フォルダーを開いていることを確認します。 

1. 仮想環境を作成し、依存関係をインストールします。

    [!include [virtual environment setup](../app-service-quickstart-python-venv.md)]

    "[Errno 2] そのようなファイルまたはディレクトリはありません: 'requirements.txt'。" と表示された場合は、 *python-docs-hello-world* フォルダーを開いていることを確認してください。

1. 開発サーバーを実行します。

    ```terminal  
    flask run
    ```
    
    既定では、サーバーによって、アプリのエントリ モジュールが、サンプルで使用されている *app.py* 内にあると想定されています (別のモジュール名を使用する場合は、`FLASK_APP` 環境変数をその名前に設定します)。

1. Web ブラウザーを開き、`http://localhost:5000/` のサンプル アプリに移動します。 アプリに、 **Hello World!** というメッセージが表示されます。

    ![サンプル Python アプリをローカルで実行する](../../media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)
    
1. ターミナル ウィンドウで **Ctrl** + **C** キーを押して、開発サーバーを終了します。
