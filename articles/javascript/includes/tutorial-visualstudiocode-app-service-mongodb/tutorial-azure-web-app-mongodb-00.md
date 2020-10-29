---
title: インクルード ファイル tutorial-azure-web-app-mongodb-00.md
description: インクルード ファイル tutorial-azure-web-app-mongodb-00.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 9903ca1ae761cb2e16a7f72fcc4c0942f7fa5991
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437320"
---
このチュートリアルでは、MongoDB ネイティブ API を使用して、MongoDB データベースで Node.js アプリを使用します。 Node.js アプリケーションを (Linux 上の) Azure App Service にデプロイし、クラウドベースのアプリが動作することを確認します。 

プログラミング作業は完了しており、このチュートリアルでは、Azure 拡張機能を使用して Visual Studio Code 内部からローカルおよびリモートの Azure 環境を正常に使用することに焦点を当てています。

## <a name="top-tasks"></a>上位のタスク

このチュートリアルには、JavaScript 開発者向けの **上位の Azure タスク** がいくつか含まれています。

* ローカルの MongoDB データベースを使用する
* コンテナーでアプリを使用する
* クラウドにアプリをデプロイする
* クラウドでホストされるアプリの設定を構成する 
* ローカル アプリをリモート データベースに接続する

## <a name="sample-application"></a>サンプル アプリケーション

GitHub で入手できる[サンプル Node.js アプリ](https://github.com/Azure-Samples/js-e2e-express-mongo)は、次の要素で構成されています。

* ポート 8080 でホストされている **Express.js サーバー**
* 単純な **React.js サーバー側ビュー** エンジン
* データを挿入、削除、検索する **MongoDB ネイティブ API** 関数

:::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="MongoDB データベースに接続された単純な Node.js アプリ。":::

## <a name="the-mongodb-connection"></a>MongoDB の接続

データベース接続を確立できない場合、アプリに `No database found.` というメッセージが表示されます。 これがアプリの初期状態になります。

データベース接続が確立されると、アプリは、送信ボタン付きのフォーム内の 2 つのテキスト フィールドで構成され、フォームの下に Mongo コレクションの内容が表示されます。

## <a name="limited-time-to-work-on-the-tutorial"></a>このチュートリアルに取り組む時間が限られている場合

このチュートリアルでは、Cosmos DB 用の Azure リソースを作成します。これは、Azure が MongoDB データベースをホストする方法です。 このリソース作成プロセスは、最長で 20 分かかる場合があります。 時間が限られている場合は、[そのプロセスを今すぐに開始](../../tutorial/web-app-mongodb.yml?tutorial-step=5)できます。その後、必要なときにリソースを使用できるようになります。 

## <a name="want-to-know-more"></a>詳細について 

チュートリアルの各手順には、「 **詳細について** 」セクションがあります。 これは、詳細な調査を可能にする " _オプションの情報_ " です。 チュートリアルを進めながら読むことも、後でチュートリアルに戻ることもできます。 