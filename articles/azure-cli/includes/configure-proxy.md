---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 05/28/2019
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 676f33377a4e7122941bc789c51465b7f34aa1d3
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030884"
---
プロキシのために外部リソースに接続できない場合、シェルに `HTTP_PROXY` および `HTTPS_PROXY` 変数を正しく設定していることを確認してください。 これらのプロキシ用にどのホストとポートを使用するかについては、システム管理者に問い合わせる必要があります。

これらの値は、多くの Linux プログラムで使用されます (インストール プロセスで使用されるものも含む)。 これらの値を設定するには、次の手順を実行します。

```bash
# No auth
export HTTP_PROXY=http://[proxy]:[port]
export HTTPS_PROXY=https://[proxy]:[port]

# Basic auth
export HTTP_PROXY=http://[username]:[password]@[proxy]:[port]
export HTTPS_PROXY=https://[username]:[password]@[proxy]:[port]
```

> [!IMPORTANT]
> プロキシの内側にいる場合は、これらのシェル変数を CLI によって Azure サービスに接続するように設定する必要があります。
> 基本認証を使用しない場合は、`.bashrc` ファイルでこれらの変数をエクスポートすることをお勧めします。
> 常に、貴社のビジネスのセキュリティ ポリシーと、システム管理者の要件に従ってください。
