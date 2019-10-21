---
title: 操作の構成用のパラメーター - Azure SDK for Python
description: Azure SDK for Python によってスローされる C
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.custom: seo-python-october2019
ms.openlocfilehash: 0730cec8470a3b55421c6c0cafa08f88819cb1d8
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72279079"
---
# <a name="parameters-for-operation-configuration"></a>操作の構成用のパラメーター

Azure SDK for Python では、操作のメソッドに追加のパラメーターを指定できます。

追加のパラメーターは `kwargs` で提供されています。 この機能は *operation_config* と呼ばれます。

操作の構成のオプションは次のとおりです。

|パラメーター名|種類|Role|
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
