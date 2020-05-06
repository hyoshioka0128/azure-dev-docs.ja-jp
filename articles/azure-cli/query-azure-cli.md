---
title: Azure CLI でのクエリ コマンドの結果
description: Azure CLI コマンドの出力に対して JMESPath クエリを実行する方法について説明します。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/23/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c9f0afdd626ac8d9af027db02275e5a553595473
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030744"
---
# <a name="query-azure-cli-command-output"></a>Azure CLI コマンドの出力のクエリ

Azure CLI では、`--query` 引数を使用して、コマンドの結果に対して [JMESPath クエリ](http://jmespath.org)を実行します。 JMESPath は、CLI の出力からデータを選択して変更できるようにする、JSON 用のクエリ言語です。 クエリは、表示書式設定の前に JSON 出力に対して実行されます。

`--query` 引数は、Azure CLI のすべてのコマンドでサポートされています。 この記事では、一連の簡単な例を使って、JMESPath の機能の使用方法を説明します。

## <a name="dictionary-and-list-cli-results"></a>CLI の結果である辞書とリスト

JSON 以外の出力形式を使用している場合でも、CLI コマンドの結果は、クエリのためにまず JSON として扱われます。 CLI の結果は、JSON 配列または JSON 辞書です。 配列はインデックスを作成できるオブジェクトのシーケンスであり、辞書はキーを使用してアクセスされる順序指定されていないオブジェクトです。 複数のオブジェクトを返すことが "_できる_" コマンドは配列を返し、"_常に_" 単一オブジェクト "_のみ_" を返すコマンドは辞書を返します。

## <a name="get-properties-in-a-dictionary"></a>辞書のプロパティを取得する

結果として辞書を使用すると、キーだけで最上位からプロパティにアクセスできます。 `.` (__部分式__) 文字は、入れ子になった辞書のプロパティにアクセスする際に使用します。 クエリを導入する前に、`az vm show` コマンドの未変更の出力を見てみましょう。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM -o json
```

このコマンドは辞書を出力します。 一部の内容は省略されています。

```json
{
  "additionalCapabilities": null,
  "availabilitySet": null,
  "diagnosticsProfile": {
    "bootDiagnostics": {
      "enabled": true,
      "storageUri": "https://xxxxxx.blob.core.windows.net/"
    }
  },
  ...
  "osProfile": {
    "adminPassword": null,
    "adminUsername": "azureuser",
    "allowExtensionOperations": true,
    "computerName": "TestVM",
    "customData": null,
    "linuxConfiguration": {
      "disablePasswordAuthentication": true,
      "provisionVmAgent": true,
      "ssh": {
        "publicKeys": [
          {
            "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso",
            "path": "/home/azureuser/.ssh/authorized_keys"
          }
        ]
      }
    },
    "secrets": [],
    "windowsConfiguration": null
  },
  ....
}
```

次のコマンドでは、クエリを追加して、VM への接続を許可された SSH 公開キーを取得します。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query osProfile.linuxConfiguration.ssh.publicKeys -o json
```

```json
[
  {
    "keyData": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso",
    "path": "/home/azureuser/.ssh/authorized_keys"
  }
]
```

## <a name="get-a-single-value"></a>単一の値を取得する

一般的なケースとして、Azure リソース ID、リソース名、ユーザー名、パスワードなどの CLI コマンドから "_1 つ_" の値のみを取得する必要があります。 その場合、ローカルの環境変数に値を格納する必要が発生することも少なくありません。 1 つのプロパティを取得するには、まず、クエリから 1 つのプロパティのみを取得していることを確認します。 最後の例を管理者ユーザー名のみを取得するように変更します。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o json
```

```JSON
"azureuser"
```

これは有効な単一の値のように見えますが、`"` 文字が出力の一部として返されることに注意してください。 これは、オブジェクトが JSON 文字列であることを示します。 この値をコマンドからの出力として環境変数に直接割り当てた場合、引用符はシェルによって解釈 "__されない__" ことに注意してください。

```bash
USER=$(az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o json)
echo $USER
```

```output
"azureuser"
```

これは、ほぼ間違いなく望ましくない状態です。 この場合、戻り値を型情報で囲まない出力形式を使用できます。 この目的のため CLI が提供する最適な出力オプションは `tsv` (タブ区切りの値) です。 特に、単一値のみの値を取得するとき (ディクショナリまたは一覧ではなく)、`tsv` 出力が引用されることはなくなります。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query 'osProfile.adminUsername' -o tsv
```

```output
azureuser
```

`tsv` 出力形式について詳しくは、「[Output formats - TSV output format (出力形式 - TSV 出力形式)](format-output-azure-cli.md#tsv-output-format)」を参照してください。

## <a name="get-multiple-values"></a>複数の値を取得する

複数のプロパティを取得するには、式をコンマ区切りリストとして角かっこ `[ ]` (__複数選択リスト__) で囲みます。 VM 名、管理者ユーザー、SSH キーのすべてを一度に取得するには、次のコマンドを使用します。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '[name, osProfile.adminUsername, osProfile.linuxConfiguration.ssh.publicKeys[0].keyData]' -o json
```

```json
[
  "TestVM",
  "azureuser",
  "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso"
]
```

これらの値は、クエリで指定された順序で結果の配列に表示されます。 結果は配列であるため、結果に関連付けられたキーはありません。

## <a name="rename-properties-in-a-query"></a>クエリでプロパティの名前を変更する

複数の値のクエリを実行するときに、配列ではなく、辞書を取得するには、`{ }` (__複数選択ハッシュ__) 演算子を使用します。
複数選択ハッシュの形式は、`{displayName:JMESPathExpression, ...}` です。
`displayName` は出力に表示される文字列であり、`JMESPathExpression` は評価する JMESPath 式です。 複数選択リストをハッシュに変更して、前のセクションの例を変更します。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '{VMName:name, admin:osProfile.adminUsername, sshKey:osProfile.linuxConfiguration.ssh.publicKeys[0].keyData }' -o json
```

```json
{
  "VMName": "TestVM",
  "admin": "azureuser",
  "ssh-key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso"
}
```

## <a name="get-properties-in-an-array"></a>配列内のプロパティを取得する

配列には独自のプロパティはありませんが、インデックスを作成できます。 この機能は、前の例の式 `publicKeys[0]` で示されています。これは、`publicKeys` 配列の最初の要素を取得します。 CLI の出力が順序付けられているという保証はないため、順序が確かである場合や取得する要素を気にかけない場合を除き、インデックスを使用しないようにしてください。 配列内の要素のプロパティにアクセスするには、"_フラット化_" と "_フィルター処理_" の 2 つの操作のいずれかを実行します。 このセクションでは、配列をフラット化する方法について説明します。

配列のフラット化は、`[]` JMESPath 演算子を使用して実行します。 `[]` 演算子の後のすべての式が、現在の配列内の各要素に適用されます。
`[]` がクエリの先頭にある場合、CLI コマンドの結果がフラット化されます。 この機能を使用して、`az vm list` の結果を検査できます。
リソース グループ内の各 VM の名前、OS、管理者名を取得するには、次のコマンドを使用します。

```azurecli-interactive
az vm list -g QueryDemo --query '[].{Name:name, OS:storageProfile.osDisk.osType, admin:osProfile.adminUsername}' -o json
```

```json
[
  {
    "Name": "Test-2",
    "OS": "Linux",
    "admin": "sttramer"
  },
  {
    "Name": "TestVM",
    "OS": "Linux",
    "admin": "azureuser"
  },
  {
    "Name": "WinTest",
    "OS": "Windows",
    "admin": "winadmin"
  }
]
```

`--output table` 出力形式と組み合わせた場合、列名は複数選択ハッシュの `displayKey` 値と一致します。

```azurecli-interactive
az vm list -g QueryDemo --query '[].{Name:name, OS:storageProfile.osDisk.osType, Admin:osProfile.adminUsername}' --output table
```

```output
Name     OS       Admin
-------  -------  ---------
Test-2   Linux    sttramer
TestVM   Linux    azureuser
WinTest  Windows  winadmin
```

> [!NOTE]
>
> 特定のキーはフィルター処理され、テーブル ビューには出力されません。 これらのキーは、`id`、`type`、および`etag` です。 これらの値を表示するには、複数選択ハッシュのキー名を変更します。
>
> ```azurecli-interactive
> az vm show -g QueryDemo -n TestVM --query "{objectID:id}" -o table
> ```

コマンドから返される最上位の結果だけでなく、配列もフラット化できます。 前のセクションでは、式 `osProfile.linuxConfiguration.ssh.publicKeys[0].keyData` を使用して、サインイン用の SSH 公開キーを取得しました。 "_すべての_" SSH 公開キーを取得するには、この式を `osProfile.linuxConfiguration.ssh.publicKeys[].keyData` と記述します。
このクエリ式では、`osProfile.linuxConfiguration.ssh.publicKeys` 配列をフラット化し、各要素に対して `keyData` 式を実行します。

```azurecli-interactive
az vm show -g QueryDemo -n TestVM --query '{VMName:name, admin:osProfile.adminUsername, sshKeys:osProfile.linuxConfiguration.ssh.publicKeys[].keyData }' -o json
```

```json
{
  "VMName": "TestVM",
  "admin": "azureuser",
  "sshKeys": [
    "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDMobZNJTqgjWn/IB5xlilvE4Y+BMYpqkDnGRUcA0g9BYPgrGSQquCES37v2e3JmpfDPHFsaR+CPKlVr2GoVJMMHeRcMJhj50ZWq0hAnkJBhlZVWy8S7dwdGAqPyPmWM2iJDCVMVrLITAJCno47O4Ees7RCH6ku7kU86b1NOanvrNwqTHr14wtnLhgZ0gQ5GV1oLWvMEVg1YFMIgPRkTsSQKWCG5lLqQ45aU/4NMJoUxGyJTL9i8YxMavaB1Z2npfTQDQo9+womZ7SXzHaIWC858gWNl9e5UFyHDnTEDc14hKkf1CqnGJVcCJkmSfmrrHk/CkmF0ZT3whTHO1DhJTtV stramer@contoso\n"
  ]
}
```

## <a name="filter-arrays"></a>配列をフィルター処理する

配列からデータを取得するときに使用するもう 1 つの操作は "_フィルター処理_" です。 フィルター処理は、`[?...]` JMESPath 演算子を使用して実行します。
この演算子は、その内容として述語を受け取ります。 述語とは、`true` または `false` に評価できるステートメントです。 述語が `true` に評価された式が出力に含まれます。

JMESPath では、標準の比較演算子と論理演算子が提供されています。 これらには、`<`、`<=`、`>`、`>=`、`==`、`!=` が含まれます。
JMESPath では、論理 AND (`&&`)、論理 OR (`||`)、論理 NOT (`!`) もサポートされています。 式をかっこ内にグループ化できるので、より複雑な述語式を使用できます。 述語と論理演算の詳細については、[JMESPath の仕様](http://jmespath.org/specification.html)をご覧ください。

前のセクションでは、配列をフラット化して、リソース グループ内のすべての VM の完全なリストを取得しました。 フィルターを使用すると、この出力を Linux VM だけに制限できます。

```azurecli-interactive
az vm list -g QueryDemo --query "[?storageProfile.osDisk.osType=='Linux'].{Name:name,  admin:osProfile.adminUsername}" --output table
```

```output
Name    Admin
------  ---------
Test-2  sttramer
TestVM  azureuser
```

> [!IMPORTANT]
>
> JMESPath では、文字列は常に単一引用符 (`'`) で囲みます。 フィルター述語の文字列の一部として二重引用符を使用すると、空の出力が返されます。

JMESPath には、フィルター処理に役立つ組み込み関数もあります。 このような関数の 1 つである `contains(string, substring)` は、文字列に部分文字列が含まれているかどうかを調べます。 この関数を呼び出す前に式が評価されるので、最初の引数には完全な JMESPath 式を指定できます。 次の例では、OS ディスクに SSD ストレージを使用するすべての VM を検出します。

```azurecli-interactive
az vm list -g QueryDemo --query "[?contains(storageProfile.osDisk.managedDisk.storageAccountType,'SSD')].{Name:name, Storage:storageProfile.osDisk.managedDisk.storageAccountType}" -o json
```

```json
[
  {
    "Name": "TestVM",
    "Storage": "StandardSSD_LRS"
  },
  {
    "Name": "WinTest",
    "Storage": "StandardSSD_LRS"
  }
]
```

このクエリは少し長くなっています。 `storageProfile.osDisk.managedDisk.storageAccountType` キーが 2 回記述され、出力でキー更新されています。 これを短くする 1 つの方法として、データをフラット化して選択した後にフィルターを適用します。

```azurecli-interactive
az vm list -g QueryDemo --query "[].{Name:name, Storage:storageProfile.osDisk.managedDisk.storageAccountType}[?contains(Storage,'SSD')]" -o json
```

```json
[
  {
    "Name": "TestVM",
    "Storage": "StandardSSD_LRS"
  },
  {
    "Name": "WinTest",
    "Storage": "StandardSSD_LRS"
  }
]
```

大きな配列では、データを選択する前にフィルターを適用する方が速い場合があります。

関数の全一覧については、[JMESPath の仕様の組み込み関数](http://jmespath.org/specification.html#built-in-functions)に関するセクションをご覧ください。

## <a name="change-output"></a>出力を変更する

JMESPath の関数には別の目的もあります。それは、クエリの結果を操作することです。 ブール値以外の値を返す関数では、式の結果が変更されます。
たとえば、`sort_by(array, &sort_expression)` を使用して、データをプロパティ値で並べ替えることができます。 JMESPath では、関数の一部として後で評価する必要がある式に、特殊演算子の `&` を使用します。 次の例は、VM のリストを OS ディスク サイズで並べ替える方法を示しています。

```azurecli-interactive
az vm list -g QueryDemo --query "sort_by([].{Name:name, Size:storageProfile.osDisk.diskSizeGb}, &Size)" --output table
```

```output
Name     Size
-------  ------
TestVM   30
Test-2   32
WinTest  127
```

関数の全一覧については、[JMESPath の仕様の組み込み関数](http://jmespath.org/specification.html#built-in-functions)に関するセクションをご覧ください。

## <a name="experiment-with-queries-interactively"></a>対話形式でのクエリの実験

JMESPath で実験を開始できるように、[JMESPath-terminal](https://github.com/jmespath/jmespath.terminal) Python パッケージには、クエリを操作するための対話型環境が用意されています。 データを入力としてパイプ処理してから、クエリを記述し、エディターで実行します。

```bash
pip install jmespath-terminal
az vm list --output json | jpterm
```
