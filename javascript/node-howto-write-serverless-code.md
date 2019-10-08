---
title: Azure Functions を使用してサーバーレス Node.js コードを作成する
description: Azure Functions を使用してサーバーレス コードを作成、デプロイする方法についてのガイダンス。
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/19/2019
ms.author: kraigb
ms.custom: seo-javascript-september2019
ms.openlocfilehash: f1cea28186282cc3f72993439f0c3a2ab43885a9
ms.sourcegitcommit: 945e92dae2fa4521eebdc049c65273ae6b5470ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813682"
---
# <a name="use-azure-functions-to-write-serverless-nodejs-code-on-azure"></a>Azure Functions を使用して Azure でサーバーレス Node.js コードを作成する

サーバーレス コードを使用すると、インフラストラクチャのプロビジョニングや管理について心配することなく、応答性に優れたエンドポイントをオンデマンドでインターネット上に作成できます。 サーバーレス コードは、さまざまなイベントに反応して作動する、スクリプトなどの小さなコードから成ります。 Azure におけるサーバーレスのプランは、Azure Functions と呼ばれます。

まずは以下をご覧ください。

- [Visual Studio Code を使用した初めての関数の作成](/azure/azure-functions/functions-create-first-function-vs-code)。 この記事では、Azure Functions が Visual Studio Code のコンテキストで紹介されており、多くの細かい点がわかりやすくなっています。

次に、以下の記事を読んで、Azure Functions でできることについての知識を広げましょう。

- [Azure Functions の概要](/azure/azure-functions/functions-overview)。サーバーレス開発のメリット、コスト、サーバーレス コードを実行するために使用できるさまざまなトリガーについて説明しています。

- [Azure Functions でのトリガーとバインドの概念](/azure/azure-functions/functions-triggers-bindings)

- [Azure Functions の開発者向けガイド](/azure/azure-functions/functions-reference)と [Azure Functions の JavaScript 開発者向けガイド](/azure/azure-functions/functions-reference-node)

- サーバーレス環境における "*ステートフル*" な関数の作成に関心がある方は、「[Durable Functions とは](/azure/azure-functions/durable/durable-functions-overview)」と「[JavaScript で最初の Durable Functions を作成する](/azure/azure-functions/durable/quickstart-js-vscode)」を参照してください。

他にも、サーバーレス コードについての知識を深めるのに役立つリソースが数多く用意されています。

- Microsoft Learn モジュール: [Azure Functions と SignalR Service を使って、Web アプリケーションの自動更新を有効にする](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)

- さまざまなトリガーを使用してサーバーレス コードを実行する方法について:

  - [タイマーでコードを実行する](/azure/azure-functions/functions-create-scheduled-function)
  - [Azure BLOB ストレージにファイルがアップロードされたりファイルが更新されたときにコードを実行する](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
  - [Azure Queue storage にメッセージが書き込まれたときにコードを実行する](/azure/azure-functions/functions-create-storage-queue-triggered-function)

- [Azure Functions と Azure Cosmos DB を使用して非構造化データを格納する](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md?tabs=javascript)。 その他のデータベースについては、[Azure のデータベースを Node.js コードに統合する方法](node-howto-integrate-databases.md)に関するページを参照してください。

- [Azure Functions をローカルでコーディングしてテストする](/azure/azure-functions/functions-develop-local)

- [Azure Functions のコードをテストするための戦略](/azure/azure-functions/functions-test-a-function)と[エラー処理](/azure/azure-functions/functions-bindings-error-pages)

- [Azure Active Directory による認証の構成](/azure/app-service/configure-authentication-provider-aad.md?toc=%2fazure%2fazure-functions%2ftoc.json)

- [Azure Pipelines を使用した継続的デプロイの設定](/azure/azure-functions/functions-how-to-azure-devops)

- [Node.js と Azure Functions のサンプル](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)
