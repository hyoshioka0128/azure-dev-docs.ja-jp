---
title: Azure Explorer for Eclipse を使用して仮想マシンを管理する
description: Azure Explorer for Eclipse を使用して Azure 仮想マシンを管理する方法について説明します。
documentationcenter: java
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: e457d4fe152f9fa5fa64bafaa4f49311e8ff4475
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831888"
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a>Azure Explorer for Eclipse を使用して仮想マシンを管理する

Azure Toolkit for Eclipse の一部である Azure Explorer は、Eclipse 統合開発環境 (IDE) 内から Azure アカウントの仮想マシンを管理するための使いやすいソリューションを Java 開発者に提供します。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-virtual-machine"></a>仮想マシンの作成

1. 「[Azure Toolkit for Eclipse のサインイン手順](./sign-in-instructions.md)」を使用して Azure アカウントにサインインします。

1. **Azure Explorer** ビューで、 **[Azure]** ノードを展開し、 **[仮想マシン]** を右クリックし、 **[VM の作成]** をクリックします。

   :::image type="content" source="media/managing-virtual-machines-using-azure-explorer/CR01.png" alt-text="Azure Explorer の [VM の作成] オプション。":::

1. **[サブスクリプションの選択]** ウィンドウで、サブスクリプションを選択し、 **[次へ]** をクリックします。

1. **[仮想マシン イメージの選択]** ウィンドウで、 **[Location]\(場所\)** ( *[West US]\(米国西部\)* など) を選択します。 推奨イメージを使用して進めることも、カスタム イメージを選択することもできます。 このクイックスタートでは、推奨イメージを使用して進めます。 

   カスタム イメージを選択する場合は、次の情報を入力します。
   * **[発行者]** : 仮想マシンの作成に使用するイメージを作成した発行元を指定します (*Microsoft* など)。

   * **[プラン]** : 選択した発行元の、仮想マシンで使用するプランを指定します (*JDK* など)。

   * **[SKU]** : 選択したプランから、使用する在庫保管単位 (SKU) を指定します ("*JDK_8*" など)。

   * **[Version #]\(バージョン番号\)** : 選択した SKU で使用するバージョンを指定します。

1. **[次へ]** をクリックします。

1. **[仮想マシンの基本設定]** ウィンドウで、次の情報を入力します。

   * **[仮想マシン名]** : 新しい仮想マシンの名前を指定します。英字、数字、ハイフンのみ使用でき、先頭には英字を使用する必要があります。

   * **Size**:仮想マシンに割り当てるコアとメモリの数を指定します。

   * **ユーザー名**:仮想マシンを管理するために作成する管理者アカウントを指定します。

   * **[パスワード]** : 管理者アカウントのパスワードを指定します。 **[Confirm]\(確認\)** ボックスにパスワードを再入力して、資格情報を確認します。

1. **[次へ]** をクリックします。

1. **[関連するリソース]** ウィンドウで、次の情報を入力します。
   * **リソース グループ**:仮想マシン用のリソース グループを指定します。 以下のオプションの 1 つを選択します。
      * **新規作成**: 新しいリソース グループを作成することを指定します。
      * **[既存のものを使用]** : Azure アカウントに既に関連付けられているリソース グループの一覧から選択することを指定します。

   * **ストレージ アカウント**: 仮想マシンを格納するために使用するストレージ アカウントを指定します。 既存のストレージ アカウントを使用するか、新しいアカウントを作成できます。

   * **[仮想ネットワーク]** と **[サブネット]** : 仮想マシンを接続する仮想ネットワークとサブネットを指定します。 既存のネットワークとサブネットを使用するか、新しいネットワークとサブネットを作成できます。 **[新規作成]** を選択した場合は、次のダイアログ ボックスが表示されます。

   * **[パブリック IP アドレス]** : 仮想マシンの外部接続 IP アドレスを指定します。 新しい IP アドレスを作成できます。仮想マシンのパブリック IP アドレスがない場合は、 **[(なし)]** を選択できます。

   * **[ネットワーク セキュリティ グループ]** : 仮想マシンのネットワーク ファイアウォールを指定します (省略可能)。 既存のファイアウォールを選択できます。仮想マシンでネットワーク ファイアウォールを使用しない場合は、 **[(なし)]** を選択できます。

   * **可用性セット**:仮想マシンが属することができる可用性セットを指定します (省略可能)。 既存の可用性セットを指定するか、新しい可用性セットを作成できます。仮想マシンが可用性セットに属さない場合は、 **[(なし)]** を選択します。

10. **[完了]** をクリックします。  

      > [!NOTE]
      > Eclipse ワークスペースの右下隅で作成の進行状況を確認できます。

## <a name="restart-a-virtual-machine"></a>仮想マシンを再起動します

Eclipse で Azure Explorer を使用して仮想マシンを再起動するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、仮想マシンを右クリックし、 **[再起動]** を選択します。

1. 確認ウィンドウで、 **[OK]** をクリックします。

   ![仮想マシンの再起動の確認ウィンドウ](media/managing-virtual-machines-using-azure-explorer/RE02.png)

## <a name="shut-down-a-virtual-machine"></a>仮想マシンをシャットダウンする

Eclipse で Azure Explorer を使用して実行中の仮想マシンをシャットダウンするには、以下の手順を実行します。

1. **Azure Explorer** ビューで、仮想マシンを右クリックし、 **[シャットダウン]** を選択します。

1. 確認ウィンドウで、 **[OK]** をクリックします。

   ![仮想マシンのシャットダウン確認ウィンドウ](media/managing-virtual-machines-using-azure-explorer/SH02.png)

## <a name="delete-a-virtual-machine"></a>仮想マシンを削除します

Eclipse で Azure Explorer を使用して仮想マシンを削除するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、仮想マシンを右クリックし、 **[削除]** を選択します。

1. 確認ウィンドウで **[はい]** をクリックします。

   ![仮想マシンの削除の確認ウィンドウ](media/managing-virtual-machines-using-azure-explorer/DE02.png)

## <a name="next-steps"></a>次のステップ

Azure 仮想マシンのサイズと料金について詳しくは、次のリソースを参照してください。

* Azure 仮想マシンのサイズ
  * [Azure の Windows 仮想マシンのサイズ]
  * [Azure の Linux 仮想マシンのサイズ]
* Azure 仮想マシンの料金
  * [Windows 仮想マシンの料金]
  * [Linux 仮想マシンの料金]

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Azure の Windows 仮想マシンのサイズ]: /azure/virtual-machines/sizes
[Azure の Linux 仮想マシンのサイズ]: /azure/virtual-machines/sizes
[Windows 仮想マシンの料金]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Linux 仮想マシンの料金]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: media/managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: media/managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: media/managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: media/managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: media/managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: media/managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: media/managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: media/managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: media/managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: media/managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: media/managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: media/managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: media/managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: media/managing-virtual-machines-using-azure-explorer/CR08.png