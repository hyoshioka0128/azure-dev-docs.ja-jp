---
title: Docker コンテナーでの Azure CLI の実行
description: Azure CLI をホストする Docker コンテナーを実行する方法
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 01/29/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 96a2bcf311cb504b08d44da3ea6033dbad9034b8
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031294"
---
# <a name="run-azure-cli-in-a-docker-container"></a>Docker コンテナーでの Azure CLI の実行

Docker を使用して、Azure CLI がプレインストールされたスタンドアロン Linux コンテナーを実行できます。 Docker を使用すると、CLI を実行する分離環境をすぐに準備できます。 独自のデプロイのベースとしてイメージを使用することもできます。

## <a name="run-in-a-docker-container"></a>Docker コンテナーでの実行

> [!NOTE]
> Azure CLI は [Microsoft コンテナー レジストリ](https://azure.microsoft.com/services/container-registry)に移行されました。 Docker Hub 上の既存のタグは引き続きサポートされますが、新しいリリースは mcr.microsoft.com/azure-cli としてのみ利用できます。

`docker run` を使用して、CLI をインストールしてください。

   ```bash
   docker run -it mcr.microsoft.com/azure-cli
   ```

> [!NOTE]
> お使いのユーザー環境から SSH キーを取得する場合は、`-v ${HOME}/.ssh:/root/.ssh` を使用して、その環境にご自身の SSH キーをマウントします。
>
> ```bash
> docker run -it -v ${HOME}/.ssh:/root/.ssh mcr.microsoft.com/azure-cli
> ```

CLI は、`/usr/local/bin` の `az` コマンドとしてイメージにインストールされます。 サインインするには、[az login](/cli/azure/reference-index#az-login) コマンドを実行します。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

さまざまな認証方法の詳細については、「[Azure CLI を使用してサインインする](authenticate-azure-cli.md)」を参照してください。

## <a name="update-docker-image"></a>Docker イメージの更新

Docker で更新するには、新しいイメージをプルし、さらに既存のすべてのコンテナーを再作成する必要があります。 そのため、CLI をホストするコンテナーをデータ ストアとして使用することは避けるようにしてください。

`docker pull` でローカル イメージを更新します。

```bash
docker pull mcr.microsoft.com/azure-cli
```

## <a name="uninstall-docker-image"></a>Docker イメージのアンインストール

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

CLI イメージを実行しているコンテナーを停止した後、それを削除します。

```bash
docker rmi mcr.microsoft.com/azure-cli
```

## <a name="next-steps"></a>次の手順

これで Azure CLI を使用する準備ができました。次は、その機能と一般的なコマンドを簡単に見ていきましょう。

> [!div class="nextstepaction"]
> [Azure CLI の概要](get-started-with-azure-cli.md)
