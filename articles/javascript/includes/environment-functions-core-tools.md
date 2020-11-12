---
title: インクルード ファイル completed.md
description: インクルード ファイル completed.md
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 09f69293062f52dfc2190cfeb040ad45ea428c67
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "94338515"
---
## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools のインストール

ローカル デバッグを有効にするには、[Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools) をインストールする必要があります。これは、Visual Studio Code で直接実行できます。

1. Visual Studio Code を起動します。

1. **コマンド パレット** を開き ( **F1** )、「 **Azure Functions: Install or Update Azure Functions Core Tools** 」と入力し、 **Enter** キーを押してコマンドを実行します。

1. インストールを確認するには、VS Code でメニュー コマンド **[Terminal]\(ターミナル\)**  >  **[New Terminal]\(新しいターミナル\)** の順に選択し、コマンド `func` を実行します。 このコマンドにより、次のような出力が表示されます (使用方法に関する情報も表示されます)。

    <pre>
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
    </pre>