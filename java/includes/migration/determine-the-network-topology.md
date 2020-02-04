---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 9403614c7ae2990a35fcbcce6d2e104f87724da5
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830920"
---
### <a name="determine-the-network-topology"></a>ネットワーク トポロジを決定する

Marketplace オファーの現在のセットが、移行の出発点となります。 移行する必要があるアーキテクチャの側面がオファーに含まれていない場合は、ソリューション テンプレートの 1 つを使用して基本的なオファーを立ち上げた後でも、既存のデプロイのネットワーク トポロジを把握し、Azure で再現する必要があります。

これは非常に広範なトピックですが、次のリファレンスを使用すると、移行作業に多少の方向性を打ち出すことができます。

* このリファレンスは、Azure へのネットワーク トポロジの移行に関連するトピックの概要を列挙しています。「[迅速なデプロイメントのガイド](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/deploying.html#GUID-E0BE4A3E-44CD-4C95-9540-7A850BF02F6A)」。
* このリファレンスでは、ネットワーク トポロジに影響を与えるクラスタリングに関する重要事項について説明しています。「[WebLogic Server クラスタリング](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/clustering.html#GUID-E39A18C2-B990-485F-BFB1-0549250FABFE)」。
* データ ソースは WebLogic システム内の別々のサーバーであるため、ネットワークト ポロジ分析の一部としてそれらを考慮する必要があります。 「[WebLogic Server データ・ソース](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html#GUID-9FD5F552-B2E4-4FEC-8C10-503A08764B52)」。
* メッセージング ソースも別々のサーバーです。 「[WebLogic Sever メッセージング](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jms.html#GUID-3B5F647D-E001-413B-AC6A-1E103BDBA93F)」
* 負荷分散は、基本要件です。 このリファレンスでは、負荷分散の WebLogic Server 側について説明しています。「[クラスタでのロード・バランシング](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/load_balancing.html#GUID-B8F6DE4B-1AAC-428B-878B-BFDCE161C054)」。
