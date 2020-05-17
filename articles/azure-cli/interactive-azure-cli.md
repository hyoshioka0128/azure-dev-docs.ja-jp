---
title: Azure CLI 対話モード
description: 対話モードで Azure CLI を使用します。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 7b3ee1e284e7f771c661bb65bf8b8ab53dafd77f
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031174"
---
# <a name="azure-cli-interactive-mode"></a>Azure CLI 対話モード

Azure CLI を対話モードで使用するには、`az interactive` コマンドを実行します。
このモードにより対話型シェルに移動することで、オートコンプリート、コマンドの説明、および例にアクセスできるようになります。

![対話モード](./media/interactive-azure-cli/webapp-create.png)

> [!NOTE]
> 黒の背景でも読みやすさは変わらないため、ここでは既定のスタイルを使用していません。

まだアカウントにサインインしていない場合は、`login` コマンドを使用します。

## <a name="configure"></a>[構成]

対話モードでは、必要に応じて、コマンドの説明、パラメーターの説明、およびコマンドの例が表示されます。
説明と例のオン/オフを切り替えるには、`F1` を使用します。

![説明と例](./media/interactive-azure-cli/descriptions-and-examples.png)

パラメーターの既定値の表示のオン/オフを切り替えるには、`F2` を使用します。

![既定値](./media/interactive-azure-cli/defaults.png)

`F3` では、一部のキー ジェスチャの表示を切り替えることができます。

![ジェスチャ](./media/interactive-azure-cli/gestures.png)

## <a name="scope"></a>スコープ

対話モードのスコープを、`vm`、`vm image` などの特定のコマンド グループに指定することができます。
これを行うと、すべてのコマンドがそのスコープ内で解釈されます。
そのコマンド グループですべての作業を行う場合は、これを使って簡略化できます。

次のコマンドを入力する代わりに、

```azurecli
az>> vm create -n myVM -g myRG --image UbuntuLTS
az>> vm list -o table
```

次のように、vm コマンド グループにスコープを指定して、対象のコマンドを入力します。

```azurecli
az>> %%vm
az vm>> create -n myVM -g myRG --image UbuntuLTS
az vm>>list -o table
```

下位レベルのコマンド グループにもスコープを指定できます。
`vm image` にスコープを指定するには、`%%vm image` を使用します。
この場合、既に `vm` にスコープを指定しているため、`%%image` を使用します。

```azurecli
az vm>> %%image
az vm image>>
```

この時点で、`%%..` を使用すると、スコープを `vm` に戻すことができます。また、`%%` だけを指定すると、スコープをルートに指定できます。

```azurecli
az vm image>> %%
az>>
```

## <a name="query"></a>クエリ

`??` と、その後に JMESPath クエリを指定して、最後に実行したコマンドの結果に対して、JMESPath クエリを実行することができます。
たとえば、グループの作成後に、その新しいグループの ID を取得することができます。

```azurecli
az>> group create -n myRG -l westEurope
az>> "?? id"
```

この構文では前に実行したコマンドの結果を引数として使用することもできます。* たとえば、すべてのグループを一覧表示した後に、最初のグループ内で西ヨーロッパにある `virtualMachine` の種類のリソースをすべて一覧表示することができます。 

```azurecli
az>> vm create --name myVM --resource-group myRG --image UbuntuLTS --no-wait -o json
az>> group list -o json
az>> resource list -g "?? [?location=='westeurope'].name | [0]" --query "[?type=='Microsoft.Compute/virtualMachines'].name
```

コマンドの結果に対してクエリを実行する方法の詳細については、[Azure CLI でのコマンド結果に対するクエリの実行](query-azure-cli.md)に関するページをご覧ください。

## <a name="bash-commands"></a>Bash コマンド

`#[cmd]` を使用して、対話モードのままシェル コマンドを実行できます。

```azurecli
az>> #dir
```

## <a name="examples"></a>例

一部のコマンドには例が多数あります。
次の例のページにスクロールするには、`CTRL-N` を使用します。前のページに戻るには、`CTRL-Y` を使用します。

![使用例](./media/interactive-azure-cli/examples.png)

`::#` を使用して、特定の例を確認することもできます。

```azurecli
az>> vm create ::8
```
