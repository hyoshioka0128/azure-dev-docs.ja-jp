---
title: Azure CLI の概要
description: コマンドの基本を学習し、Azure CLI を使い始めます。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 01/30/2020
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: bc9b86db6fb9c5b3731550df9dda96debcbfba9f
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030724"
---
# <a name="get-started-with-azure-cli"></a>Azure CLI の概要

Azure CLI へようこそ。  この記事では、CLI について説明し、一般的なタスクの実行に役立つ情報を示します。

> [!NOTE]
>
> スクリプトおよび Microsoft ドキュメント サイトでは、Azure CLI の例は `bash` シェル向けに記述されています。 1 行の例であれば任意のプラットフォームで実行できます。 行継続 (`\`) または変数の代入を含む、より長い例は、PowerShell などの他のシェルで動作するように変更する必要があります。

## <a name="install-or-run-in-azure-cloud-shell"></a>Azure Cloud Shell でのインストールまたは実行

Azure CLI で作業を開始するには、お使いのブラウザーから Azure Cloud Shell 環境で実行するのが最も簡単です。 Cloud Shell については、「[Azure Cloud Shell の Bash のクイックスタート](/azure/cloud-shell/quickstart)」を参照してください。

CLI をインストールする準備ができたら、[インストール手順](install-azure-cli.md)に関するページをご覧ください。

初めて CLI をインストールしたら、`az --version` を実行して、CLI の正しいバージョンがインストールされていること確認します。

> [!NOTE]
> Azure クラシック デプロイ モデルを使用している場合は、[Azure クラシック CLI をインストール](install-classic-cli.md)してください。

## <a name="sign-in"></a>サインイン

ローカル インストールで CLI コマンドを使用する前に、[az login](/cli/azure/reference-index#az-login) でサインインする必要があります。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

ログインすると、ご使用のアカウントに関連付けられているサブスクリプションの一覧が表示されます。 `isDefault: true` が示されているサブスクリプション情報が、ログイン後に、現在アクティブになっているサブスクリプションです。 別のサブスクリプションを選択するには、切り替え先のサブスクリプション ID を指定して [az account set](/cli/azure/account#az-account-set) コマンドを実行します。 サブスクリプションの選択の詳細については、「[複数の Azure サブスクリプションの使用](manage-azure-subscriptions-azure-cli.md)」を参照してください。

対話型以外のサインイン方法も用意されています。詳細については、「[Azure CLI を使用してサインインする](authenticate-azure-cli.md)」を参照してください。

## <a name="common-commands"></a>一般的なコマンド

次の表は、CLI で使用される一般的なコマンドの一部を示しており、リファレンス ドキュメントにリンクされています。

| リソースの種類 | Azure CLI コマンド グループ |
|---------------|-------------------------|
| [リソース グループ](/azure/azure-resource-manager/resource-group-overview) | [az group](/cli/azure/group) |
| [仮想マシン](/azure/virtual-machines) | [az vm](/cli/azure/vm) |
| [ストレージ アカウント](/azure/storage/common/storage-introduction) | [az storage account](/cli/azure/storage/account) |
| [Key Vault](/azure/key-vault/key-vault-whatis) | [az keyvault](/cli/azure/keyvault) |
| [Web アプリケーション](/azure/app-service) | [az webapp](/cli/azure/webapp) |
| [SQL データベース](/azure/sql-database) | [az sql server](/cli/azure/sql/server) |
| [CosmosDB](/azure/cosmos-db) | [az cosmosdb](/cli/azure/cosmosdb) |

## <a name="finding-commands"></a>コマンドを見つける

CLI のコマンドは、"_グループ_" の "_サブコマンド_" として整理されています。 各グループは、Azure のサービスを表し、コマンドはそのサービスに対して動作します。

コマンドを検索するには、[az find](/cli/azure/reference-index#az-find) を使用します。 たとえば、`secret` を含むコマンド名を検索するには、次のコマンドを使用します。

```azurecli-interactive
az find secret
```

コマンドと、グループのサブグループの完全な一覧を取得するには、`--help` 引数を使用します。 たとえば、ネットワーク セキュリティ グループ (NSG) で使用する CLI コマンドを確認するには、次のコマンドを使用します。

```azurecli-interactive
az network nsg --help
```

CLI では Bash シェルにコマンドの完全タブ補完が用意されています。

## <a name="globally-available-arguments"></a>グローバルに使用できる引数

引数の中には、すべてのコマンドで使用できるものがあります。

* `--help` は、コマンドとその引数に関する CLI 参照情報を出力し、利用可能なサブグループとコマンドの一覧を表示します。
* `--output` は出力形式を変更します。 使用可能な出力形式は `json`、`jsonc` (色付けされた JSON)、`tsv` (タブ区切り値)、および `table` (人間が判読できる ASCII テーブル)、および `yaml` です。 既定では、CLI は `json` を出力します。 使用可能な出力形式の詳細については、[Azure CLI の出力形式](format-output-azure-cli.md)に関するページをご覧ください。
* `--query` は、[JMESPath クエリ言語](http://jmespath.org/)を使用して、Azure サービスから返された出力をフィルター処理します。 クエリの詳細については、[Azure CLI でのコマンド結果に対するクエリの実行](query-azure-cli.md)に関するページ、および「[JMESPath tutorial (JMESPath チュートリアル)](http://jmespath.org/tutorial.html)」を参照してください。
* `--verbose` は、操作中に Azure で作成されたリソースに関する情報と、その他の有用な情報を出力します。
* `--debug` は、デバッグの目的で使用する、CLI 操作に関する詳細情報を出力します。 バグを見つけた場合は、バグ レポートを送信するときに、`--debug` フラグをオンにして生成した出力を提供してください。

## <a name="interactive-mode"></a>対話モード

CLI には対話モードが用意されています。このモードでは、ヘルプ情報が自動的に表示され、サブコマンドが選択しやすくなっています。 対話モードには [az interactive](/cli/azure/reference-index#az-interactive) コマンドで切り替えます。

```azurecli-interactive
az interactive
```

対話モードの詳細については、[Azure CLI 対話モード](interactive-azure-cli.md)に関する記事をご覧ください。

また、オートコンプリート、マウス オーバー ドキュメントなど、対話型エクスペリエンスを提供する [Visual Studio Code プラグイン](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)も用意されています。

## <a name="learn-cli-basics-with-quickstarts-and-tutorials"></a>クイックスタートとチュートリアルを利用して CLI の基本について学習する

Azure CLI の使用を開始するには、詳細なチュートリアルをお試しください。チュートリアルでは、仮想マシンを設定し、CLI の機能を使用して Azure リソースにクエリを実行します。

> [!div class="nextstepaction"]
> [Azure CLI での仮想マシンの作成チュートリアル](azure-cli-vm-tutorial.yml)

その他の人気のあるサービス用のクイック スタートもあります。

* [Azure CLI を使用したストレージ アカウントの作成](/azure/storage/common/storage-quickstart-create-storage-account-cli)
* [CLI を使用した Azure Blob Storage との間でのオブジェクトの転送](/azure/storage/blobs/storage-quickstart-blobs-cli)
* [Azure CLI を使用した単一の Azure SQL データベースの作成](/azure/sql-database/sql-database-get-started-cli)
* [Azure CLI を使用した Azure Database for MySQL サーバーの作成](/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli)
* [Azure CLI を使用した Azure Database for PostgreSQL の作成](/azure/postgresql/quickstart-create-server-database-azure-cli)
* [Azure での Python Web アプリの作成](/azure/app-service/app-service-web-get-started-python)
* [Azure Web Apps for Containers でのカスタム Docker Hub イメージの実行](/azure/app-service/containers/quickstart-custom-docker-image)

## <a name="give-feedback"></a>フィードバックを送る

機能強化とバグ解決に活かすために、CLI に関する皆様のご意見をお待ちしております。 [GitHub で問題を報告](https://github.com/azure/azure-cli/issues)するか、CLI の組み込み機能を使用して、[az feedback](/cli/azure/reference-index#az-feedback) コマンドで一般的なフィードバックをお寄せください。

```azurecli-interactive
az feedback
```

## <a name="see-also"></a>関連項目

* [Azure CLI で管理できるサービス](azure-services-the-azure-cli-can-manage.md)
* [Azure CLI の完全なコマンド リファレンス一覧](/cli/azure/reference-index)
* [Azure CLI の使用に関する人気のある記事](popular-articles-using-the-azure-cli.md)
