---
title: Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する
description: Azure Explorer for IntelliJ を使用して Azure ストレージ アカウントを管理する方法について説明します。
documentationcenter: java
ms.date: 09/09/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 2c0de20cd28725d8cf9b482841978640c8a37ee8
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846783"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a>Azure Explorer for IntelliJ を使用してストレージ アカウントを管理する

> [!NOTE]
> Azure Explorer のストレージ アカウント機能は非推奨です。 Azure portal を利用して、ストレージ アカウントとコンテナーを作成して管理できます。 ストレージ アカウントの管理方法に関するクイックスタートについては、 [Azure Storage](/azure/storage/blobs/storage-quickstart-blobs-portal) のドキュメントをご覧ください。

Azure Toolkit for IntelliJ の一部である Azure Explorer は、IntelliJ 統合開発環境 (IDE) 内から Azure アカウントのストレージ アカウントを管理するための使いやすいソリューションを Java 開発者に提供します。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account"></a>ストレージ アカウントの作成

Azure Explorer を使用してストレージ アカウントを作成するには、以下の手順を実行します。

1. 「[Azure Toolkit for IntelliJ のサインイン手順]」を使用して Azure アカウントにサインインします。 

2. **Azure Explorer** ビューで、 **[Azure]** ノードを展開し、 **[ストレージ アカウント]** を右クリックし、 **[ストレージ アカウントの作成]** をクリックします。

3. **[ストレージ アカウントの作成]** ダイアログ ボックスで、次のオプションを指定します。

   * **Name**:新しいストレージ アカウントの名前を指定します。

   * **[アカウントの種類]** : 作成するストレージ アカウントの種類 ("Blob Storage" など) を指定します。 詳細については、「[Azure ストレージ アカウントについて]」を参照してください。 

   * **パフォーマンス**: 選択された発行元からどのストレージ アカウント サービスを使用するかを指定します ("Premium" など)。 詳細については、「[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]」を参照してください。 

   * **[Replication]\(レプリケーション\)** :ストレージ アカウントのレプリケーション ("ゾーン冗長" など) を指定します。 詳細については、「[Azure Storage のレプリケーション]」を参照してください。 

   * **サブスクリプション**:新しいストレージ アカウントに使用する Azure サブスクリプションを指定します。

   * **[場所]** :ストレージ アカウントが作成される場所 ("米国西部" など) を指定します。

   * **リソース グループ**:仮想マシン用のリソース グループを指定します。 以下のオプションの 1 つを選択します。
      * **新規作成**: 新しいリソース グループを作成することを指定します。
      * **[Use Existing]\(既存の使用\)** : Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。

4. 上記のオプションをすべて指定したら、 **[OK]** をクリックします。

## <a name="delete-a-storage-account"></a>ストレージ アカウントを削除する

Azure Explorer を使用してストレージ アカウントを削除するには、以下の手順を実行します。

1. **Azure Explorer** ビューで、ストレージ アカウントを右クリックし、 **[削除]** を選択します。

2. 確認ウィンドウで **[はい]** をクリックします。


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
[Naming and referencing containers, blobs, and metadata]: /rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata

[Azure の Windows ストレージ アカウントのサイズ]: /azure/virtual-machines/sizes
[Azure の Linux ストレージ アカウントのサイズ]: /azure/virtual-machines/sizes
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