---
title: Windows での Azure CLI のインストール
description: Windows で Azure CLI をインストールする方法
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 05/01/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c5e9118a04b0dc608309093866307fdc7083f591
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030754"
---
# <a name="install-azure-cli-on-windows"></a>Windows での Azure CLI のインストール

Windows では、MSI を使用して Azure CLI がインストールされるので、Windows コマンド プロンプト (CMD) または PowerShell から CLI にアクセスできます。
Windows Subsystem for Linux (WSL) 用にインストールする場合は、お使いの Linux ディストリビューションで使用できるパッケージがあります。 サポートされているパッケージ マネージャーの一覧または WSL での手動インストール方法については、[メインのインストール ページ](install-azure-cli.md)を参照してください。

[!INCLUDE [current-version](includes/current-version.md)]

## <a name="install-or-update"></a>インストールまたは更新

再頒布可能 MSI は、Windows での Azure CLI のインストールまたは更新に使用されます。 MSI インストーラーの使用前に現在のバージョンをアンインストールする必要はありません。

> [!div class="nextstepaction"]
> [MSI インストーラーのダウンロード](https://aka.ms/installazurecliwindows)

インストーラーによって、コンピューターに変更を加えるかどうかを尋ねるメッセージが表示されたら、[はい] をクリックします。

Azure CLI は PowerShell を使用してインストールすることもできます。 管理者として PowerShell を開始し、次のコマンドを実行します。

   ```PowerShell
   Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
   ```
これにより、最新バージョンの Windows 用 Azure CLI がダウンロードされインストールされます。 既にインストールされているバージョンがある場合、既存のバージョンが更新されます。 インストールが完了したら、Azure CLI を使用するには PowerShell を再度開く必要があります。

これで、Windows コマンド プロンプトまたは PowerShell のいずれかから、`az` コマンドで Azure CLI を実行できるようになりました。 PowerShell では、Windows コマンド プロンプトでは利用できないタブ補完機能が提供されます。 サインインするには、[az login](/cli/azure/reference-index#az-login) コマンドを実行します。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

さまざまな認証方法の詳細については、「[Azure CLI を使用してサインインする](authenticate-azure-cli.md)」を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

ここでは、Windows でのインストール時に発生する一般的な問題をいくつか示します。 ここで取り上げていない問題が発生した場合は、[GitHub で問題を報告](https://github.com/Azure/azure-cli/issues)してください。

### <a name="proxy-blocks-connection"></a>プロキシによる接続のブロック

プロキシにより接続がブロックされているため MSI インストーラーをダウンロードできない場合、プロキシを正しく構成していることを確認します。 Windows 10 の場合、これらの設定は `Settings > Network & Internet > Proxy` ウィンドウで管理されます。 必要な設定またはお使いのマシンの構成が管理されている状況や高度なセットアップが必要な状況については、システム管理者にお問い合わせください。

> [!IMPORTANT]
> また、これらの設定は、CLI を使用した Azure サービスに PowerShell またはコマンド プロンプトの両方からアクセスできるようにするためにも必要です。 これを行うには、PowerShell で次のコマンドを使用します。
>
> ```powershell
> (New-Object System.Net.WebClient).Proxy.Credentials = `
>   [System.Net.CredentialCache]::DefaultNetworkCredentials
> ```

MSI を取得するには、プロキシで次のアドレスへの HTTPS 接続を許可する必要があります。

* `https://aka.ms/`
* `https://azurecliprod.blob.core.windows.net/`

## <a name="uninstall"></a>アンインストール

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

Windows の "アプリと機能" の一覧から Azure CLI をアンインストールします。 アンインストールするには、次のようにします。

| プラットフォーム | Instructions |
|---|---|
| Windows 10 | [スタート] > [設定] > [アプリ] |
| Windows 8<br/>Windows 7 | [スタート] > [コントロール パネル] > [プログラム] > [プログラムのアンインストール] |

この画面に移動したら、プログラムの検索バーに "__Azure CLI__" と入力します。 アンインストールするプログラムが "__Microsoft CLI 2.0 for Azure__" として一覧に表示されます。 このアプリケーションを選択し、`Uninstall` ボタンをクリックします。

## <a name="next-steps"></a>次の手順

これで Azure CLI をインストールできました。次は、その機能と一般的なコマンドを簡単に見ていきましょう。

> [!div class="nextstepaction"]
> [Azure CLI の概要](get-started-with-azure-cli.md)
