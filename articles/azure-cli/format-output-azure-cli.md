---
title: Azure CLI の出力形式
description: テーブル、リスト、または JSON への Azure CLI の出力形式を設定する方法について説明します。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 7f28a5cc7acd7e5d41e1ab91424a9f360a25b702
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031034"
---
# <a name="output-formats-for-azure-cli-commands"></a>Azure CLI コマンドの出力形式

Azure CLI では既定の出力形式として JSON が使用されますが、他の形式も用意されています。  CLI の出力を書式設定するには、`--output` (`--out` または `-o`) パラメーターを使用します。 引数値と出力の種類は、次のとおりです。

--output | 説明
---------|-------------------------------
`json`   | JSON 文字列。 この設定が既定値です
`jsonc`  | 色付けされた JSON
`yaml`   | YAML。JSON に代わる、コンピューターが読み取り可能な形式
`table`  | 列見出しとしてキーが使用されている ASCII テーブル
`tsv`    | タブ区切りの値 (キーなし)
`none`   | エラーと警告以外は出力されません

## <a name="json-output-format"></a>JSON 出力形式

次の例では、既定の JSON 形式で、サブスクリプション内の仮想マシンの一覧が表示されます。

```azurecli-interactive
az vm list --output json
```

次の出力では、簡潔にするため、また置き換えた情報を識別するために、一部のフィールドが省略されています。

```json
[
  {
    "availabilitySet": null,
    "diagnosticsProfile": null,
    "hardwareProfile": {
      "vmSize": "Standard_DS1"
    },
    "id": "/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010",
    "instanceView": null,
    "licenseType": null,
    "location": "westus",
    "name": "DemoVM010",
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/.../resourceGroups/demorg1/providers/Microsoft.Network/networkInterfaces/DemoVM010VMNic",
          "primary": null,
          "resourceGroup": "demorg1"
        }
      ]
    },
          ...
          ...
          ...
]
```

## <a name="yaml-output-format"></a>YAML 出力形式

`yaml` 形式では、出力が [YAML](http://yaml.org/) として、つまりプレーン テキスト データのシリアル化形式として出力されます。 YAML は JSON より読みやすくなる傾向があり、その形式に簡単にマップされます。 一部のアプリケーションおよび CLI コマンドは、構成の入力として、JSON ではなく YAML を受け取ります。

```azurecli-interactive
az vm list --out yaml
```

次の出力では、簡潔にするため、また置き換えた情報を識別するために、一部のフィールドが省略されています。

```yaml
- availabilitySet: null
  diagnosticsProfile: null
  hardwareProfile:
    vmSize: Standard_DS1_v2
  id: /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010
  identity: null
  instanceView: null
  licenseType: null
  location: westus
  name: ExampleVM1
  networkProfile:
    networkInterfaces:
    - id: /subscriptions/.../resourceGroups/DemoRG1/providers/Microsoft.Network/networkInterfaces/DemoVM010Nic
      primary: null
      resourceGroup: DemoRG1
  ...
...
```

## <a name="table-output-format"></a>テーブル出力形式

`table` 形式では出力が ASCII テーブルとして生成されるため、読み取りやスキャンが容易になります。 入れ子になったオブジェクトはテーブル出力には含まれませんが、クエリの一部としてフィルター処理することもできます。 この形式では一部のフィールドがテーブルに含まれないため、人の目ですばやく検索できるデータ概要が必要な場合に最適です。

```azurecli-interactive
az vm list --out table
```

```output
Name         ResourceGroup    Location
-----------  ---------------  ----------
DemoVM010    DEMORG1          westus
demovm212    DEMORG1          westus
demovm213    DEMORG1          westus
KBDemo001VM  RGDEMO001        westus
KBDemo020    RGDEMO001        westus
```

`--query` パラメーターを使用すると、一覧の出力に表示するプロパティと列をカスタマイズすることができます。 次の例は、`list` コマンドで VM 名とリソース グループ名だけを選択する方法を示しています。

```azurecli-interactive
az vm list --query "[].{resource:resourceGroup, name:name}" -o table
```

```output
Resource    Name
----------  -----------
DEMORG1     DemoVM010
DEMORG1     demovm212
DEMORG1     demovm213
RGDEMO001   KBDemo001VM
RGDEMO001   KBDemo020
```

> [!NOTE]
> 一部のキーは、既定でテーブル ビューには出力されません。 `id`、`type`、および `etag` が、これに相当します。 出力でこれらを表示する必要がある場合は、JMESPath キー更新機能を使用してキー名を変更し、フィルター処理を回避します。
>
> ```azurecli-interactive
> az vm list --query "[].{objectID:id}" -o table
> ```

クエリを使用してデータをフィルター処理する方法の詳細については、「[Azure CLI で JMESPath クエリを使用する](/cli/azure/query-azure-cli)」を参照してください。

## <a name="tsv-output-format"></a>TSV 出力形式

`tsv` 出力形式では、追加の書式設定、キー、またはその他の記号なしで、タブと改行で区切られた値が返されます。 この形式を使用すると、なんらかの形式でテキストを処理する必要がある他のコマンドやツールで出力を簡単に利用できるようになります。 `table` 形式と同じく、`tsv` では、入れ子になったオブジェクトは出力されません。

前の例で `tsv` オプションを使用すると、タブ区切りの結果が出力されます。

```azurecli-interactive
az vm list --out tsv
```

```output
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010    None    None    westus    DemoVM010            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    cbd56d9b-9340-44bc-a722-25f15b578444
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm212    None    None    westus    demovm212            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    4bdac85d-c2f7-410f-9907-ca7921d930b4
None    None        /subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm213    None    None    westus    demovm213            None    Succeeded    DEMORG1    None            Microsoft.Compute/virtualMachines    2131c664-221a-4b7f-9653-f6d542fbfa34
None    None        /subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo001VM    None    None    westus    KBDemo001VM            None    Succeeded    RGDEMO001    None            Microsoft.Compute/virtualMachines    14e74761-c17e-4530-a7be-9e4ff06ea74b
None    None        /subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo020   None    None    westus    KBDemo020            None    Succeeded    RGDEMO001    None            Microsoft.Compute/virtualMachines    36baa9-9b80-48a8-b4a9-854c7a858ece
```

TSV 出力形式の制限の 1 つは、出力順序が保証されないことです。 CLI では、順序を最大限維持するために、応答 JSON 内のキーをアルファベット順に並べ替え、次にそれらの値を TSV 出力用の順序で表示します。 これは、順序が常に同じであることを保証するものではありません。Azure サービスの応答形式が変更される可能性があるためです。

一貫した順序を強制するには、`--query` パラメーターと[複数選択リスト](query-azure-cli.md#get-multiple-values)の形式を使用する必要があります。 CLI コマンドが 1 つの JSON 辞書を返す場合は、一般的な形式 (`[key1, key2, ..., keyN]`) を使用してキーの順序を強制します。  配列を返す CLI コマンドの場合、一般的な形式 (`[].[key1, key2, ..., keyN]`) を使用して列の値を並べ替えます。

たとえば、上に表示されている情報を ID、場所、リソース グループ、VM 名で並べ替えるには、次のようにします。

```azurecli-interactive
az vm list --out tsv --query '[].[id, location, resourceGroup, name]'
```

```output
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/DemoVM010    westus    DEMORG1    DemoVM010
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm212    westus    DEMORG1    demovm212
/subscriptions/.../resourceGroups/DEMORG1/providers/Microsoft.Compute/virtualMachines/demovm213    westus    DEMORG1    demovm213
/subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo001VM     westus  RGDEMO001       KBDemo001VM
/subscriptions/.../resourceGroups/RGDEMO001/providers/Microsoft.Compute/virtualMachines/KBDemo020       westus  RGDEMO001       KBDemo020
```

次の例は、`tsv` の出力を Bash の他のコマンドにパイプ処理する方法を示しています。 このクエリを使用して出力をフィルターにかけて順序を強制し、`grep` によって "RGD" というテキストが含まれている項目を選択し、`cut` コマンドによって 4 番目のフィールドの値を選択して VM の名前を出力に表示します。

```azurecli-interactive
az vm list --out tsv --query '[].[id, location, resourceGroup, name]' | grep RGD | cut -f4
```

```output
KBDemo001VM
KBDemo020
```

## <a name="set-the-default-output-format"></a>既定の出力形式を設定する

対話型の `az configure` コマンドを使用して、環境を設定し、出力形式の既定の設定を確立します。 既定の出力形式は `json` です。

```azurecli-interactive
az configure
```

```output
Welcome to the Azure CLI! This command will guide you through logging in and setting some default values.

Your settings can be found at /home/defaultuser/.azure/config
Your current configuration is as follows:

  ...

Do you wish to change your settings? (y/N): y

What default output format would you like?
 [1] json - JSON formatted output that most closely matches API responses.
 [2] jsonc - Colored JSON formatted output that most closely matches API responses.
 [3] table - Human-readable output format.
 [4] tsv - Tab- and Newline-delimited. Great for GREP, AWK, etc.
 [5] yaml - YAML formatted output. An alternative to JSON. Great for configuration files.
 [6] none - No output, except for errors and warnings.
Please enter a choice [1]:
```

ご利用環境の構成方法の詳細については、「[Azure CLI の構成](/cli/azure/azure-cli-configuration)」を参照してください。
