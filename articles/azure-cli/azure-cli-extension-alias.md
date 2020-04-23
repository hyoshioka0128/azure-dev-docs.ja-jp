---
title: Azure CLI のエイリアス拡張機能
description: Azure CLI のエイリアス拡張機能を使用する方法
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 4d324235360e017970a215226572e3effeb9b917
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030854"
---
# <a name="the-azure-cli-alias-extension"></a>Azure CLI のエイリアス拡張機能

エイリアス拡張機能を使用すると、既存のコマンドを使用して、Azure CLI のカスタム コマンドを定義することができます。 エイリアスではショートカットが使用できるので、ワークフローをシンプルに保つことができます。 エイリアスには Jinja2 テンプレート エンジンが使用されているため、高度な引数の処理にも対応できます。

> [!NOTE]
> エイリアス拡張機能はパブリック プレビュー段階にあります。 機能と構成ファイルの形式は変更される可能性があります。

## <a name="install-the-alias-extension"></a>エイリアス拡張機能のインストール

エイリアス拡張機能を使用するために最低限必要な Azure CLI バージョンは **2.0.28** です。 CLI のバージョンを確認するには、`az --version` を実行してください。 インストールを更新する必要がある場合は、「[Azure CLI のインストール](./install-azure-cli.md)」を参照してください。

[az extension add](/cli/azure/extension#az-extension-add) コマンドを使用して拡張機能をインストールします。

```azurecli-interactive
az extension add --name alias
```

[az extension list](/cli/azure/extension#az-extension-list) を使用して拡張機能のインストールを確認します。 エイリアス拡張機能が正しくインストールされていれば、コマンドの出力に表示されます。

```azurecli-interactive
az extension list --output table --query '[].{Name:name}'
```

```output
Name
------
alias
```

## <a name="keep-the-extension-up-to-date"></a>拡張機能を最新の状態に保つ

エイリアス拡張機能は活発に開発が行われており、新しいバージョンが定期的にリリースされます。 CLI を更新しても、新しいバージョンがインストールされるわけではありません。 [az extension update](/cli/azure/extension#az-extension-update) を使用して拡張機能の更新プログラムをインストールします。

```azurecli-interactive
az extension update --name alias
```

## <a name="manage-aliases-for-the-azure-cli"></a>Azure CLI のエイリアスを管理する

エイリアス拡張機能を使用すると、他の CLI コマンドのエイリアスを作成して管理できます。 使用可能なすべてのコマンドおよびパラメーターの詳細を表示するには、`--help` でエイリアス コマンドを実行します。

```azurecli-interactive
az alias --help
```

## <a name="create-simple-alias-commands"></a>単純なエイリアス コマンドを作成する

エイリアスの用途の 1 つは、既存のコマンド グループやコマンド名の短縮です。 たとえば、`group` コマンド グループを `rg` に、`list` コマンドを `ls` に短縮できます。

```azurecli-interactive
az alias create --name rg --command group
az alias create --name ls --command list
```

新しく定義したこれらのエイリアスは、定義が存在する任意の場所で使用できるようになります。

```azurecli-interactive
az rg list
az rg ls
az vm ls
```

コマンドの一部として `az` を含めないでください。

エイリアスは、完全なコマンドのショートカットにすることもできます。 次の例では、使用可能なリソース グループとその位置がテーブル出力で表示されています。

```azurecli-interactive
az alias create --name ls-groups --command "group list --query '[].{Name:name, Location:location}' --output table"
```

これで `ls-groups` を他の CLI コマンドのように実行できます。

```azurecli-interactive
az ls-groups
```

## <a name="create-an-alias-command-with-arguments"></a>引数付きのエイリアス コマンドを作成する

エイリアス名に `{{ arg_name }}` として含めることで、位置引数をエイリアス コマンドに追加することもできます。 中かっこ内には空白が必要です。

```azurecli-interactive
az alias create --name "alias_name {{ arg1 }} {{ arg2 }} ..." --command "invoke_including_args"
```

次の例のエイリアスは、位置引数を使用して VM のパブリック IP アドレスを取得する方法を示しています。

```azurecli-interactive
az alias create \
    --name "get-vm-ip {{ resourceGroup }} {{ vmName }}" \
    --command "vm list-ip-addresses --resource-group {{ resourceGroup }} --name {{ vmName }}
        --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress"
```

このコマンドを実行するときに、位置引数に値を指定します。

```azurecli-interactive
az get-vm-ip MyResourceGroup MyVM
```

エイリアス化されたコマンドで環境変数を使用することもできます。この環境変数は実行時に評価されます。 次の例では、`create-rg` エイリアスを追加します。これにより、`eastus` 内にリソース グループが作成され、`owner` タグが追加されます。 このタグには、ローカル環境変数 `USER` の値が割り当てられます。

```azurecli-interactive
az alias create \
    --name "create-rg {{ groupName }}" \
    --command "group create --name {{ groupName }} --location eastus --tags owner=\$USER"
```

エイリアスのコマンド内に環境変数を登録するには、ドル記号 `$` をエスケープする必要があります。

## <a name="process-arguments-using-jinja2-templates"></a>Jinja2 テンプレートを使用した引数の処理

エイリアス拡張機能での引数の置換は、[Jinja2](http://jinja.pocoo.org/docs/2.10/) によって実行されます。 Jinja2 テンプレートでは、引数を操作することができます。

Jinja2 テンプレートを使用すると、基になるコマンドとは異なる型の引数を受け入れるエイリアスを作成できます。 たとえば、ストレージ URL を受け入れるエイリアスを作成できます。 この URL が解析され、アカウント名とコンテナー名がストレージ コマンドに渡されます。

```azurecli-interactive
az alias create \
    --name 'storage-ls {{ url }}' \
    --command "storage blob list
        --account-name {{ url.replace('https://', '').split('.')[0] }}
        --container-name {{ url.replace('https://', '').split('/')[1] }}"
```

Jinja2 テンプレート エンジンについては、[Jinja2 のドキュメント](http://jinja.pocoo.org/docs/2.10/templates/)を参照してください。

## <a name="alias-configuration-file"></a>エイリアス構成ファイル

エイリアス構成ファイルを変更することで、エイリアスを作成および変更することもできます。 エイリアス コマンド定義は、`$AZURE_USER_CONFIG/alias` にある構成ファイルに書き込まれます。 `AZURE_USER_CONFIG` の既定値は、macOS と Linux の場合は `$HOME/.azure`、Windows の場合は `%USERPROFILE%\.azure` です。 エイリアス構成ファイルは、INI 構成ファイル形式で記述されます。 エイリアス コマンドの形式は次のとおりです。

```ini
[alias_name]
command = invoked_commands
```

位置引数を含むエイリアスの場合、エイリアス コマンドの形式は次のとおりです。

```ini
[alias_name {{ arg1 }} {{ arg2 }} ...]
command = invoked_commands_including_args
```

## <a name="create-an-alias-command-with-arguments-via-the-alias-configuration-file"></a>エイリアス構成ファイルで引数付きのエイリアス コマンドを作成する

次の例では、引数が指定されたコマンドのエイリアスを示します。 このコマンドにより、VM のパブリック IP アドレスが取得されます。 エイリアス化されたコマンドは、1 行で全体を指定する必要があります。また、エイリアス名にはすべての引数を含める必要があります。

```ini
[get-vm-ip {{ resourceGroup }} {{ vmName }}]
command = vm list-ip-addresses --resource-group {{ resourceGroup }} --name {{ vmName }} --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress
```

## <a name="uninstall-the-alias-extension"></a>エイリアス拡張機能のアンインストール

拡張機能をアンインストールするには、[az extension remove](/cli/azure/extension#az-extension-remove) コマンドを使用します。

```azurecli-interactive
az extension remove --name alias
```

拡張機能に関するバグなどの問題が原因でアンインストールした場合は、Microsoft が修正プログラムを提供できるように、[GitHub に問題を提出](https://github.com/Azure/azure-cli-extensions/issues)してください。
