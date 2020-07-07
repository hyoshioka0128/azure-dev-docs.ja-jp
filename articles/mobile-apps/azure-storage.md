---
title: 高度なセキュリティで保護され、耐久性に優れたスケーラブルなアプリを Azure Storage で構築するためのクラウド ストレージ
description: モバイル アプリケーションの大規模な構造化データおよび非構造化データをクラウドに格納するためのサービスについて説明します。
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 7a1171ebe64b0353fc57180adce63ee702431207
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632364"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>高度なセキュリティで保護され、耐久性に優れたスケーラブルなアプリを Azure Storage で実現するためのクラウド ストレージ

[Azure Storage](https://azure.microsoft.com/services/storage/) は、データ オブジェクトのための高度にスケーラブルなオブジェクト ストア、クラウドのためのファイル システム サービス、信頼性の高いメッセージングのためのメッセージング ストア、および NoSQL ストアを提供する、最新のアプリケーション向けの Microsoft のクラウド ストレージ ソリューションです。 Azure Storage の特徴を次に示します。

- **持続性と高可用性:** 冗長性により、一時的なハードウェア障害が発生した場合でも、データが安全に保たれます。 地域的な災害または自然災害から保護するために、データ センターまたは地理的リージョンにまたがってデータをレプリケートすることもできます。 この方法でレプリケートされたデータは、予期しない停止が発生した場合でも高可用性を維持します。
- **セキュリティ保護:** Azure Storage に書き込まれたすべてのデータは、サービスによって暗号化されます。 Azure Storage では、データにアクセスできるユーザーをきめ細かく制御できます。
- **スケーラブル:** サービスは、今日のアプリケーションのデータ ストレージとパフォーマンスに関するニーズを満たすために、高度にスケーラブルな設計となっています。
- **マネージド:** Microsoft Azure では、ユーザーに代わって、ハードウェアのメンテナンス、更新、および重大な問題が処理されます。
- **アクセス可能:** 世界中のどこからでも、HTTP または HTTPS 経由でデータにアクセスできます。 Microsoft では、.NET、Java、Node.js、Python、PHP、Ruby、Go などのさまざまな言語のクライアント ライブラリと、成熟した REST API を提供しています。 スクリプトは Azure PowerShell または Azure CLI でサポートされています。 Azure portal と Azure Storage Explorer により、データを操作するための使いやすい視覚的ソリューションが提供されます。

ご利用のモバイル アプリ内でクラウド ストレージを有効にするには、次のサービスを使用します。

## <a name="azure-blob-storage"></a>Azure BLOB ストレージ

[Azure BLOB ストレージ](https://azure.microsoft.com/services/storage/blobs/)は、クラウド用のオブジェクト ストレージ ソリューションを提供します。 BLOB ストレージは、テキストやバイナリなどの特定のデータ モデルまたは定義に準拠していない非構造化データを大量に格納するために最適化されています。 クライアント ライブラリが使用するさまざまな言語がサポートされています。 BLOB ストレージは、次の用途向けに設計されています。

- 画像またはドキュメントをブラウザーに直接配信する。
- 分散アクセス用にファイルを格納する。
- ビデオおよびオーディオをストリーミング配信する。
- ログ ファイルに書き込む。
- バックアップと復元、ディザスター リカバリー、アーカイブのためのデータを格納する。
- オンプレミスまたは Azure ホステッド サービスで分析するデータを格納する。

### <a name="azure-blob-storage-references"></a>Azure Blob Storage の参考資料

- [Azure Portal](https://portal.azure.com)
- [Azure Blob Storage のドキュメント](/azure/storage/blobs/storage-blobs-introduction)
- [クイック スタート](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [サンプル](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Azure Table Storage

[Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) は、NoSQL の構造化データをクラウド内に格納するサービスです。スキーマレスのデザインでキー/属性ストアを実現します。 Azure Table Storage は、大量の構造化データを格納します。 このサービスは、Azure クラウドの内部および外部からの認証された呼び出しを受け付ける NoSQL データストアです。 Azure のテーブルは、構造化された非リレーショナル データを格納するのに最適です。 Table Storage は通常、次の目的で使用します。

- Web スケール アプリケーションにサービスを提供できる数テラバイトの構造化データを格納する。
- 複雑な結合、外部キー、またはストアド プロシージャを必要とせず、高速アクセスのために非正規化できるデータセットを格納する。
- クラスター化インデックスを使用して高速なデータのクエリを実行する。
- OData プロトコルおよび LINQ クエリと Windows Communication Foundation (WCF) Data Services .NET ライブラリを使用してデータにアクセスする。

Table Storage を使用して、構造化された非リレーショナル データの巨大なセットを格納および照会できます。 必要に応じてテーブルが拡大縮小されます。

### <a name="azure-table-storage-references"></a>Azure Table Storage の参考資料

- [Azure Portal](https://portal.azure.com)
- [Azure Table Storage のドキュメント](/azure/storage/tables/table-storage-overview)
- [サンプル](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [クイック スタート](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-queue-storage"></a>Azure Queue Storage

[Azure Queue storage](https://azure.microsoft.com/services/storage/queues/) は、多数のメッセージを格納するためのサービスです。 メッセージには、HTTP または HTTPS を使用して、認証された呼び出しを介して世界中のどこからでもアクセスできます。 キュー メッセージの許容される最大サイズは 64 KB です。 キューには、ストレージ アカウントの総容量の上限を超えない限り、数百万のメッセージを含めることができます。 キューは通常、非同期的な処理用に作業のバックログを作成するために使用されます。

###  <a name="azure-queue-storage-references"></a>Azure Queue storage の参考資料

- [Azure Portal](https://portal.azure.com)
- [Azure Queue storage のドキュメント](/azure/storage/queues/)
- [クイック スタート](/azure/storage/queues/storage-quickstart-queues-portal)
- [サンプル](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
