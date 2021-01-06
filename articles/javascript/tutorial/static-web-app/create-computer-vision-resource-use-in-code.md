---
title: Computer Vision リソースを作成する
description: Cognitive Services Computer Vision リソースを作成して環境変数に設定します。
ms.topic: tutorial
ms.date: 12/16/2020
ms.custom: devx-track-js
ms.openlocfilehash: 338a3040381768e5987e84676339e8ee46c49fc7
ms.sourcegitcommit: 1c508f5ba73a12e4baeacc88ad9a8359301acb50
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97687479"
---
# <a name="4-create-computer-vision-resource-and-use-in-code"></a>4.Computer Vision リソースを作成してコード内で使用する

この手順では、Computer Vision リソースを作成し、環境変数に設定します。 

## <a name="create-azure-resources"></a>Azure リソースを作成する

リソース グループを作成することにより、リソースを見つけやすくなり、完了時に削除しやすくなります。

この一連の手順の最後に、ご使用のリソースの **キーとエンドポイント** が必要です。

1. ターミナルまたは bash シェルで、[Azure リソース グループを作成する Azure CLI コマンドを入力](/cli/azure/group?view=azure-cli-latest#az_group_create)して、`rg-demo` という名前を指定します。

    ```azurecli
    az group create \
        --location eastus \
        --name rg-demo 
    ```
1. 次のコマンドを実行して、[Computer Vision リソースを作成します](/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create)。


    ```azurecli
    az cognitiveservices account create \
        --name demo-ComputerVision \
        --resource-group rg-demo \
        --kind ComputerVision \
        --sku F0 \
        --location eastus \
        --yes
    ```

1. その結果から `properties.endpoint` を見つけてコピーします。 この情報は後で必要になります。

    ```json
    ...
    "properties":{
        ...
        "endpoint": "https://eastus.api.cognitive.microsoft.com/",
        ...
    }
    ...
    ```

1. 次の[コマンド](/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list)を実行して、キーを取得します。 

    ```azurecli
    az cognitiveservices account keys list \
    --name demo-ComputerVision \
    --resource-group rg-demo
    ```

1. キーの 1 つをコピーします。これは後で必要になります。

    ```json
    {
      "key1": "8eb7f878bdce4e96b26c89b2b8d05319",
      "key2": "c2067cea18254bdda71c8ba6428c1e1a"
    }
    ```

## <a name="add-environment-variables-to-your-local-environment"></a>環境変数をローカル環境に追加する

リソースを使用するには、ローカル コードでキーとエンドポイントを使用できるようにする必要があります。 このコード ベースは、これらを環境変数に格納します。

* REACT_APP_COMPUTERVISIONKEY
* REACT_APP_COMPUTERVISIONENDPOINT 

1. 次のコマンドを実行して、これらの変数を環境に追加します。

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    export REACT_APP_COMPUTERVISIONKEY="REPLACE-WITH-YOUR-KEY"
    export REACT_APP_COMPUTERVISIONENDPOINT="REPLACE-WITH-YOUR-ENDPOINT"
    ```
    
    # <a name="cmd"></a>[cmd](#tab/cmd)
    
    ```cmd
    set REACT_APP_COMPUTERVISIONKEY="REPLACE-WITH-YOUR-KEY"
    set REACT_APP_COMPUTERVISIONENDPOINT="REPLACE-WITH-YOUR-ENDPOINT"
    ```
    ---

## <a name="add-environment-variables-to-your-remote-environment"></a>環境変数をリモート環境に追加する

Azure Static Web Apps を使用する場合、シークレットなどの環境変数を GitHub アクションから静的 Web アプリに渡す必要があります。 この GitHub アクションでは、そのリポジトリの GitHub シークレットから渡された Computer Vision キーとエンドポイントを含むアプリがビルドされ、その後、環境変数を含むコードが静的 Web アプリにプッシュされます。

1. Web ブラウザーの GitHub リポジトリで、 **[Settings]\(設定\)** 、 **[Secrets]\(シークレット\)** 、 **[New repository secret]\(新しいリポジトリ シークレット\)** を選択します。

    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-create-new-repository-secret.png" alt-text="キーとエンドポイントの設定前のイメージ分析のための React Cognitive Service Computer Vision サンプルの部分的なブラウザー スクリーンショット。":::

1. エンドポイントに対して前のセクションで使用したものと同じ名前と値を入力します。 次に、前のセクションで使用したものと同じ名前と値のキーを持つ、別のシークレットを作成します。 
    
    :::image type="content" source="../../media/static-web-app/browser-screenshot-github-add-secret.png" alt-text="エンドポイントに対して同じ名前と値を入力します。次に、同じ名前と値のキーを持つ、別のシークレットを作成します。":::

## <a name="run-react-app-with-computervision-resource"></a>ComputerVision リソースで React アプリを実行する

1. コマンドラインで再度アプリを起動します。

    ```bash
    npm start
    ```

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-start-up.png" alt-text="URL の指定または Enter を押す準備ができた React Cognitive Service Computer Vision サンプルの部分的なブラウザー スクリーンショット。":::

1. 既定のカタログからイメージを選択するために、テキスト フィールドを空のままにして、 **[Analyze]** ボタンを選択します。 

    :::image type="content" source="../../media/static-web-app/browser-screenshot-react-computervision-app-image-analysis-result.png" alt-text="React Cognitive Service Computer Vision サンプル結果の部分的なブラウザー スクリーンショット。":::

    イメージは、`./src/DefaultImages.js` で定義されているイメージのカタログからランダムに選択されます。 

1. 他のイメージと結果を表示するには、 **[Analyze]** ボタンを選択して続行します。 

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure Static Web アプリ を作成する](create-static-web-app-visual-studio-code-extension.md)