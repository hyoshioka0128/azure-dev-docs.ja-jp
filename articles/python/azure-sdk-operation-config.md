---
title: 操作の構成用のパラメーター - Azure SDK for Python
description: Azure SDK for Python によってスローされる C
ms.date: 03/07/2018
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: b0aaaf5bcd51bce42ab38830d5dbce508db226b1
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441697"
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
