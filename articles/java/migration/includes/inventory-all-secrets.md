---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a771d0689e83e0b61bf9b8c31e0cbf36949fdc20
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673398"
---
### <a name="inventory-all-secrets"></a>すべてのシークレットをインベントリする

Azure Key Vault などの "サービスとしての構成" テクノロジが登場する前に、"シークレット" の明確に定義された概念はありませんでした。 代わりに、現在 "シークレット" を呼ばれるものとして効果的に機能する、さまざまな種類の構成設定のセットがありました。 WebLogic Server などのアプリ サーバーでは、これらのシークレットは多くの異なる構成ファイルと構成ストアにあります。 すべてのシークレットとパスワードについて、実稼働サーバー上のすべてのプロパティと構成ファイルを確認します。 必ず、WAR 内の *weblogic.xml* を確認してください。 また、パスワードや資格情報を含む構成ファイルがアプリケーション内に見つかる場合もあります。 詳細については、「[Azure Key Vault の基本的な概念](/azure/key-vault/basic-concepts)」をご覧ください。
