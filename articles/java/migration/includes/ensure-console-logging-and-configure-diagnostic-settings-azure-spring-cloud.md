---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: b53308d4a9db52a25665d0daa74be678e726c499
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990184"
---
### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>コンソール ログを確認して診断設定を構成する

Azure Spring Cloud のすべてのアプリケーションが、ファイルではなくコンソールに記録されるようにログ記録を構成します。

アプリケーションが Azure Spring Cloud にデプロイされたら、[診断設定を追加して](/azure/spring-cloud/diagnostic-services)、たとえば Azure Monitor Log Analytics を使用して、ログに記録されたイベントを使用できるようにします。

#### <a name="logstashelk-stack"></a>LogStash/ELK スタック

ログの集計に LogStash/ELK スタックを使用する場合は、コンソールの出力を [Azure Event Hub](/azure/event-hubs/) にストリーミングするように診断設定を構成します。 次に、[LogStash EventHub プラグイン](https://github.com/logstash-plugins/logstash-input-azure_event_hubs)を使用して、ログに記録されたイベントを LogStash に取り込みます。

#### <a name="splunk"></a>Splunk

ログの集計に Splunk を使用する場合は、コンソールの出力を [Azure Blob Storage](/azure/storage/blobs/) にストリーミングするように診断設定を構成します。 次に、[Microsoft Cloud Services 用の Splunk アドオン](https://splunkbase.splunk.com/app/3757/)を使用して、ログに記録されたイベントを Splunk に取り込みます。
