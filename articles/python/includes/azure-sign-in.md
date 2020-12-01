---
ms.openlocfilehash: c78b25e2111e48ed9337bcd85f6585c4bdd4a081
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035517"
---
Azure 拡張機能をインストールしたら、Azure アカウントにサインインします。

1. **Azure** エクスプローラーに移動します
1. **[Azure にサインイン]** を選択して、プロンプトに従います。 (複数の Azure 拡張機能がインストールされている場合は、App Service、Functions など、自分が作業している分野のものを選択します)。

    ![VS Code から Azure にサインインする](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

1. サインインしたら、"**Azure: サインイン**" がステータス バーに表示され、サブスクリプションが **Azure** エクスプローラーに表示されていることを確認します。

    ![Visual Studio Code で Azure アカウントが表示されているステータス バー](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

    ![Visual Studio Code のサブスクリプションが表示されている Azure App Service エクスプローラー](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> **"Cannot find subscription with name <subscription ID> (<サブスクリプション ID> という名前のサブスクリプションが見つかりません)"** というエラーが表示された場合、原因としては、プロキシの内側にいるため、Azure API に到達できないことが考えられます。 ご利用のターミナルのプロキシ情報に環境変数の `HTTP_PROXY` と `HTTPS_PROXY` を構成してください。
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
