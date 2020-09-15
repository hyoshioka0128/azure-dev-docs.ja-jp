---
title: Azure および Azure Stack 用の Azul Zulu JDK をインストールする
description: Windows、Linux、Mac での Azure 開発用 Azul Zulu Java Development Kit (JDK) のインストール方法
ms.date: 04/19/2019
ms.topic: conceptual
ms.custom: devx-track-java
ms.openlocfilehash: e5b9f0874202373400640687df2b1558f7f41526
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473493"
---
# <a name="install-the-jdk-for-azure-and-azure-stack"></a>Azure 用の JDK および Azure Stack をインストールする

Azul Zulu for Azure - Enterprise Edition JDK ビルドは、Microsoft および Azul Systems によってサポートされる、無料でマルチプラットフォーム対応かつ実稼働可能な、Azure と Azure Stack 用の OpenJDK のディストリビューションです。 これらには、Java SE アプリケーションを構築および実行するためのすべてのコンポーネントが含まれています。

[クライアント OS ごとに複数のダウンロード パッケージ タイプがサポート](https://www.azul.com/downloads/azure-only/zulu/)されています。 [Azure Marketplace ギャラリーから仮想マシン イメージを入手](#get-virtual-machine-images-from-the-azure-marketplace-gallery)することもできます。

> [!NOTE]
> 次の手順は、64 ビット Java 8 バージョンの JDK を対象としています。 Azul には、スタンドアロン インストールとして Java Runtime Environment (JRE) も用意されています。 JRE は JDK のインストールに含まれています。
>
> Java 11 パッケージも [Azul の Azure ダウンロードページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-windows"></a>Windows 用の Azul Zulu for Azure - Enterprise Edition JDK ビルドをダウンロードしてインストールする

1. MSI として 64 ビット Azul Zulu JDK 8 をダウンロードします。

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-win_x64.msi](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-win_x64.msi)
   * または、 *.msi* ファイルの [Zulu 8 バージョン以降](http://repos.azul.com/azure-only/zulu/packages/zulu-8)がないか確認します。

   `C:\Users\<your_login>\Downloads` などのクライアント上の場所にそれを保存します。 (.ZIP パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)

2. そのディレクトリに移動し、ダウンロードした MSI ファイルをダブルクリックしてインストールを開始します。

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-mac"></a>Mac 用の Azul Zulu for Azure - Enterprise Edition JDK ビルドをダウンロードしてインストールする

これらの手順では、ZIP ファイルを Mac にダウンロードします。 DMG バージョンも利用できます。

1. ZIP ファイルとして 64 ビット Azul Zulu JDK 8 をダウンロードします。

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-macosx_x64.zip](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-macosx_x64.zip)
   * または、 *.zip* ファイルの [Zulu 8 バージョン以降](http://repos.azul.com/azure-only/zulu/packages/zulu-8)がないか確認します。

   `/Library/Java/JavaVirtualMachines/` などのクライアント上の場所にそれを保存します。 (.DMG パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)

2. Finder を起動し、ダウンロード ディレクトリに移動して、ZIP ファイルをダブルクリックします。 または、ターミナル コマンド ウィンドウを起動し、そのディレクトリに移動して、次を実行することもできます。

    ```cli
    unzip <name_of_zulu_package>.zip
    ```

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdk-builds-for-alpine-linux"></a>Alpine Linux 用の Azul Zulu for Azure - Enterprise Edition JDK ビルドをダウンロードしてインストールする

1. TAR として 64 ビット Azul Zulu JDK 8 をダウンロードします。

   * [zulu-8-azure-jdk_8.44.0.11-8.0.242-linux_x64.tar.gz](http://repos.azul.com/azure-only/zulu/packages/zulu-8/8u242/zulu-8-azure-jdk_8.44.0.11-8.0.242-linux_x64.tar.gz)
   * または、 *.tar.gz* ファイルの [Zulu 8 バージョン以降](https://repos.azul.com/azure-only/zulu/packages/zulu-8)がないか確認します。

   `/usr/lib/jvm` などのクライアント上の場所にそれを保存します。 (.RPM と .DEB パッケージも [Azul の Azure ダウンロード ページ](https://www.azul.com/downloads/azure-only/zulu/)で提供されています。)

2. 該当のディレクトリに移動し、次のコマンドを実行してファイルを解凍し、展開します。

    ```cli
    tar -xvf <name_of_zulu_package>.tar
    ```

## <a name="confirm-your-installation"></a>インストールを確認する

インストールを確認するには、コマンド ラインに移動し、`java -version` を実行します。

このコマンドの出力は次の例のようになります。

```cli
$ java -version

openjdk version "1.8.0_242"
OpenJDK Runtime Environment (Zulu 8.44.0.11-linux64)-Microsoft-Azure-restricted (build 1.8.0_242-b20)
OpenJDK 64-Bit Server VM (Zulu 8.44.0.11-linux64)-Microsoft-Azure-restricted (build 25.242-b20, mixed mode)
```

## <a name="get-virtual-machine-images-from-the-azure-marketplace-gallery"></a>Azure Marketplace ギャラリーから仮想マシン イメージを入手する

次のプラットフォーム用の仮想マシン イメージを入手できます。

* [Azul Zulu:Ubuntu 20.04 上の Java 8](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-ubuntu-2004?tab=Overview)
* [Azul Zulu:Windows Server 2019 上の Java 8](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu8-windows-2019)
* [Azul Zulu:Ubuntu 20.04 上の Java 11](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-ubuntu-2004?tab=Overview)
* [Azul Zulu:Windows Server 2019 上の Java 11](https://azuremarketplace.microsoft.com/marketplace/apps/azul.azul-zulu11-windows-2019)

## <a name="download-and-install-the-azul-zulu-for-azure---enterprise-edition-jdks-from-a-yum-repository"></a>Azul Zulu for Azure - Enterprise Edition JDK ビルドを Yum リポジトリからダウンロードしてインストールする

Azul Zulu JDK は、Azul によって [Yum リポジトリ](https://repos.azul.com/azure-only/zulu-azure.repo)で提供されています。

**Java 8 用の Azul Zulu JDK をインストールするには、CLI から次のコマンドを実行します。**

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-8-azure-jdk
```

Java 11 の場合は、次を実行します。

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-11-azure-jdk
```

Java 13 (プレビュー) の場合は、次を実行します。

```cli
sudo rpm --import http://repos.azul.com/azul-repo.key
sudo curl http://repos.azul.com/azure-only/zulu-azure.repo -o /etc/yum.repos.d/zulu-azure.repo
sudo yum -q -y update
sudo yum -q -y install zulu-13-azure-jdk
```

**Yum リポジトリから Zulu JDK 8 パッケージを更新するには:**

```cli
sudo yum -q -y install zulu-8-azure-jdk
```

(別のバージョンを使用している場合は、上記のコマンドでバージョン番号を変更します。)

**Yum リポジトリから Zulu JDK 8 パッケージを削除するには:**

```cli
sudo yum -y erase zulu-8-azure-jdk
```

(別のバージョンを使用している場合は、上記のコマンドでバージョン番号を変更します。)

## <a name="download-and-install-the-azul-zulu-jdks-from-an-apt-get-repository"></a>apt-get リポジトリから Azul Zulu JDK をダウンロードしてインストールする

Azul Zulu JDK は、Azul によって [apt-get リポジトリ](https://repos.azul.com/azure-only/zulu/apt)でも提供されています。

**apt-get で Java 8 用の Azul Zulu JDK をインストールするには、CLI から次のコマンドを実行します。**

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

Java 11 の場合は、次を実行します。

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-11-azure-jdk
```

Java 13 (プレビュー) の場合は、次を実行します。

```cli
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 0xB1998361219BD9C9
sudo apt-add-repository "deb http://repos.azul.com/azure-only/zulu/apt stable main"
sudo apt-get -q update
sudo apt-get -y install zulu-13-azure-jdk
```

**apt-get リポジトリから Zulu JDK 8 パッケージを更新するには:**

```cli
sudo apt-get -q update
sudo apt-get -y install zulu-8-azure-jdk
```

以前のリリースは自動的に削除されます。
(別のバージョンを使用している場合は、上記のコマンドでバージョン番号を変更します。)

**apt-get リポジトリから Zulu JDK 8 パッケージを削除するには:**

```cli
sudo apt-get -y purge zulu-8-azure-jdk
```

(別のバージョンを使用している場合は、上記のコマンドでバージョン番号を変更します。)

Azure 開発用 Azul Zulu JDK の準備、インストール、管理に関する詳細なガイダンスについては、[Zulu の公式ドキュメント](https://docs.azul.com/zulu/zuludocs/index.htm)をご覧ください。
