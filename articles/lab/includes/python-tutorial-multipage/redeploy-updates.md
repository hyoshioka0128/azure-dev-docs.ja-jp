---
title: インクルード ファイル azure-sign-in.md
description: インクルード ファイル azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: e500a760430d92fa6640d964320a3ef3f161b7cc
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405240"
---
このセクションでは、小さなコード変更を行ってから、コードを Azure に再デプロイします。 このコード変更には、次のセクションで使用するログ出力を生成するための `print` ステートメントが含まれます。

エディターで *app.py* を開き、次のコードに一致するように `hello` 関数を更新します。 

```python
def hello():
    print("Handling request to home page.")
    return "Hello, Azure!"
```

    
変更を保存してから、もう一度 `az webapp up` コマンドを使用してアプリを再デプロイします。

```azurecli
az webapp up
```

このコマンドでは、 *.azure/config* ファイルにローカルでキャッシュされている値 (アプリ名、リソース グループ、App Service プランなど) を使用します。

デプロイが完了すると、`http://<app-name>.azurewebsites.net` が開かれたブラウザー ウィンドウに戻ります。 ページを最新の情報に更新すると、変更されたメッセージが表示されます。

![更新したサンプル Python アプリを Azure で実行する](../../media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code には、Python と Azure App Service 用の強力な拡張機能が用意されています。これを使うと、App Service に Python Web アプリをデプロイするプロセスが簡単になります。 詳細については、[Visual Studio Code から App Service への Python アプリのデプロイ](/azure/python/tutorial-deploy-app-service-on-linux-01)に関する記事をご覧ください。