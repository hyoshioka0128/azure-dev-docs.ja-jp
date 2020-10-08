---
title: Azure 開発のためのローカル サービス プリンシパルの管理
description: Azure portal または Azure CLI を使用して、ローカル開発のために作成されたサービス プリンシパルを管理する方法。
ms.date: 08/18/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 9d090a4615621c60485b64fac22929472c0cd175
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764793"
---
# <a name="how-to-manage-service-principals"></a>サービス プリンシパルを管理する方法

[アプリの認証方法](azure-sdk-authenticate.md)に関する記事で説明されているように、マネージド ID を使用している場合を除き、多くの場合、サービス プリンシパルを使用して Azure でアプリを識別します。

時間の経過と共に、通常、これらのサービス プリンシパルの削除、名前変更、または管理が必要になります。これは、Azure portal を介して、または Azure CLI を使用して行うことができます。

## <a name="manage-service-principals-using-the-azure-portal"></a>Azure portal を使用してサービス プリンシパルを管理する

1. [Azure portal](https://portal.azure.com) にサインインします。

1. ポータルのホーム ページのアイコンを使用するか、ポータルの検索バーで "Azure Active Directory" を検索して、 **[Azure Active Directory]** ページに移動します。

    ![Azure portal での Azure Active Directory の検索](media/how-to-manage-service-principals/azure-ad-portal-search.png)

1. 左側のナビゲーション メニューで **[管理]**  >  **[アプリの登録]** を選択します。 ローカル開発サービス プリンシパルが一覧に表示されます。

    ![[Azure Active Directory] の [アプリの登録]](media/how-to-manage-service-principals/azure-ad-app-registrations.png)

1. いずれかのサービス プリンシパルを選択して、そのプロパティ ページに移動します。このページでは、ID 値の確認、サービス プリンシパルの名前変更または削除、およびさまざまなエンドポイント URL の取得を行うことができます。

1. 通常、特定のリソースへのアクセスをサービス プリンシパルに承認するプロセスは、対象のサービスによって異なります。 詳細については、対象のサービスのドキュメントを参照してください。 たとえば、[Blob Storage のための承認](/azure/storage/common/storage-auth-aad-rbac-portal)に関する記事と [Queue storage のための承認](/azure/storage/common/storage-auth-aad-rbac-portal)に関する記事では、Azure Storage の一部のプロセスについて説明しています。

## <a name="manage-service-principals-using-the-azure-cli"></a>Azure CLI を使用してサービス プリンシパルを管理する

Azure CLI を使用すると、Azure portal を通じて実行できる操作の多くをサービス プリンシパルに対して実行できます。

- サービス プリンシパルを作成、表示、更新、削除する: [az ad sp](/cli/azure/ad/sp) コマンド。 「[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)」も参照してください。
- ロールの割り当てを管理する: [az role assignment](/cli/azure/role/assignment) コマンド。

関連項目:

- [Azure ライブラリを使った Azure での認証](azure-sdk-authenticate.md)
