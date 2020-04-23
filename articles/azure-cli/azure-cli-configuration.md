---
title: Azure CLI 構成オプション
description: Azure CLI を構成する方法
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 06/11/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: ff5f9f5a5add52bc05009a42aeb00855eb2703fa
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030844"
---
# <a name="azure-cli-configuration"></a>Azure CLI の構成

Azure CLI を使用すると、ログ記録、データ収集、既定の引数値などの設定をユーザーが構成できます。
CLI には、いくつかの既定値を管理するための便利なコマンドとして、`az configure` が用意されています。 他の値については、構成ファイルまたは環境変数を使用して設定できます。

CLI で使用される構成値は、次の優先順位で上から順番に評価されます。

1. コマンドライン パラメーター
2. 環境変数
3. 構成ファイルの値または `az configure` で設定された値

## <a name="cli-configuration-with-az-configure"></a>az configure での CLI 構成

CLI の既定値を [az configure](/cli/azure/reference-index#az-configure) コマンドで設定します。
このコマンドは 1 つの引数 `--defaults` を取ります。この引数は、スペースで区切られた `key=value` ペアのリストです。 指定した値は、必須の引数の代わりに、CLI によって使用されます。

次の表に、使用可能な構成キーの一覧を示します。

| 名前 | 説明 |
|------|-------------|
| group | すべてのコマンドに使用する既定のリソース グループ。 |
| location | すべてのコマンドに使用する既定の場所。 |
| web | `az webapp` コマンドに使用する既定のアプリ名。 |
| vm | `az vm` コマンドに使用する既定の VM 名。 |
| vmss | `az vmss` コマンドに使用する既定の仮想マシン スケール セット (VMSS) 名。 |
| acr | `az acr` コマンドに使用する既定のコンテナー レジストリ名。 |

例として、すべてのコマンドの既定のリソース グループと場所を設定する方法を次に示します。

```azurecli-interactive
az configure --defaults location=westus2 group=MyResourceGroup
```

## <a name="cli-configuration-file"></a>CLI 構成ファイル

CLI 構成ファイルには、CLI の動作の管理に使用されるその他の設定が含まれています。 構成ファイル自体は `$AZURE_CONFIG_DIR/config` にあります。 `AZURE_CONFIG_DIR` の既定値は、Linux と macOS の場合は `$HOME/.azure`、Windows の場合は `%USERPROFILE%\.azure` です。

構成ファイルは、INI ファイル形式で記述されます。 このファイルの形式は、セクション ヘッダーと、これに続くキーと値のエントリの一覧となります。

* セクション ヘッダーは、`[section-name]` のように記述されます。 セクション名は、大文字と小文字が区別されます。
* エントリは、`key=value` のように記述されます。 キー名の大文字と小文字は、区別されません。
* `#` または `;` で始まる行はすべてコメントです。 インライン コメントは許可されていません。

ブール値は、大文字と小文字が区別されず、次の値によって表されます。

* __True__:1、yes、true、on
* __False__:0、no、false、off

次の例は、確認のプロンプトを無効にして、`/var/log/azure` ディレクトリへのログ記録を設定する CLI 構成ファイルです。

```ini
[core]
disable_confirm_prompt=Yes

[logging]
enable_log_file=yes
log_dir=/var/log/azure
```

使用できる構成値とその意味の詳細については、次のセクションを参照してください。 INI ファイル形式の詳細については、[INI に関する Python のドキュメント](https://docs.python.org/3/library/configparser.html#supported-ini-file-structure)を参照してください。

## <a name="cli-configuration-values-and-environment-variables"></a>CLI 構成値と環境変数

次の表は、構成ファイルで指定できるすべてのセクションとオプションの名前を示しています。 対応する環境変数は、`AZURE_{section}_{name}` のようにすべて大文字で設定できます。 たとえば、`batchai` の既定値 `storage_account` は、`AZURE_BATCHAI_STORAGE_ACCOUNT` 変数で設定されます。

既定値を指定すると、その引数は任意のコマンドで不要になります。 代わりに、その既定値が使用されます。

| Section | 名前      | Type | 説明|
|---------|-----------|------|------------|
| __core__ | output | string | 既定の出力形式。 `json`、`jsonc`、`tsv`、`table` のいずれかを指定できます。 |
| | disable\_confirm\_prompt | boolean | 確認のプロンプトをオン/オフにします。 |
| | collect\_telemetry | boolean | Microsoft による、CLI の使用に関する匿名データの収集を許可します。 プライバシー情報については、[Azure CLI の使用条件](https://aka.ms/AzureCliLegal)に関するページをご覧ください。 |
| __logging__ | enable\_log\_file | boolean | ログ記録をオン/オフにします。 |
| | log\_dir | string | ログを書き込むディレクトリ。 この値の既定値は `${AZURE_CONFIG_DIR}/logs` です。 |
| __storage__ | connection\_string | string | `az storage` コマンドに使用する既定の接続文字列。 |
| | account | string | `az storage` コマンドに使用する既定のアカウント名。 |
| | key | string | `az storage` コマンドに使用する既定のアカウント キー。 |
| | sas\_token | string | `az storage` コマンドに使用する既定の SAS トークン。 |
| __batchai__ | storage\_account | string | `az batchai` コマンドに使用する既定のストレージ アカウント。 |
| | storage\_key | string | `az batchai` コマンドに使用する既定のストレージ キー。 |
| __batch__ | account | string | `az batch` コマンドに使用する既定の Azure Batch アカウント名。 |
| | access\_key | string | `az batch` コマンドに使用する既定のアクセス キー。 `aad` 承認でのみ使用されます。 |
| | endpoint | string | `az batch` コマンドに対する既定の接続先エンドポイント。 |
| | auth\_mode | string | `az batch` コマンドに使用する承認モード。 `shared_key` または `aad` を指定できます。 |
| __cloud__ | name | string | すべての `az` コマンドの既定のクラウド。  値には `AzureCloud` (既定)、`AzureChinaCloud`、`AzureUSGovernment`、`AzureGermanCloud` があります。 クラウドを変更するには、`az cloud set –name` コマンドを使用します。  例については、[Azure CLI を使用したクラウドの管理](manage-clouds-azure-cli.md)に関するページを参照してください。 |

> [!NOTE]
> 構成ファイルに他の値が含まれる場合もありますが、その値は、`az configure` などの CLI コマンドで直接管理されます。 上記の表は、自身で変更する必要がある値のみを示しています。
