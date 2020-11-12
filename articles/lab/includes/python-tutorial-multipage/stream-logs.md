---
title: インクルード ファイル azure-sign-in.md
description: インクルード ファイル azure-sign-in.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 5868697895ad067065627cca99f604d531dcb173
ms.sourcegitcommit: 5f64710b2b0822e789c7f15acba5a3a257c033f9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93405308"
---
アプリ、およびそれを実行するコンテナー内から生成されたコンソール ログに、アクセスすることができます。 ログには、`print` ステートメントを使って生成されたすべての出力が含まれます。

ログをストリーミングするには、[az webapp log tail](/cli/azure/webapp/log?view=azure-cli-latest&preserve-view=true#az_webapp_log_tail) コマンドを実行します。

```azurecli
az webapp log tail
```

また、`az webapp up` コマンドに `--logs` パラメーターを指定して、デプロイ時にログ ストリームを自動的に開くこともできます。

ブラウザーでアプリを最新の情報に更新して、コンソール ログを生成します。これには、アプリに対する HTTP 要求に関するメッセージが含まれています。 すぐに出力が表示されない場合は、30 秒後に再試行してください。

`https://<app-name>.scm.azurewebsites.net/api/logs/docker` で、ブラウザーからログ ファイルを検査することもできます。

ログ ストリーミングを任意のタイミングで停止するには、ターミナルで **Ctrl** + **C** キーを押します。
