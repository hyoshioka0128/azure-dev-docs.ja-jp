---
title: アプリ ID またはサービス プリンシパルにロールのアクセス許可を割り当てる
description: Azure CLI を使用してサービス プリンシパルまたはアプリ ID にアクセス許可を付与する方法
ms.date: 05/12/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: facfa1663e6f62a7458f99ee20c86f66ee67b17d
ms.sourcegitcommit: 800c5e05ad3c0b899295d381964dd3d47436ff90
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2020
ms.locfileid: "88614482"
---
# <a name="how-to-assign-role-permissions-to-an-app-identity-or-service-principal"></a>アプリ ID またはサービス プリンシパルにロールのアクセス許可を割り当てる方法

Azure のロールベースのアクセス制御 (RBAC) システムでは、さまざまなリソースに対する特定のアクセス許可を管理します。 "*ロール*" は、基本的には、通常同時に必要となる関連するアクセス許可のコレクションです。 アクセス許可を有効にするには、ロールを、そのロールが適用される特定の "*スコープ*" を持つ "*セキュリティ プリンシパル*" (ユーザー、グループ、サービス プリンシパル、またはアプリ ID) に割り当てます。

実際には、常に、セキュリティ プリンシパルにとって最も限定的なスコープで本当に必要であるロールのみを割り当てるようにしてください。 当初、より広範なスコープでより広範なロールを割り当てる方が便利に思われたとしても、避けてください。 ロールとスコープを制限することにより、セキュリティ プリンシパルが侵害された場合 (つまり、そのプリンシパルの資格情報がデータ侵害やその他のセキュリティ インシデントで公開されてしまった場合) にリスクにさらされるリソースを制限できます。

開発環境と運用環境では異なるセキュリティ プリンシパルを使用するため、各環境でロールの割り当てを繰り返します。 つまり、開発中は通常、ワークステーションで作成されたローカル サービス プリンシパルにロールを割り当てます ([ローカル Python 開発環境の認証の構成](configure-local-development-environment.md#configure-authentication)に関するセクションを参照してください)。 運用環境では、デプロイの前にアプリケーションのマネージド ID またはサービス プリンシパルにロールを割り当てて、アプリケーションが起動時にアクセスできるようにします。 詳細については、[認証 - Azure で実行されている場合のアプリの ID](azure-sdk-authenticate.md#identity-when-running-the-app-on-azure) に関する記事を参照してください。

一般的な RBAC の詳細については、[Azure のロールベースのアクセス制御](/azure/role-based-access-control/overview)に関するページを参照してください。

## <a name="role-assignment-process"></a>ロールの割り当てプロセス

ロールを割り当てるには、次の 3 つの手順を実行します。

1. 関連するリソースの種類と承認したい操作に[適したロール](#find-the-roles-you-need)を見つけます。

1. 対象のロールに必要なスコープを特定します。スコープは、操作の承認対象となるリソースの範囲を示します。

1. ロールをセキュリティ プリンシパルに割り当てます。

手順 1 は、Azure portal と Azure CLI の両方で同じです。 手順 2 と 3 についてはポータルと CLI で異なるため、後続のセクションでまとめて説明します: 「[Azure portal でスコープを識別し、ロールを割り当てる](#azure-portal)」および「[Azure CLI を使用し、スコープを識別してロールを割り当てる](#azure-cli)」。

> [!NOTE]
> ご自分のサブスクリプション内でロールを割り当てるためのアクセス許可がご自分のユーザー アカウントにない場合は、ご自分のアカウントが "does not have authorization to perform action 'Microsoft.Authorization/roleAssignments/write' ('Microsoft.Authorization/roleAssignments/write' に対するアクションの実行を承認されていません)" というエラー メッセージが表示されます。この場合は、自分の代わりにアクセス許可を割り当てることができるサブスクリプションの管理者に連絡してください。

## <a name="find-the-roles-you-need"></a>必要なロールを見つける

1. まず、包括的な記事である「[Azure 組み込みロール](/azure/role-based-access-control/built-in-roles)」から始めます。 この記事の冒頭にある表は、記事の後の方にある詳細説明へのインデックスです。

1. この記事で、アクセス許可を付与する対象リソースのサービス カテゴリ (コンピューティング、ストレージ、データベースなど) に移動します。 通常、探しているものを見つけるのに最も簡単な方法は、"BLOB"、"仮想マシン" などの関連キーワードをページ内で検索することです。

1. サービス カテゴリに一覧表示されているロールを確認し、必要な特定の操作を特定します。 繰り返しになりますが、常に、最も制限の厳しいロールから始めるようにしてください。

    たとえば、セキュリティ プリンシパルが Azure Storage アカウントの BLOB を読み取る必要がある一方で、書き込みアクセスを必要としない場合は、"ストレージ BLOB データ共同作成者" ではなく "ストレージ BLOB データ閲覧者" を選択します (決して管理者レベルの "ストレージ BLOB データ所有者" ロールを選択しないようにしてください)。 ロールの割り当ては、必要に応じて後でいつでも更新できます。

    セキュリティ プリンシパルがキュー ストレージにもアクセスする必要がある場合は、"ストレージ キュー データ閲覧者" や "ストレージ キュー データ共同作成者" などのロールを使用できます。 繰り返しますが、できる限り限定的なロールを使用してください。"閲覧者とデータ アクセス" などの広範なロールを割り当てた場合、アカウント キーへのアクセスが提供され、ストレージ アカウント全体のあらゆるものにプリンシパルがアクセスできるようになるため、避けてください。

1. 適切なロールが見つからない場合は、[カスタム ロール](/azure/role-based-access-control/custom-roles)を作成できます。

## <a name="identify-scope-and-assign-a-role-on-the-azure-portal"></a><a name="azure-portal"></a>Azure portal でスコープを識別し、ロールを割り当てる

1. [Azure portal](https://portal.azure.com) で、ロールを割り当てるリソースに移動します。 割り当てのスコープは、リソースのスコープによって決まります。

    たとえば、ストレージ アカウントに移動した場合、ロールの割り当てはすべてストレージ アカウント全体に適用されます。 そのストレージ アカウント内の特定の BLOB コンテナーに移動した場合、ロールの割り当てはそのコンテナーにのみ適用されます。

1. **[アクセス制御 (IAM)]** ブレードを選択します (IAM は "ID およびアクセス管理" の略です)。

1. このブレード内には **[ロールの割り当て]** セクションがあり、任意のセキュリティ プリンシパルにロールの割り当てを追加したり、割り当てられたロールを削除したりできます。

詳細と UI の説明については、Azure RBAC ドキュメントの [Azure ロールの割り当ての追加または削除](/azure/role-based-access-control/role-assignments-portal)に関するページを参照してください。

## <a name="identify-scope-and-assign-a-role-through-the-azure-cli"></a><a name="azure-cli"></a>Azure CLI を使用してスコープを識別し、ロールを割り当てる

Azure CLI でロールを割り当てるには、[`az role assignment`](/cli/azure/role/assignment?view=azure-cli-latest) コマンドを使用します。 割り当てを追加するには `az role assignment create` を使用し、割り当てを削除するには `az role assignment delete` を使用します。

完全なプロセスについては、「[Azure CLI を使用して Azure でのロールの割り当てを追加または削除する](/azure/role-based-access-control/role-assignments-cli)」で説明されています。次の概要では、このデベロッパー センターの他の記事に関連する具体的な例を示します。

`az role assignment create` コマンドの構文は次のとおりです。

```azurecli
az role assignment create --assignee <assignee> --role <role> --scope <scope>
```

- `<assignee>` は、セキュリティ プリンシパルを識別します。ローカル開発中に使用するサービス プリンシパルの場合、割り当て先はそのプリンシパルのクライアント ID です。 クラウドにデプロイされたアプリケーションの場合、割り当て先はアプリケーションの名前です。
- `<role>` は、割り当てるロールの名前 ("ストレージ BLOB データ共同作成者" など) またはその GUID ("ba92f5b4-2d11-453d-a403-e96b0029c9fe" など) です。
- `<scope>` は、割り当ての正確なスコープを識別する文字列です (この文字列は長くなる可能性があります)。

スコープは、/ 文字で区切られた一連の識別子で構成されます。 この文字列は、次の階層を表すと考えることができます。ここで、プレースホルダー (`<>`) のないテキストは固定識別子です。

<pre>
/subscriptions
  /&lt;subscription_id&gt;
    /resourcegroups
      /&lt;resource_group_name&gt;
        /providers
          /&lt;provider_name&gt;
            /&lt;resource_type&gt;
              /&lt;resource_sub_type_1&gt;
                /&lt;resource_sub_type_2&gt;
                  /&lt;resource_name&gt;
</pre>

- `<subscription_id>` は、使用するサブスクリプションの ID (GUID) です。
- `<resources_group_name>` は、包含リソース グループの名前です。
- `<provider_name>` は、リソースを処理するサービスの名前です。`<resource_type>` と `<resource_sub_type_*>` は、そのサービス内のレベルをさらに識別します。
  
    これらの名前と種類については、「[Azure 組み込みロール](/azure/role-based-access-control/built-in-roles)」リファレンスを参照してください。 冒頭の表内でロールを見つけて選択すると、ロールの具体的な説明に移動できます。 そこで、プロバイダー名、リソースの種類、およびリソースのサブタイプを含む文字列が見つかります。 たとえば、"ストレージ BLOB データ共同作成者" の場合、"Microsoft.Storage/storageAccounts/blobServices/containers/" が見つかります。 "Cosmos DB アカウントの閲覧者ロール" の場合、"Microsoft.DocumentDB/databaseAccounts/readonlykeys" が見つかります。これには、サブタイプが 1 つだけ含まれます。

- `<resource_name>` は、特定のリソースを識別する文字列の最後の部分です。

最も広い (最も限定的でない) スコープは `/subscriptions/<subscription_id>` で、サブスクリプション全体に割り当てが適用されます。 階層のレベルを追加するごとに、スコープがより限定的になります。

### <a name="examples"></a>例

次の例では、次の条件を想定しています (「[例: Azure Storage のプロビジョニングと使用](azure-sdk-example-storage.md)):

- Azure サブスクリプション ID は、`AZURE_SUBSCRIPTION_ID` という名前の環境変数に含まれています。
- ロールを割り当てるサービス プリンシパルのクライアント ID は、`AZURE_CLIENT_ID` という名前の環境変数に含まれています。
- サブスクリプションには、"PythonAzureExample-Storage-rg" という名前のリソース グループがあります。
- このリソース グループには、"pythonazurestorage-12345" という名前の Azure Storage アカウントが含まれています。
- このストレージ アカウントには、"blob-container-01" という名前の BLOB コンテナーがあります。
- サービス プリンシパルに "ストレージ BLOB データ共同作成者" ロールを割り当てます。

> [!TIP]
> ロールの割り当ての変更が Azure 内に反映されるまでに 1 分かかる場合があります。 その結果、アクセス許可を削除した後もコードがしばらく機能する場合があります。 予期しない動作が発生した場合は、1、2 分待ってから再試行してください。

#### <a name="grant-permissions-for-the-specific-container-only"></a>特定のコンテナーのみに対するアクセス許可を付与する

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
```

---

#### <a name="grant-permissions-for-all-blob-containers-in-the-storage-account"></a>ストレージ アカウントのすべての BLOB コンテナーに対するアクセス許可を付与する

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345"
```

---

#### <a name="grant-permissions-for-all-blob-containers-in-the-resource-group"></a>リソース グループ内のすべての BLOB コンテナーに対するアクセス許可を付与する

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg"
```

または、`--resource-group` パラメーターを使用してリソース グループを指定することもできます。

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --resource-group "PythonAzureExample-Storage-rg"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg"
```

または、`--resource-group` パラメーターを使用してリソース グループを指定することもできます。

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --resource-group "PythonAzureExample-Storage-rg"
```

---

#### <a name="grant-permissions-to-all-blob-containers-in-the-subscription"></a>サブスクリプションのすべての BLOB コンテナーに対するアクセス許可を付与する

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az role assignment create --assignee %AZURE_CLIENT_ID% ^
    --role "Storage Blob Data Contributor" ^
    --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%"
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az role assignment create --assignee $AZURE_CLIENT_ID \
    --role "Storage Blob Data Contributor" \
    --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID"
```

---
