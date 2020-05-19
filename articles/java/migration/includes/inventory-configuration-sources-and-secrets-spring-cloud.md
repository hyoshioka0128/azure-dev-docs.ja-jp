---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: b15ebf1491dd494701dd5c18e0248e73bdd86f2c
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990204"
---
### <a name="inventory-configuration-sources-and-secrets"></a>構成のソースとシークレットをインベントリする

#### <a name="inventory-passwords-and-secure-strings"></a>パスワードとセキュリティで保護された文字列をインベントリする

すべてのシークレット文字列とパスワードについて、実稼働デプロイ上のすべてのプロパティおよび構成ファイルとすべての環境変数を確認します。 Spring Cloud アプリケーションでは、これらの文字列は通常、個々のサービスの *application.properties* または *application.yml* ファイル内、または Spring Cloud Config リポジトリ内にあります。

[!INCLUDE [inventory-certificates-h4](inventory-certificates-h4.md)]

#### <a name="determine-whether-spring-cloud-vault-is-used"></a>Spring Cloud Vault が使用されているかどうかを判断する

シークレットを格納およびアクセスするために Spring Cloud Vault を使用する場合、バッキング シークレット ストア (HashiCorp Vault、CredHub など) を特定します。 次に、アプリケーション コードによって使用されるすべてのシークレットを識別します。

#### <a name="locate-the-configuration-server-source"></a>構成サーバー ソースを見つける

アプリケーションで [Spring Cloud Config サーバー](https://cloud.spring.io/spring-cloud-config/reference/html/#_spring_cloud_config_server)を使用する場合は、構成が格納されている場所を特定します。 通常、この設定は、*bootstrap.yml* または *bootstrap.properties* ファイルにありますが、*application.yml* または *application.properties* ファイルにある場合もあります。 設定は、次の例のようになります。

```properties
spring.cloud.config.server.git.uri: file://${user.home}/spring-cloud-config-repo
```

前に示したように、Spring Cloud Config のバッキング データストアとして git が最もよく使用されていますが、その他のバックエンド候補のいずれかが使用されている場合もあります。 [リレーショナル データベース (JDBC)](https://cloud.spring.io/spring-cloud-config/reference/html/#_jdbc_backend)、[SVN](https://cloud.spring.io/spring-cloud-config/reference/html/#_version_control_backend_filesystem_use)、および[ローカル ファイル システム](https://cloud.spring.io/spring-cloud-config/reference/html/#_file_system_backend)などのその他のバックエンドについては、[Spring Cloud Config のドキュメント](https://cloud.spring.io/spring-cloud-config/reference/html/#_environment_repository)を参照してください。

> [!NOTE]
> 構成サーバーのデータが GitHub Enterprise などのオンプレミスに格納されている場合は、Git リポジトリを介して Azure Spring Cloud で使用できるようにする必要があります。
