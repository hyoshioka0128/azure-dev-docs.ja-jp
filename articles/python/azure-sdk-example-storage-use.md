---
title: Azure SDK for Python で Azure Storage を使用する
description: Azure SDK for Python ライブラリを使用して、Azure Storage アカウントの事前にプロビジョニングされた BLOB コンテナーにアクセスし、そのコンテナーにファイルをアップロードします。
ms.date: 08/05/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 161e27f1323053b45c687e60ade90e863ce71c64
ms.sourcegitcommit: b03cb337db8a35e6e62b063c347891e44a8a5a13
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2020
ms.locfileid: "91110490"
---
# <a name="example-access-azure-storage-using-the-azure-libraries-for-python"></a>例:Python 用 Azure ライブラリを使用して Azure Storage にアクセスする

この例では、Python アプリケーション コードで Azure クライアント ライブラリを使用して、その Blob Storage コンテナーにファイルをアップロードする方法を示しています。 この例は、[Azure Storage のプロビジョニングの例](azure-sdk-example-storage.md)に示されているリソースをプロビジョニングしていることを前提としています。

特に記載のない限り、この記事で使用されているコマンドはいずれも、Linux と Mac OS の bash および Windows のコマンド シェルで同じように動作します。

## <a name="1-set-up-your-local-development-environment"></a>1:ローカルの開発環境を設定する

「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」の手順をすべて実行します (まだ済んでいない場合)。

必ず、ローカル開発用のサービス プリンシパルを作成し、サービス プリンシパルの環境変数を設定 (下記参照) し、このプロジェクト用の仮想環境を作成してアクティブ化してください。

この例では、次の環境変数が既に設定されていることを前提としています。

| 変数名 | 必要な値 |
| --- | --- |
| AZURE_SUBSCRIPTION_ID | Azure サブスクリプションの GUID。 |
| AZURE_CLIENT_ID | ローカル サービス プリンシパルのクライアント ID。 |
| AZURE_CLIENT_SECRET | サービス プリンシパルのクライアント シークレット。 |
| AZURE_TENANT_ID | サービス プリンシパルのテナント ID。 |

## <a name="2-install-library-packages"></a>2:ライブラリ パッケージをインストールする

1. *requirements.txt* ファイルに、必要なクライアント ライブラリ パッケージの行を追加して、ファイルを保存します。

    ```text
    azure-storage-blob
    azure-identity
    ```

1. ターミナルまたはコマンド プロンプトで、要件を再インストールします。

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-create-a-file-to-upload"></a>3:アップロードするファイルを作成する

次のような内容を含んだソース ファイルを *sample-source.txt* という名前 (コードで想定) で作成します。

```text
Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.
```

## <a name="4-use-blob-storage-from-app-code"></a>4:アプリ コードから Blob Storage を使用する

以降のセクション (番号 4a と 4b) では、[Azure Storage のプロビジョニングの例](azure-sdk-example-storage.md)に関するページでプロビジョニングされた BLOB コンテナーにアクセスする 2 つの方法を示します。

[1 つ目の方法 (以下のセクション 4a)](#4a-use-blob-storage-with-authentication) は、[Python アプリの認証方法](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential)に関する記事で説明されている `DefaultAzureCredential` を使用してアプリを認証します。 この方法では最初に、アプリの ID に適切なアクセス許可を割り当てる必要があります。これが、推奨されるプラクティスです。

[2 つ目の方法 (以下のセクション 4b)](#4b-use-blob-storage-with-a-connection-string) は、接続文字列を使用してストレージ アカウントに直接アクセスします。 こちらの方法の方が一見シンプルに見えますが、大きな欠点が 2 つあります。

- 接続文字列は本質的に、接続元のエージェントをストレージ "*アカウント*" に対して認証するものであり、そのアカウント内の個々のリソースに対して認証を行うものではありません。 そのため、接続文字列では、許可する承認の範囲が必要以上に広くなってしまう可能性があります。

- 接続文字列はアクセス キーをプレーンテキストで保持するため、適切に構築されていなかったり、セキュリティが適切に確保されていなかったりした場合に脆弱性のリスクが生じます。 そのような接続文字列が公開されれば、ストレージ アカウント内の広範なリソースへのアクセスに使用される可能性があります。

このような理由から、運用環境のコードでは認証方法を使用することをお勧めします。

### <a name="4a-use-blob-storage-with-authentication"></a>4a:認証によって Blob Storage を使用する

1. `STORAGE_BLOB_URL` という名前の環境変数を作成します。

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    ---

    "pythonazurestorage12345" は、実際のストレージ アカウントの名前に置き換えてください。

    この環境変数は、この例でのみ使用され、Azure ライブラリでは使用されません。

1. 次のコードを含んだ *use_blob_auth.py* という名前のファイルを作成します。 ステップは、コメントで説明しています。

    ```python
    import os
    from azure.identity import DefaultAzureCredential

    # Import the client object from the Azure library
    from azure.storage.blob import BlobClient

    credential = DefaultAzureCredential()

    # Retrieve the storage blob service URL, which is of the form
    # https://pythonazurestorage12345.blob.core.windows.net/
    storage_url = os.environ["STORAGE_BLOB_URL"]

    # Create the client object using the storage URL and the credential
    blob_client = BlobClient(storage_url, container_name="blob-container-01", blob_name="sample-blob.txt", credential=credential)

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

    参照リンク:
      - [DefaultAzureCredential (azure.identity)](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python)
      - [BlobClient (azure.storage.blob)](/python/api/azure-storage-blob/azure.storage.blob.blobclient?view=azure-python)

1. コードを実行しようとします (意図的に失敗します)。

    ```cmd
    python use_blob_auth.py
    ```

    使用しているローカル サービス プリンシパルには、BLOB コンテナーへのアクセス許可がないため、"This request is not authorized to perform this operation using this permission." (この要求にはこのアクセス許可を使用してこの操作を実行する権限がありません) という内容のエラーが表示されます。

1. Azure CLI コマンド [az role assignment create](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create) を使用して、サービス プリンシパルにコンテナーのアクセス許可を付与します。コマンドが長いのでご注意ください。

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

    `--scope` 引数は、対象となるロールの割り当て先を指定するものです。 この例では、"ストレージ BLOB データ共同作成者" ロールを、"blob-container-01" という名前の "*特定*" のコンテナーに付与しています。

    `pythonazurestorage12345` は、ストレージ アカウントの正確な名前に置き換えてください。 必要であれば、リソース グループと BLOB コンテナーの名前を調整することもできます。 間違った名前を使用すると、"Can not perform requested operation on nested resource. Parent resource 'pythonazurestorage12345' not found." (ネストされたリソースに対して要求された操作を実行できません。親リソース 'pythonazurestorage12345' が見つかりません) というエラーが表示されます。

    このコマンドの `--scope` 引数には、AZURE_CLIENT_ID と AZURE_SUBSCRIPTION_ID という環境変数が使用されています。これらは、「[Azure 用のローカル Python 開発環境を構成する](configure-local-development-environment.md)」に従い、ローカル環境でサービス プリンシパルに設定したものです。

1. アクセス許可が反映されるまで 1、2 分待機した後、コードをもう一度実行して、今度は動作することを確認します。 アクセス許可のエラーが再度表示された場合は、もう少し待ってから、もう一度コードを試してください。

スコープとロールの割り当てについて詳しくは、[ロールのアクセス許可を割り当てる方法](how-to-assign-role-permissions.md)に関するページを参照してください。

### <a name="4b-use-blob-storage-with-a-connection-string"></a>4b:接続文字列を利用して Blob Storage を使用する

1. `AZURE_STORAGE_CONNECTION_STRING` という名前の環境変数を作成します。この値は、ストレージ アカウントの完全な接続文字列です。 (この環境変数は、さまざまな Azure CLI のコメントでも使用されます。)

1. 次のコードを使用して、*use_blob_conn_string.py* という名前の Python ファイルを作成します。 ステップは、コメントで説明しています。

    ```python
    import os

    # Import the client object from the Azure library
    from azure.storage.blob import BlobClient

    # Retrieve the connection string from an environment variable.
    conn_string = os.environ["AZURE_STORAGE_CONNECTION_STRING"]

    # Create the client object for the resource identified by the connection string,
    # indicating also the blob container and the name of the specific blob we want.
    blob_client = BlobClient.from_connection_string(conn_string, container_name="blob-container-01", blob_name="sample-blob.txt")

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

1. コードを実行します。

    ```bash
    python use_blob_conn_string.py
    ```

既に述べたように、この方法はシンプルではありますが、接続文字列によって、ストレージ アカウント内のすべての操作が承認されます。 前のセクションで説明したように、運用環境のコードでは、具体的なアクセス許可を使用することをお勧めします。

## <a name="5-verify-blob-creation"></a>5.BLOB の作成を確認する

どちらかの方法のコードを実行した後、[Azure portal](https://portal.azure.com) にアクセスし、BLOB コンテナーに移動して、*sample-source.txt* ファイルと同じ内容の新しい BLOB が *sample-blob.txt* という名前で存在することを確認します。

![BLOB コンテナーの Azure portal ページに表示されるアップロード済みのファイル](media/azure-sdk-example-storage/portal-blob-container-file.png)

## <a name="6-clean-up-resources"></a>6: リソースをクリーンアップする

```azurecli
az group delete -n PythonAzureExample-Storage-rg  --no-wait
```

この例でプロビジョニングしたリソースを残す必要がなければ、今後サブスクリプションに課金されないようにするために、このコマンドを実行してください。

コードから [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) メソッドを使用してリソース グループを削除することもできます。

## <a name="see-also"></a>関連項目

- [例:リソース グループをプロビジョニングする](azure-sdk-example-resource-group.md)
- [例:Web アプリをプロビジョニングしてコードをデプロイする](azure-sdk-example-web-app.md)
- [例:Azure Storage をプロビジョニングする](azure-sdk-example-storage.md)
- [例:データベースをプロビジョニングしてクエリを実行する](azure-sdk-example-database.md)
- [例:仮想マシンをプロビジョニングする](azure-sdk-example-virtual-machines.md)
