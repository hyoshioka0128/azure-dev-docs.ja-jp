---
title: Python 用 Azure SDK ライブラリ パッケージをインストールする方法
description: pip を使用して Azure SDK または Python ライブラリをインストール、アンインストール、および検証する方法。 特定のバージョンおよびプレビュー パッケージをインストールする方法の詳細も含まれています。
ms.date: 05/26/2020
ms.topic: conceptual
ms.openlocfilehash: f50de734ab1d007c9e5efac8cd6559a2c03d83f5
ms.sourcegitcommit: efab6be74671ea4300162e0b30aa8ac134d3b0a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/01/2020
ms.locfileid: "84256257"
---
# <a name="how-to-install-azure-library-packages-for-python"></a>Python 用 Azure ライブラリ パッケージをインストールする方法

Azure SDK for Python は、[Azure SDK for Python のインデックス ページ](https://azure.github.io/azure-sdk/releases/latest/all/python.html)に記載されている多数の個別ライブラリのみで構成されています。 `pip install` を使用して、プロジェクトに必要な特定のライブラリ パッケージをインストールします。

これらのライブラリを使用すると、(名前に `-mgmt` が付いた管理ライブラリを使用して) Azure サービス上のリソースをプロビジョニングして管理し、(クライアント ライブラリを使用して) アプリ コードからそれらのリソースに接続できます。

## <a name="install-the-latest-version-of-a-library"></a>ライブラリの最新バージョンをインストールする

```cmd
pip install azure-storage-blob
```

```cmd
pip install azure-mgmt-storage
```

`pip install` は、現在の Python 環境にライブラリの最新バージョンを取得します。

Linux システムでは、ユーザーごとに別々にライブラリをインストールする必要があります。 `sudo pip install` を使用してすべてのユーザーのライブラリをインストールすることはサポートされていません。

## <a name="install-specific-library-versions"></a>特定のライブラリ バージョンをインストールする

```cmd
pip install azure-storage-blob==12.0.0
```

```cmd
pip install azure-mgmt-storage==10.0.0
```

`pip install` を使用して、コマンド ラインで目的のバージョンを指定します。

## <a name="install-preview-packages"></a>プレビュー パッケージをインストールする

```cmd
pip install --pre azure-storage-blob
```

```cmd
pip install --pre azure-mgmt-storage
```

ライブラリの最新のプレビューをインストールするには、コマンド ラインで `--pre` フラグを含めます。

Microsoft では、今後予定されている機能をサポートするプレビュー ライブラリ パッケージを定期的にリリースしています。ライブラリは変更される可能性があるため、実稼働プロジェクトでは使用できないことに注意してください。

## <a name="verify-a-library-installation"></a>ライブラリのインストールの確認

```cmd
pip show azure-storage-blob
```

```cmd
pip show azure-mgmt-storage
```

`pip show <library>` を使用して、ライブラリがインストールされていることを確認します。 ライブラリがインストールされている場合は、このコマンドによってバージョンとその他の概要情報が表示されます。それ以外の場合、コマンドでは何も表示されません。

`pip freeze` または `pip list` を使用して、現在の Python 環境にインストールされているすべてのライブラリを表示することもできます。

## <a name="uninstall-a-library"></a>ライブラリをアンインストールする

```cmd
pip uninstall azure-storage-blob
```

ライブラリをアンインストールするには、`pip uninstall <library>` を使用します。
