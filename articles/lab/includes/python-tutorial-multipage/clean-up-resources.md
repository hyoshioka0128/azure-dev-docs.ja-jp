---
title: インクルード ファイル azure-sign-in.md
description: インクルード ファイル azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: cc541400419bf3269a7bb31b36fab788271539f4
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405230"
---
前の手順では、リソース グループ内に Azure リソースを作成しました。 リソース グループには、お客様の場所に応じて "appsvc_rg_Linux_CentralUS" のような名前が付いています。 無料の F1 レベル以外の App Service SKU を使用する場合は、これらのリソースによって継続的なコストが発生します (「[App Service の価格](https://azure.microsoft.com/pricing/details/app-service/linux/)」を参照してください)。

今後これらのリソースが不要である場合は、次のコマンドを実行してリソース グループを削除します。

```azurecli
az group delete --no-wait
```

このコマンドでは、 *azure/config* ファイルにキャッシュされているリソース グループ名を使用します。

`--no-wait` 引数を使用すると、操作が完了する前にコマンドから戻ることができます。