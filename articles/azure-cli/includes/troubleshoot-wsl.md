---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 11/26/2018
ms.topic: include
ms.openlocfilehash: ee0b2b0c8c557aa54255f28429bb3a174c0e21a9
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030894"
---
### <a name="cli-fails-to-install-or-run-on-windows-subsystem-for-linux"></a>Windows Subsystem for Linux で CLI をインストールまたは実行できない

[Windows Subsystem for Linux (WSL)](/windows/wsl/about) は Windows プラットフォームを基盤とするシステム呼び出し変換レイヤーであるため、Azure CLI をインストールまたは実行しようとするとエラーが発生する場合があります。 CLI は、WSL にバグが含まれる可能性のあるいくつかの機能に依存しています。 どのように CLI をインストールしてもエラーが発生する場合は、CLI のインストール プロセスではなく WSL に問題があると考えられます。

WSL インストールのトラブルシューティングを行い、問題を解決するには、以下を行ってください。

* 可能であれば、同じインストール プロセスを Linux マシンまたは VM で実行し、正常にインストールできるかどうかを確認します。 インストールできる場合、問題はほぼ間違いなく WSL に関連するものです。 Azure で Linux VM を開始するには、[Azure portal での Linux 仮想マシンの作成](/azure/virtual-machines/linux/quick-create-portal)に関するページを参照してください。
* 最新バージョンの WSL を実行していることを確認します。 最新バージョンを入手するには、[Windows 10 インストールを更新](https://support.microsoft.com/help/4027667/windows-10-update)してください。
* 現在の問題に対処している、WSL 関連の[未解決案件](https://github.com/Microsoft/WSL/issues)がないか確認します。
  この問題を回避する方法についての提案や、問題が修正される予定のリリースに関する情報が掲載されることがよくあります。
* 現在の問題に関する案件がまだない場合は、[WSL に関する新規案件を提出](https://github.com/Microsoft/WSL/issues/new)します。その際は、必ず、可能な限り多くの情報を提供してください。

引き続き WSL でのインストールまたは実行の問題が発生する場合は、[Windows 用の CLI のインストール](../install-azure-cli-windows.md)を検討してください。
