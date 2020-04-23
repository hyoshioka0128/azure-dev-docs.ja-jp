---
title: apt を使用して Linux に Azure CLI をインストールする
description: apt パッケージ マネージャーで Azure CLI をインストールする方法
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 302b98717ee422de9bd60a57b18d900bcf5fcaf9
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030974"
---
# <a name="install-azure-cli-with-apt"></a>apt での Azure CLI のインストール

Ubuntu や Debian など、`apt` が付属するディストリビューションを実行している場合は、Azure CLI 用に x86_64 パッケージを使用できます。 このパッケージは、以下でテストされ、サポートされています。

* Ubuntu trusty、xenial、artful、bionic、および disco
* Debian wheezy、jessie、stretch、および buster

[!INCLUDE [current-version](includes/current-version.md)]

> [!NOTE]
>
> Azure CLI のパッケージによって独自の Python インタープリターがインストールされ、システム上の Python は使用されません。

## <a name="install"></a>インストール

`apt` をサポートするディストリビューションで Azure CLI をインストールするために、次の 2 つの方法が提供されています。ユーザーに代わってコマンドを実行してインストールするオールインワンのスクリプトと、ユーザーが段階的なプロセスを自分で実行できる手順です。

### <a name="install-with-one-command"></a>1 つのコマンドでインストールする

1 つの手順ですべてのインストール コマンドを実行するスクリプトを提供および保守しています。 `curl` を使用してコマンドを実行し、`bash` に直接パイプするか、ファイルにスクリプトをダウンロードして実行前に検査します。

> [!IMPORTANT]
> このスクリプトは、Ubuntu 16.04 以降および Debian 8 以降でのみ検証されています。 他のディストリビューションでは機能しない可能性があります。
> Linux Mint などの派生ディストリビューションを使用している場合は、手動のインストール手順に従い、必要なトラブルシューティングを実行します。

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### <a name="manual-install-instructions"></a>手動のインストール手順

スーパーユーザーとしてスクリプトを実行しない場合またはオールインワン スクリプトが失敗する場合は、次の手順に従って Azure CLI をインストールします。

1. インストール プロセスに必要なパッケージを取得します。

    ```bash
    sudo apt-get update
    sudo apt-get install ca-certificates curl apt-transport-https lsb-release gnupg
    ```

2. Microsoft の署名キーをダウンロードしてインストールします。

    ```bash
    curl -sL https://packages.microsoft.com/keys/microsoft.asc |
        gpg --dearmor |
        sudo tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null
    ```

3. <div id="set-release"/>Azure CLI  ソフトウェア リポジトリを追加します。

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

4. リポジトリ情報を更新し、`azure-cli` パッケージをインストールします。

    ```bash
    sudo apt-get update
    sudo apt-get install azure-cli
    ```

`az` コマンドで Azure CLI を実行します。 サインインするには、[az login](/cli/azure/reference-index#az-login) コマンドを使用します。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

さまざまな認証方法の詳細については、「[Azure CLI を使用してサインインする](authenticate-azure-cli.md)」を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

ここでは、`apt` でのインストール時に発生する一般的な問題をいくつか示します。 ここで取り上げていない問題が発生した場合は、[GitHub で問題を報告](https://github.com/Azure/azure-cli/issues)してください。

### <a name="lsb_release-does-not-return-the-correct-base-distribution-version"></a>lsb_release がベース ディストリビューション バージョンを返さない

Linux Mint など、Ubuntu や Debian から派生する一部のディストリビューションでは、正しいバージョン名が `lsb_release` から返されない場合があります。 この値は、インストール プロセスで、インストールするパッケージを特定するときに使用されます。 ディストリビューションの派生元である Ubuntu バージョンまたは Debian バージョンのコード名がわかっている場合は、[リポジトリを追加する](#set-release)ときに `AZ_REPO` 値を手動で設定できます。 それ以外の場合は、ご自身のディストリビューションについて、ベース ディストリビューションのコード名を調べて、`AZ_REPO` を正しい値に設定する方法をご確認ください。

### <a name="no-package-for-your-distribution"></a>ご使用のディストリビューションのパッケージがない

ディストリビューションがリリースされてから、そのディストリビューション用の Azure CLI パッケージが利用可能になるまではしばらくかかることがあります。 Azure CLI は、以降のバージョンの依存関係に関して弾力性を持つように、また可能な限り以降のバージョンに依存しないように設計されています。 ベース ディストリビューションに使用できるパッケージがない場合は、以前のディストリビューション用のパッケージをお試しください。

これを行うには、[リポジトリを追加する](#set-release)ときに `AZ_REPO` の値を手動で設定します。 Ubuntu ディストリビューションには `bionic` リポジトリーを使用し、Debian ディストリビューションには `stretch` を使用します。 Ubuntu Trusty および Debian Wheezy より前にリリースされたディストリビューションはサポートされていません。

### <a name="elementary-os-eos-fails-to-install-the-azure-cli"></a>elementary OS (EOS) で Azure CLI のインストールが失敗する

`lsb_release` によって `HERA` (EOS リリース名) が返されるため、EOS で Azure CLI のインストールが失敗します。  解決策は、ファイル `/etc/apt/sources.list.d/azure-cli.list` を修正して、`hera main` を `bionic main` に変更することです。

元のファイルの内容:

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ hera main
```

修正したファイルの内容

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ bionic main
```

### <a name="proxy-blocks-connection"></a>プロキシによる接続のブロック

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

常にこのプロキシを使用するように `apt` を明示的に構成することが必要な場合もあります。 次の行が `/etc/apt/apt.conf.d/` の `apt` 構成ファイルに表示されることを確認します。 既存のグローバル構成ファイル、既存のプロキシ構成ファイル、`40proxies`、または `99local` を使用することをお勧めしますが、システム管理の要件に従ってください。

```apt.conf
Acquire {
    http::proxy "http://[username]:[password]@[proxy]:[port]";
    https::proxy "https://[username]:[password]@[proxy]:[port]";
}
```

プロキシで基本認証を使用しない場合、プロキシ URL の `[username]:[password]@` ポーションを__削除__します。 プロキシ構成について詳しくは、Ubuntu の公式ドキュメントを参照してください。

* [apt.conf マニュアル ページ](http://manpages.ubuntu.com/manpages/bionic/en/man5/apt.conf.5.html)
* [Ubuntu wiki - apt-get ハウツー](https://help.ubuntu.com/community/AptGet/Howto#Setting_up_apt-get_to_use_a_http-proxy)

Microsoft 署名キーを取得し、リポジトリからパッケージを取得するには、お使いのプロキシで次のアドレスへの HTTPS 接続を許可する必要があります。

* `https://packages.microsoft.com`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>更新

CLI パッケージを更新するには、`apt-get upgrade` を使用します。

   ```bash
   sudo apt-get update && sudo apt-get upgrade
   ```

> [!NOTE]
> このコマンドにより、システムにインストールされている、依存関係が変更されていないすべてのパッケージがアップグレードされます。
> CLI だけをアップグレードするには、`apt-get install` を使用します。
>
> ```bash
> sudo apt-get update && sudo apt-get install --only-upgrade -y azure-cli
> ```

## <a name="uninstall"></a>アンインストール

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. `apt-get remove` を使用してアンインストールします。

    ```bash
    sudo apt-get remove -y azure-cli
    ```

2. CLI を再インストールする予定がない場合は、Azure CLI リポジトリ情報を削除します。

   ```bash
   sudo rm /etc/apt/sources.list.d/azure-cli.list
   ```

3. Microsoft の他のパッケージを使用しない場合は、署名キーを削除します。

    ```bash
    sudo rm /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    ```

4. 不要なパッケージを削除します。

   ```bash
   sudo apt autoremove
   ```

## <a name="next-steps"></a>次の手順

これで Azure CLI をインストールできました。次は、その機能と一般的なコマンドを簡単に見ていきましょう。

> [!div class="nextstepaction"]
> [Azure CLI の概要](get-started-with-azure-cli.md)
