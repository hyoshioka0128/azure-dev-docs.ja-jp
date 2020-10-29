---
title: Node.js を使った Azure サービス プリンシパルの作成
description: Azure で Node.js および JavaScript を使用してサービス プリンシパルを認証する方法について説明します
ms.topic: how-to
ms.date: 09/30/2020
ms.custom: devx-track-js
ms.openlocfilehash: 88724ca9ecdb24088e437a5b9b407fbdde69650a
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437279"
---
# <a name="create-an-azure-service-principal-for-nodejs"></a>Node.js の Azure サービス プリンシパルを作成する

リソースへのアクセスを必要とするアプリには、ID を設定し、アプリをそれ自体の資格情報で認証できます。 この ID は、" *サービス プリンシパル* " と呼ばれます。 ユーザーの介入やユーザー名/パスワードの入力を求めるのではなく、SDK に与える Azure Active Directory アカウントのキーを認証用に作成するのが基本的な原理となります。

サービス プリンシパルを使ったアプローチによって次のことが可能となります。
- ユーザー自身のアクセス許可とは異なるアクセス許可を、アプリケーション ID に割り当てることができます。 通常、こうしたアクセス許可は、アプリが行う必要があることに制限されます。
- 無人スクリプトを実行するときに、証明書を使用して認証できます。

このトピックでは、サービス プリンシパルを作成するための 3 つの手法を紹介します。

- Azure portal
- Azure CLI 2.0

[!INCLUDE [chrome-note](../includes/chrome-note.md)]

## <a name="create-a-service-principal-using-the-azure-portal"></a>Azure Portal を使用してサービス プリンシパルを作成する

サービス プリンシパルの作成については、「[リソースにアクセスできる Azure Active Directory アプリケーションとサービス プリンシパルをポータルで作成する](/azure/active-directory/develop/howto-create-service-principal-portal)」のトピックで説明されている手順を参考にしてください。

## <a name="create-a-service-principal-using-the-azure-cli-20"></a>Azure CLI 2.0 を使用してサービス プリンシパルを作成する

[Azure CLI 2.0](/cli/azure/install-az-cli2) を使ってサービス プリンシパルを作成するには、次の手順に従います。

1. [Azure CLI 2.0](/cli/azure/install-az-cli2) をダウンロードします。

2. ターミナル ウィンドウを開き、コマンド `az login` を入力して、ログイン プロセスを開始します。

3. `az login` を呼び出すと、URL とコードが返されます。 指定された URL にブラウザーでアクセスし、コードを入力して、自分の Azure ID でログインします (既にログインしている場合は自動的に実行されます)。 その後は CLI から自分のアカウントにアクセスできるようになります。

4. `az account list` コマンドを使用して、ササブスクリプションとテナントの ID を取得します。 これらは Azure パッケージを使用するときに必要になります。 このコマンドの出力の例を次に示します。

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

5. 「[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)」のトピックで説明されている手順に従って、サービス プリンシパルを生成します。 出力の JSON オブジェクトには、Azure での認証に必要な情報が含まれています。


## <a name="using-the-service-principal"></a>サービス プリンシパルの使用

サービス プリンシパルを作成したら、「[JavaScript 用 Azure 管理モジュールを使用した認証](./node-sdk-azure-authenticate.md)」のトピックの、Azure Active Directory でクライアントを認証するために使用できる資格情報オブジェクトを作成する方法に従います。
