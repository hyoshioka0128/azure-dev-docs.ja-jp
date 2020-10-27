---
title: インクルード ファイル tutorial-azure-web-app-mongodb-00.md
description: インクルード ファイル tutorial-azure-web-app-mongodb-00.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: b9ab409c845b7690474eb47117df4401ecaf59cd
ms.sourcegitcommit: ced8331ba36b28e6e2eacd23a64b39ddc7ffe6ab
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344232"
---
このチュートリアルでは、React アプリを使用して、ファイルを Azure Storage BLOB にアップロードします。 

プログラミング作業は完了しており、このチュートリアルでは、Azure 拡張機能を使用して Visual Studio Code 内部からローカルおよびリモートの Azure 環境を正常に使用することに焦点を当てています。

## <a name="top-tasks"></a>上位のタスク

このチュートリアルには、JavaScript 開発者向けの **上位の Azure タスク** がいくつか含まれています。

* Visual Studio Code で React アプリをローカルで実行する
* Storage リソースを作成し、ファイル アップロード用に構成する
    * CORS を構成する
    * Shared Access Signature (SAS) トークンを作成する
* サービスに対する認証に SAS トークンを使用するために、Azure SDK クライアント ライブラリ用にコードを構成する

## <a name="sample-application"></a>サンプル アプリケーション

[GitHub で入手できる](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)サンプル React アプリは、次の要素で構成されています。

* ポート 3000 でホストされている **React アプリ**
* Storage BLOB にアップロードするための Azure SDK クライアント ライブラリ スクリプト

:::image type="content" source="../../media/tutorial-browser-file-upload/browser-react-app-azure-storage-resource-image-uploaded-displayed.png" alt-text="Azure Storage BLOB に接続された単純な React アプリ。":::
