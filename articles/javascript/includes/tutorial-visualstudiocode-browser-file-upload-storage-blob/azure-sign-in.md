---
title: インクルード ファイル azure-sign-in.md
description: インクルード ファイル azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 085958cd12352273d5433279bae5f3edac934714
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344216"
---
Visual Studio Code で [Azure Storage 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)をインストールしたら、お使いの Azure アカウントにサインインします。それには、 **Azure** エクスプローラーに移動し、 **[Azure にサインイン]** を選択して、画面の指示に従います。 

![VS Code から Azure にサインインする](../../media/tutorial-browser-file-upload/azure-sign-in.png)

サインイン後、お使いの Azure アカウントのメール アドレス (または "サインイン済み" ) がステータス バーに表示されていることと、サブスクリプションが **Azure** エクスプローラーに表示されていることを確認します。

![Azure アカウントが表示されている、VS Code のステータス バー](../../media/tutorial-browser-file-upload/azure-account-status-bar.png)

![サブスクリプションが表示されている、VS Code の Azure エクスプローラー](../../media/tutorial-browser-file-upload/azure-subscription-view.png)

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
