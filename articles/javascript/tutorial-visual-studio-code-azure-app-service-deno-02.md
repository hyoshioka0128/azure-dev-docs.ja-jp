---
title: Visual Studio Code から Deno Azure App Service を作成する
description: チュートリアル パート 2、Deno アプリを作成してローカルで実行する
ms.topic: conceptual
ms.date: 06/01/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 6440f75d11bd4eb68dfbcf4e1dcf6b2b27c0496f
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "90772925"
---
# <a name="test-local-deno-apps"></a>ローカル Deno アプリをテストする

[前の手順:概要と前提条件](tutorial-visual-studio-code-azure-app-service-deno-01.md)

この手順では、Deno の組み込み Web サーバーを利用し、単純な Deno API を作成します。 次に、アプリをローカルで実行します。

## <a name="create-and-run-a-local-deno-app"></a>ローカル Deno アプリを作成して実行する

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

    ![デモ サーバーの実行](media/deploy-azure/deno-hello-world.png)

    「`deno run --allow-net https://gist.githubusercontent.com/khaosdoctor/cd2bbb28e682feb8d20a7aba47fc1e17/raw/92de998fd11f2a24ae40bbcb84f5262cfe9389b2/deno-demo.ts`」と入力し、このコードを実行することもできます。

1. ターミナルで **Ctrl** + **C** キーを押してサーバーを停止します。

> [!div class="nextstepaction"]
> [Deno アプリを作成しました](tutorial-visual-studio-code-azure-app-service-deno-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=create-app)

## <a name="next-steps"></a>次のステップ

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [終わりました](node-howto-deploy-web-app.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
