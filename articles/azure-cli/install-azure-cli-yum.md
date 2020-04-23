---
title: yum を使用して Linux に Azure CLI をインストールする
description: yum で Azure CLI をインストールする方法
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 11/26/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: a98a51e4dc3ac85d27e27ef9b9164a7f98431d31
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82031114"
---
# <a name="install-azure-cli-with-yum"></a>yum での Azure CLI のインストール

RHEL、Fedora、CentOS など、`yum` が付属する Linux ディストリビューションには、Azure CLI 用のパッケージが用意されています。 このパッケージは、RHEL 7.7、RHEL 8、Fedora 24 以降、CentOS 7、および CentOS 8 でテストされています。

[!INCLUDE [current-version](includes/current-version.md)]

[!INCLUDE [rpm-warning](includes/rpm-warning.md)]

## <a name="install"></a>インストール

1. Microsoft リポジトリ キーをインポートします。

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

2. ローカル `azure-cli` リポジトリ情報を作成します。

   ```bash
   sudo sh -c 'echo -e "[azure-cli]
   name=Azure CLI
   baseurl=https://packages.microsoft.com/yumrepos/azure-cli
   enabled=1
   gpgcheck=1
   gpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'
   ```

3. `yum install` コマンドを使用してインストールします。

   ```bash
   sudo yum install azure-cli
   ```

`az` コマンドで Azure CLI を実行します。 サインインするには、[az login](/cli/azure/reference-index#az-login) コマンドを使用します。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

さまざまな認証方法の詳細については、「[Azure CLI を使用してサインインする](authenticate-azure-cli.md)」を参照してください。

## <a name="troubleshooting"></a>トラブルシューティング

ここでは、`yum` でのインストール時に発生する一般的な問題をいくつか示します。 ここで取り上げていない問題が発生した場合は、[GitHub で問題を報告](https://github.com/Azure/azure-cli/issues)してください。

### <a name="install-on-rhel-76-or-other-systems-without-python-3"></a>Python 3 を含まない RHEL 7.6 またはその他のシステムにインストールする

可能な場合は、`python3` パッケージの正式なサポートを含むバージョンにシステムをアップグレードしてください。 それ以外の場合は、最初に `python3` パッケージをインストールする ([ソースからビルドする](https://github.com/linux-on-ibm-z/docs/wiki/Building-Python-3.6.x)か、[追加のリポジトリ](https://developers.redhat.com/blog/2018/08/13/install-python3-rhel/)を使用してインストールする) 必要があります。 その後、パッケージをダウンロードして、依存関係なしでインストールできます。
```bash
$ sudo yum install yum-utils
$ sudo yumdownloader azure-cli
$ sudo rpm -ivh --nodeps azure-cli-*.rpm
```

python3 を設定しても、CLI を実行しようするとエラー `python3: command not found` が依然として表示される場合は、パスにそれを追加する必要があります。
```bash
$ scl enable rh-python36 bash
```

### <a name="proxy-blocks-connection"></a>プロキシによる接続のブロック

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

常にこのプロキシを使用するように `yum` を明示的に構成することが必要な場合もあります。 次の行が `/etc/yum.conf` の `[main]` セクションの下に表示されていることを確認してください。

```yum.conf
[main]
# ...
proxy=http://[proxy]:[port] # If your proxy requires https, change http->https
proxy_username=[username] # Only required for basic auth
proxy_password=[password] # Only required for basic auth
```

Microsoft 署名キーを取得し、リポジトリからパッケージを取得するには、お使いのプロキシで次のアドレスへの HTTPS 接続を許可する必要があります。

* `https://packages.microsoft.com`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>更新

`yum update` コマンドで Azure CLI を更新します。

```bash
sudo yum update azure-cli
```

## <a name="uninstall"></a>アンインストール

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. システムからパッケージを削除します。

   ```bash
   sudo yum remove azure-cli
   ```

2. CLI を再インストールする予定がない場合は、リポジトリ情報を削除します。

   ```bash
   sudo rm /etc/yum.repos.d/azure-cli.repo
   ```

3. 他の Microsoft パッケージを使用しない場合は、署名キーを削除します。

   ```bash
   MSFT_KEY=`rpm -qa gpg-pubkey /* --qf "%{version}-%{release} %{summary}\n" | grep Microsoft | awk '{print $1}'`
   sudo rpm -e --allmatches gpg-pubkey-$MSFT_KEY
   ```

## <a name="next-steps"></a>次の手順

これで Azure CLI をインストールできました。次は、その機能と一般的なコマンドを簡単に見ていきましょう。

> [!div class="nextstepaction"]
> [Azure CLI の概要](get-started-with-azure-cli.md)
