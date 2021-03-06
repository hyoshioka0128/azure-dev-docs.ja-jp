---
ms.openlocfilehash: 0a9b06932baa51c3cf003a4485a3a25261ffe91d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "81671868"
---
[認証ファイル](../java-sdk-azure-authenticate.md#mgmt-file)を作成し、環境変数 `AZURE_AUTH_LOCATION` と認証ファイルの完全なパスをコマンド ラインでエクスポートします。

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

Azure リソースを定義、作成、構成するために管理ライブラリで使用されるエントリ ポイント `Azure` オブジェクトを構成するために、この認証ファイルが使用されます。

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

Azure Management Libraries for Java を使用する際の認証オプションの詳細については、[こちら](../java-sdk-azure-authenticate.md#mgmt-auth)を参照してください。