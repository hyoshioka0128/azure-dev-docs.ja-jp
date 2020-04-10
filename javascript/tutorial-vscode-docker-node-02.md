---
title: Visual Studio Code からコンテナー レジストリを使用する
description: チュートリアル パート2、コンテナー レジストリを使用する
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: e6dde135a2e6482284488fb83d9f811b02249c4d
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740551"
---
# <a name="use-a-container-registry"></a>コンテナー レジストリを使用する

[前の手順:概要と前提条件](tutorial-vscode-docker-node-01.md)

この手順では、お使いのアプリ イメージに適したコンテナー レジストリを設定します。 その後、コンテナー対応のホスティング サービス (Azure App Service など) により、レジストリからイメージがプルされます。

このチュートリアルでは、イメージに対して [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR) を使用します。これは、セキュリティで保護されたプライベートなホスト型レジストリです。 ただし、ここで示されているツールとプロセスは、[Docker Hub](https://hub.docker.com/) などの他のレジストリでも機能します。

## <a name="create-an-azure-container-registry"></a>Azure Container Registry を作成する

1. Visual Studio Code で、<kbd>F1</kbd> キーを押して**コマンド パレット**を開きます。

1. 検索ボックスに「registry」と入力し、 **[Azure Container Registry:Create Registry]** を選択します。

   ![VS Code の Docker エクスプローラー](media/deploy-containers/docker-create-registry.jpg)

1. プロンプトで次の値を指定します

    - **レジストリ名**は Azure 全体で一意にする必要があり、英数字で 5 から 50 文字にする必要があります。
    - **[SKU]** には、 **[Basic]** を選択します。
    - **[リソース グループ]** は、ご使用のサブスクリプション内でのみ一意である必要があります。
    - **[場所]** では、お近くのリージョンを選択します。

    Visual Studio Code によって、Azure でのレジストリ作成プロセスが開始されます。 完了すると、次のような通知が表示され、レジストリが正常に作成されたことを確認できます。

   ![Visual Studio Code でのレジストリが作成されたことの確認](media/deploy-containers/registry-created.jpg)

1. **Docker** エクスプローラーを開き、たった今設定したレジストリ エンドポイントが **[Registries]\(レジストリ\)** の下に表示されていることを確認します。

   ![レジストリが Docker エクスプローラーに表示されていることを確認する](media/deploy-containers/docker-explorer-registry.jpg)

## <a name="sign-in-to-azure-container-registry"></a>Azure Container Registry にサインインする

Azure レジストリは Docker 拡張機能に表示されますが、Azure Container Registry (ACR) にログインするまでは、それらにイメージをプッシュすることはできません。

1. <kbd>Ctrl と `</kbd> キーを押して、VS Code で**統合ターミナル**を開きます。

1. 次の Azure CLI コマンドを実行して、ACR にログインします。 "<your-registry-name>" を、先ほど作成したレジストリの名前に置き換えます。

    ```bash
    az acr login --name <your-registry-name>
    ```

> [!div class="nextstepaction"]
> [レジストリを作成しました](tutorial-vscode-docker-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
