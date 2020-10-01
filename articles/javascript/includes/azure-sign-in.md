---
ms.custom: devx-track-js
ms.openlocfilehash: 659f538569d2e2155c0eccd3c08684ee835e7449
ms.sourcegitcommit: 4af22924a0eaf01e6902631c0714045c02557de4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91208781"
---
VS Code で Azure 拡張機能をインストールしたら、お使いの Azure アカウントにサインインします。それには、**Azure** エクスプローラーに移動し、 **[Azure にサインイン]** を選択して、画面の指示に従います。 (複数の Azure 拡張機能がインストールされている場合は、App Service、Functions など、自分が作業している分野のものを選択します)。

![VS Code から Azure にサインインする](../media/deploy-azure/azure-sign-in.png)

サインイン後、お使いの Azure アカウントのメール アドレス (または "サインイン済み" ) がステータス バーに表示されていることと、サブスクリプションが **Azure** エクスプローラーに表示されていることを確認します。

![Azure アカウントが表示されている、VS Code のステータス バー](../media/deploy-azure/azure-account-status-bar.png)

![サブスクリプションが表示されている、VS Code の Azure エクスプローラー](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> **"Cannot find subscription with name <subscription ID> (<サブスクリプション ID> という名前のサブスクリプションが見つかりません)"** というエラーが表示された場合、原因としては、プロキシの内側にいるため、Azure API に到達できないことが考えられます。 ご利用のターミナルのプロキシ情報に環境変数の `HTTP_PROXY` と `HTTPS_PROXY` を構成してください。
>
> # <a name="bash"></a>[bash](#tab/bash)
>
> ```bash
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> # <a name="powershell"></a>[PowerShell](#tab/powershell)
>
> ```powershell
> $env: HTTPS_PROXY = "https://username:password@proxy:8080"
> $env: HTTP_PROXY = "http://username:password@proxy:8080"
> ```
>
> # <a name="cmd"></a>[Cmd](#tab/cmd)
>
> ```cmd
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ---
