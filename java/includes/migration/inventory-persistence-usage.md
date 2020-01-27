---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 23289c7dc4b608c6fe8bb75af479ea877a2000d9
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288621"
---
### <a name="inventory-persistence-usage"></a>永続化の使用をインベントリする

アプリケーション サーバーでファイル システムを使用する場合は、再構成や、まれにアーキテクチャの変更が必要になります。 ファイル システムは、Tomcat モジュールまたはアプリケーション コードによって使用される場合があります。 次のシナリオの一部または全部を確認できます。

#### <a name="read-only-static-content"></a>読み取り専用の静的コンテンツ

現在、アプリケーションで静的コンテンツを (Apache 統合などを介して) 提供している場合は、その静的コンテンツに対して別の場所が必要になります。 静的コンテンツを Azure Blob Storage に移動し、グローバルな高速ダウンロードのために Azure CDN を追加することを検討できます。 詳細については、「[Azure Storage での静的 Web サイト ホスティング](/azure/storage/blobs/storage-blob-static-website)」および「[ストレージ アカウントの Azure CDN を有効にする](/azure/cdn/cdn-create-a-storage-account-with-cdn#enable-azure-cdn-for-the-storage-account)」を参照してください。

#### <a name="dynamically-published-static-content"></a>動的に公開される静的コンテンツ

アプリケーションによってアップロードまたは生成されるが、作成後に変更できない静的コンテンツをアプリケーションで許可する場合は、前述のように Azure Blob Storage と Azure CDN を使用し、Azure Function でアップロードと CDN の更新を処理します。 「[Azure Functions を使用した静的コンテンツのアップロードと CDN の事前読み込み](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)」で、ご利用いただけるサンプルの実装を提供しています。
