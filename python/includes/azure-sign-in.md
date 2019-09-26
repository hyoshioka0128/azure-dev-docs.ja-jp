---
ms.openlocfilehash: d64a5c908915b5d020f27e1c501aee852f04ae38
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019660"
---
Azure 拡張機能をインストールしたら、お使いの Azure アカウントにサインインします。それには、 **[Azure: App Service]** エクスプローラーに移動し、 **[Azure にサインイン]** を選択して、画面の指示に従います。

![VS Code から Azure にサインインする](../media/deploy-azure/azure-sign-in.png)

サインイン後、お使いの Azure アカウントのメール アドレスがステータス バーに表示されていることと、サブスクリプションが **[Azure: App Service]** エクスプローラーに表示されていることを確認します。

![Azure アカウントが表示されている、VS Code のステータス バー](../media/deploy-azure/azure-account-status-bar.png)

![サブスクリプションが表示されている、VS Code の Azure App Service エクスプローラー](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> **"Cannot find subscription with name <subscription ID> (<サブスクリプション ID> という名前のサブスクリプションが見つかりません)"** というエラーが表示された場合、原因としては、プロキシの内側にいるため、Azure API に到達できないことが考えられます。 ご利用のターミナルのプロキシ情報に環境変数の `HTTP_PROXY` と `HTTPS_PROXY` を構成してください。
>
> ```sh
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
>
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
