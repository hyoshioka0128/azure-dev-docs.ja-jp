---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 29ea7925e91b1f42e0f83b88fbc7f7d9a8fd3629
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897642"
---
### <a name="deploy-to-aks"></a>AKS にデプロイする

Kubernetes YAML ファイルを作成して適用します。 詳細については、「[クイック スタート: Azure CLI を使用して Azure Kubernetes Service クラスターをデプロイする](/azure/aks/kubernetes-walkthrough#run-the-application)」を参照してください。 外部ロード バランサーを (アプリケーションまたはイングレス コントローラー用に) 作成する場合は、前のセクションで `LoadBalancerIP` としてプロビジョニングした IP アドレスを必ず指定してください。

外部化したパラメーターを環境変数として含めます。 詳細については、「[コンテナーの環境変数の定義](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)」を参照してください。 シークレット (パスワード、API キー、JDBC 接続文字列など) は含めないでください。 これらについては以下のセクションで説明します。

デプロイ YAML を作成するときは、必ずメモリと CPU の設定を含めて、コンテナーのサイズが適切に設定されるようにしてください。
