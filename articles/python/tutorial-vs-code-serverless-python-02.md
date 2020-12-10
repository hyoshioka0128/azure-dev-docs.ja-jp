---
title: 手順 2:VS Code を使用して Azure Functions 用にサーバーレス Python 関数を作成する
description: チュートリアルの手順 2、VS Code 用の Azure Functions 拡張機能を使用してサーバーレス Python 関数を追加する。
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-python, seo-python-october2019, contperfq2
ms.openlocfilehash: ac9bf8f90399a5ad2421689104fd5a6d9772c462
ms.sourcegitcommit: 0cda024089784b92c1db3a4506c1dccd6bfe6339
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "96759309"
---
# <a name="2-create-a-python-function-for-azure-functions"></a>2:Azure Functions 用に Python 関数を作成する

[前のステップ: 環境を構成する](tutorial-vs-code-serverless-python-01.md)

この記事では、Visual Studio Code を使用してサーバーレス Azure Functions 用に Python 関数を作成します。 Azure Functions のコードは、コードの作成前に最初に作成する Functions "_プロジェクト_" 内で管理されます。

1. **Azure:Functions** エクスプローラーで (左側の Azure アイコンを使用して開きます)、**新しいプロジェクトの作成** コマンド アイコンを選択するか、コマンド パレット (F1) を開き、**Azure Functions:新しいプロジェクトの作成** を選択します。

    ![Azure Functions エクスプローラーで新しいプロジェクトを作成する](media/tutorial-vs-code-serverless-python/create-a-new-project-in-azure-functions-explorer.png)

1. 後続のプロンプトの内容:

    | Prompt | 値 | 説明 |
    | --- | --- | --- |
    | Functions プロジェクトを格納するフォルダーを選択する | 現在開いているフォルダー | プロジェクトを作成するフォルダー。 サブフォルダーにプロジェクトを作成することもできます。 |
    | 言語を選択する | **Python** | 関数に使用する言語。これによりコードに使用するテンプレートが決定されます。 |
    | 仮想環境を作成する Python インタープリターを選択する | (指定された既定のパスを使用するか、何も指定されていない場合は、適切なインタープリターへのパスを手動で入力してください。) | 仮想環境に使用する Python インタープリター。 |
    | Select a template for your project's first function (プロジェクトの最初の関数のテンプレートを選択してください) | **HTTP トリガー** | HTTP トリガーを使用する関数は、関数のエンドポイントに HTTP 要求が行われるたびに実行されます。 (Azure Functions には他にもさまざまなトリガーがあります。 詳細は、「[Functions でできること](/azure/azure-functions/functions-overview#what-can-i-do-with-functions)」を参照してください。) |
    | Provide a function name (関数名を指定してください) | HttpExample | この名前は、構成データと共に関数のコードが含まれるサブフォルダーに使用されます。また、HTTP エンドポイントの名前がこの名前で定義されます。 関数自体とトリガーを区別する目的で、既定の "HTTPTrigger1" をそのまま使用せず、"HttpExample" を使用します。 |
    | 承認レベル | **Anonymous** | 匿名認証の場合、関数は公開され、誰でもアクセスできます。 |

1. このプロセスでは、エクスプローラーに表示される **Local Project** 項目が作成され、その下に **Functions** という名前のコレクションがあり、その中に指定した名前の関数 (ここでは **HttpExample**) が含まれています。

    ![Azure Functions の新しい Python プロジェクトのエクスプローラー ビュー](media/tutorial-vs-code-serverless-python/explorer-view-new-python-project-in-azure-functions.png)

    *\_\_init\_\_.py* を開いたとき、Python インタープリターが選択されていないと Visual Studio Code に表示されたら、コマンド パレット (**F1**) を開き、**Python: Select Interpreter** コマンドを選択します。次に、(プロジェクトの一部として作成された) ローカルの `.venv` フォルダーで仮想環境を選択します。

    ![Python プロジェクトで作成された仮想環境を選択する](media/tutorial-vs-code-serverless-python/select-virtual-environment-created-with-the-python-project.png)

1. VS Code エディターも、関数の既定のコードが含まれる *\_\_init\_\_.py* ファイルに対して開かれます。 エディターの上部にある **HttpExample > __init.py__** コントロールに注目してください。

    ![VS Code の関数のファイル セレクター コントロール](media/tutorial-vs-code-serverless-python/file-selector-in-azure-functions-editor-01.png)

    **__init.py__** を選択した場合、VS Code によって、関数を構成するすべてのファイル ( *\_\_init\_\_.py* のコード、*function.json* の構成、*sample.dat* のサンプル データ) が表示されます。 いずれかのファイルを選択して切り替えることができます。

    ![関数コード ファイルのファイル セレクター コントロール](media/tutorial-vs-code-serverless-python/file-selector-in-azure-functions-editor-02.png)

    同様に、**HttpExample** を選択した場合、VS Code によって "*プロジェクト*" レベルのすべてのファイルとフォルダーが表示されます。

    ![VS Code の関数のプロジェクト セレクター コントロール](media/tutorial-vs-code-serverless-python/file-selector-in-azure-functions-editor-03.png)

> [!TIP]
> 同じプロジェクトで別の関数を作成するときは必ず、**Create Function** コマンドを **[Azure:Functions]** エクスプローラーで使用するか、コマンド パレット (**F1**) を開き、**Azure Functions: Create Function** コマンドを探して選択します。 いずれのコマンドでも、関数名 (エンドポイントの名前) が求められ、独自の *\_\_init\_\_.py*、*function.json*、および *sample.dat* ファイルを持つ別の関数サブフォルダーがプロジェクト内に作成されます。
>
> ![Azure Functions エクスプローラーで新規関数を使用して関数を作成する](media/tutorial-vs-code-serverless-python/create-new-functions-in-azure-functions-explorer.png)

> [!div class="nextstepaction"]
> [関数を作成しました - 手順 3 に進む >>>](tutorial-vs-code-serverless-python-03.md)

[問題がある場合は、お知らせください。](https://aka.ms/python-functions-qs-ms-survey)
