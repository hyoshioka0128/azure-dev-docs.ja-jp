---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 4d2df28d5c752d20454c28a6d4a53698aa7ff5b6
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673018"
---
### <a name="determine-whether-your-application-relies-on-scheduled-jobs"></a>スケジュールされたジョブにアプリケーションが依存しているかどうかを判断する

スケジュールされたジョブ (Quartz Scheduler タスクや Unix cron ジョブなど) は、Azure Kubernetes Service では使用できません。 Azure Kubernetes Service では、スケジュールされたタスクを含むアプリケーションの内部でのデプロイが妨げられることはありません。 ただし、アプリケーションをスケールアウトすると、同じスケジュールされたジョブが、スケジュールされた期間に複数回実行されることがあります。 このような場合、意図しない結果になることがあります。

AKS クラスターでスケジュールされたジョブを実行するには、必要に応じて Kubernetes CronJob を定義します。 詳細については、「[CronJob を使用した自動タスクの実行](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)」を参照してください。
