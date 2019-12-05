---
title: Azure SDK for Python をインストールする
description: pip または GitHub を使用して Azure SDK for Python をインストールする方法。 Azure SDK は、個々のライブラリとしてインストールすることも、完全なパッケージとしてインストールすることもできます。
ms.date: 10/31/2019
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: 39de0959f3d73306412c39b32a4e13766d1500e9
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466404"
---
# <a name="install-the-azure-sdk-for-python"></a>Azure SDK for Python をインストールする

Azure SDK for Python には、Python コードから Azure を操作するための API が用意されています。 必要に応じて SDK から個々のライブラリをインストールすることも、ライブラリの完全なセットをまとめてインストールすることもできます。

Azure SDK for Python は、CPython バージョン2.7 と 3.5.3 以降、および PyPy 5.4 以降でテストおよびサポートされています。 また、開発者は SDK を IronPython や Jython などの他のインタープリターと共に使用しますが、個別の問題や非互換性が発生する場合があります。 Python インタープリターが必要な場合は [python.org/downloads](https://www.python.org/downloads) から最新バージョンをインストールしてください。

## <a name="install-sdk-libraries-using-pip"></a>pip を使用して SDK ライブラリをインストールする

Azure SDK for Python はいくつかの個別のライブラリで構成されており、それぞれが特定の Azure サービスをプロビジョニングまたは使用します。 [SDK ライブラリの一覧](https://github.com/Azure/azure-sdk-for-python/blob/master/packages.md)に表示されている名前と共に `pip install <library>` を使用して、それぞれをインストールできます。 (この一覧には、各ライブラリの便利な README ファイルへのリンクが記載されています。)

たとえば、Azure Storage を使用している場合は、`azure-storage-file`、`azure-storage-blob`、または `azure-storage-queue` ライブラリをインストールすることができます。 Azure Cosmos DB テーブルを使用している場合は、`azure-cosmosdb-table` をインストールします。 Azure Functions は、`azure-functions` ライブラリなどでサポートされています。 `azure-mgmt-` で始まるライブラリは、Azure リソースをプロビジョニングするための API を提供します。

### <a name="install-specific-library-versions"></a>特定のライブラリ バージョンをインストールする

特定のバージョンのライブラリをインストールする必要がある場合は、コマンド ラインでバージョンを指定します。

```bash
pip install azure-storage-blob==12.0.0
```

### <a name="install-preview-packages"></a>プレビュー パッケージをインストールする

Microsoft は、今後の機能をサポートするプレビュー SDK ライブラリを定期的にリリースしています。 ライブラリの最新のプレビューをインストールするには、コマンド ラインで `--pre` フラグを含めます。 

```bash
# Install all preview versions of the Azure SDK for Python
pip install --pre azure

# Install the preview version for azure-storage-blob only.
pip install --pre azure-storage-blob
```

## <a name="verify-sdk-installation-details-with-pip"></a>pip を使用して SDK のインストール詳細を確認する

`pip show <library>` コマンドを使用して、ライブラリがインストールされていることを確認します。 ライブラリがインストールされている場合は、このコマンドによってバージョンとその他の概要情報が表示されます。 ライブラリがインストールされていない場合は、このコマンドで何も表示されません。

```bash
# Check installation of the Azure SDK for Python
pip show azure

# Check installation of a specific library
pip show azure-storage-blob
```

`pip freeze` または `pip list` を使用して、現在の Python 環境にインストールされているすべてのライブラリを表示することもできます。

## <a name="uninstall-azure-sdk-for-python-libraries"></a>Azure SDK for Python ライブラリをアンインストールする

個々のライブラリをアンインストールするには、`pip uninstall <library>` を使用します。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [SDK の使用方法を確認](python-sdk-azure-get-started.yml)
