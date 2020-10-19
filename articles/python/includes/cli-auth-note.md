---
ms.openlocfilehash: f644d66895b7b8bcce345cd42c6efedfaddb4240
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764479"
---
このコードは、Azure CLI で直接認証を行う場合の操作をデモンストレーションしているため、(`AzureCliCredential` を使用した) CLI ベースの認証方法を使用しています。 どちらの場合も、同じ ID を認証に使用することになります。

VM の管理を自動化するなど、そのようなコードを本番スクリプトで使用する場合は、`DefaultAzureCredential` (推奨) またはサービス プリンシパル ベースの方法を使用してください (「[Azure サービスを使用して Python アプリを認証する方法](../azure-sdk-authenticate.md)」を参照)。
