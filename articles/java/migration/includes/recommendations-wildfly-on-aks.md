---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 1ffc12fa2a99e2abdbbc43e0024afd8e5e98c4b9
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673258"
---
### <a name="recommendations"></a>Recommendations

* イングレス コントローラーまたはアプリケーション ロード バランサーに割り当てられた IP アドレスに DNS 名を追加することを検討してください。 詳細については、「[AKS で静的パブリック IP アドレスを使用してイングレス コントローラーを作成する](/azure/aks/ingress-static-ip)」を参照してください。

* アプリケーションに [HELM チャート](https://helm.sh/docs/topics/charts/)を追加することを検討してください。 Helm チャートを使用すると、より多様な顧客によって使用およびカスタマイズされるように、アプリケーションのデプロイをパラメーター化できます。

* DevOps の戦略を設計し、実装します。 信頼性を維持しながら開発速度を向上させるには、Azure Pipelines を使用してデプロイとテストを自動化することを検討してください。 詳細については、[AKS へのビルドとデプロイ](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)に関するページをご覧ください。

* クラスターに対して Azure 監視を有効にします。 詳しくは、「[既にデプロイされている AKS クラスターの監視を有効にする](/azure/azure-monitor/insights/container-insights-enable-existing-clusters)」をご覧ください。 これにより、Azure Monitor はコンテナー ログの収集、使用状況の追跡などを行えます。

* Prometheus を使用して、アプリケーション固有のメトリックを公開することを検討してください。 Prometheus は、Kubernetes コミュニティで広く採用されているオープンソースのメトリック フレームワークです。 独自の Prometheus サーバーをホストするのではなく、Azure Monitor で Prometheus メトリックのスクレーピングを構成して、アプリケーションからのメトリックの集計と、異常な状態に対する自動応答またはエスカレーションを有効にできます。 詳しくは、「[Azure Monitor for containers で Prometheus メトリックのスクレーピングを構成する](/azure/azure-monitor/insights/container-insights-prometheus-integration)」をご覧ください。

* 事業継続とディザスター リカバリー戦略を設計し、実装します。 ミッション クリティカルなアプリケーションの場合は、複数リージョン デプロイのアーキテクチャを検討してください。 詳細については、「[AKS での事業継続とディザスター リカバリーに関するベスト プラクティス](/azure/aks/operator-best-practices-multi-region)」を参照してください。

* [Kubernetes バージョン サポート ポリシー](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy)を確認します。 常にサポートされているバージョンが実行されるように、継続的に AKS クラスターを更新するのは開発者の責任です。 詳細については、「[AKS クラスターのアップグレード](/azure/aks/upgrade-cluster)」を参照してください。

* クラスター管理とアプリケーション開発を担当するすべてのチーム メンバーに、関連する AKS のベスト プラクティスを確認してもらいます。 詳しくは、「[Azure Kubernetes Service (AKS) でのアプリケーションの構築および管理のためのクラスター オペレーターと開発者のベスト プラクティス](/azure/aks/best-practices)」をご覧ください。

* デプロイ ファイルでローリング アップデートの実行方法が指定されていることを確認します。 詳細については、Kubernetes のドキュメントの「[ローリング アップデートのデプロイ](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)」を参照してください。

* ピーク時の負荷に対処するように自動スケーリングを設定します。 詳細については、「[AKS でのアプリケーションの需要を満たすようにクラスターを自動的にスケーリング](/azure/aks/cluster-autoscaler)」を参照してください。

* さらにパフォーマンスを最適化するために、コードのキャッシュ サイズを監視し、Dockerfile に JVM パラメーター `-XX:InitialCodeCacheSize` および `-XX:ReservedCodeCacheSize` を追加することを検討します。 詳細については、Oracle のドキュメントの「[Codecache チューニング](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm)」をご覧ください。
