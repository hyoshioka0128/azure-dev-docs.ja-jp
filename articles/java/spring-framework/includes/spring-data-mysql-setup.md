---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: 5216c009929906f619075841cda559d02d006cf7
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84562264"
---
## <a name="prepare-the-working-environment"></a>作業環境を準備する

まず、次のコマンドを使用して、いくつかの環境変数を設定します。

```bash
AZ_RESOURCE_GROUP=database-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_MYSQL_USERNAME=spring
AZ_MYSQL_PASSWORD=<YOUR_MYSQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

プレースホルダーは、この記事全体で使用される次の値に置き換えてください。

- `<YOUR_DATABASE_NAME>`:MySQL サーバーの名前。 Azure 全体で一意である必要があります。
- `<YOUR_AZURE_REGION>`:使用する Azure リージョン。 既定で `eastus` を使用できますが、居住地に近いリージョンを構成することをお勧めします。 「`az account list-locations`」を入力すると、使用可能なリージョンの完全な一覧を表示できます。
- `<YOUR_MYSQL_PASSWORD>`:MySQL データベース サーバーのパスワード。 パスワードは 8 文字以上にする必要があります。 これには、英大文字、英小文字、数字 (0 から 9)、英数字以外の文字 (!、$、#、% など) のうち、3 つのカテゴリの文字が含まれている必要があります。
- `<YOUR_LOCAL_IP_ADDRESS>`:ローカル コンピューターの IP アドレス。そこから、Spring Boot アプリケーションを実行します。 これを確認する簡単な方法は、ブラウザーで [whatismyip.akamai.com](http://whatismyip.akamai.com/)にアクセスすることです。

次に、リソース グループを作成します。

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
    | jq
```

> [!NOTE]
> JSON データを表示して読みやすくするために [Azure Cloud Shell](https://shell.azure.com/) に既定でインストールされている `jq` ユーティリティを使用します。
> このツールを使用しない場合は、使用するすべてのコマンドの `| jq` の部分を削除しても問題ありません。

## <a name="create-an-azure-database-for-mysql-instance"></a>Azure Database for MySQL インスタンスを作成する

最初に作成するのは、マネージド MySQL サーバーです。

> [!NOTE]
> MySQL サーバーの作成に関する詳細については、「[Azure portal を使用した Azure Database for MySQL サーバーの作成](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)」を参照してください。

[Azure Cloud Shell](https://shell.azure.com/) で次のスクリプトを実行します。

```azurecli
az mysql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_MYSQL_USERNAME \
    --admin-password $AZ_MYSQL_PASSWORD \
    | jq
```

このコマンドにより、小規模な MySQL サーバーが作成されます。

### <a name="configure-a-firewall-rule-for-your-mysql-server"></a>MySQL サーバーのファイアウォール規則を構成する

Azure Database for MySQL インスタンスは、既定でセキュリティ保護されています。 受信接続を一切許可しないファイアウォールがあります。 データベースを使用できるようにするには、データベース サーバーにアクセスするためのローカル IP アドレスを許可するファイアウォール規則を追加する必要があります。

この記事の冒頭でローカル IP アドレスを構成したので、次を実行してサーバーのファイアウォールを開くことができます。

```azurecli
az mysql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-mysql-database"></a>MySQL データベースを構成する

先ほど作成した MySQL サーバーは空です。 Spring Boot アプリケーションで使用できるデータベースはありません。 `demo` という新しいデータベースを作成します。

```azurecli
az mysql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server-name $AZ_DATABASE_NAME \
    | jq
```
