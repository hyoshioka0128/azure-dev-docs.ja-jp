---
title: Azure SDK for JavaScript を使用したログ
description: Azure SDK for JavaScript クライアント ライブラリを使用してログを有効にする方法について説明します
ms.topic: article
ms.date: 07/23/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: fb4be05552a5ed1ea239776985f4f35ca6e900fa
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218844"
---
# <a name="logging-with-the-azure-sdk-for-javascript"></a>Azure SDK for JavaScript を使用したログ

Azure SDK for JavaScript コア ライブラリには、アプリケーションのログを簡単に有効にできるロガー ライブラリが含まれています。 

[NPM パッケージ](https://www.npmjs.com/package/@azure/logger) | [ライブラリ ソース コード](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/core/logger)

## <a name="enable-logging"></a>ログの有効化

1 つの環境変数を使用してアプリケーション全体でログを有効にすることも、アプリケーションの一部に対してログを動的に構成することもできます。 この記事では、ロガー パッケージに関する主な概念と、次の手順でログを有効にする方法について説明します。

- `AZURE_LOG_LEVEL` 環境変数を設定します。
- ロガー ライブラリからインポートされた `setLogLevel` を呼び出します。
- 特定のロガーで `enable()` を呼び出します。

> [!NOTE]
> この記事は、Azure SDK の最新バージョンを使用するクライアント ライブラリに適用されます。 ライブラリがサポートされているかどうかを確認するには、「[Azure SDK の最新リリース](https://azure.github.io/azure-sdk/releases/latest/index.html#javascript)」の一覧を参照してください。 Azure SDK クライアント ライブラリの古いバージョンをアプリケーションで使用している場合は、該当するサービスのドキュメントで具体的な手順を参照してください。

## <a name="log-levels"></a>ログ レベル

`@azure/logger` パッケージでは、詳細度が最も高いものから最も低いものの順に指定された、次のログ レベルがサポートされています。

- verbose
- info
- warning
- error

プログラムで、または `AZURE_LOG_LEVEL` 環境変数を使用してログ レベルを設定すると、選択したログ レベル以下のレベルで書き込まれたログが出力されます。 たとえば、ログ レベルを `warning` に設定すると、レベルが `warning` または `error` のすべてのログが出力されます。

## <a name="install-the-logger-package"></a>ロガー パッケージをインストールする

Azure SDK for JavaScript ロガー ライブラリは [npm](https://www.npmjs.com/) パッケージとして提供されます。 npm を使用して、`@azure/logger` パッケージをインストールします。

```cmd
npm install @azure/logger
```

## <a name="set-the-logging-environment-variable"></a>ログ環境変数を設定する

1 つの `AZURE_LOG_LEVEL` 環境変数を使用して、アプリケーション全体でログを有効にすることができます。 ログは stderr に出力されます。 この環境変数を設定した後に、アプリケーションを再起動してログの生成を開始します。

この bash の例では、ログ レベルを verbose に設定しています。

```bash
export AZURE_LOG_LEVEL="verbose"
```

この例は、PowerShell を使用しています。

```powershell
$env:AZURE_LOG_LEVEL="verbose"
```

この例は、CMD を使用しています。

```cmd
set AZURE_LOG_LEVEL="verbose"
```

## <a name="configure-dynamic-logging"></a>動的なログを構成する

Azure SDK for JavaScript を使用すると、必要に応じて、または特定のクライアント ライブラリに対して、ログを動的に有効にすることができます。 これは、一部のアプリケーション コンポーネントにカスタム ログの実装を使用したい、またはデバッグのために一時ログが必要な開発者を考慮しています。

 `@azure/logger` モジュールを使用して、コード内にログ レベルを設定できます。

```js
import { setLogLevel } from "@azure/logger";

setLogLevel("verbose");
```

特定のログ チャネルを有効にするには、ログの出力対象のパッケージから `logger` をインポートします。 次の例では、Event Hubs の info ログのみを有効にします。

```js
import { logger } from "@azure/event-hubs";

logger.info.enable();
```

## <a name="redirect-log-output"></a>ログ出力をリダイレクトする

ログ メッセージを自分で処理するには、`AzureLogger` またはクライアント ライブラリからインポートされたロガーに `log` メソッドを再割り当てします。

この例では、ログ メッセージを stderr にリダイレクトします。

```js
import { AzureLogger } from "@azure/logger";

AzureLogger.log = msg => console.error(msg);
```

この例では、Azure EventHub からの info ログ メッセージのみをリダイレクトします。

```js
import { logger } from "@azure/event-hubs";
logger.info.log = msg => console.error(msg);
```

## <a name="next-steps"></a>次の手順

- [Azure App Service のアプリの診断ログの有効化](/azure/app-service/troubleshoot-diagnostic-logs)
- [Azure のセキュリティ ログと監査のオプションを確認する](/azure/security/fundamentals/log-audit)
- [Azure プラットフォーム ログを操作する方法を確認する](/azure/azure-monitor/platform/platform-logs-overview)