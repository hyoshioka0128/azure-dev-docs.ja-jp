---
title: Azure Toolkit for Eclipse のインストール
description: Azure Toolkit for Eclipse プラグインをインストールして、クラウド アプリケーションを作成し、Azure にデプロイする方法を説明します。
documentationcenter: java
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: b269be52157516b97905b0a917fec856127a761f
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81674338"
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse のインストール

Azure Toolkit for Eclipse をインストールするには次の 2 つの方法があります。

  - [Eclipse Marketplace](#eclipse-marketplace)
  - [新しいソフトウェアのインストール](#install-new-software)

> [!NOTE] 
> 
> Azure Toolkit for Eclipse はオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから MIT License で入手できます。URL は次のとおりです。 
> 
> <https://github.com/microsoft/azure-tools-for-java> 
> 

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="eclipse-marketplace"></a>Eclipse Marketplace

1. 次のボタンを実行中の Eclipse ワークスペースにドラッグします。

    [![実行している Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要です](https://marketplace.eclipse.org/sites/all/themes/solstice/public/images/marketplace/btn-install.png)](http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=1919278 "実行している Eclipse* ワークスペースにドラッグします。*Eclipse Marketplace Client が必要です")

2. あるいは、 **[ヘルプ]/[Eclipse Marketplace]** で **Azure Toolkit for Eclipse プラグイン**を検索してインストールすることもできます。

    ![マーケットプレース](media/installation/marketplace.png)

## <a name="install-new-software"></a>新しいソフトウェアのインストール

1. Eclipse を起動します。

1. 次の図に示すように、 **[Help]\(ヘルプ\)** メニューをクリックし、 **[Install New Software]\(新しいソフトウェアのインストール\)** をクリックします。

   ![Azure Toolkit for Eclipse のインストール][01]

1. **[Available Software]\(利用可能なソフトウェア\)** ダイアログで、 **[Work with]\(対象\)** テキスト ボックスに「`http://dl.microsoft.com/eclipse/`」と入力し、**Enter** キーを押します。

1. **[Name]\(名前\)** ウィンドウで、 **[Azure Toolkit for Java]** をオンにし、 **[Contact all update sites during install to find required software]\(インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる\)** をオフにします。 画面は次のようになります。

   ![Azure Toolkit for Eclipse のインストール][02]

1. **Azure Toolkit for Eclipse** を展開すると、インストールされるコンポーネントの一覧が表示されます。次に例を示します。

   | 機能 | 説明 | 
   |---|---| 
   | **Application Insights Plugin for Java** | アプリケーションとサーバー インスタンスに対する Azure のテレメトリ ログと分析サービスを使用できるようにします。 | 
   | **Azure Common Plugin** | 他のツールキット コンポーネントで必要な一般的な機能を提供します。 | 
   | **Azure Container Tools for Eclipse** | .WAR を Docker コンテナーとして Docker マシンに構築およびデプロイできるようにします。 | 
   | **Azure Containers for Eclipse** | .WAR または.JAR アーティファクトを Docker コンテナーとして Azure 仮想マシンにデプロイできるようにします。 | 
   | **Azure Explorer for Eclipse** | Azure リソースを管理するためのエクスプローラー スタイルのインターフェイスを提供します。 | 
   | **Microsoft JDBC Driver 6.1 for SQL Server** | SQL Server と Microsoft Azure SQL Database 向けに、Java Platform Enterprise Edition 8 の JDBC API を提供します。 | 
   | **Package for Microsoft Azure Libraries for Java** | Microsoft Azure の各種サービス (ストレージ、Service Bus、サービス ランタイムなど) にアクセスするための API を提供します。 | 

1. **[次へ]** をクリックします。 (ツールキットのインストール時に異常な遅延が発生する場合は、 **[Contact all update sites during install to find required software (インストール中にすべての更新サイトに接続して必要なソフトウェアを調べる)]** がオフになっていることを確認してください)。

1. **[Install Details (インストールの詳細)]** ダイアログで、 **[Next (次へ)]** をクリックします。

   ![Review Installation Details][03]

1. **[Review Licenses (ライセンスの確認)]** ダイアログで、ライセンス契約の条項を確認します。 ライセンス契約の条項に同意する場合は、 **[I accept the terms of the license agreements (ライセンス契約の条項に同意する)]** をクリックし、 **[Finish (完了)]** をクリックします。 (残りの手順は、ライセンス契約の条項に同意したことを前提としています。 ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)

   ![Review Licenses][04]

   必要なパッケージが Eclipse によってダウンロードされてインストールされます。

   ![Installation Progress][05]

1. インストールを完了するために Eclipse を再起動することを求めるメッセージが表示されたら、 **[Yes]** をクリックします。

   ![Restart Prompt][06]

## <a name="next-steps"></a>次のステップ

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

<!-- IMG List -->
[01]: media/installation/eclipse-installation-01.png
[02]: media/installation/eclipse-installation-02.png
[03]: media/installation/eclipse-installation-03.png
[04]: media/installation/eclipse-installation-04.png
[05]: media/installation/eclipse-installation-05.png
[06]: media/installation/eclipse-installation-06.png
