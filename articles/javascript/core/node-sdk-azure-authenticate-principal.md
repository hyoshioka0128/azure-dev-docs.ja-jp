---
title: Node.js を使った Azure サービス プリンシパルの作成
description: Azure で Node.js および JavaScript を使用してサービス プリンシパルを認証する方法について説明します
ms.topic: how-to
ms.date: 11/05/2020
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: dc9b82ad8e664beeeedaa0002f76457bb01793e3
ms.sourcegitcommit: c1ef7aa8ed2e88e98b190e42cffde52cf301958d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "97034503"
---
# <a name="create-an-azure-service-principal-for-nodejs"></a>Node.js の Azure サービス プリンシパルを作成する

リソースへのアクセスを必要とするアプリには、ID を設定し、アプリをそれ自体の資格情報で認証できます。 この ID は、"*サービス プリンシパル*" と呼ばれます。 ユーザーの介入やユーザー名/パスワードの入力を求めるのではなく、SDK に与える Azure Active Directory アカウントのキーを認証用に作成するのが基本的な原理となります。

サービス プリンシパルを使ったアプローチによって次のことが可能となります。
- ユーザー自身のアクセス許可とは異なるアクセス許可を、アプリケーション ID に割り当てることができます。 通常、こうしたアクセス許可は、アプリが行う必要があることに制限されます。
- 無人スクリプトを実行するときに、証明書を使用して認証できます。

このトピックでは、サービス プリンシパルを作成するための 3 つの手法を紹介します。

- Azure portal
- Azure CLI 2.0

## <a name="create-a-service-principal-using-the-azure-cli-20"></a>Azure CLI 2.0 を使用してサービス プリンシパルを作成する

次のステップを実行するには、[Azure CLI をインストール](/cli/azure/install-azure-cli)し、[Azure にサインイン](/cli/azure/authenticate-azure-cli)します。 

1. `az account list` コマンドを使用して、ササブスクリプションとテナントの ID を取得します。 これらは Azure パッケージを使用するときに必要になります。 このコマンドの出力の例を次に示します。

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

1. 「[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)」のトピックで説明されている手順に従って、サービス プリンシパルを生成します。 出力の JSON オブジェクトには、Azure での認証に必要な情報が含まれています。

## <a name="using-the-service-principal"></a>サービス プリンシパルの使用

サービス プリンシパルがあれば、次のことができます。

1. 証明書、環境変数、または `.json` ファイルを使用して、サービス プリンシパルをプログラムで Azure に対して認証します。 
1. サービス プリンシパルを使用して Azure リソースを作成し、サービスを使用します。

「[JavaScript 用 Azure 管理モジュールを使用した認証](./node-sdk-azure-authenticate.md)」のトピックの、Azure Active Directory でクライアントを認証するために使用できる資格情報オブジェクトを作成する方法に従います。
