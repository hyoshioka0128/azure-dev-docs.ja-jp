---
title: Azure Cognitive Search を使用したモバイル アプリケーション開発向けの AI 搭載クラウド検索サービス
description: モバイル アプリケーション開発向けの AI 搭載クラウド検索サービスを使用するためのサービスについて説明します。
author: codemillmatt
ms.assetid: 34a8a070-0123-8982-8345-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 0857d4732d15ed389b72273156b5b404f29eb862
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632554"
---
# <a name="ai-powered-cloud-service-with-azure-cognitive-search"></a>Azure Cognitive Search を使用した AI 搭載クラウド サービス

[Azure Cognitive Search](https://azure.microsoft.com/services/search/) は、Web、モバイル、およびエンタープライズ アプリケーションのプライベートな異種コンテンツに対するリッチな検索機能を追加するための API とツールを開発者に提供する、サービスとしての検索クラウド ソリューションです。 カスタム コードを使用して、データ インジェスト (インデックス作成) を呼び出し、インデックスを作成して読み込みます。 一方で、アプリケーション コードを使用して、クエリ要求を発行し、応答を処理します。 検索エクスペリエンスは、Azure Cognitive Search の機能を使用してクライアント内で定義します。クエリは、自分で作成および所有し、サービスに保存する永続化されたインデックスに対して実行します。

## <a name="azure-cognitive-search-features"></a>Azure Cognitive Search の機能

- **自由形式のテキスト検索**:フルテキスト検索、シンプルなクエリ構文、および Lucene クエリ構文があります。
- **シンプルなスコアリング プロファイル**:ドキュメント自体の値の関数として、関連性をモデル化します。
- **地理空間検索**:物理的な場所に対する検索結果の近接度に基づいてデータを探索します。
- **ユーザー エクスペリエンス機能**:オートコンプリート、検索候補、並べ替え、およびページングがあります。
- **コグニティブ検索**:インデックス作成パイプラインに適用すると、イメージおよびテキスト分析のために生のコンテンツからテキスト情報を抽出できます。
- **ナレッジ ストア**:インデックス付けの際に作成されたエンリッチメントを保存します。
- **データ ソース**:Azure SQL Database、Azure Cosmos DB、および Azure Blob Storage に接続している場合に、プライマリ データ ストア内の検索可能なコンテンツを抽出します。
- **データのインポート ウィザード**:インデクサーを構成します。 
- **インデックス デザイナー**:インデックスを設定します。 
- **Search エクスプローラー**:クエリをテストしたり、スコアリング プロファイルを調整したりします。

Azure Cognitive Search 機能の詳細については、[こちら](/azure/search/search-what-is-azure-search#feature-descriptions)を参照してください。

## <a name="azure-cognitive-search-references"></a>Azure Cognitive Search の参考資料

- [Azure Portal](https://portal.azure.com) 
- [Azure Cognitive Search のドキュメント](/azure/search/)
- [開発者ガイド](https://azure.microsoft.com/resources/iot-developers-guide/)
- [Azure Cognitive Search の使用方法](/azure/search/search-what-is-azure-search#how-to-use-azure-cognitive-search)
- [クイック スタート](/azure/search/search-create-service-portal)

 