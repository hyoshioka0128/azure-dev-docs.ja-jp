---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 1015c179c0f93485decd77bd89a3ceec8833652e
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81672158"
---
### <a name="migrate-scheduled-jobs"></a>スケジュールされたジョブを移行する

スケジュールされたジョブを Azure で実行するには、[Azure Functions のタイマー トリガー](/azure/azure-functions/functions-bindings-timer)を使用することを検討してください。 ジョブ コード自体を関数に移行する必要はありません。 この関数では、アプリケーションで URL を呼び出すだけでジョブをトリガーできます。 このようなジョブ実行を動的に呼び出したり、一元的に追跡したりする必要がある場合は、[Spring Batch](https://spring.io/projects/spring-batch) の使用を検討してください。

または、繰り返しトリガーを使用してロジック アプリを作成し、アプリケーションの外部でコードを記述せずに URL を呼び出すこともできます。 詳細については、「[概要 - Azure Logic Apps とは](/azure/logic-apps/logic-apps-overview)」と「[Azure Logic Apps で繰り返しトリガーを使用して繰り返しタスクおよびワークフローを作成、スケジュール設定、および実行する](/azure/connectors/connectors-native-recurrence)」を参照してください。

> [!NOTE]
> 悪意のある使用を防ぐために、ジョブの呼び出しエンドポイントで資格情報が求められるようにする必要があります。 この場合、トリガー関数が資格情報を提供する必要があります。
