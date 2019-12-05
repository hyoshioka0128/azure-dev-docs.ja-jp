---
title: Visual Studio Code からコンテナー レジストリを使用する
description: チュートリアル パート2、コンテナー レジストリを使用する
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 7d51e8011824ec19e9530f9bc94bcb2ce07f2851
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466631"
---
# <a name="use-a-container-registry"></a>コンテナー レジストリを使用する

[前の手順:概要と前提条件](tutorial-vscode-docker-node-01.md)

この手順では、お使いのアプリ イメージに適したコンテナー レジストリを設定します。 その後、コンテナー対応のホスティング サービス (Azure App Service など) により、レジストリからイメージがプルされます。

このチュートリアルでは、イメージに対して [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR) を使用します。これは、セキュリティで保護されたプライベートなホスト型レジストリです。 ただし、ここで示されているツールとプロセスは、[Docker Hub](https://hub.docker.com/) などの他のレジストリでも機能します。

## <a name="create-an-azure-container-registry"></a>Azure Container Registry を作成する

1. [Azure portal](https://portal.azure.com) にサインインし、 **[リソースの作成]**  >  **[コンテナー]**  >  **[コンテナー レジストリ]** の順に選択します。

    ![Azure portal でコンテナー レジストリを作成する](media/deploy-containers/portal-01.png)

1. 表示された **[コンテナー レジストリの作成]** フォームに、次のように適切な値を入力します。

    - **レジストリ名**は Azure 全体で一意にする必要があり、英数字で 5 から 50 文字にする必要があります。
    - **[サブスクリプション]** では、ご使用のサブスクリプションを選択します。
    - **[リソース グループ]** は、ご使用のサブスクリプション内でのみ一意である必要があります。
    - **[場所]** では、お近くのリージョンを選択します。
    - **[管理者ユーザー]** は、 **[有効にする]** に設定します。
    - **[SKU]** には、 **[Basic]** を選択します。

    ![コンテナー レジストリ フォームの値](media/deploy-containers/portal-02.png)

1. **[作成]** を選択して、レジストリを作成します。

1. レジストリが作成されたら、ポータルで通知を開き、レジストリに対して **[リソースに移動]** を選択します。

    ![新しく作成されたレジストリ リソースを開く](media/deploy-containers/portal-03.png)

1. レジストリ ページで、 **[アクセス キー]** を選択し、管理者の資格情報をメモします。

    ![Azure portal 上のレジストリのレジストリ資格情報](media/deploy-containers/portal-04.png)

1. コマンド プロンプトまたはターミナルで、以下のコマンドを使って Docker にログインします。`<registry_name>` はお使いのレジストリの名前に置き換え、`<username>` と `<password>` は Azure portal に示されている管理者ユーザーの値に置き換えてください。

    ```bash
    docker login <registry_name>.azurecr.io -u <username> -p <password>
    ```

1. Visual Studio Code で、**Docker** エクスプローラーを開き、たった今設定したレジストリ エンドポイントが **[Registries]\(レジストリ\)** の下に表示されていることを確認します。

    ![レジストリが Docker エクスプローラーに表示されていることを確認する](media/deploy-containers/registries.png)

> [!div class="nextstepaction"]
> [レジストリを作成しました](tutorial-vscode-docker-node-03.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
