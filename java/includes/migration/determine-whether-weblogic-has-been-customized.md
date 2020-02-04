---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: d56ba6d8f101b1d90468a436f0a111f78a19fc83
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830850"
---
### <a name="determine-whether-weblogic-has-been-customized"></a>WebLogic がカスタマイズされているかどうか確認する

次のうち、どのカスタマイズが行われたかを確認し、作業内容を把握します。

* スタートアップ スクリプトは変更されていますか? このようなスクリプトには、*setDomainEnv*、*commEnv*、*startWebLogic*、*stopWebLogic* などがあります。
* JVM に渡される特定のパラメーターはありますか?
* サーバーのクラスパスに追加された JAR はありますか?
