---
title: Azure SDK for Python ライブラリをインストールする
description: pip を使用して Azure SDK または Python ライブラリをインストール、アンインストール、および検証する方法。 特定のバージョンおよびプレビュー パッケージをインストールする方法の詳細も含まれています。
ms.date: 05/13/2020
ms.topic: conceptual
ms.openlocfilehash: 302d17211480f9c8793d7be1de20a6ab5dec3c95
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2020
ms.locfileid: "83403670"
---
# <a name="install-azure-sdk-for-python-libraries"></a>Azure SDK for Python ライブラリをインストールする

Azure SDK for Python には、Python コードから Azure を操作するための API が用意されています。 現在のすべてのライブラリの名前は、[Azure SDK for Python インデックス ページ](https://azure.github.io/azure-sdk/releases/latest/all/python.html)にあります。

名前が `azure-mgmt` で始まるライブラリは "*管理*" ライブラリです。これを使用して、[Azure portal](https://portal.azure.com) または [Azure CLI](/cli/azure/install-azure-cli) を使用する場合と同様に、Azure リソースのプロビジョニングと管理を行います。 たとえば、Azure Storage リソースをプロビジョニングして管理するには、`azure-mgmt-storage` ライブラリを使用します。

SDK 内の他のすべてのライブラリは、既にプロビジョニングされているリソースを操作するためにアプリケーション コードから使用する "*クライアント*" ライブラリです。 たとえば、アプリケーション コードから Azure Storage BLOB を操作するには、`azure-storage-blob` ライブラリを使用します。

## <a name="install-the-latest-version-of-a-library"></a>ライブラリの最新バージョンをインストールする

```bash
pip install azure-storage-blob
```

`pip install` を実行すると、ライブラリの最新バージョンが現在の Python 環境にインストールされます。

Linux システムでは、SDK は `sudo pip install` を使用してすべてのユーザーにライブラリをインストールすることをサポートしていません。 各ユーザーは `pip install` を個別に使用する必要があります。

## <a name="install-specific-library-versions"></a>特定のライブラリ バージョンをインストールする

```bash
pip install azure-storage-blob==12.0.0
```

`pip install` を使用して、コマンド ラインでバージョンを指定します。

## <a name="install-preview-packages"></a>プレビュー パッケージをインストールする

```bash
pip install --pre azure-storage-blob
```

ライブラリの最新のプレビューをインストールするには、コマンド ラインで `--pre` フラグを含めます。

Microsoft では、今後予定されている機能をサポートするプレビュー SDK ライブラリを定期的にリリースしています。ライブラリは変更される可能性があるため、実稼働プロジェクトでは使用できません。

## <a name="verify-a-library-installation"></a>ライブラリのインストールの確認

```bash
pip show azure-storage-blob
```

`pip show <library>` を使用して、ライブラリがインストールされていることを確認します。 ライブラリがインストールされている場合は、このコマンドによってバージョンとその他の概要情報が表示されます。それ以外の場合、コマンドでは何も表示されません。

`pip freeze` または `pip list` を使用して、現在の Python 環境にインストールされているすべてのライブラリを表示することもできます。

## <a name="uninstall-a-library"></a>ライブラリをアンインストールする

```bash
pip uninstall azure-storage-blob
```

ライブラリをアンインストールするには、`pip uninstall <library>` を使用します。

## <a name="next-steps"></a>次のステップ

これで、コードを記述して実行する準備ができました。これは、次の例のいずれかを使用して実行できます。

> [!div class="nextstepaction"]
> [例:リソース グループを作成する >>>](azure-sdk-example-resource-group.md)
