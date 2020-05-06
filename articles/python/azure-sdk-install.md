---
title: Azure SDK for Python ライブラリをインストールする
description: pip を使用して Azure SDK または Python ライブラリをインストール、アンインストール、および検証する方法。 特定のバージョンおよびプレビュー パッケージをインストールする方法の詳細も含まれています。
ms.date: 04/23/2020
ms.topic: conceptual
ms.openlocfilehash: 172091bcdff5ba55ccc7c2f13c60f3c7d645efc1
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82138748"
---
# <a name="install-azure-sdk-for-python-libraries"></a>Azure SDK for Python ライブラリをインストールする

Azure SDK for Python には、Python コードから Azure を操作するための API が用意されています。 現在のすべてのライブラリの名前は、[Azure SDK for Python インデックス ページ](https://azure.github.io/azure-sdk/releases/latest/all/python.html)にあります。

名前が `azure-mgmt` で始まるライブラリは "*管理*" ライブラリです。これを使用して、[Azure portal](https://portal.azure.com) または [Azure CLI](/cli/azure/install-azure-cli) を使用する場合と同様に、Azure リソースのプロビジョニングと管理を行います。 たとえば、Azure Storage リソースをプロビジョニングして管理するには、`azure-mgmt-storage` ライブラリを使用します。

SDK 内の他のすべてのライブラリは、既にプロビジョニングされているリソースを操作するためにアプリケーション コードから使用する "*クライアント*" ライブラリです。 たとえば、アプリケーション コードから Azure Storage BLOB を操作するには、`azure-storage-blob` ライブラリを使用します。

## <a name="install-the-latest-version-of-a-library"></a>ライブラリの最新バージョンをインストールする

`pip install` を実行すると、最新バージョンのライブラリが現在の Python 環境にインストールされます。

```bash
pip install azure-storage-blob
```

Linux システムでは、SDK は `sudo pip install` を使用してすべてのユーザーにライブラリをインストールすることをサポートしていません。 各ユーザーは `pip install` を個別に使用する必要があります。

## <a name="install-specific-library-versions"></a>特定のライブラリ バージョンをインストールする

特定のバージョンのライブラリをインストールする必要がある場合は、コマンド ラインでバージョンを指定します。

```bash
pip install azure-storage-blob==12.0.0
```

## <a name="install-preview-packages"></a>プレビュー パッケージをインストールする

Microsoft では、今後予定されている機能をサポートするプレビュー SDK ライブラリを定期的にリリースしています。ライブラリは変更される可能性があるため、実稼働プロジェクトでは使用できません。

ライブラリの最新のプレビューをインストールするには、コマンド ラインで `--pre` フラグを含めます。

```bash
pip install --pre azure-storage-blob
```

## <a name="verify-a-library-installation"></a>ライブラリのインストールの確認

`pip show <library>` を使用して、ライブラリがインストールされていることを確認します。 ライブラリがインストールされている場合は、このコマンドによってバージョンとその他の概要情報が表示されます。それ以外の場合、コマンドでは何も表示されません。

```bash
pip show azure-storage-blob
```

`pip freeze` または `pip list` を使用して、現在の Python 環境にインストールされているすべてのライブラリを表示することもできます。

## <a name="uninstall-a-library"></a>ライブラリをアンインストールする

ライブラリをアンインストールするには、`pip uninstall <library>` を使用します。

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [SDK の使用方法を確認](azure-sdk-get-started.yml)
