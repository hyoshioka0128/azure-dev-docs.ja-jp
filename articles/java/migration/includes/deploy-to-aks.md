---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: abf3824dd83d0da27a411694e144a8e6bf7285ac
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738493"
---
### <a name="deploy-to-aks"></a>AKS にデプロイする

Kubernetes YAML ファイルを作成して適用します。 詳細については、「[クイック スタート: Azure CLI を使用して Azure Kubernetes Service クラスターをデプロイする](/azure/aks/kubernetes-walkthrough#run-the-application)」を参照してください。 外部ロード バランサーを (アプリケーションまたはイングレス コントローラー用に) 作成する場合は、前のセクションで `LoadBalancerIP` としてプロビジョニングした IP アドレスを必ず指定してください。

外部化したパラメーターを環境変数として含めます。 詳細については、「[コンテナーの環境変数の定義](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)」を参照してください。 シークレット (パスワード、API キー、JDBC 接続文字列など) は含めないでください。 これらについては以下のセクションで説明します。

デプロイ YAML を作成するときは、必ずメモリと CPU の設定を含めて、コンテナーのサイズが適切に設定されるようにしてください。
