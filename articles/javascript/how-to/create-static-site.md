---
title: Node.js、API、マークアップを使用して Azure 上で静的サイトを構築する
description: Azure を使用して JAMstack アプリ (JavaScript、API、Markup) を作成する方法
ms.topic: how-to
ms.date: 08/20/2019
ms.custom: seo-javascript-september2019, devx-track-js
ms.openlocfilehash: 9b00c9b27767109a16526f70dfcb6bfb592fa4a4
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98561108"
---
# <a name="build-jamstack-static-site-web-apps-on-azure-with-nodejs"></a>Node.js を使用して Azure で JAMstack (静的サイト) Web アプリを作成する

優れた Web アプリは、*JavaScript* フロント エンドと *API* (サーバーレス コードとして作成されたサードパーティまたはカスタムの API)、そして、静的ページとして返されるテンプレート化された "*マークアップ*" (HTML と CSS) とを組み合わせて生産的に作成、保守することができます。 JAMstack としても知られるこの組み合わせを利用すれば、Web ページを返す複雑なバック エンド コードを書かずに済みます。 システムから返されるのは静的ページ (HTML、CSS、JavaScript) のみであり、サーバー側の処理用の API は、それらのページから要求されます。 それらの API は、自動でスケーリングするサーバーレス技術を使って記述できるため、標準的な常時稼動のサーバーや Web ホストを使用するのに伴うコストやセキュリティ上の懸念は一切ありません  (詳細については、[jamstack.org](https://jamstack.org/) を参照)。

Azure で静的 (JAMstack) サイトを実装するには、さまざまなツールとサービスを使用します。

- 必要に応じてデータベースを構成します。
- サーバーレス API コードを Azure Functions で実装します。 これらの API では通常、データベースが使用されます。
- フロントエンド開発に使用したいライブラリを選択します (Angular など)。 次に、それらの静的な HTML、CSS、JavaScript ファイルを Azure Blob Storage にアップロードすれば、それがビルトインの Web サーバーとなります。
- すべてのトラフィックが 1 つの URL ドメインを経由するようにリバース プロキシを作成します。

//build 2019 セッション「[JavaScript、Visual Studio Code、Azure を使用した生産的なフロントエンド開発](https://azure.microsoft.com/resources/videos/build-2019-productive-front-end-development-with-javascript-visual-studio-code-and-azure/)」で、このプロセスのデモンストレーションを視聴できます。

> [!VIDEO https://medius.studios.ms/Embed/Video-nc/B19-BRK3021?latestplayer=true]

ステップ バイ ステップのチュートリアルは、[静的な Web サイトの Azure へのデプロイ](../tutorial/tutorial-vscode-static-website-node/tutorial-vscode-static-website-node-01.md)に関するページでご覧いただけます。

詳細については、次の記事も参照してください。

- **データベース**: 「[Azure のデータベースを Node.js アプリに統合する方法](integrate-database.md)」で説明されている Azure の各種データベース サービスなど、任意のデータベースを使用できます。
  
- **サーバーレス API**:

  - 「[Visual Studio Code から Azure Functions をデプロイする](../tutorial/tutorial-vscode-serverless-node-install.md)」からお始めください。Azure Functions が Visual Studio Code のコンテキストで紹介されており、多くの細かい点がわかりやすくなっています。
  - 記事を完了すると、関数にちなんだ (その HTTP エンドポイントと同じ) 名前のサブフォルダーが格納された Azure Functions プロジェクト (フォルダー) が得られます。 その関数フォルダーに、コードを含んだ *index.js* ファイルが格納されています。
  - 必要に応じてその関数を変更したり、他の関数をプロジェクトに追加したりしたうえで、それらを再び Azure にデプロイしてパブリックに公開することができます。
  - サーバーレス開発に関するその他のリソースについては、「[Azure でサーバーレス Node.js コードを作成する方法](develop-serverless-apps.md)」を参照してください。

- **Azure Storage へのフロントエンドのデプロイ**: 自分の API が完成したら、任意のフレームワークを使用して、それらの API を使用するためのフロントエンド コードを書くことができます。 準備ができたら、記事「[チュートリアル:Blob Storage で静的な Web サイトをホストする](/azure/storage/blobs/storage-blob-static-website-host)」に従って、それらのファイルを Azure にアップロードし、静的な Web サイトのホスティングを有効にします。

- **リバース プロキシの作成**: リバース プロキシ (「[Azure Functions プロキシの操作](/azure/azure-functions/functions-proxies)」を参照) を使用すると、特定の要求をさまざまな URL に簡単に誘導できます。 このケースでは、自分のフロントエンド ファイルの要求を、それらのファイルがデプロイされている Azure Storage の URL に、また、API 要求を Azure Functions の URL に誘導する必要があります。

  - それらのプロキシを作成するには、自分の Functions プロジェクトの *proxies.json* ファイルを次のように編集します。`<storage_url>` と `<api_url>` は、実際の URL に置き換えてください。
  
    ```json
    {
      "$schema": "http://json.schemastore.org/proxies",
      "proxies": {
        "Static frontend on Azure Storage": {
          "matchCondition": {
            "route": "/{*restOfPath}"
          },
          "backendUri": "<storage_url>/{restOfPath}"
        },
        "Azure Functions API": {
          "matchCondition": {
            "route": "/api/{*restOfPath}"
          },
          "backendUri": "<api_url>/api/{restOfPath}"
        }
      }
    }
    ```