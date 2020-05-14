---
author: yevster
ms.author: yebronsh
ms.date: 4/10/2020
ms.openlocfilehash: c6f4f3b58ff7d5a8733a0f2df2ab0bf3c6eb6fe4
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988759"
---
#### <a name="read-only-static-content"></a>読み取り専用の静的コンテンツ

現在、アプリケーションで静的コンテンツを提供している場合は、そのための別の場所が必要になります。 静的コンテンツを Azure Blob Storage に移動し、グローバルな高速ダウンロードのために Azure CDN を追加することを検討できます。 詳細については、「[Azure Storage での静的 Web サイト ホスティング](/azure/storage/blobs/storage-blob-static-website)」と[「クイック スタート:Azure ストレージ アカウントと Azure CDN との統合](/azure/cdn/cdn-create-a-storage-account-with-cdn)」を参照してください。

#### <a name="dynamically-published-static-content"></a>動的に公開される静的コンテンツ

アプリケーションによってアップロードまたは生成されるが、作成後に変更できない静的コンテンツをアプリケーションで許可する場合は、前述のように Azure Blob Storage と Azure CDN を使用し、Azure Function でアップロードと CDN の更新を処理します。 「[Azure Functions を使用した静的コンテンツのアップロードと CDN の事前読み込み](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)」で、ご利用いただけるサンプルの実装を提供しています。
