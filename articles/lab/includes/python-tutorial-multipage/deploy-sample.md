---
title: インクルード ファイル azure-sign-in.md
description: インクルード ファイル azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: c8cf3e2f478265a81742093315e2a4041b2f0ed9
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405207"
---
`az webapp up` コマンドを使用して、ローカル フォルダー ( *python-docs-hello-world* ) にコードをデプロイします。

```azurecli
az webapp up --sku F1 --name <app-name>
```

- `az` コマンドが認識されない場合は、「[初期環境を設定する](../../quickstart-python-flask-multipage.yml?tutorial-step=1)」の説明に従って Azure CLI がインストールされていることを確認してください。
- `webapp` コマンドが認識されない場合、それはご利用の Azure CLI のバージョンが 2.0.80 以上だからです。 そうでない場合は、最[新バージョンをインストール](/cli/azure/install-azure-cli)してください。
- `<app_name>` を Azure 全体で一意の名前で置き換えます (" *有効な文字は、`a-z`、`0-9`、および `-` です* ")。 会社名とアプリ識別子を組み合わせて使用すると、適切なパターンになります。
- `--sku F1` 引数を使用すると、Free 価格レベルで Web アプリが作成されます。 この引数を省略するとより高速な Premium レベルが使用されるため、時間単位のコストが発生します。
- 必要に応じて、引数 `--location <location-name>` を含めることができます。ここで、`<location_name>` は利用可能な Azure リージョンです。 [`az account list-locations`](/cli/azure/appservice#az-appservice-list-locations) コマンドを実行すると、お使いの Azure アカウントで使用可能なリージョンの一覧を取得できます。
- "Could not auto-detect the runtime stack of your app (アプリのランタイム スタックを自動検出できませんでした)" というエラーが表示された場合は、 *requirements.txt* ファイルがある *python-docs-hello-world* フォルダー (Flask) または *python-docs-hello-django フォルダー (Django)* でコマンドを実行していることを確認してください。 (GitHub の [az webapp up を使用して自動検出の問題をトラブルシューティングする方法](https://github.com/Azure/app-service-linux-docs/blob/master/AzWebAppUP/runtime_detection.md)に関するページを参照してください。)

コマンドが完了するまでに数分かかる場合があります。 実行中には、リソース グループ、App Service プラン、およびホスティング アプリの作成、ログ記録の構成、ZIP デプロイの実行に関するメッセージが表示されます。 次に、"http://&lt;app-name&gt;.azurewebsites.net でアプリを起動することができます" という内容のメッセージが表示されます。これは、Azure 上のアプリの URL です。

![az webapp up コマンドの出力例](../../media/quickstart-python/az-webapp-up-output.png)

[!include [az webapp up command note](../app-service-web-az-webapp-up-note.md)]