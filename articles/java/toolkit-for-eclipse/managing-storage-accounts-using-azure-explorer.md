---
title: Azure Explorer for Eclipse を使用してストレージ アカウントを管理する
description: Azure Explorer for Eclipse を使用して Azure ストレージ アカウントを管理する方法について説明します。
documentationcenter: java
ms.date: 08/25/2020
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 7a2a3406e43e1bc65ac61b7197399cdce08ef29b
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91846413"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a>Azure Explorer for Eclipse を使用してストレージ アカウントを管理する

> [!NOTE]
> Azure Explorer のストレージ アカウント機能は非推奨です。 Azure portal を利用して、ストレージ アカウントとコンテナーを作成して管理できます。 ストレージ アカウントの管理方法に関するクイックスタートについては、 [Azure Storage](/azure/storage/blobs/storage-quickstart-blobs-portal) のドキュメントをご覧ください。

Azure Toolkit for Eclipse の一部である Azure Explorer は、Eclipse 統合開発環境 (IDE) 内から Azure アカウントのストレージ アカウントを管理するための使いやすいソリューションを Java 開発者に提供します。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

[!INCLUDE [show-azure-explorer](includes/show-azure-explorer.md)]

## <a name="create-a-storage-account"></a>ストレージ アカウントの作成

1. 「[Azure Toolkit for Eclipse のサインイン手順](./sign-in-instructions.md)」を使用して Azure アカウントにサインインします。

1. **Azure Explorer** ビューで、 **[Azure]** ノードを展開し、 **[ストレージ アカウント]** を右クリックし、 **[ストレージ アカウントの作成]** をクリックします。

1. **[ストレージ アカウントの作成]** ダイアログ ボックスで、次のオプションを指定します。

   * **Name**:新しいストレージ アカウントの名前を指定します。

   * **サブスクリプション**:新しいストレージ アカウントに使用する Azure サブスクリプションを指定します。

   * **リソース グループ**:仮想マシン用のリソース グループを指定します。 以下のオプションの 1 つを選択します。
      * **[新規作成]** : 新しいリソース グループを作成することを指定します。
      * **[Use Existing]\(既存の使用\)** :Azure アカウントに関連付けられているリソース グループの一覧から選択することを指定します。

   * **[リージョン]** :ストレージ アカウントが作成される場所 ("米国西部" など) を指定します。

   * **[アカウントの種類]** : 作成するストレージ アカウントの種類 ("汎用 v1" など) を指定します。 詳細については、「[Azure ストレージ アカウントについて]」を参照してください。

   * **パフォーマンス**: 選択された発行元からどのストレージ アカウント オファリングを使用するかを指定します ("Standard" など)。 詳細については、「[Azure Storage のスケーラビリティおよびパフォーマンスのターゲット]」を参照してください。

   * **[Replication]\(レプリケーション\)** :ストレージ アカウントのレプリケーション ("ローカル冗長" など) を指定します。 詳細については、「[Azure Storage のレプリケーション]」を参照してください。

1. 上記のオプションをすべて指定したら、 **[作成]** をクリックします。

## <a name="create-and-manage-storage-containers"></a>ストレージ コンテナーの作成と管理

ストレージ コンテナーを作成して管理するには、Azure portal にアクセスするか、プログラムでリソースをプロビジョニングします。

Azure portal を使用して Azure Storage でコンテナーを作成し、そのコンテナー内のブロック BLOB をアップロードおよびダウンロードする方法の詳細なチュートリアルについては、「[Azure portal を使用して BLOB をアップロード、ダウンロード、および一覧表示する](/azure/storage/blobs/storage-quickstart-blobs-portal)」を参照してください。

## <a name="delete-a-storage-account"></a>ストレージ アカウントを削除する

1. **Azure Explorer** ビューで、ストレージ アカウントを右クリックし、 **[削除]** をクリックします。

1. 確認ウィンドウで、 **[OK]** をクリックします。


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