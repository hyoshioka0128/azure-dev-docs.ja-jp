---
title: Linux 仮想マシンの作成
description: Azure CLI を使用して、仮想マシンを作成および構成します。 このチュートリアルのこの時点で、ターミナル ウィンドウを開いて、仮想マシンを作成するサブスクリプションで Azure CLI を使用して Azure クラウドにサインインしている必要があります。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: d2da4cac2a93ad953ada9cd98de679abba034908
ms.sourcegitcommit: a2a51e0c6530eb5794a2fe667cf4c9a60b2a7470
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625031"
---
# <a name="3-create-linux-virtual-machine-using-azure-cli"></a>3.Azure CLI を使用して Linux 仮想マシンを作成する

チュートリアルのこのセクションでは、Azure CLI を使用して仮想マシンを作成および構成します。 このチュートリアルのこの時点で、ターミナル ウィンドウを開いて、仮想マシンを作成するサブスクリプションで Azure クラウドにサインインしている必要があります。 

Azure CLI のすべての手順は、Azure CLI の 1 つのインスタンスから実行できます。 ウィンドウを閉じた場合、または Azure CLI を使用する場所を切り替えた (Cloud Shell とローカル ターミナルの間など) 場合は、もう一度サインインする必要があります。 

## <a name="create-a-cloud-init-file-to-expedite-linux-virtual-machine-creation"></a>cloud-init ファイルを作成して、Linux 仮想マシンを迅速に作成する

このチュートリアルでは、cloud-init 構成ファイルを使用して、NGINX リバース プロキシ サーバーと Express.js サーバーの両方を作成します。 NGINX は、Express.js ポート (3000) をパブリック ポート (80) に転送するために使用されます。 

`runcmd` には、いくつかのタスクがあります。
* Node.js をダウンロードしてインストールします
* サンプルの Express.js リポジトリのクローンを作成します
* Express.js の依存関係をインストールします
* PM2 を使用して Express.js アプリを開始します

1. `cloud-init-github.txt` という名前のローカル ファイルを作成して次の内容をファイルに保存するか、ローカル コンピューターに[リポジトリのファイルを保存](https://github.com/Azure-Samples/js-e2e-vm/blob/main/cloud-init-github.txt)することができます。 [cloud init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html#yaml-examples) 形式のファイルは、Azure CLI コマンドのターミナル パスと同じフォルダーに存在している必要があります。

    :::code language="yaml" source="~/../js-e2e-vm/cloud-init-github.txt" :::

## <a name="create-a-virtual-machine-resource"></a>仮想マシンのリソースを作成する 

1. ターミナルで [Azure CLI コマンド](/cli/azure/vm?view=azure-cli-latest#az_vm_create)を入力して、Linux 仮想マシンの Azure リソースを作成します。 このコマンドでは、cloud-init ファイルから VM が作成され、SSH キーが生成されます。 コマンドを実行すると、キーが格納されている場所が表示されます。 

    ```azurecli
    az vm create \
      --resource-group rg-demo-vm-eastus \
      --name demo-vm \
      --location eastus \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data cloud-init-github.txt
    ```

    この処理は数分かかることがあります。 この処理が完了すると、Azure CLI によって、新しいリソースに関する情報が返されます。 `publicIpAddress` 値を控えておきます。ブラウザーで Web アプリを表示し、VM に接続するために必要です。 
     

1. 仮想マシンを最初に作成したときに、開いているポートは "_ありません_"。 次の [Azure CLI コマンド](/cli/azure/vm?view=azure-cli-latest#az_vm_open_port)を使用してポート 80 を開き、Web アプリが一般公開されるようにします。

    ```azurecli
    az vm open-port \
      --port 80 \
      --resource-group rg-demo-vm-eastus \
      --name demo-vm
    ```

1. Web ブラウザーでパブリック IP アドレスを使用して、仮想マシンが使用可能で実行されていることを確認します。 `publicIpAddress` の値を使用するように URL を変更します。

    ```HTTP
    http://YOUR-PUBLIC-IP-ADDRESS
    ```

    次の図は Web アプリを示していますが、ご使用のアプリでは異なる IP アドレスが使用されます。 ゲートウェイ エラーが発生してリソースにアクセスできなかった場合は、しばらくしてからもう一度やり直してください。Web アプリが開始されるまでに 1 分ほどかかることがあります。 

    :::image type="content" source="../../media/tutorial-vm/basic-web-app.png" alt-text="Azure の Linux 仮想マシンから表示された簡単なアプリ。":::

    Web アプリの初期コード ファイルには、NGINX プロキシを介して渡されるクライアントの IP アドレスを表示する 1 つのルートがあります。 

    :::code language="JavaScript" source="~/../js-e2e-vm/index.js" :::

1. このターミナルは開いたままにしてください。チュートリアルの以降のステップで使用します。

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [SSH を使用して VM に接続する](connect-linux-virtual-machine-ssh.md) 