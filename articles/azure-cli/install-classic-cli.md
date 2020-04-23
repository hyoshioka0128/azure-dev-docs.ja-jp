---
title: Azure クラシック CLI のインストール
description: Mac、Linux、および Windows に Azure クラシック CLI をインストールして Azure サービスの利用を開始する
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 06/11/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 0cc1d7811223bf6f473c2c4516d0919306aa74c7
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030764"
---
# <a name="install-the-azure-classic-cli"></a>Azure クラシック CLI のインストール

> [!IMPORTANT]
> このトピックでは、Azure クラシック CLI のインストール方法について説明します。 クラシック CLI は非推奨です。クラシック デプロイ モデルでのみ使用してください。
> その他すべてのデプロイについては、[Azure CLI](/cli/azure) を使用してください。

Azure クラシック CLI を簡単にインストールして、コマンド ライン シェルからオープン ソースのコマンドを使って Microsoft Azure 上のリソースを作成および管理します。 お使いのコンピューターにこれらのクロスプラットフォーム ツールをインストールするオプションは複数あります。

* **npm パッケージ** - npm (JavaScript 用のパッケージ マネージャー) を実行して、Linux ディストリビューションまたは OS に Azure クラシック CLI パッケージをインストールします。 node.js と npm が必要です。
* **インストーラー** - macOS または Windows に簡単インストールするにはインストーラーをダウンロードします。
* **Docker コンテナー** - すぐに実行できる Docker コンテナーでクラシック CLI の使用を開始します。 Docker ホストが必要です。

その他のオプションと背景については、 [GitHub](https://github.com/azure/azure-xplat-cli)のプロジェクト リポジトリを参照してください。

Azure クラシック CLI をインストールした後、`azure login` に接続し、お使いのコマンド ライン インターフェイス (Bash、ターミナル、コマンド プロンプトなど) から `azure` コマンドを実行して、Azure リソースを操作します。

## <a name="option-1-install-an-npm-package"></a>オプション 1: npm パッケージのインストール

クラシック CLI を npm パッケージからインストールするには、[最新の Node.js と npm](https://nodejs.org/en/download/package-manager/) をダウンロードし、インストールしていることを確認してください。 次に、`npm install` を実行して、azure-cli パッケージをインストールします。

```bash
npm install -g azure-cli
```

Linux ディストリビューションの場合、`npm` コマンドを正常に実行するには、次のように `sudo` の使用が必要になる場合があります。

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Node.js と npm をご自身の OS にインストールまたは更新する必要がある場合は、Node.js LTS バージョン 4.x 以降をインストールすることをお勧めします。 以前のバージョンを使用すると、インストール エラーが発生する場合があります。

必要に応じて、[GitHub リリース](https://github.com/Azure/azure-xplat-cli/releases)から tar ファイルをダウンロードすることもできます。 その後、ダウンロードした npm パッケージを次のようにインストールします (Linux ディストリビューションでは `sudo` を使用しなければならないことがあります)。

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a>オプション 2:インストーラーの使用

Mac または Windows コンピューターを使用している場合は、[GitHub リリース](https://github.com/Azure/azure-xplat-cli/releases)から DMG および MSI インストーラーを入手できます。

> [!TIP]
> Windows では、[Web Platform Installer](https://go.microsoft.com/?linkid=9828653) をダウンロードして、クラシック CLI をインストールすることもできます。 このインストーラーを使用すると、その他の Azure SDK とコマンドライン ツールをインストールすることもできます。

## <a name="option-3-use-a-docker-container"></a>オプション 3:Docker コンテナーの使用

お使いのコンピューターを [Docker](https://docs.docker.com/engine/understanding-docker/) ホストとして設定すると、Docker コンテナーで Azure クラシック CLI を実行できるようになります。 次のコマンドを実行します (Linux ディストリビューションの場合、`sudo` の使用が必要になる場合があります)。

```bash
docker run -it microsoft/azure-cli:0.10.17
```

## <a name="run-azure-classic-cli-commands"></a>Azure クラシック CLI コマンドの実行

クラシック CLI をインストールした後、コマンド ライン ユーザー インターフェイス (Bash、ターミナル、コマンド プロンプトなど) から `azure` コマンドを実行します。 たとえば、ヘルプ コマンドを実行するには、次のように入力します。

```azurecli-interactive
azure help
```

> [!NOTE]
> 一部の Linux ディストリビューションでは、`/usr/bin/env: ‘node’: No such file or directory` のようなエラーが表示されることがあります。 このエラーは、/usr/bin/nodejs に最近インストールされた Node.js が原因で発生します。 このエラーを修正するには、次のコマンドを実行して /usr/bin/node へのシンボリック リンクを作成します。

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

インストールした Azure クラシック CLI のバージョンを確認するには、次のように入力します。

```azurecli-interactive
azure --version
```

> [!NOTE]
> Azure クラシック CLI を初めて使用する場合、Microsoft が使用状況についての情報を収集することを許可するかどうかをたずねるメッセージが表示されます。 参加は任意です。 参加した後でも、 `azure telemetry --disable`を実行するといつでも停止できます。 参加を有効にするには、任意のタイミングで `azure telemetry --enable`を実行します。

## <a name="update-the-classic-cli"></a>クラシック CLI の更新

Microsoft は、Azure クラシック CLI の更新バージョンをリリースする場合があります。 ご利用のオペレーティング システム用のインストーラーを使用するか、最新の Docker コンテナーを実行して、クラシック CLI を再インストールします。 または、最新の Node.js と npm がインストールされている場合は、次のコマンドを入力して更新します (Linux ディストリビューションでは、`sudo` の使用が必要になる場合があります)。

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>タブ補完の有効化

Mac と Linux では、クラシック CLI コマンドのタブ補完がサポートされます。

zsh で有効化する場合は、次のコマンドを実行します。

```bash
echo '. <(azure --completion)' >> .zshrc
```

bash で有効化する場合は、次のコマンドを実行します。

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```

## <a name="next-steps"></a>次のステップ

* Azure クラシック CLI の詳細、ソース コードのダウンロード、問題のレポート、プロジェクトへの協力については、[Azure クラシック CLI 用の GitHub リポジトリ](https://github.com/azure/azure-xplat-cli)のページを参照してください。
