---
title: インクルード ファイル 1
description: インクルード ファイル 1
ms.topic: include
ms.date: 06/01/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 8f0bda8929699f61fd3c79f9ae6fb38762e622c9
ms.sourcegitcommit: 5541f993c01ce356e1b0eaa8f95aea9051c3c21e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278664"
---
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

[!INCLUDE [interactive-login](../../../azure-cli/includes/interactive-login.md)]

ログインすると、ご使用のアカウントに関連付けられているサブスクリプションの一覧が表示されます。 `isDefault: true` が示されているサブスクリプション情報が、ログイン後に、現在アクティブになっているサブスクリプションです。 別のサブスクリプションを選択するには、切り替え先のサブスクリプション ID を指定して [az account set](/cli/azure/account#az-account-set) コマンドを実行します。 サブスクリプションの選択の詳細については、「[複数の Azure サブスクリプションの使用](/cli/azure/manage-azure-subscriptions-azure-cli)」を参照してください。
