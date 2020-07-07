---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 3ff76d977c231b4b238f9dbec7f3bfaddfe5e484
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507606"
---
### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>コンソール ログを確認して診断設定を構成する

すべての出力がファイルではなく、コンソールに送られるようにログ記録を構成します。

アプリケーションが Azure Spring Cloud にデプロイされたら、[診断設定を追加して](/azure/spring-cloud/diagnostic-services)、たとえば Azure Monitor Log Analytics を使用して、ログに記録されたイベントを使用できるようにします。

#### <a name="logstashelk-stack"></a>LogStash/ELK スタック

ログの集計に LogStash/ELK スタックを使用する場合は、コンソールの出力を [Azure Event Hub](/azure/event-hubs/) にストリーミングするように診断設定を構成します。 次に、[LogStash EventHub プラグイン](https://github.com/logstash-plugins/logstash-input-azure_event_hubs)を使用して、ログに記録されたイベントを LogStash に取り込みます。

#### <a name="splunk"></a>Splunk

ログの集計に Splunk を使用する場合は、コンソールの出力を [Azure Blob Storage](/azure/storage/blobs/) にストリーミングするように診断設定を構成します。 次に、[Microsoft Cloud Services 用の Splunk アドオン](https://splunkbase.splunk.com/app/3757/)を使用して、ログに記録されたイベントを Splunk に取り込みます。
