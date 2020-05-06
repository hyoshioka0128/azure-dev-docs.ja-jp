---
title: Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する
description: Azure Explorer for IntelliJ を使用して Azure ストレージ アカウントを管理する方法について説明します。
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 762ac74b73a274e792ed19ac8ca7c2b68d98e778
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82105073"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する

Azure Toolkit for IntelliJ の一部である Azure Explorer は、IntelliJ 統合開発環境 (IDE) 内から Azure アカウントのストレージ アカウントを管理するための使いやすいソリューションを Java 開発者に提供します。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a>IntelliJ でストレージ アカウントを作成する

Azure Explorer を使用してストレージ アカウントを作成するには、以下の手順を実行します。

1. 「[Azure Toolkit for IntelliJ のサインイン手順]」を使用して Azure アカウントにサインインします。 

2. **Azure Explorer** ビューで、 **[Azure]** ノードを展開し、 **[ストレージ アカウント]** を右クリックし、 **[ストレージ アカウントの作成]** をクリックします。

   ![[ストレージ アカウントの作成] コマンド][CS01]

3. **[ストレージ アカウントの作成]** ダイアログ ボックスで、次のオプションを指定します。

   ![[新しいストレージ アカウントの作成] ダイアログ ボックス][CS02]

   * **[名前]** : 新しいストレージ アカウントの名前を指定します。

   * **[アカウントの種類]** : 作成するストレージ アカウントの種類 ("Blob Storage" など) を指定します。 詳細については、「[Azure ストレージ アカウントについて]」を参照してください。 

   * **[パフォーマンス]** : 選択された発行元からどのストレージ アカウント サービスを使用するかを指定します ("Premium" など)。 詳細については、「[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]」を参照してください。 

   * **[レプリケーション]** : ストレージ アカウントのレプリケーション ("ゾーン冗長" など) を指定します。 詳細については、「[Azure Storage のレプリケーション]」を参照してください。 

   * **[サブスクリプション]** : 新しいストレージ アカウントに使用する Azure サブスクリプションを指定します。

   * **[場所]** : ストレージ アカウントを作成する場所を指定します ("米国西部" など)。

   * **[リソース グループ]** : 仮想マシン用のリソース グループを指定します。 以下のオプションの 1 つを選択します。
      * **[新規作成]** : 新しいリソース グループを作成することを指定します。
      * **[Use Existing\(既存の使用\)]** : Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。

4. 上記のオプションをすべて指定したら、 **[OK]** をクリックします。

## <a name="create-a-storage-container-in-intellij"></a>IntelliJ にストレージ コンテナーを作成する

Azure Explorer を使用してストレージ コンテナーを作成するには、以下の手順を実行します。

1. Azure Explorer ビューで、コンテナーを作成するストレージ アカウントを右クリックし、 **[BLOB コンテナーの作成]** をクリックします。

   ![[BLOB コンテナーを作成する] コマンド][CC01]

2. **[BLOB コンテナーを作成する]** ダイアログ ボックスで、コンテナーの名前を指定し、 **[OK]** をクリックします。 ストレージ コンテナーの名前付けの詳細については、「[コンテナー、BLOB、メタデータの名前付けと参照]」を参照してください。

   ![[BLOB コンテナーの作成] ダイアログ ボックス][CC02]

## <a name="delete-a-storage-container-in-intellij"></a>IntelliJ でストレージ コンテナーを削除する

Azure Explorer を使用してストレージ コンテナーを削除するには、以下の手順を実行します。

1. Azure Explorer ビューで、ストレージ コンテナーを右クリックし、 **[削除]** をクリックします。

   ![[ストレージ コンテナーの削除] コマンド][DC01]

2. 確認ウィンドウで **[はい]** をクリックします。

   ![[ストレージ コンテナーの削除] 確認ウィンドウ][DC02]

## <a name="delete-a-storage-account-in-intellij"></a>IntelliJ でストレージ アカウントを削除する

Azure Explorer を使用してストレージ アカウントを削除するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、ストレージ アカウントを右クリックし、 **[削除]** を選択します。

   ![ストレージ アカウントの [削除] メニュー][DS01]

2. 確認ウィンドウで **[はい]** をクリックします。

   ![[Delete storage account] (ストレージ アカウントの削除) 確認ウィンドウ][DS02]

## <a name="next-steps"></a>次のステップ

Azure ストレージ アカウント、サイズ、および料金の詳細については、次のリソースを参照してください。

* [Microsoft Azure Storage の概要]
* [Azure ストレージ アカウントについて]
* Azure ストレージ アカウントのサイズ
  * [Azure の Windows ストレージ アカウントのサイズ]
  * [Azure の Linux ストレージ アカウントのサイズ]
* Azure ストレージ アカウントの料金
  * [Windows ストレージ アカウントの料金]
  * [Linux ストレージ アカウントの料金]

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

[Azure Toolkit for IntelliJ のサインイン手順]: ./sign-in-instructions.md
[Microsoft Azure Storage の概要]: /azure/storage/common/storage-introduction
[Azure ストレージ アカウントについて]: /azure/storage/storage-create-storage-account
[Azure Storage のレプリケーション]: /azure/storage/storage-redundancy
[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]: /azure/storage/storage-scalability-targets
[コンテナー、BLOB、メタデータの名前付けと参照]: https://go.microsoft.com/fwlink/?LinkId=255555

[Azure の Windows ストレージ アカウントのサイズ]: /azure/virtual-machines/virtual-machines-windows-sizes
[Azure の Linux ストレージ アカウントのサイズ]: /azure/virtual-machines/virtual-machines-linux-sizes
[Windows ストレージ アカウントの料金]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Linux ストレージ アカウントの料金]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/managing-storage-accounts-using-azure-explorer/DC02.png
