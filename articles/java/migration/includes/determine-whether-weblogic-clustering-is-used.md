---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: f50115be65be8e746527b3d4ee833a738109ddbc
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/10/2020
ms.locfileid: "88052261"
---
### <a name="determine-whether-weblogic-clustering-is-used"></a>WebLogic クラスタリングが使用されているかどうか確認する

多くの場合、高可用性を実現するために、アプリケーションは複数の WebLogic サーバーにデプロイされています。 これらのクラスターをオンプレミスのインストールから Azure Virtual Machines で実行されている WebLogic に直接移行できます。 詳細については、Oracle のドキュメントの「[ドメイン構成ファイル](https://docs.oracle.com/middleware/12213/wls/DOMCF/config_files.htm#DOMCF127)」を参照してください。

### <a name="account-for-load-balancing-requirements"></a>負荷分散の要件の考慮

負荷分散は、Oracle WebLogic Server クラスターの Azure への移行において不可欠な部分です。  最も簡単な解決策は、Oracle WebLogic Server クラスター向けの Azure Marketplace オファーで提供される [Azure Application Gateway](/azure/application-gateway/overview) の組み込みサポートを使用することです。  このトピックに関するチュートリアルについては、「[チュートリアル: ロード バランサーとして Azure Application Gateway を使用して、Azure に WebLogic Server クラスターを移行する](../migrate-weblogic-with-app-gateway.md)」を参照してください。

その他の Azure 負荷分散ソリューションと比較した Azure Application Gateway の機能の概要については、「[Azure の負荷分散オプションの概要](/azure/architecture/guide/technology-choices/load-balancing-overview)」を参照してください。
