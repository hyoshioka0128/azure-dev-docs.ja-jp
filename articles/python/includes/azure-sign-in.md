---
ms.openlocfilehash: 90ac4162e1a6032bcb1f3cfab7f249e4538314f7
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441477"
---
Azure 拡張機能をインストールしたら、お使いの Azure アカウントにサインインします。それには、**Azure** エクスプローラーに移動し、 **[Azure にサインイン]** を選択して、画面の指示に従います。 (複数の Azure 拡張機能がインストールされている場合は、App Service、Functions など、自分が作業している分野のものを選択します)。

![VS Code から Azure にサインインする](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

サインイン後、お使いの Azure アカウントのメール アドレス (または "サインイン済み" ) がステータス バーに表示されていることと、サブスクリプションが **Azure** エクスプローラーに表示されていることを確認します。

![Visual Studio Code で Azure アカウントが表示されているステータス バー](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

![Visual Studio Code のサブスクリプションが表示されている Azure App Service エクスプローラー](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> **"Cannot find subscription with name <subscription ID> (<サブスクリプション ID> という名前のサブスクリプションが見つかりません)"** というエラーが表示された場合、原因としては、プロキシの内側にいるため、Azure API に到達できないことが考えられます。 ご利用のターミナルのプロキシ情報に環境変数の `HTTP_PROXY` と `HTTPS_PROXY` を構成してください。
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```cmd
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
