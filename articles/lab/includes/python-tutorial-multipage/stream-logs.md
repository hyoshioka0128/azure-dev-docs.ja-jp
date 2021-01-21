---
title: インクルード ファイル azure-sign-in.md
description: インクルード ファイル azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: b0da4ffd08f324bec7a404c21fb0e01d80228560
ms.sourcegitcommit: 593d177cfb5f56f236ea59389e43a984da30f104
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98566144"
---
アプリ、およびそれを実行するコンテナー内から生成されたコンソール ログに、アクセスすることができます。 ログには、`print` ステートメントを使って生成されたすべての出力が含まれます。

ログをストリーミングするには、[az webapp log tail](/cli/azure/webapp/log#az_webapp_log_tail) コマンドを実行します。

```azurecli
az webapp log tail
```

また、`az webapp up` コマンドに `--logs` パラメーターを指定して、デプロイ時にログ ストリームを自動的に開くこともできます。

ブラウザーでアプリを最新の情報に更新して、コンソール ログを生成します。これには、アプリに対する HTTP 要求に関するメッセージが含まれています。 すぐに出力が表示されない場合は、30 秒後に再試行してください。

`https://<app-name>.scm.azurewebsites.net/api/logs/docker` で、ブラウザーからログ ファイルを検査することもできます。

ログ ストリーミングを任意のタイミングで停止するには、ターミナルで **Ctrl** + **C** キーを押します。
