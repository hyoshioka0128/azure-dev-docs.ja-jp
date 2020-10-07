---
title: Node.js 用 Azure 管理モジュールを使った認証
description: Node.js 用 Azure 管理モジュールへの認証にサービス プリンシパルを使う方法について説明します。
ms.topic: how-to
ms.date: 06/17/2017
ms.custom: devx-track-js
ms.openlocfilehash: 150b00c4dbb21d0514d1d7c7d34813272bbf06e1
ms.sourcegitcommit: 717e32b68fc5f4c986f16b2790f4211967c0524b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91586120"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>JavaScript 用 Azure 管理モジュールを使用した認証

リソース管理を支援する Azure サービスの管理パッケージには、2 つのセットがあります。
- Azure SDK for Node.js
- Azure SDK for JavaScript

Azure SDK for Node.js は、Azure サービス用の古い一連の管理パッケージであり、次の特徴があります。 
- Node.js でのみ使用でき、ブラウザーでは使用できません
- 手動で記述された型宣言ファイルを使用して JavaScript で記述されます
- Azure SDK for JavaScript パッケージが優先されるため、アクティブな開発は行われておらず、非推奨となっています
- `azure-arm-` で始まるパッケージ名を持ちます
- Azure Active Directory を使用して認証するために、パッケージ内のクライアント クラスに渡すことができる資格情報を作成する [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) パッケージが必要です。
- https://github.com/Azure/azure-sdk-for-node リポジトリに存在します

Azure SDK for JavaScript は、Azure サービス用の新しい一連の管理パッケージであり、次の特徴があります。
- Node.js とブラウザーの両方で使用可能です
- TypeScript で記述され、JavaScript と TypeScript の両方のプロジェクトで使用できます
- アクティブな開発がなされており、Azure サービスによってリソース管理 API が更新されたときに更新プログラムを受信します
- `@azure/arm-` で始まるパッケージ名を持ちます
- Azure Active Directory を使用して認証するために、パッケージ内のクライアント クラスに渡すことができる資格情報を作成する [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) パッケージが必要です。 アプリケーションがブラウザーで実行されている場合は、代わりに [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth) を使用します。
- https://github.com/Azure/azure-sdk-for-js リポジトリに存在します

パッケージの 2 つのセットを区別する簡単な方法は、パッケージ名を確認することです。

すべてのサービス API は、インスタンス化する際に、`credentials` オブジェクトを介して認証を行う必要があります。 Azure SDK for Node.js と Azure SDK for JavaScript の両方で、パッケージに必要な資格情報を認証および作成する方法は複数あります。

一般的な方法を以下に示します。

- ユーザー名とパスワードを使用する基本認証
- 対話型ログイン。最も簡単な認証方法ですが、ユーザー アカウントでログインする必要があります。
- サービス プリンシパル認証。 サービス プリンシパルの作成に関するさまざまな手法については、「[Node.js を使った Azure サービス プリンシパルの作成](./node-sdk-azure-authenticate-principal.md)」というトピックで説明しています。 

以下の各パッケージの readme には、資格情報オブジェクトを取得するさまざまな方法について詳しく記載されています。
- [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth): Node.js で Azure SDK for JavaScript の管理パッケージを使用する場合
- [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth): ブラウザーで Azure SDK for JavaScript の管理パッケージを使用する場合
- [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure): 以前の Azure SDK for Node.js で管理パッケージを使用する場合

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="next-steps"></a>次のステップ   

* [Visual Studio Code から静的 Web サイトを Azure にデプロイする](tutorial-vscode-static-website-node-01.md)