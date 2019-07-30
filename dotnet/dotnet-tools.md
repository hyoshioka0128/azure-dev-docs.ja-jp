---
title: Azure .NET および .NET Core 開発者向けツール
description: Windows、Linux、Mac 環境から Azure .NET ライブラリを使い始めるためのツールを入手します。
ms.date: 08/11/2018
ms.custom: azure-dotnet-devcenter, azure-dotnet-devcenter-authored, azure-dotnet-devcenter-conceptual, vs-azure
ms.openlocfilehash: e9921ae1c8e64426bdefdbcf201708d9195a6a51
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2019
ms.locfileid: "68280593"
---
# <a name="tools-for-net-and-net-core-azure-developers"></a>.NET および .NET Core Azure 開発者向けツール

お使いのオペレーティング システムが何であっても、.NET、.NET Core、ASP.NET、ASP.NET Core を使用した Azure での開発に役立つ一連の優れたツールが用意されています。

## <a name="windowstabwindows"></a>[Windows](#tab/windows)

<table>
  <tr>
    <td width="50">
        <img src="https://docs.microsoft.com/media/logos/logo_vs-ide.svg" width="50" height="50"></img>
    </td>
    <td>
        Visual Studio 2017 には、Azure 開発の組み込みサポートがあります。
    </td>
  </tr>
</table>

### <a name="step-1-download-visual-studio-2017"></a>手順 1:Visual Studio 2017 をダウンロードする

Visual Studio 2017 を既にインストールしてある場合は省略できます。

まだ Visual Studio 2015 をお使いの場合は、  [こちらをクリックしてインストール手順に従ってください](dotnet-sdk-vs2015-install.md)。

> [!div class="nextstepaction"]
> [Visual Studio 2017 をダウンロードする](https://www.visualstudio.com/downloads/)

### <a name="step-2-install-the-two-azure-workloads"></a>手順 2:2 つの Azure ワークロードをインストールする

Visual Studio 2017 インストーラーを起動した後、Azure と ASP.NET Web の開発ワークロードがオンになっていることを確認します。

![Visual Studio インストーラー](media/dotnet-tools/azure-workloads.png)

### <a name="step-3-develop-with-net-on-azure"></a>手順 3:Azure で .NET を使って開発する

まず、[最初の ASP.NET Core Web アプリを Azure App Service にデプロイ](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)します。

## <a name="macostabmacos"></a>[macOS](#tab/macos)
<table>
  <tr>
    <td width="50">
        <img src="https://docs.microsoft.com/media/logos/logo_vs-mac.svg" width="50" height="50"></img>
    </td>
    <td>
        Visual Studio for Mac には、Azure での開発に必要なすべてのものが揃っています。
    </td>
  </tr>
</table>

### <a name="step-1-download-visual-studio-for-mac"></a>手順 1:Visual Studio for Mac をダウンロードする

> [!div class="nextstepaction"]
> [Visual Studio for Mac をダウンロードする](https://www.visualstudio.com/vs/visual-studio-mac/)

Azure ツールは、インストールの間に既定で有効になります。

![Visual Studio for Mac インストーラー](media/dotnet-tools/azure-vsmac.png)

## <a name="linuxtablinux"></a>[Linux](#tab/linux)

<table>
  <tr>
    <td width="50">
        <img src="https://docs.microsoft.com/media/logos/logo_vs-code.svg" width="50" height="50"></img>
    </td>
    <td>
        Visual Studio Code には、Linux における Azure での開発に必要なすべてのものが揃っています。
    </td>
  </tr>
</table>

### <a name="step-1-download-the-net-core-sdk"></a>手順 1:.NET Core SDK をダウンロードする

.NET Core アプリ用の SDK とコマンド ライン ツールです。

> [!div class="nextstepaction"]
> [.NET Core SDK をダウンロードする](https://www.microsoft.com/net/core)

### <a name="step-2-visual-studio-code"></a>手順 2:Visual Studio Code

任意のオペレーティング システム上で .NET Core アプリを編集し、デバッグします (Windows、Mac、Linux)。

> [!div class="nextstepaction"]
> [Visual Studio Code をダウンロードする](https://code.visualstudio.com)
