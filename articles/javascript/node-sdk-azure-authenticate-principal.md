---
title: Node.js を使った Azure サービス プリンシパルの作成
description: Azure で Node.js および JavaScript を使用してサービス プリンシパルを認証する方法について説明します
ms.topic: article
ms.date: 06/17/2017
ms.custom: devx-track-javascript
ms.openlocfilehash: 156892d9fd8e8014e3dacaae2492126ac9bf5836
ms.sourcegitcommit: b03cb337db8a35e6e62b063c347891e44a8a5a13
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2020
ms.locfileid: "91110435"
---
# <a name="create-an-azure-service-principal-for-nodejs"></a>Node.js の Azure サービス プリンシパルを作成する

リソースへのアクセスを必要とするアプリには、ID を設定し、アプリをそれ自体の資格情報で認証できます。 この ID は、"*サービス プリンシパル*" と呼ばれます。 ユーザーの介入やユーザー名/パスワードの入力を求めるのではなく、SDK に与える Azure Active Directory アカウントのキーを認証用に作成するのが基本的な原理となります。

サービス プリンシパルを使ったアプローチによって次のことが可能となります。
- ユーザー自身のアクセス許可とは異なるアクセス許可を、アプリケーション ID に割り当てることができます。 通常、こうしたアクセス許可は、アプリが行う必要があることに制限されます。
- 無人スクリプトを実行するときに、証明書を使用して認証できます。

このトピックでは、サービス プリンシパルを作成するための 3 つの手法を紹介します。

- Azure portal
- Azure CLI 2.0
- Azure SDK for Node.js

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="create-a-service-principal-using-the-azure-portal"></a>Azure Portal を使用してサービス プリンシパルを作成する

サービス プリンシパルの作成については、「[リソースにアクセスできる Azure Active Directory アプリケーションとサービス プリンシパルをポータルで作成する](/azure/active-directory/develop/howto-create-service-principal-portal)」のトピックで説明されている手順を参考にしてください。

## <a name="create-a-service-principal-using-the-azure-cli-20"></a>Azure CLI 2.0 を使用してサービス プリンシパルを作成する

[Azure CLI 2.0](/cli/azure/install-az-cli2) を使ってサービス プリンシパルを作成するには、次の手順に従います。

1. [Azure CLI 2.0](/cli/azure/install-az-cli2) をダウンロードします。

2. ターミナル ウィンドウを開きます。

3. 次のコマンドを入力してログイン プロセスを開始します。

    ```shell
    $ az login
    ```

4. `az login` を呼び出すと、URL とコードが返されます。 指定された URL にブラウザーでアクセスし、コードを入力して、自分の Azure ID でログインします (既にログインしている場合は自動的に実行されます)。
その後は CLI から自分のアカウントにアクセスできるようになります。

5. サブスクリプションとテナントの ID を取得します。

    ```shell
    $ az account list
    ```

    出力例を次に示します。

    ```shell
    {
    "cloudName": "AzureCloud",
    "id": "<subscriptionId>",
    "isDefault": true,
    "name": "<subscriptionName>",
    "registeredProviders": [],
    "state": "Enabled",
    "tenantId": "<tenantId>",
        "user": {
            "name": "hello@example.com",
            "type": "user"
        }
    }
    ```

    **サブスクリプション ID を書き留めてください。手順 7. で必要になります。**

6. サービス プリンシパルを作成すると、Azure への認証に必要な他の情報を含んだ JSON オブジェクトが得られます。

    ```shell
    $ az ad sp create-for-rbac
    ```

    出力例を次に示します。

    ```shell
    {
    "appId": "<appId>",
    "displayName": "<displayName>",
    "name": "<name>",
    "password": "<password>",
    "tenant": "<tenant>"
    }
    ```

    **テナント、名前、パスワードの値を書き留めてください。手順 7. で必要になります。**

7. 環境変数を設定します。&lt;subscriptionId>、&lt;tenant>、&lt;name>、&lt;password> の各プレースホルダーには、手順 4. と手順 5. で得た値を指定してください。

    **bash の使用**

    ```shell
    export azureSubId='<subscriptionId>'
    export azureServicePrincipalTenantId='<tenant>'
    export azureServicePrincipalClientId='<name>'
    export azureServicePrincipalPassword='<password>'
    ```

    **PowerShell の使用**

    ```shell
    $env:azureSubId='<subscriptionId>'
    $env:azureServicePrincipalTenantId='<tenant>'
    $env:azureServicePrincipalClientId='<name>'
    $env:azureServicePrincipalPassword='<password>'
    ```

## <a name="create-a-service-principal-using-the-azure-sdk-for-nodejs"></a>Azure SDK for Node.js を使用してサービス プリンシパルを作成する

JavaScript を使ってプログラムでサービス プリンシパルを作成するには、[ServicePrincipal スクリプト](https://github.com/Azure/azure-sdk-for-node/tree/master/Documentation/ServicePrincipal)を使用します。

## <a name="using-the-service-principal"></a>サービス プリンシパルの使用

次の JavaScript コード スニペットは、サービス プリンシパルの作成後、Azure SDK for Node.js でサービス プリンシパル キーを使って認証を行う方法を示したものです。 &lt;clientId or appId>、&lt;secret or password>、&lt;domain or tenant> の各プレースホルダーには、実際の値を指定してください。

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.loginWithServicePrincipalSecret(
  <clientId or appId>,
  <secret or password>,
  <domain or tenant>,
  (err, credentials) => {
    if (err) throw err

    let storageClient = Azure.createARMStorageManagementClient(credentials, '<azure-subscription-id>');

    // ..use the client instance to manage service resources.
  }
);
```

## <a name="next-steps"></a>次の手順

* [Node.js 用 Azure モジュールを使った認証](node-sdk-azure-authenticate.md)