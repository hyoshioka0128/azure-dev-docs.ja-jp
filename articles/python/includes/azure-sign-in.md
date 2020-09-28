---
ms.openlocfilehash: 1e6e3f46fc7dd2f4ddf708be65941deb9ca18096
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2020
ms.locfileid: "90772612"
---
Azure 拡張機能をインストールしたら、お使いの Azure アカウントにサインインします。それには、**Azure** エクスプローラーに移動し、 **[Azure にサインイン]** を選択して、画面の指示に従います。 (複数の Azure 拡張機能がインストールされている場合は、App Service、Functions など、自分が作業している分野のものを選択します)。

![VS Code から Azure にサインインする](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

サインインしたら、"**Azure: サインイン**" がステータス バーに表示され、サブスクリプションが **Azure** エクスプローラーに表示されていることを確認します。

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
