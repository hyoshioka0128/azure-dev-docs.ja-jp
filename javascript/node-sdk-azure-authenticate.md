---
title: Node.js 用 Azure 管理モジュールを使った認証
description: Node.js 用 Azure 管理モジュールへの認証にサービス プリンシパルを使う方法について説明します。
ms.topic: article
ms.date: 06/17/2017
ms.openlocfilehash: 76233fb6e6d15829783ff98b3af672abc7eba226
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "75010516"
---
# <a name="authenticate-with-the-azure-modules-for-nodejs"></a>Node.js 用 Azure モジュールを使った認証

すべてのサービス API は、インスタンス化する際に、`credentials` オブジェクトを介して認証を行う必要があります。 必要な資格情報を Azure SDK for Node.js で認証したり作成したりするには、次の 3 とおりの方法があります。 

- [基本認証]
- 対話型ログイン
- サービス プリンシパルの認証

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="basic-authentication"></a>[基本認証]

Azure アカウントの資格情報を使ってプログラムで認証するには、`loginWithUsernamePassword` 関数を使用します。 次の JavaScript コード スニペットは、環境変数として格納された資格情報を使った基本認証の方法を示したものです。 

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.loginWithUsernamePassword(process.env.AZURE_USER, 
                                 process.env.AZURE_PASS, 
                                 (err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, 
                                                             '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="interactive-login"></a>対話型ログイン

対話型ログインでは、ユーザーがブラウザーから認証を行うためのリンクとコードが提供されます。 この方法は、同じスクリプトで複数のアカウントが使用される場合や、ユーザーが介入した方が望ましい状況で使用します。

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.interactiveLogin((err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="service-principal-authentication"></a>サービス プリンシパルの認証

認証方法としては、[対話型ログイン](#interactive-login)が最も簡単です。 ただし、Node.js SDK を使うときは、アカウントの資格情報を提供するよりも、サービス プリンシパル認証を使った方がよい場合もあります。 サービス プリンシパルの作成 (および使用) に関するさまざまな手法については、「[Node.js を使った Azure サービス プリンシパルの作成](./node-sdk-azure-authenticate-principal.md)」というトピックで説明しています。 
