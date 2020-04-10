---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: 4b5b73eee66c4a5c9eb28b79804e0dc610f639d6
ms.sourcegitcommit: 951fc116a9519577b5d35b6fb584abee6ae72b0f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2020
ms.locfileid: "80612076"
---
### <a name="inventory-external-resources"></a>外部リソースをインベントリする

データ ソース、JMS メッセージ ブローカー、およびその他の外部リソースは、JNDI (Java Naming and Directory Interface) を介して挿入されます。 こうしたリソースの一部では、移行または再構成が必要な場合があります。

#### <a name="inside-your-application"></a>アプリケーション内

*META-INF/context.xml* ファイルを調べます。 `<Context>` 要素内の `<Resource>` 要素を探します。

#### <a name="on-the-application-servers"></a>アプリケーション サーバー上

*$CATALINA_BASE/conf/context.xml* および *$CATALINA_BASE/conf/server.xml* ファイルと、 *$CATALINA_BASE/conf/[engine-name]/[host-name]* ディレクトリにある *.xml*ファイルを調べます。

*context.xml*ファイルでは、JNDI リソースは、最上位の `<Context>` 要素内の `<Resource>` 要素で記述されます。

*server.xml* ファイルでは、JNDI リソースは `<GlobalNamingResources>` 要素内の `<Resource>` 要素で記述されます。

#### <a name="datasources"></a>データソース

データソースは、`type` 属性が `javax.sql.DataSource` に設定されている JNDI リソースです。 データソースごとに、次の情報を文書にまとめます。

* データソース名
* 接続プールの構成
* JDBC ドライバーの JAR ファイルの場所

詳細については、Tomcat のドキュメントの「[JNDI Datasource How-To](https://tomcat.apache.org/tomcat-9.0-doc/jndi-datasource-examples-howto.html)」を参照してください。

#### <a name="all-other-external-resources"></a>その他のすべての外部リソース

このガイドでは、考えられるすべての外部依存関係を記載することはできません。 アプリケーションの外部依存関係がすべて満たされるよう確認するのは、担当チームの責任です。
