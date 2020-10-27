---
title: インクルード ファイル create-storage-resource.md
description: インクルード ファイル create-storage-resource.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: a4eb48afd578de7ddc3426a3907b8b4192387cbb
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344247"
---
チュートリアルのこのセクションでは、Visual Studio 拡張機能を使用して Azure Storage リソースを作成し、Azure portal でリソースを構成します。 

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[!INCLUDE [azure-sign-in](azure-sign-in.md)]

## <a name="create-storage-resource"></a>Storage リソースを作成する 

Visual Studio Code 拡張機能を使って Storage リソースを作成します。 

1. Azure Storage 拡張機能に移動します。 サブスクリプションを右クリックし、[`Create Storage Account...`] を選択します。

    :::image type="content" source="../../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource.png" alt-text="Azure Storage 拡張機能に移動します。サブスクリプションを右クリックし、[ストレージ アカウントの作成] を選択します。":::

1. 値がどのように使用されるかを理解するために次の表を使用しながら、プロンプトに従います。

    |プロパティ|値|
    |--|--|
    |新しい Web アプリのグローバルに一意の名前を入力します。| Storage リソース名として、`fileuploadyourname` などの値を入力します。 `yourname` を自分の名前 (小文字) または一意の ID に置き換えます。 この一意の名前は、ブラウザーでリソースにアクセスするための URL の一部としても使用されます。 文字と数字のみを使用します。長さは 24 文字までです。 この **アカウント名** は、後で使用するために必要です。|

1. アプリの作成プロセスが完了すると、新しいリソースに関する情報を含む通知が表示されます。 

    :::image type="content" source="../../media/tutorial-browser-file-upload/visualstudiocode-storage-extension-create-resource-complete.png" alt-text="Azure Storage 拡張機能に移動します。サブスクリプションを右クリックし、[ストレージ アカウントの作成] を選択します。":::

## <a name="set-storage-account-name-in-code-file"></a>コード ファイルにストレージ アカウント名を設定する

`src/uploadToBlob.ts` で、ストレージ キー名を空の文字列に追加して、`storageAccountName` 値にリソース名を設定します。 コードの残りの部分はそのままにします。 

```typescript
const storageAccountName = process.env.storageresourcename || ""; 
```

## <a name="generate-your-shared-access-signature-sas-token"></a>Shared Access Signature (SAS) トークンを生成する 

CORS を構成する前に、SAS トークンを生成してください。 

1. Storage 用の Visual Studio Code 拡張機能で、リソースを右クリックし、 **[ポータルで開く]** を選択します。 これにより、正しい Storage リソースが Azure portal で開きます。
1. **[設定]** セクションで、 **[Shared Access Signature]** を選択します。 
1. 次の設定を使用して、SAS トークンを構成します。 

    | プロパティ|値|
    |--|--|
    |使用できるサービス|BLOB|
    |許可されるリソースの種類|サービス、コンテナー、オブジェクト|
    |許可されるアクセス許可|読み取り、書き込み、削除、一覧表示、追加、作成|
    |バージョンの削除を有効にする|オン|
    |開始日時と終了日時|開始日時を受け入れ、終了日時を 24 時間後に設定します。 SAS トークンは 24 時間のみ有効です。|
    |HTTPS のみ|オン|
    |優先ルーティング レベル|Basic|
    |署名キー|key1 が選択済み|

    :::image type="content" source="../../media/tutorial-browser-file-upload/azure-portal-storage-blob-generate-sas-token.png" alt-text="Azure Storage 拡張機能に移動します。サブスクリプションを右クリックし、[ストレージ アカウントの作成] を選択します。":::

1.  **[SAS と接続文字列を生成する]** を選択します。 SAS トークンを直ちにコピーします。 このトークンを一覧表示することはできません。したがって、コピーしていない場合は、新しい SAS トークンの生成が必要になります。 

## <a name="set-sas-token-in-code-file"></a>コード ファイルに SAS トークンを設定する

SAS トークンの値は、クエリ文字列の一部であり、クラウドベースのリソースに対してクエリを実行するときに URL で使用されます。

トークンの形式は、作成に使用したツールによって異なります。 
* **Azure ポータル** :ポータルで SAS トークンを作成すると、トークンには文字列の最初の文字として `?` が含まれます。
* **Azure CLI** :Azure CLI を使用して SAS トークンを作成した場合、返された値には文字列の最初の文字として `?` は含まれません。 

1. `?` がトークンの最初の文字である場合は、それを削除します。 `?` は、コード ファイルによって自動的に提供されるため、トークン内には必要ありません。

1. `src/uploadToBlob.ts` で、SAS トークンを空の文字列に追加して、sasToken 値に対して SAS トークンを設定します。 コードの残りの部分はそのままにします。 

```typescript
// remove `?` if it is first character of token
const sasToken = process.env.storagesastoken || "";
```

## <a name="configure-your-azure-storage-resource-for-cors-with-azure-cli"></a>Azure CLI を使用して Azure Storage リソースを CORS 用に構成する

クライアント側の React コードがストレージ アカウントにアクセスできるように、リソースに対して CORS を構成します。 

1. 引き続き Azure portal から、 **[設定]** セクションで **[CORS]** を選択します。 
1. 図に示されているように CORS を構成します。 この設定の説明は、図の下にあります。 

    | プロパティ|値|
    |--|--|
    |許可されるオリジン|`*`|
    |許可されたメソッド|パッチを除くすべて。|
    |許可されるヘッダー|`*`|
    |公開されるヘッダー|`*`|
    |最長有効期間|86400|

    :::image type="content" source="../../media/tutorial-browser-file-upload/azure-portal-storage-blob-cors.png" alt-text="Azure Storage 拡張機能に移動します。サブスクリプションを右クリックし、[ストレージ アカウントの作成] を選択します。":::

1. 設定の上の **[保存]** を選択すると、リソースに保存されます。 これらの CORS 設定に合わせるために、コードを変更する必要はありません。 

## <a name="run-project-locally-to-verify-connection-to-storage-account"></a>ストレージ アカウントへの接続を確認するためにプロジェクトをローカルで実行する

SAS トークンとストレージ アカウント名を `src/uploadToBlob.ts` ファイルに設定したので、アプリケーションを実行する準備ができました。

1. Visual Studio Code ターミナルで、次のコマンドを入力します。

    ```javascript
    npm start
    ```

1. ターミナルに `http://localhost:3000` などの URL が表示されたら、アプリの準備ができています。 ブラウザーを開き、その URL を入力します。 Azure Storage BLOB に接続されている Web サイトには、[ファイルの選択] ボタンと [ファイルのアップロード] ボタンが表示されます。 

    :::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-configured-upload-button-displayed.png" alt-text="Azure Storage 拡張機能に移動します。サブスクリプションを右クリックし、[ストレージ アカウントの作成] を選択します。":::

1. `images` フォルダーから、アップロードするイメージを選択します。 `spring-flowers.jpg` は、このテストに適したビジュアルです。 **[アップロード]** ボタンを選択します。 を追加します。 

    React フロントエンド クライアント コードによって、`src/uploadToBlob.ts` が呼び出されて Azure に対する認証が行われ、ストレージ コンテナーが作成され (まだ存在しない場合)、そのコンテナーに BLOB がアップロードされます。 

## <a name="want-to-know-more"></a>詳細について 

ストレージ アカウントを構成するその他の方法は次のとおりです。
* [PowerShell](/azure/powershell/module/azure.storage/new-azurestorageblobsastoken) を使用した SAS トークン
* ポータルを使用した SAS トークン
* [PowerShell](/azure/powershell/module/azure.storage/set-azurestoragecorsrule) を使用した CORS
* ポータルを使用した CORS

[Shared Access Signatures](/azure/storage/common/storage-sas-overview.md) の詳細について参照してください。