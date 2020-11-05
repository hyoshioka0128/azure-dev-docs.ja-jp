---
title: Azure CLI から Azure App Service に Deno アプリをデプロイする
description: Deno チュートリアル パート 1、概要と前提条件。
ms.topic: tutorial
ms.date: 06/01/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: d9adff6e04699a26b14fb2fb1cdbe9dafe923eca
ms.sourcegitcommit: e1175aa94709b14b283645986a34a385999fb3f7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93233413"
---
# <a name="deploy-deno-to-azure-app-service-using-visual-studio-code"></a>Visual Studio Code を使用して Deno を Azure App Service にデプロイする

このチュートリアルでは、Azure CLI を使用して、Deno アプリケーションを Azure App Service (Linux または Windows 上) にデプロイします。

## <a name="prerequisites"></a>前提条件

- アクティブなサブスクリプションが含まれる Azure アカウント。 [無料で作成できます](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-deno&mktingSource=vscode-tutorial-appservice-deno)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Deno](https://deno.land/#installation)
- Azure CLI をインストールし、ログインしておく

## <a name="install-or-run-in-azure-cloud-shell"></a>Azure Cloud Shell でのインストールまたは実行

Azure CLI で作業を開始するには、お使いのブラウザーから Azure Cloud Shell 環境で実行するのが最も簡単です。 Cloud Shell については、「[Azure Cloud Shell の Bash のクイックスタート](/azure/cloud-shell/quickstart)」を参照してください。

CLI をインストールする準備ができたら、[インストール手順](/cli/azure/install-azure-cli)に関するページをご覧ください。

初めて CLI をインストールしたら、`az --version` を実行して、CLI の正しいバージョンがインストールされていること確認します。

> [!NOTE]
> Azure クラシック デプロイ モデルを使用している場合は、[Azure クラシック CLI をインストール](/cli/azure/install-classic-cli)してください。

## <a name="sign-in"></a>サインイン

ローカル インストールで CLI コマンドを使用する前に、[az login](/cli/azure/reference-index#az-login) でサインインする必要があります。

[!INCLUDE [interactive-login](../azure-cli/includes/interactive-login.md)]

ログインすると、ご使用のアカウントに関連付けられているサブスクリプションの一覧が表示されます。 `isDefault: true` が示されているサブスクリプション情報が、ログイン後に、現在アクティブになっているサブスクリプションです。 別のサブスクリプションを選択するには、切り替え先のサブスクリプション ID を指定して [az account set](/cli/azure/account#az-account-set) コマンドを実行します。 サブスクリプションの選択の詳細については、「[複数の Azure サブスクリプションの使用](/cli/azure/manage-azure-subscriptions-azure-cli)」を参照してください。

> [!div class="nextstepaction"]
> [Azure CLI をインストールし、ログインしました](tutorial-visual-studio-code-azure-app-service-deno-02.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=getting-started)

## <a name="next-steps"></a>次のステップ

[!INCLUDE [tutorial-next-steps](includes/tutorial-next-steps.md)]

> [!div class="nextstepaction"]
> [終わりました](node-howto-deploy-web-app.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=deno-deployment-azureappservice&step=clean-up-resources)
