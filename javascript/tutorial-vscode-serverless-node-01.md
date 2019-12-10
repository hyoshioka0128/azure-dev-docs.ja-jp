---
title: Visual Studio Code から Node.js で Azure Functions をデプロイする
description: チュートリアル パート 1、概要と前提条件。
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: a4e60d45f5bddb358e596fc3b9e7db72e7e2f9eb
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466618"
---
# <a name="deploy-azure-functions-from-visual-studio-code"></a>Visual Studio Code から Azure Functions をデプロイする

このチュートリアルでは、Visual Studio Code と Azure Functions 拡張機能を使用して、JavaScript で記述された Azure Functions アプリケーションを作成してデプロイします。 

## <a name="prerequisites"></a>前提条件

- [Azure サブスクリプション](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure Functions 拡張機能](vscode:extension/ms-azuretools.vscode-azurefunctions)
- [Node.js と npm](https://nodejs.org/en/download)、Node.js パッケージ マネージャー。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azurefunctions">Azure Functions 拡張機能をインストールする</a>

### <a name="azure-subscription"></a>Azure サブスクリプション

Azure サブスクリプションをお持ちでない場合は、無料アカウントに[今すぐご登録](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)いただけます。Azure クレジットの 200 ドルを使用してさまざまな組み合わせのサービスをお試しください。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools のインストール

ローカル デバッグを有効にするには、[Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools) をインストールする必要があります。これは、Visual Studio Code で直接実行できます。

1. Visual Studio Code を起動します。

1. **コマンド パレット** を開き (**F1**)、「**Azure Functions: Install or Update Azure Functions Core Tools**」と入力し、**Enter** キーを押してコマンドを実行します。

1. インストールを確認するには、VS Code でメニュー コマンド **[Terminal]\(ターミナル\)**  >  **[New Terminal]\(新しいターミナル\)** の順に選択し、コマンド `func` を実行します。 このコマンドにより、次のような出力が表示されます (使用方法に関する情報も表示されます)。

    ```output
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %

    Azure Functions Core Tools (2.4.419 Commit hash: c9c1724d002bd90b2e6b41393915ea3a26bcf0ce)
    Function Runtime Version: 2.0.12332.0
    ```

> [!div class="nextstepaction"]
> [前提条件をインストールしました](tutorial-vscode-serverless-node-02.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=getting-started)
