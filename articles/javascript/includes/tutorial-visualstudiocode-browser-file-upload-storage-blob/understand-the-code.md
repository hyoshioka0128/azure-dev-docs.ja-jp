---
ms.topic: include
ms.date: 10/13/2020
ms.custom: devx-track-javascript
title: インクルード ファイル understand-the-code.md
description: インクルード ファイル understand-the-code.md
ms.openlocfilehash: fd9d2eb2c6c8dd3a39d737cdcdf609b9cf04cf7e
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344240"
---
チュートリアルのこのセクションでは、サンプルのコードを理解して、Web アプリでファイルをアップロードします。

## <a name="sample-created-with-create-react-app"></a>create-react-app を使用して作成されたサンプル

[サンプル](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)は、TypeScript テンプレートで [create-react-app](https://create-react-app.dev/docs/adding-typescript/) を使用して作成された基本的な React アプリです。

```typescript
npx create-react-app my-app --template typescript
```

create-react-app フレームワークは、次のようなことに役立ちます。
* ブラウザーで Azure クライアント ライブラリを使用するための要件であるファイル バンドルを自動的に提供する
* トランスパイルとビルドのスクリプトを提供する 

## <a name="upload-button-functionality"></a>[アップロード] ボタンの機能

`src/app` ファイルは、create-react-app を使用したそのアプリ作成の一部として提供されています。 このファイルは、[ファイルの選択] ボタンと [アップロード] ボタン、およびその機能を提供するためのサポート コードを提供するように変更されています。 

Azure BLOB ストレージ コードに接続するコードは強調表示されています。 `uploadFileToBlob` を呼び出すと、コンテナー内のすべての BLOB (ファイル) がフラット リストとして返されます。 そのリストは、`DisplayImagesFromContainer` 関数を使用して表示されます。

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/App.tsx" highlight="3,28":::

## <a name="upload-file-to-azure-storage-blob-with-azure-sdk-client-library"></a>Azure SDK クライアント ライブラリを使用して Azure Storage BLOB にファイルをアップロードする

ファイルを Azure Storage にアップロードするコードは、フレームワークに依存しません。 コードはチュートリアル用に構築されているため、単純でわかりやすくなるように選択が行われました。 これらの選択について説明しています。意図的な使用、セキュリティ、および効率性については、独自のプロジェクトを確認してください。 

このサンプルでは、パブリックにアクセスできるコンテナーとファイルを作成して使用します。 独自のプロジェクト内のファイルをセキュリティで保護する場合は、リソースへの全体的な認証を要求することから、各 BLOB オブジェクトに対する非常に限定的なアクセス許可までを制御できる多くのレイヤーがあります。 

### <a name="dependencies-and-variables"></a>依存関係と変数

`uploadToBlob.ts` ファイルは、依存関係を読み込み、環境変数またはハードコーディングされた文字列によって、必要な変数を取り込みます。

| 変数 | 説明 |
|--|--|
|`sasToken`|Azure portal で作成された SAS トークンには、`?` が先頭に付加されます。 `sasToken` 変数に設定する前に、それを削除してください。| 
|`container`|BLOB ストレージ内のコンテナーの名前。 これは、ファイル システムのフォルダーやディレクトリに相当するものと考えることができます。|
|`storageAccountName`|対象のリソース名。|

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="2,5,16" id="snippet_package":::

### <a name="security-for-azure-credentials"></a>Azure 資格情報のセキュリティ

独自のプロジェクト内で、SAS トークンなどのシークレットを格納する場所を検討してください。 Azure 情報をセキュリティで保護することがアプリケーションによって要求される場合は、このストレージ コードを、クライアント上ではなく [Azure 関数](/azure/azure-functions/)でホストし、React アプリからその Azure 関数を呼び出すことを検討してください。  

### <a name="create-storage-client-and-manage-steps"></a>ストレージ クライアントを作成し、手順を管理する

`uploadFileToBlob` 関数は、ファイルの main 関数です。 これにより、Storage サービスのクライアント オブジェクトが作成され、コンテナー オブジェクトに対してクライアントが作成され、ファイルがアップロードされ、コンテナー内のすべての BLOB の一覧が取得されます。 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="5,6,7" id="snippet_uploadFileToBlob":::

### <a name="upload-file-to-blob"></a>ファイルを BLOB にアップロードする

`createBlobInContainer` 関数は、`uploadBrowserData` クライアント ライブラリ メソッドを使用して、ファイルをコンテナーにアップロードします。 画像の表示など、ファイルの種類に依存するブラウザーの機能を使用する場合は、要求と共にコンテンツの種類を送信する必要があります。 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="10" id="snippet_createBlobInContainer":::

### <a name="get-list-of-blobs"></a>BLOB の一覧を取得する

`getBlobsInContainer` 関数は、コンテナー内の BLOB の URL の一覧を取得します。 これらの URL は、HTML のイメージ表示の `src` として使用されるように構成されています: `<img src={item} alt={item} height="200" />`。 

:::code language="typescript" source="~/../js-e2e-browser-file-upload-storage-blob/src/uploadToBlob.ts" highlight="10" id="snippet_getBlobsInContainer":::

