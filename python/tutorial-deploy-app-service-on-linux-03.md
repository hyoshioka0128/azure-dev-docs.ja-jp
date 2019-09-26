---
title: Visual Studio Code から App Service を作成する
description: チュートリアルの手順 3、VS Code 拡張機能から App Service を作成する。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 15e4e3b4ec5ab79110b1785335ce064cd90fd602
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019810"
---
# <a name="create-the-app-service"></a>App Service の作成

[前の手順: アプリを準備する](tutorial-deploy-app-service-on-linux-01.md)

この手順では、アプリのデプロイ先となる Azure App Service のインスタンスを作成します。 次の手順で必要に応じてカスタム スタートアップ ファイルを構成できるよう、この手順は、コードをデプロイする前に行います。

1. **[Azure: App Service]** エクスプローラーで、 **+** コマンドを選択して新しい App Service を作成するか、またはコマンド パレット (**F1**) を開いて **[Azure App Service: Create New Web App]\(Azure App Service: 新しい Web アプリを作成する\)** を選択します (App Service の用語では、"Web アプリ" とは、Web アプリ コードの**ホスト**であって、アプリのコード自体ではありません)。

    ![App Service エクスプローラーの新しい App Service の作成ボタン](media/deploy-azure/app-service-create-new.png)

1. 後続のプロンプトの内容:

    - アプリの名前を入力します。これは、App Service 上でグローバルに一意であることが必要です。通常は、自分の名前や会社名に続けてアプリ名を使用します。
    - ランタイムとして **[Python 3.7]** を選択します。

1. 新しい App Service が作成されたことを示すメッセージが表示されたら、 **[View Output]\(出力の表示\)** を選択して、VS Code の **[出力]** ウィンドウに切り替えます。 この出力には、作成された Azure リソース グループと App Service プランの名前が、App Service の URL と共に表示されます。

    ![App Service の作成後に表示されるメッセージ](media/deploy-azure/app-service-created.png)

1. App Service が正常に動作していることを確認するために、 **[Azure: App Service]** エクスプローラーでサブスクリプションを展開し、App Service の名前を右クリックして、 **[Web サイトの参照]** を選択します。

    ![App Service エクスプローラーの [App Service] にある [Web サイトの参照] コマンド](media/deploy-azure/browse-website-command.png)

1. まだ App Service に独自のコードをデプロイしていないため (これは次の手順で行います)、表示されるのは既定のアプリのみです。

    ![App Service on Linux での既定の Python アプリ](media/deploy-azure/default-python-app.png)

## <a name="optional-upload-an-environment-variable-definitions-file"></a>(省略可) 環境変数定義ファイルをアップロードする

環境変数定義ファイルがある場合は、そのファイルを使用して App Service Environment を構成することもできます (一般的に *.env* 拡張子が付いているこのようなファイルの詳細については、[Visual Studio Code での Python 環境](https://code.visualstudio.com/docs/python/environments#environment-variable-definitions-file)に関するページを参照してください)。

1. **[Azure: App Service]** エクスプローラーで、目的の App Service のノードを展開し、 **[Application Settings]\(アプリケーション設定\)** ノードを右クリックして **[Upload Local Settings]\(ローカル設定のアップロード\)** を選択します。

1. VS Code によって、 *.env* ファイルの場所を指定するよう求められた後、App Service にファイルがアップロードされます。

1. アップロードが完了したら、 **[Application Settings]\(アプリケーション設定\)** ノードを展開して個々の値を確認します。 また、Azure portal で [App Service] に移動し、 **[Configuration]\(構成\)** を選択して確認することもできます。

1. Azure portal で直接設定を作成した場合は、 **[Application Settings]\(アプリケーション設定\)** ノードを右クリックし、 **[Download Remote Settings]\(リモート設定のダウンロード\)** を選択することで、それらの設定を定義ファイルに保存できます。 このプロセスを通じて、それらの設定をポータルだけでなく自分のリポジトリにも確保することができます。

> [!div class="nextstepaction"]
> [App Service を作成しました](tutorial-deploy-app-service-on-linux-04.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=03-create-app-service)
