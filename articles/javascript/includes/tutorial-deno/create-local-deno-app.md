---
title: インクルード ファイル 2
description: インクルード ファイル 2
ms.topic: include
ms.date: 06/01/2020
ms.custom: devx-track-js
ms.openlocfilehash: 05f3e5fb847e7589e41d041736c986dce0bc5b29
ms.sourcegitcommit: 5541f993c01ce356e1b0eaa8f95aea9051c3c21e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278680"
---
この手順では、Deno の組み込み Web サーバーを利用し、単純な Deno API を作成します。 次に、アプリをローカルで実行します。

1. ターミナルまたはコマンド プロンプトで、アプリ フォルダーを作成する場所に移動し、`deno-demo` という名前の新しいフォルダーを作成します。

1. `demo.ts` という名前の新しいファイルを作成します。
1. Deno では、URL から直接、実行中のコードを受け取ります。 すべての要求に "Hello World" で答える HTTP サーバーを作成します。 次のコードを使用します。

    ```typescript
    import { serve } from "https://deno.land/std@0.54.0/http/server.ts"
    const handler = serve({ port: 80 })

    console.log("Serving at 80")

    for await (const req of handler) {
     req.respond({ body: "Hello World!\n" })
    }
    ```

1. 次のスクリプトを実行し、アプリを実行します。

    ```bash
    deno run --allow-net ./demo.ts
    ```

1. ブラウザーを開き、`http://localhost:80` を参照して、アプリをテストします。 サイトは次のように表示されます。

    ![デモ サーバーの実行](../../media/deploy-azure/deno-hello-world.png)

    「`deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts`」と入力し、このコードを実行することもできます。

1. ターミナルで **Ctrl** + **C** キーを押してサーバーを停止します。
