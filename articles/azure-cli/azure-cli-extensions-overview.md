---
title: Azure CLI の拡張機能
description: Azure CLI での拡張機能の使用
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 73488fc464ed052f22f071f6373fb6b8f682b778
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030984"
---
# <a name="use-extensions-with-azure-cli"></a>Azure CLI で拡張機能を使用する 

Azure CLI には、拡張機能を読み込む機能が用意されています。 拡張機能は Python の wheel 形式で、CLI には付属していませんが CLI コマンドとして実行できます。
拡張機能を使用すると、独自の CLI インターフェイスを記述する機能と共に、リリース前の実験的なコマンドにアクセスすることができます。 この記事では、拡張機能の管理方法について説明し、その使用に関する一般的な質問に回答します。

## <a name="find-extensions"></a>拡張機能の検索

Microsoft によって提供および管理されている拡張機能を確認するには、[az extension list-available](/cli/azure/extension#az-extension-list-available) コマンドを使用します。

```azurecli-interactive
az extension list-available --output table
```

Microsoft では、ドキュメント サイトでも[拡張機能の一覧](azure-cli-extensions-list.md)をホストしています。

## <a name="install-extensions"></a>拡張機能のインストール

インストールする拡張機能を見つけたら、[az extension add](https://docs.microsoft.com/cli/azure/extension#az-extension-add) を使用して取得します。 その拡張機能が `az extension list-available` に表示される場合は、名前によってインストールすることができます。

```azurecli-interactive
az extension add --name <extension-name>
```

拡張機能のソースが外部リソースであるか、その拡張機能への直接リンクがある場合は、ソース URL またはローカル パスを指定します。 拡張機能は、コンパイルされた Python の wheel 形式ファイルである "_必要があります_"。

```azurecli-interactive
az extension add --source <URL-or-path>
```

インストールされた拡張機能は、`$AZURE_EXTENSION_DIR` シェル変数の値で確認できます。 この変数が設定されていない場合、既定では、この値は `$HOME/.azure/cliextensions` (Linux と macOS の場合) または `%USERPROFILE%\.azure\cliextensions` (Windows の場合) になります。

## <a name="update-extensions"></a>拡張機能の更新

拡張機能を名前でインストールした場合、[az extension update](https://docs.microsoft.com/cli/azure/extension#az-extension-update) を使用してその拡張機能を更新します。

```azurecli-interactive
az extension update --name <extension-name>
```

それ以外の場合は、「[拡張機能のインストール](#install-extensions)」の説明に従って、ソースから拡張機能を更新できます。

CLI で拡張機能の名前を解決できない場合は、アンインストールしてから再インストールを試みてください。 拡張機能が、基本 CLI の一部になっている可能性もあります。
「[Azure CLI のインストール](install-azure-cli.md)」の説明に従って CLI の更新を試み、拡張機能のコマンドが追加されているかどうかを確認してください。

## <a name="uninstall-extensions"></a>拡張機能のアンインストール

拡張機能が不要になった場合は、[az extension remove](https://docs.microsoft.com/cli/azure/extension#az-extension-remove) を使用して削除します。

```azurecli-interactive
az extension remove --name <extension-name>
```

また、手動で削除することもできます。それには、インストールした場所から拡張機能を削除します。 `$AZURE_EXTENSION_DIR` シェル変数によって、モジュールがインストールされている場所が定義されています。
この変数が設定されていない場合、既定では、この値は `$HOME/.azure/cliextensions` (Linux と macOS の場合) または `%USERPROFILE%\.azure\cliextensions` (Windows の場合) になります。

```bash
rm -rf $AZURE_EXTENSION_DIR/<extension-name>
```

## <a name="faq"></a>よく寄せられる質問

ここでは、CLI 拡張機能に関するその他の一般的な質問に回答します。

### <a name="what-file-formats-are-allowed-for-installation"></a>インストールでは、どのファイル形式が許可されていますか。

現時点では、Python のコンパイルされた wheel 形式のみを拡張機能としてインストールできます。

### <a name="can-extensions-replace-existing-commands"></a>拡張機能で既存のコマンドを置き換えることはできますか。

はい。 拡張機能で既存のコマンドを置き換えることはできますが、置き換えられたコマンドを実行する前に、CLI によって警告が発せられます。

### <a name="how-can-i-tell-if-an-extension-is-in-pre-release"></a>拡張機能がプレリリースかどうかをどのように確認できますか。

拡張機能がプレリリースに含まれているかどうかは、拡張機能のドキュメントとバージョン管理に示されています。 Microsoft では、多くの場合、プレビューのコマンドを CLI 拡張機能としてリリースしますが、後でそのコマンドをメインの CLI 製品に追加します。 コマンドが拡張機能から移動された場合は、古い拡張機能をアンインストールする必要があります。 

### <a name="can-extensions-depend-upon-each-other"></a>拡張機能は相互に依存できますか。

いいえ。 CLI では読み込みの順序が保証されていないため、依存関係が満たされていないことがあります。 拡張機能を削除しても、他の拡張機能には影響しません。

### <a name="are-extensions-updated-along-with-the-cli"></a>拡張機能は CLI と共に更新されますか。

いいえ。 「[拡張機能の更新](#update-extensions)」で説明したように、拡張機能は個別に更新する必要があります。

### <a name="how-to-develop-our-own-extension"></a>独自の拡張機能を開発する方法
詳細については、公式リポジトリを参照してください。 [Azure/azure-cli-extensions](https://github.com/Azure/azure-cli/tree/master/doc/extensions)
