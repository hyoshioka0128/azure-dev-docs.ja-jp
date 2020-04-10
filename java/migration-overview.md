---
title: Java アプリケーションを Azure に移行する
description: このトピックでは、Java アプリケーションを Azure に移行するために推奨されている方法の概要を示します。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: e6215502b54bedf62f40a024f9e7b3acc01cdc1f
ms.sourcegitcommit: 951fc116a9519577b5d35b6fb584abee6ae72b0f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2020
ms.locfileid: "80612006"
---
# <a name="migrate-java-applications-to-azure"></a>Java アプリケーションを Azure に移行する

このトピックでは、Java アプリケーションを Azure に移行する場合に推奨されている方法について概要を示します。

この移行ガイダンスは、Azure 上の Java のメインストリームのシナリオを取り上げ、高度な計画の推奨事項と考慮事項を提供するように設計されています。 Microsoft Java on Azure チームと特定の Java アプリの移行シナリオについて検討したい場合は、次のアンケートにご記入ください。担当者からお客様にご連絡いたします。

> [!div class="nextstepaction"]
> [Java 移行のアンケート](https://aka.ms/migrate-my-Java-app-requested-thru-docs)

## <a name="identifying-application-type"></a>アプリケーションの種類の特定

Java アプリケーションのクラウドの移行先を選択する前に、アプリケーションの種類を特定する必要があります。 ほとんどの Java アプリケーションは、次のいずれかの種類になります。

* [Spring Boot/JAR アプリケーション](#spring-boot--jar-applications)
* [Spring Cloud/マイクロサービス](#spring-cloud--microservices)
* [Web アプリケーション](#web-applications)
* [Java EE アプリケーション](#java-ee-applications)
* [バッチ/スケジュールされたジョブ](#batch--scheduled-jobs)

以降のセクションでは、ここに挙げた種類について説明します。

### <a name="spring-boot--jar-applications"></a>Spring Boot/JAR アプリケーション

新しい多くのアプリケーションは、コマンド ラインから直接呼び出されます。 その場合でもこれらのアプリケーションは Web 要求を処理しますが、アプリケーション サーバーに依存して HTTP 要求の処理を行うのではなく、HTTP 通信と他のすべての依存関係を直接アプリケーション パッケージに組み込みます。 このようなアプリケーションは、多くの場合、Spring Boot、Dropwizard、Micronaut、MicroProfile、Vert.x などのフレームワークを使用して構築されます。

これらのアプリケーションは、 *.jar* 拡張子 (JAR ファイル) を使用してアーカイブにパッケージ化されます。

### <a name="spring-cloud--microservices"></a>Spring Cloud/マイクロサービス

マイクロサービス アーキテクチャ スタイルは、1 つのアプリケーションを一連の小規模なサービスとして開発するためのアプローチであり、それぞれのサービスは独自のプロセスで実行され、軽量のメカニズム (多くの場合は HTTP リソース API) と通信します。 これらのサービスはビジネス機能を基に作成され、完全に自動化されたデプロイ メカニズムによって個別にデプロイできます。 これらのサービスは、さまざまなプログラミング言語で記述され、さまざまなデータ ストレージ テクノロジを使用している可能性があり、最小限の集中管理が行われます。 こうしたサービスは、多くの場合、Spring Cloud などのフレームワークを使用して作成されます。

これらのサービスは、 *.jar* 拡張子 (JAR ファイル) を使用して複数のアプリケーションにパッケージ化されます。

### <a name="web-applications"></a>Web アプリケーション

Web アプリケーションは、[サーブレット](https://en.wikipedia.org/wiki/Java_servlet) コンテナー内で実行されます。 一部はサーブレット API を直接使用しますが、多くは、Apache Struts、Spring MVC、JavaServer Faces (JSF) など、サーブレット API をカプセル化する追加のフレームワークを使用します。

Web アプリケーションは、 *.war* 拡張子 (WAR ファイル) を使用してアーカイブにパッケージ化されます。

### <a name="java-ee-applications"></a>Java EE アプリケーション

Java EE アプリケーション (J2EE アプリケーションや、さらに最近では Jakarta EE アプリケーションとも呼ばれます) は、Web アプリケーションの一部またはすべての要素を含む場合や、まったく含んでいない場合があります。 また、[Java EE 仕様](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition)で定義されているような、多くの追加コンポーネントを格納して使用する場合もあります。

Java EE アプリケーションは、 *.ear* 拡張子 (EAR ファイル) を使用してアーカイブとしてパッケージ化することも、 *.war* 拡張子 (WAR ファイル) を使用してアーカイブとしてパッケージ化することもできます。

Java EE アプリケーションは、Java EE 準拠のアプリケーション サーバー (WebLogic、WebSphere、WildFly、GlassFish、Payara など) にデプロイする必要があります。

Java EE 仕様によって提供される機能のみに依存するアプリケーション (つまり、アプリ サーバー非依存アプリケーション) は、準拠アプリケーション サーバー間で移行できます。 アプリケーションが特定のアプリケーション サーバーに依存している (アプリ サーバー依存) 場合は、そのアプリケーション サーバーをホストすることが許可される Azure サービス移行先を選択することが必要になる可能性があります。

### <a name="batch--scheduled-jobs"></a>バッチ/スケジュールされたジョブ

アプリケーションによっては、要求やユーザー入力を待機するのではなく、短時間だけ実行し、特定のワークロードを実行して終了することを意図したものがあります。 場合によっては、このようなジョブは 1 回だけ、または一定のスケジュールされた間隔で実行する必要があります。 オンプレミスでは、多くの場合、このようなジョブはサーバーの crontab から呼び出されます。

これらのアプリケーションは、 *.jar* 拡張子 (JAR ファイル) を使用してアーカイブにパッケージ化されます。

> [!NOTE]
> アプリケーションでスケジューラ (Spring Batch や Quartz など) を使用してスケジュールされたタスクを実行する場合は、このようなタスクがアプリケーションの外部で実行されるようにすることを強くお勧めします。 アプリケーションをクラウド内の複数のインスタンスにスケーリングすると、同じジョブが複数回実行されます。 さらに、スケジュール メカニズムでホストのローカル タイム ゾーンが使用されている場合、複数のリージョンにまたがってアプリケーションをスケーリングすると、望ましくない動作が生じる可能性があります。

## <a name="selecting-the-target-azure-service-destination"></a>対象の Azure サービスの移行先を選択する

以降のセクションでは、どのサービス移行先がアプリケーションの要件を満たしているか、およびどのような役割が必要になるかを示します。

### <a name="feature-grid"></a>機能グリッド

次のグリッドを使用して、必要なアプリケーションの種類と機能をサポートする移行先を特定してください。

|   |アプリ<br>サービス<br>Java SE|アプリ<br>サービス<br>Tomcat|Azure<br>Spring<br>クラウド|AKS|Virtual Machines|
|---|---|---|---|---|---|---|
| Spring Boot/JAR アプリケーション                                    |&#x2714;|        |        |&#x2714;|&#x2714;|
| Spring Cloud/マイクロサービス                                      |        |        |&#x2714;|&#x2714;|&#x2714;|
| Web アプリケーション                                                  |        |&#x2714;|        |&#x2714;|&#x2714;|
| Java EE アプリケーション                                              |        |        |        |&#x2714;|&#x2714;|
| 商用アプリケーション サーバー<br>(WebLogic や WebSphere など) |        |        |        |&#x2714;|&#x2714;|
| ローカル ファイル システムでの長期保存                         |&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| アプリケーション サーバー レベルのクラスタリング                               |        |        |        |&#x2714;|&#x2714;|
| バッチ/スケジュールされたジョブ                                            |        |        |&#x2714;|&#x2714;|&#x2714;|

### <a name="ongoing-responsibility-grid"></a>継続的な役割グリッド

次のグリッドを使用して、移行後に各移行先によってチームに課せられる役割を把握してください。

チームは、"&#x1F449;" で示されるタスクを継続的に担います。 これらすべての役割を果たすために、堅牢で高度に自動化されたプロセスを実装することをお勧めします。 

> [!NOTE]
> これは、すべての役割を網羅したリストではありません。

|   | App Service | Azure Spring Cloud | AKS | Virtual Machines |
|---|---|---|---|---|
| ライブラリを更新する<br>(脆弱性の修復を含む)                 | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |
| アプリケーション サーバーを更新する<br>(脆弱性の修復を含む)    | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Java ランタイムを更新する<br>(脆弱性の修復を含む)          | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Kubernetes の更新をトリガーする<br>(手動トリガーを使用して Azure によって実行) | 該当なし         | 該当なし         | &#x1F449;   | 該当なし       |
| 下位互換性のない Kubernetes API の変更を調整する                  | 該当なし         | 該当なし         | &#x1F449;   | 該当なし       |
| コンテナーの基本イメージを更新する<br>(脆弱性の修復を含む)      | 該当なし         | 該当なし         | &#x1F449;   | 該当なし       |
| オペレーティング システムを更新する<br>(脆弱性の修復を含む)      | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 失敗したインスタンスを検出して再起動する                                   | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 更新のためのドレインとローリング再起動を実施する                       | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| インフラストラクチャの管理                                                   | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| 監視とアラート管理                                             | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |

アプリケーションの一部としてサーブレット コンテナー (Spring Boot など) をデプロイする場合は、それをライブラリであると考える必要があるため、常にご自身の責任となります。

## <a name="ensuring-on-premises-connectivity"></a>オンプレミスの接続を確保する

アプリケーションからオンプレミスのサービスのいずれかにアクセスする必要がある場合、Azure の接続サービスの 1 つをプロビジョニングする必要があります。 詳しくは、「[オンプレミス ネットワークを Azure に接続するためのソリューションを選択する](/azure/architecture/reference-architectures/hybrid-networking/)」をご覧ください。 または、オンプレミスのリソースで公開されている一般公開の API を使用するように、アプリケーションをリファクタリングする必要があります。

移行を開始する前に、この作業を完了しておく必要があります。

## <a name="inventory-current-capacity-and-resource-usage"></a>現在の容量とリソースの使用状況をインベントリする

現在の実稼働サーバーのハードウェア、および平均とピーク時の要求数とリソース使用率を文書化します。 この情報は、サービス移行先にリソースをプロビジョニングするために必要になります。

## <a name="migration-guidance"></a>移行ガイダンス

以下のグリッドを使用して、アプリケーションの種類と対象の Azure サービスの移行先ごとの移行ガイダンスを確認してください。

**Java アプリケーション**

次の行では Java アプリケーションの種類を確認し、列ではアプリケーションをホストする Azure サービス移行先を確認します。

App Service 上の Tomcat に JBoss EAP アプリを移行する場合は、まず、Tomcat 上で実行されている Java Web Apps (サーブレット) に Java EE アプリを変換し、次に示すガイダンスに従ってください。

Tomcat 上の Web アプリを Azure Spring Cloud に移行する場合は、まずアプリを Spring Cloud マイクロサービスに変換し、次に示すガイダンスに従ってください。

|移行先&nbsp;→<br><br>アプリケーションの&nbsp;種類&nbsp;↓|アプリ<br>サービス<br>Java SE|アプリ<br>サービス<br>Tomcat|Azure<br>Spring<br>クラウド|AKS|Virtual Machines|
|---|---|---|---|---|---|---|
| Spring Boot/<br>JAR アプリケーション | [ガイダンス][5] | ガイダンス<br>予定 | ガイダンス<br>予定 | ガイダンス<br>予定 | ガイダンス<br>予定 |
| Spring Cloud/<br>マイクロサービス   | 該当なし           | 該当なし                 | ガイダンス<br>予定 | ガイダンス<br>予定 | ガイダンス<br>予定 |
| Web アプリケーション<br>(Tomcat 上)     | 該当なし           | [ガイダンス][2]       | 該当なし                 | [ガイダンス][3]       | ガイダンス<br>予定 |

**Java EE アプリケーション**

次の行では、特定のアプリ サーバーで実行されている Java EE アプリケーションの種類を確認します。 列では、アプリケーションをホストする Azure サービス移行先を確認します。

|移行先&nbsp;→<br><br>アプリ サーバー&nbsp;↓|アプリ<br>サービス<br>Java SE|アプリ<br>サービス<br>Tomcat|Azure<br>Spring<br>クラウド|AKS|Virtual Machines|
|---|---|---|---|---|---|---|
| WildFly/<br>JBoss AS | 該当なし | 該当なし | 該当なし | [ガイダンス][9] | ガイダンス<br>予定 |
| WebLogic              | 該当なし | 該当なし | 該当なし | [ガイダンス][6] | [ガイダンス][4]       |
| WebSphere             | 該当なし | 該当なし | 該当なし | [ガイダンス][7] | ガイダンス<br>予定 |
| JBoss EAP             | 該当なし | 該当なし | 該当なし | [ガイダンス][8] | ガイダンス<br>予定 |

<!-- reference links, for use with tables -->
[1]: media/migration-overview/logo_azure.svg
[2]: migrate-tomcat-to-tomcat-app-service.md
[3]: migrate-tomcat-to-containers-on-azure-kubernetes-service.md
[4]: migrate-weblogic-to-virtual-machines.md
[5]: migrate-java-se-to-java-se-app-service.md
[6]: migrate-weblogic-to-wildfly-on-azure-kubernetes-service.md
[7]: migrate-websphere-to-wildfly-on-azure-kubernetes-service.md
[8]: migrate-jboss-eap-to-wildfly-on-azure-kubernetes-service.md
[9]: migrate-wildfly-to-wildfly-on-azure-kubernetes-service.md
