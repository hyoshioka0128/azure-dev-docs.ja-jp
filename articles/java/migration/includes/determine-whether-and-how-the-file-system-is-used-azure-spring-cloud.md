---
author: yevster
ms.author: yebronsh
ms.date: 5/26/2020
ms.openlocfilehash: 30d39530700806dc910c085c36a7834efa9c1414
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507607"
---
#### <a name="determine-whether-and-how-the-file-system-is-used"></a>ファイル システムが使用されているかどうかとその使用方法を判断する

お使いのサービスがローカル ファイル システムに対して書き込みや読み取りを行うすべてのインスタンスを検索します。 短期または一時ファイルの書き込みと読み取りが行われる場所と、存続期間が長いファイルの書き込みと読み取りが行われる場所を特定します。

> [!NOTE]
> Azure Spring Cloud では、`/tmp` にマウントされた Azure Spring Cloud インスタンスごとに 5 GB の一時ストレージが提供されます。 一時ファイルがその制限を超えて、または別の場所に書き込まれる場合は、コードの変更が必要になります。

<!-- The following two "static content" sections should be identical to the contents of includes\static-content.md except that here we use H5 headings. -->

##### <a name="read-only-static-content"></a>読み取り専用の静的コンテンツ

現在、アプリケーションで静的コンテンツを提供している場合は、そのための別の場所が必要になります。 静的コンテンツを Azure Blob Storage に移動し、グローバルな高速ダウンロードのために Azure CDN を追加することを検討できます。 詳細については、「[Azure Storage での静的 Web サイト ホスティング](/azure/storage/blobs/storage-blob-static-website)」と[「クイック スタート:Azure ストレージ アカウントと Azure CDN との統合](/azure/cdn/cdn-create-a-storage-account-with-cdn)」を参照してください。

##### <a name="dynamically-published-static-content"></a>動的に公開される静的コンテンツ

アプリケーションによってアップロードまたは生成されるが、作成後に変更できない静的コンテンツをアプリケーションで許可する場合は、前述のように Azure Blob Storage と Azure CDN を使用し、Azure Function でアップロードと CDN の更新を処理します。 「[Azure Functions を使用した静的コンテンツのアップロードと CDN の事前読み込み](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)」で、ご利用いただけるサンプルの実装を提供しています。
