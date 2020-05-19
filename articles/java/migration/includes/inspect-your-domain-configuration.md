---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 750a8cdd34ead8390a0c4c2cb028f4624bd256a9
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988907"
---
### <a name="inspect-your-domain-configuration"></a>ドメインの構成を検査する

WebLogic Server の主な構成単位は、ドメインです。 そのため、*config.xml* ファイルには、移行のために慎重に検討する必要がある多数の構成が含まれています。 このファイルには、サブディレクトリに格納されている追加の XML ファイルへの参照が記載されています。 Oracle では、通常、 **[管理コンソール]** を使用して WebLogic Server の管理可能なオブジェクトとサービスを構成し、WebLogic Server で *config.xml* ファイルを保守できるようにすることを推奨しています。 詳細については、「[ドメイン構成ファイル](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/domcf/config_files.html)」を参照してください。

#### <a name="inside-your-application"></a>アプリケーション内

*WEB-INF/weblogic.xml* ファイルおよび *WEB-INF/web.xml* ファイルを調べます。
