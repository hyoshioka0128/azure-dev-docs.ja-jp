---
title: Azure Toolkit for IntelliJ のインストール
description: Azure Toolkit for IntelliJ プラグインをインストールして、クラウド アプリケーションを作成し、Azure にデプロイする方法を説明します。
documentationcenter: java
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: fe8b07257ff3a9fc5523d13dd13e19982103ab05
ms.sourcegitcommit: a139e25190960ba89c9e31f861f0996a6067cd6c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2020
ms.locfileid: "90534474"
---
# <a name="installing-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ のインストール

Azure Toolkit for IntelliJ は、IntelliJ IDEA 開発環境を使ってクラウド アプリケーションを簡単に作成、開発、およびテストし、Azure にデプロイできるテンプレートと機能を提供します。

> [!NOTE] 
> 
> Azure Toolkit for IntelliJ はオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから MIT License で入手できます。URL は次のとおりです。 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

Azure Toolkit for IntelliJ には 2 とおりのインストール方法があります。 **[設定]** ダイアログ ボックスを使用する方法と、スタート画面の **[構成]** メニューを使用する方法です。 以降の手順では、両方のインストール方法を説明します。

## <a name="prerequisites"></a>前提条件

Azure Toolkit for IntelliJ には、以下のソフトウェア コンポーネントが必要です。

* Java Development Kit (JDK) 8 以降がインストールされていること。例:[OpenJDK](https://openjdk.java.net/) または [Oracle JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition または Community Edition がインストールされていること

> [!NOTE]
> 
> JetBrains Plugin Repository の [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) に関するページに、このツールキットと互換性のあるビルドが一覧表示されています。
> 

<!--
> [!IMPORTANT]
> 
> If you are using the Azure Toolkit for IntelliJ on Windows, the toolkit requires installing the Azure SDK 2.9.6 or later in order to use the Azure emulator. You have two options for installing the Azure SDK:
> 
> * You can download and install the Azure SDK by using the [Web Platform Installer (WebPI)](https://go.microsoft.com/fwlink/?LinkID=252838).
> * If you do not have the Azure SDK installed when you create your first Azure deployment project, you will be prompted to automatically download install the requisite version of the Azure SDK.
> 
> Note that the Azure SDK is only required on Windows.
> 
-->


## <a name="from-the-settings-dialog-box"></a>[settings]\(設定\) ダイアログ ボックスから

1. IntelliJ ツールバーで、 **[File]\(ファイル\)** をクリックし、 **[Settings]\(設定\)** をクリックします。

1. [Settings]\(設定\) ダイアログ ボックスの左側のナビゲーション メニューで、 **[Plugins]\(プラグイン\)** をクリックします。

1. **Marketplace** の検索バーに「Azure」と入力してプラグインの一覧をフィルター処理します。 **[Azure Toolkit for IntelliJ]** を選択し、 **[Install]\(インストール\)** をクリックします。 IntelliJ の *[Third-party Plugins Privacy Note]\(サード パーティ プラグインのプライバシーに関するノート\)* を読み、 **[Accept]\(同意\)** をクリックします。

   :::image type="content" source="media/installation/03-intellij-search-plugin.png" alt-text="Azure Toolkit for IntelliJ プラグインを検索します。"::: 

1. インストールが完了したら、 **[Restart IDE]\(IDE を再起動する\)** をクリックします。

1. IntelliJ IDEA を再起動するよう求めるプロンプトが表示されたら、 **[Restart]\(再起動\)** をクリックします。
   
   :::image type="content" source="media/installation/07-restart-intellij.png" alt-text="IntelliJ IDEA を再起動します。"::: 

## <a name="from-the-start-screen"></a>スタート画面から

1. IntelliJ IDEA のスタート画面で、 **[Configure]\(構成\)** をクリックし、 **[Plugins]\(プラグイン\)** をクリックします。

   :::image type="content" source="media/installation/01-intellij-configure-dropdown.png" alt-text="スタート画面から [Plugins]\(プラグイン\)。"::: 

1. **Marketplace** の検索バーに「Azure」と入力してプラグインの一覧をフィルター処理します。 **[Azure Toolkit for IntelliJ]** を選択し、 **[Install]\(インストール\)** をクリックします。 IntelliJ の *[Third-party Plugins Privacy Note]\(サード パーティ プラグインのプライバシーに関するノート\)* を読み、 **[Accept]\(同意\)** をクリックします。

   :::image type="content" source="media/installation/01-intellij-start-screen-marketplace.png" alt-text="スタート画面から Marketplace の [Plugins]\(プラグイン\)。":::

1. インストールが完了したら、 **[Restart IDE]\(IDE を再起動する\)** をクリックします。

1. IntelliJ IDEA を再起動するよう求めるプロンプトが表示されたら、 **[Restart]\(再起動\)** をクリックします。
   
   :::image type="content" source="media/installation/01-intellij-start-screen-marketplace-restart.png" alt-text="スタート画面から再起動してインストールします。":::

## <a name="next-steps"></a>次のステップ

[!INCLUDE [additional-resources](includes/additional-resources.md)]

