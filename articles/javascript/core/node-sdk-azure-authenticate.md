---
title: Node.js 用 Azure 管理モジュールを使った認証
description: Node.js 用 Azure 管理モジュールへの認証にサービス プリンシパルを使う方法について説明します。
ms.topic: how-to
ms.date: 09/29/2020
ms.custom: devx-track-js
ms.openlocfilehash: 084a7154bf52314886c0dfb57db3e9c879c8e2c0
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437255"
---
# <a name="authenticate-with-the-azure-management-modules-for-javascript"></a>JavaScript 用 Azure 管理モジュールを使用した認証

すべての [SDK クライアント ライブラリ](../azure-sdk-library-package-index.md)は、インスタンスが生成される際に、`credentials` オブジェクトを介して認証を行う必要があります。 必要な資格情報を認証したり作成したりするには、複数の方法があります。

必要な資格情報を作成するための一般的な方法は次のとおりです。

- **サービス プリンシパル** 認証 ( _推奨される方法_ )。 [Azure サービス プリンシパルを作成する](node-sdk-azure-authenticate-principal.md)方法をご覧ください。 
- **対話型ログイン** 。最も簡単な認証方法ですが、ユーザー アカウントでのログインとブラウザーが必要です。
- **基本認証** (ユーザー名とパスワードを使用)。 これは、セキュリティ レベルが最も低い方法です。 

## <a name="samples"></a>サンプル

|認証パッケージ|認証スクリプトのサンプル|
|--|--|
|[@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) <br>(推奨)|[証明書を使用したサービス プリンシパル](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpCert.ts)<br>[ファイルからのサービス プリンシパル](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/authFileWithSpSecret.ts)<br>[対話](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/interactivePersonalAccount.ts)<br>[Basic](https://github.com/Azure/ms-rest-nodeauth/blob/master/samples/usernamePassword.ts)|
|[@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)<br>(推奨)|[ポップアップを使用した認証 (create-react-app)](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/authentication-with-popup)<br>[ポップアップを使用しない react](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/react-app)<br>[[ログイン] ボタンを使用した HTML](https://github.com/Azure/ms-rest-browserauth/tree/master/samples/vanilla)|
|[ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure)|[サービス プリンシパル](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#service-principal-authentication)<br>[対話](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#interactive-login)<br>[Basic](https://github.com/Azure/azure-sdk-for-node/blob/master/Documentation/Authentication.md#basic-authentication)|

[!INCLUDE [chrome-note](../includes/chrome-note.md)]

## <a name="next-steps"></a>次のステップ   

* [Visual Studio Code から静的 Web サイトを Azure にデプロイする](../tutorial-vscode-static-website-node-01.md)