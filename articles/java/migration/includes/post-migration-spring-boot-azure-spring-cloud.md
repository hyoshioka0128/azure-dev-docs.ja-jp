---
author: yevster
ms.author: yebronsh
ms.date: 8/25/2020
ms.openlocfilehash: 6b2ad5c8490cd4b1c450426f8e7d728cd39eaea3
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062062"
---
これで移行が完了したので、アプリケーションが予想どおりに動作することを確認します。 その後、次の推奨事項を利用することでアプリケーションをよりクラウドネイティブにすることができます。

* Spring Cloud レジストリと連動するようにアプリケーションを有効にすることを検討してください。 これにより、デプロイされた他のマイクロサービスやクライアントでアプリケーションを動的に検出できます。 詳細については、[Java Spring アプリをデプロイ用に準備する](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)」を参照してください。 次に、Spring Client ロード バランサーを使用するようにアプリケーション クライアントを変更します。 これにより、クライアントでは実行中のすべてのアプリケーション インスタンスのアドレスを取得し、別のインスタンスが壊れたか、応答しなくなった場合に動作するインスタンスを見つけることができます。 詳細については、Spring ブログの「[Spring Tips:Spring Cloud LoadBalancer](https://spring.io/blog/2020/03/25/spring-tips-spring-cloud-loadbalancer)」(Spring に関するヒント: Spring Cloud LoadBalancer) を参照してください。

* アプリケーションをパブリックにする代わりに、[Spring Cloud Gateway](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/current/reference/html/) インスタンスの追加を検討してください。 Spring Cloud Gateway は、Azure Spring Cloud インスタンスにデプロイされたすべてのアプリケーションまたはマイクロサービスの単一エンドポイントとなります。 Spring Cloud Gateway が既にデプロイされている場合は、確実に新しくデプロイされたアプリケーションにトラフィックを送信するように構成します。

* Spring Cloud Config サーバーを追加し、すべての Spring Cloud マイクロサービスの構成を、バージョン管理も含め、一元的に管理することを検討してください。 まず、構成を格納するための Git リポジトリを作成し、それを使用するように Azure Spring Cloud インスタンスを構成します。 詳細については、[自分のサービス向けに Spring Cloud Config Server インスタンスを設定する](/azure/spring-cloud/spring-cloud-tutorial-config-server)」を参照してください。 次に、以下の手順で構成を移行します。

  1. アプリケーションの *src/main/resources* ディレクトリ内に、次の内容を含む *bootstrap.yml* ファイルを作成します。

        ```yml
          spring:
            application:
              name: <your-application-name>
        ```

  1. 構成 Git リポジトリで、 *<your-application-name>.yml* ファイルを作成します。ここで、`your-application-name` は、前の手順と同じです。 *src/main/resources* 内の *application.yml* ファイルから、先ほど作成した新しいファイルに設定を移動します。 設定が以前に *.properties* ファイルに含まれていた場合は、まず YAML に変換します。 この変換を実行するには、オンライン ツールまたは IntelliJ プラグインを検索します。

  1. 上記のディレクトリに *application.yml* ファイルを作成します。 このファイルを使用して、Azure Spring Cloud インスタンス上のすべてのアプリケーション間で共有される設定とリソースを定義できます。 通常、このような設定には、データ ソース、ログ設定、Spring Boot アクチュエータの構成などが含まれます。

  1. これらの変更を Git リポジトリにコミットし、プッシュします。

  1. *application.properties* または *application.yml* ファイルをアプリケーションから削除します。

* 一貫性のある自動デプロイのためにデプロイ パイプラインを追加することを検討します。 [Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd)、[GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions)、 [Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service) についての手順が用意されています。

* ステージング環境のデプロイを使用してコードの変更を運用環境でテストした後に、一部またはすべてのエンド ユーザーがそれらを使用できるようにすることを検討します。 詳細については、「[Azure Spring Cloud でステージング環境を設定する](/azure/spring-cloud/spring-cloud-howto-staging-environment)」を参照してください。

* サポートされている Azure データベースにアプリケーションを接続するために、サービス バインドを追加することを検討します。 これらのサービス バインドにより、資格情報などの接続情報を Spring Cloud アプリケーションに提供する必要がなくなります。

* アプリケーションのパフォーマンスおよび相互作用を監視するために、分散トレースと Azure App Insights の使用を検討します。 詳細については、「[Azure Spring Cloud で分散トレースを使用する](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)」を参照してください。

* 異常な状態を迅速に検出して対処するために、Azure Monitor のアラート ルールおよびアクション グループを追加することを検討します。 詳細については、[アラートとアクション グループを使用して Spring Cloud のリソースを監視する](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)」を参照してください。

* 待機時間を短くし、信頼性とフォールト トレランスを高めるために、別のリージョンに Azure Spring Cloud デプロイをレプリケートすることを検討します。 [Azure Traffic Manager](/azure/traffic-manager) を使用してデプロイ間で負荷を分散するか、[Azure Front Door](/azure/frontdoor) を使用して SSL オフロードと DDoS 保護付きの Web アプリケーション ファイアウォールを追加します。

* geo レプリケーションが不要な場合は、[Azure Application Gateway](/azure/application-gateway) を追加して、SSL オフロードと DDoS 保護付きの Web アプリケーション ファイアウォールを追加することを検討します。
