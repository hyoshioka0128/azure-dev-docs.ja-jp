---
title: Azure でのログ記録、メトリック、テレメトリ
description: Azure でのログ記録のオプションについて説明します
ms.topic: how-to
ms.date: 10/22/2020
ms.custom: devx-track-js
ms.openlocfilehash: 32abae7005cea4076c000ec92039df4d27a0ec10
ms.sourcegitcommit: 03240a3beece1407a140656d246e11856dc72ac7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/23/2020
ms.locfileid: "92493947"
---
# <a name="logging-metrics-and-telemetry-in-azure"></a>Azure でのログ記録、メトリック、テレメトリ 

Azure を使用する場合、ログ記録、メトリック、テレメトリにはいくつかのオプションがあります。 オプションを確認して、必要なツールまたはサービスを見つけてください。

* [リソース メトリック](#resource-metrics-provided-by-azure-services) - Azure サービスを使用する場合、Azure は個々のリソースを監視し、メトリックを収集します。  
* [カスタム メトリック](#custom-logging-to-azure) - アプリケーション (オンプレミス、クラウド、またはハイブリッド) で情報をログに記録する必要がある場合。
* [Azure SDK クライアント ライブラリ](#azure-sdk-client-library-logging) - Azure クライアント ライブラリに既に組み込まれているログを表示する必要がある場合。

## <a name="resource-metrics-provided-by-azure-services"></a>Azure サービスによって提供されるリソース メトリック

[Azure Monitor](/azure/azure-monitor/overview) は、クラウドおよびオンプレミス環境からテレメトリを収集、分析し、それに対応する包括的なソリューションを提供することで、アプリケーションやサービスの可用性とパフォーマンスを最大限に高めます。

メトリックは、Azure portal のリソース内で利用できます。 

:::image type="content" source="../media/logging-metrics/azure-resource-metrics-portal.png" alt-text="MongoDB データベースに接続された単純な Node.js アプリ。":::

データが監視されたら、Azure portal を使用して、一般的なクエリでデータを照会するか、[Kusto クエリ](/azure/data-explorer/kusto/query/)を作成します。 

## <a name="custom-logging-to-azure"></a>Azure へのカスタムのログ記録

[サーバー](/azure/azure-monitor/app/nodejs) (Node.js) と[クライアント](/azure/azure-monitor/app/javascript) (ブラウザー) のシナリオを提供する Azure Monitor の [Application Insights](/azure/azure-monitor/app/app-insights-overview) を使用します。

* サーバー - [Application Insights](/azure/azure-monitor/app/app-insights-overview) を使用して Node.js からログに記録します  - [npm パッケージ](https://www.npmjs.com/package/applicationinsights)
* クライアント - クライアント コードからログに記録します - [npm パッケージ](https://www.npmjs.com/package/@microsoft/applicationinsights-web)
* コンテナーと VM - [Kubernetes クラスター](/azure/azure-monitor/insights/container-insights-overview)または [Azure 仮想マシン](/azure/azure-monitor/insights/vminsights-overview)からログに記録します
 
## <a name="azure-sdk-client-library-logging"></a>Azure SDK クライアント ライブラリのログ

一般に、Azure SDK クライアント ライブラリの内部ログへのアクセスは必要ありません。 ログ用の Azure クライアント コア ライブラリは、Azure サービスで使用するために構築されています。 

[NPM パッケージ](https://www.npmjs.com/package/@azure/logger) | [ライブラリ ソース コード](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/core/logger)

### <a name="enable-logging"></a>ログの有効化

1 つの環境変数を使用してアプリケーション全体でログを有効にすることも、アプリケーションの一部に対してログを動的に構成することもできます。 この記事では、ロガー パッケージに関する主な概念と、次の手順でログを有効にする方法について説明します。

- `AZURE_LOG_LEVEL` 環境変数を設定します。
- ロガー ライブラリからインポートされた `setLogLevel` を呼び出します。
- 特定のロガーで `enable()` を呼び出します。

> [!NOTE]
> この記事は、Azure SDK の最新バージョンを使用するクライアント ライブラリに適用されます。 ライブラリがサポートされているかどうかを確認するには、「[Azure SDK の最新リリース](https://azure.github.io/azure-sdk/releases/latest/index.html#javascript)」の一覧を参照してください。 Azure SDK クライアント ライブラリの古いバージョンをアプリケーションで使用している場合は、該当するサービスのドキュメントで具体的な手順を参照してください。

### <a name="log-levels"></a>ログ レベル

`@azure/logger` パッケージでは、詳細度が最も高いものから最も低いものの順に指定された、次のログ レベルがサポートされています。

- verbose
- info
- warning
- error

プログラムで、または `AZURE_LOG_LEVEL` 環境変数を使用してログ レベルを設定すると、選択したログ レベル以下のレベルで書き込まれたログが出力されます。 たとえば、ログ レベルを `warning` に設定すると、レベルが `warning` または `error` のすべてのログが出力されます。

### <a name="install-the-logger-package"></a>ロガー パッケージをインストールする

Azure SDK for JavaScript ロガー ライブラリは [npm](https://www.npmjs.com/) パッケージとして提供されます。 npm を使用して、`@azure/logger` パッケージをインストールします。

```cmd
npm install @azure/logger
```

### <a name="set-the-logging-environment-variable"></a>ログ環境変数を設定する

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

### <a name="configure-dynamic-logging"></a>動的なログを構成する

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

### <a name="redirect-log-output"></a>ログ出力をリダイレクトする

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