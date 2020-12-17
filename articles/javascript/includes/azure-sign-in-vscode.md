---
ms.custom: devx-track-js
ms.topic: include
ms.date: 10/16/2020
ms.openlocfilehash: 624ba9739549b260018e05450598d17460f2e5d8
ms.sourcegitcommit: c1ef7aa8ed2e88e98b190e42cffde52cf301958d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97096287"
---
Azure サービスの拡張機能を既に使用している場合は、既にログイン済みなので、この手順を省略できます。 Azure サービスの拡張機能を使用していない場合は、このセクションを続行して Azure にサインインします。

Visual Studio Code に Azure サービスの拡張機能をインストールしたら、お使いの Azure アカウントにサインインします。それには、**Azure** エクスプローラーに移動し、 **[Azure にサインイン]** を選択して、画面の指示に従います。 (複数の Azure 拡張機能がインストールされている場合は、App Service、Functions など、自分が作業している分野のものを選択します)。

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
