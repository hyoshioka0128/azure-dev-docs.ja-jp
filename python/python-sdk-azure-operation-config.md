---
title: Azure SDK for Python の操作の構成
description: Azure SDK for Python によってスローされる C
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.openlocfilehash: 9638aa4602f96e2da0155a7b3840e5be4857eb98
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2019
ms.locfileid: "68285513"
---
# <a name="operation-config"></a>操作の構成 

操作のメソッドには、`kwargs` で指定できる追加のパラメーターがあります。 これは operation_config と呼ばれます。

操作の構成のオプションは次のとおりです。

|パラメーター名|Type|Role|
|----------------------|------|---------------|
| 確認 |`bool`|SSL 証明書を確認するかどうか。 既定値は True です。|
|  cert |`str`| クライアント側で確認するためのローカル証明書へのパス。|
|  timeout |`int`| サーバー接続を確立するときのタイムアウト時間 (秒単位)。|
|  allow_redirects |`bool` | リダイレクトを許可するかどうか。|
|  max_redirects  |`int`| 許可されるリダイレクトの最大回数。|
|  proxies  |`dict` |プロキシ サーバーの設定。|
|  use_env_proxies |`bool` |ローカル環境変数からプロキシ設定を読み取るかどうか。|
|  retries  |`int` | 再試行回数の合計。|
|  enable_http_logger | `bool`| デバッグ モードで HTTP のログを有効にする (既定では False)。|
