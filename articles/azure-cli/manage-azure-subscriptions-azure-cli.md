---
title: Azure CLI を使用して Azure サブスクリプションを管理する
description: Azure CLI を使用して Azure サブスクリプションを管理します。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: dad217dff159baa39bd1361258fb308eea872564
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030904"
---
# <a name="use-multiple-azure-subscriptions"></a>複数の Azure サブスクリプションの使用

ほとんどの Azure ユーザーは、サブスクリプションを 1 つしか持っていません。 ただし、複数の組織に属している場合や、組織がグループ間で特定のリソースへのアクセスを分けている場合、Azure に複数のサブスクリプションを持っていることがあります。 CLI では、サブスクリプションをグローバルに選択することも、コマンドごとに選択することもできます。

サブスクリプション、請求、およびコスト管理の詳細については、[課金とコスト管理に関するドキュメント](/azure/billing/)をご覧ください。

## <a name="tenants-users-and-subscriptions"></a>テナント、ユーザー、サブスクリプション

Azure におけるテナント、ユーザー、サブスクリプションが混同されている場合があります。 "_テナント_" とは、組織全体を含む Azure Active Directory エンティティです。 このテナントは、少なくとも 1 つの "_サブスクリプション_" を持ち、少なくとも 1 人の "_ユーザー_" が含まれます。 ユーザーとは個人であり、1 つのテナント (ユーザーが属している組織) にのみ関連付けられています。 ユーザーは、Azure にサインインしてリソースを作成、管理、および使用するアカウントです。
1 人のユーザーが複数の "_サブスクリプション_" にアクセスできる場合があります。サブスクリプションとは、Azure をはじめとするクラウド サービスを使用するための Microsoft との契約です。 各リソースは、サブスクリプションに関連付けられています。

テナント、ユーザー、サブスクリプションの違いの詳細については、[Azure クラウド用語集](/azure/azure-glossary-cloud-terminology)のページをご覧ください。  Azure Active Directory テナントに新しいサブスクリプションを追加する方法については、「[Azure サブスクリプションを Azure Active Directory に追加する方法](/azure/active-directory/active-directory-how-subscriptions-associated-directory)」をご覧ください。
特定のテナントにサインインする方法を確認するには、[Azure CLI を使用したサインイン](/cli/azure/authenticate-azure-cli)に関するページをご覧ください。

## <a name="change-the-active-subscription"></a>アクティブなサブスクリプションを変更する

サブスクリプションのリソースにアクセスするには、アクティブなサブスクリプションを切り替えるか、または `--subscription` 引数を使用します。 すべてのコマンドについてサブスクリプションを切り替えるには、[az account set](/cli/azure/account#az-account-set) を使用します。

アクティブなサブスクリプションを切り替えるには、次の手順を実行します。

1. [az account list](/cli/azure/account#az-account-list) コマンドを使用して、サブスクリプションの一覧を取得します。

    ```azurecli-interactive
    az account list --output table
    ```
2. サブスクリプション ID または切り替え先の名前を指定して、`az account set` を実行します。

    ```azurecli-interactive
    az account set --subscription "My Demos"
    ```

異なるサブスクリプションで 1 つのコマンドのみを実行するには、`--subscription` 引数を使用します。 この引数には、サブスクリプション ID またはサブスクリプション名を指定します。

```azurecli-interactive
az vm create --subscription "My Demos" --resource-group MyGroup --name NewVM --image Ubuntu
```
