---
title: Azure 開発のためのローカル サービス プリンシパルの管理
description: Azure portal または Azure CLI を使用して、ローカル開発のために作成されたサービス プリンシパルを管理する方法。
ms.date: 05/12/2020
ms.topic: conceptual
ms.custom: devx-track-azurecli
ms.openlocfilehash: e29ee999472d0d9d141cfad728e62f1fe518e85b
ms.sourcegitcommit: e451e4360d9c5956cc6a50880b3a7a55aa4efd2f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/31/2020
ms.locfileid: "87478361"
---
# <a name="how-to-manage-service-principals"></a>サービス プリンシパルを管理する方法

セキュリティ上の理由から、Azure リソースにアクセスして変更するためのアプリ コードの承認方法を常に慎重に管理する必要があります。 コードをローカルでテストする場合は、[ローカル Python 開発環境の認証の構成](configure-local-development-environment.md#configure-authentication)に関するページに説明されているように、完全な特権を持つユーザーとして実行するのではなく、常にローカルの "*サービス プリンシパル*" を使用する必要があります。

やがて、これらのサービス プリンシパルの削除、名前の変更、または管理が必要になる場合があることでしょう。これは、Azure portal または Azure CLI を使用して実行できます。

## <a name="basics-of-azure-authorization"></a>Azure 承認の基礎

コードが Azure リソースに対して何らかの操作 (Azure ライブラリのクラスを通じて行われる) を実行しようとするたびに、Azure によって、そのアクションを実行する権限がアプリケーションにあることが確認されます。 特定のロールベースまたはリソースベースのアクセス許可をアプリケーションの ID に付与するには、[Azure portal](https://portal.azure.com) または Azure CLI を使用します (この手順により、アプリケーションのセキュリティが侵害された場合に悪用される可能性のあるアプリケーションに対して過度なアクセス許可が付与されることを回避できます)。

Azure にデプロイされた場合、アプリケーションの ID は、通常、それをホストしているサービス (マネージド ID が有効な場合の Azure App Service、Azure Functions、仮想マシンなど) 内でアプリに付けた名前と同じになります。 ただし、コードをローカルで実行する場合、そのようなホスティング サービスは使用されないため、適切な代わりのものを Azure に示す必要があります。

この目的のために、ローカル "*サービス プリンシパル*" を使用します。これは、ユーザー ID ではなく、アプリ ID のもう 1 つの名前です。 サービス プリンシパルは、名前、"テナント" 識別子 (基本的には組織の ID)、アプリまたは "クライアント" 識別子、およびシークレットまたはパスワードを含みます。 これらの資格情報は Azure で ID を認証するのに十分であり、その ID が特定のリソースにアクセスする権限を持っているかどうかを確認できます。

それぞれの開発者は、ワークステーションのユーザー アカウント内でセキュリティ保護され、ソース管理リポジトリに保存されることのない独自のサービス プリンシパルを持つ必要があります。 サービス プリンシパルが盗まれた場合や侵害された場合は、Azure portal で簡単にそのサービス プリンシパルを削除してそのすべてのアクセス許可を取り消した後、その開発者のサービス プリンシパルを再作成できます。

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

- サービス プリンシパルを作成、表示、更新、削除する: [az ad sp](/cli/azure/ad/sp?view=azure-cli-latest) コマンド。 「[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)」も参照してください。
- ロールの割り当てを管理する: [az role assignment](/cli/azure/role/assignment?view=azure-cli-latest) コマンド。

関連項目:

- [Azure ライブラリを使った Azure での認証](azure-sdk-authenticate.md)
