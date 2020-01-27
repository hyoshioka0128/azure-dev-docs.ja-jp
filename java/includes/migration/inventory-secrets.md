---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 4228d1db44ac16da1c05fe97d20a3491cc9c5f51
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288571"
---
### <a name="inventory-secrets"></a>シークレットをインベントリする

#### <a name="passwords-and-secure-strings"></a>パスワードとセキュリティで保護された文字列

すべてのシークレット文字列とパスワードについて、運用サーバー上のすべてのプロパティと構成ファイルを確認します。 $CATALINA_BASE/conf にある *server.xml* と *context.xml* を必ず確認してください。 また、アプリケーション内にパスワードや資格情報を含むファイルが存在する場合もあります。 これらには、*META-INF/context.xml*や、Spring Boot アプリケーションの場合は *application.properties* または *application.yml* ファイルなどがあります。

#### <a name="certificates"></a>証明書

パブリック SSL エンドポイントで使用されるすべての証明書を文書化します。 次のコマンドを実行して、運用サーバー上のすべての証明書を表示できます。

```bash
keytool -list -v -keystore <path to keystore>
```
