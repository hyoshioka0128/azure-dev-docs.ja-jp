---
title: 手順 2:VS Code を使用して Azure Functions 用に Python 関数を作成する
description: チュートリアルの手順 2、VS Code 用の Azure Functions 拡張機能の使用方法を紹介する。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 3d33213d00c2b20d3a712c5cf9ca6888d25ebb8a
ms.sourcegitcommit: 44d1abfb836f90b8731d7ea5d5a5af09245b2b89
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2020
ms.locfileid: "77422169"
---
# <a name="2-create-a-python-function-for-azure-functions"></a>2:Azure Functions 用に Python 関数を作成する

[前の手順: 前提条件](tutorial-vs-code-serverless-python-01.md)

この記事では、Visual Studio Code を使用して Azure Functions 用に Python 関数を作成します。 Azure Functions のコードは、コードの作成前に最初に作成する Functions "_プロジェクト_" 内で管理されます。

1. **[Azure:Functions]** エクスプローラーで (左側の Azure アイコンを使用して開きます)、 **[新しいプロジェクト]** コマンド アイコンを選択するか、コマンド パレット (F1) を開き、 **[Azure Functions:新しいプロジェクトの作成]** を選択します。

    ![Azure Functions エクスプローラーで新しいプロジェクトを作成する](media/tutorial-vs-code-serverless-python/create-a-new-project-in-azure-functions-explorer.png)

1. 後続のプロンプトの内容:

    | Prompt | Value | 説明 |
    | --- | --- | --- |
    | Specify a folder for the project (プロジェクトのフォルダーを指定してください) | 現在開いているフォルダー | プロジェクトを作成するフォルダー。 サブフォルダーにプロジェクトを作成することもできます。 |
    | Select a language for your function app project (関数アプリ プロジェクトの言語を選択してください) | **Python** | 関数に使用する言語。これによりコードに使用するテンプレートが決定されます。 |
    | Select a template for your project's first function (プロジェクトの最初の関数のテンプレートを選択してください) | **HTTP トリガー** | HTTP トリガーを使用する関数は、関数のエンドポイントに HTTP 要求が行われるたびに実行されます。 (Azure Functions には他にもさまざまなトリガーがあります。 詳細は、「[Functions でできること](/azure/azure-functions/functions-overview#what-can-i-do-with-functions)」を参照してください。) |
    | Provide a function name (関数名を指定してください) | HttpExample | この名前は、構成データと共に関数のコードが含まれるサブフォルダーに使用されます。また、HTTP エンドポイントの名前がこの名前で定義されます。 関数自体とトリガーを区別する目的で、既定の "HTTPTrigger" をそのまま使用せず、"HttpExample" を使用します。 |
    | 承認レベル | **Anonymous** | 匿名認証の場合、関数は公開され、誰でもアクセスできます。 |
    | Select how you would like to open your project (プロジェクトを開く方法を選択してください) | **現在のウィンドウで開く** | 現在の Visual Studio Code ウィンドウでプロジェクトを開きます。 |

    > [!NOTE]
    > Python 3.6 と 3.7 の両方がインストールされている場合、Visual Studio Code では Azure Functions プロジェクトに対して既定で Python 3.6 が使用されます。 現時点で Python 3.7 を使用するには、まず Python 3.7 環境を作成してアクティブ化した後、ターミナルから `func init` コマンドを使用します。 その後、`code .` コマンドを使用して、そのフォルダーから Visual Studio Code を再起動します。

1. 短い時間の経過後、新しいプロジェクトが作成されたことを示すメッセージが表示されます。 **エクスプローラー**には、関数のために作成されたサブフォルダーがあります。Visual Studio Code によって *\_\_init\_\_.py* ファイルが開きますが、このファイルには既定の関数コードが含まれます。

    ![Azure Functions で新しい Python プロジェクトを作成した結果](media/tutorial-vs-code-serverless-python/display-results-of-new-python-project-in-azure-functions.png)

    > [!NOTE]
    > *\_\_init\_\_.py* を開いたとき、Python インタープリターが選択されていないと Visual Studio Code に表示されたら、コマンド パレット (**F1**) を開き、**Python: Select Interpreter** コマンドを選択します。次に、(プロジェクトの一部として作成された) ローカルの `.env` フォルダーで仮想環境を選択します。 前の記事の「[前提条件](tutorial-vs-code-serverless-python-01.md#prerequisites)」で説明したように、この環境は厳密に Python 3.6x をベースにする必要があります。
    >
    > ![Python プロジェクトで作成された仮想環境を選択する](media/tutorial-vs-code-serverless-python/select-virtual-environment-created-with-the-python-project.png)

> [!TIP]
> 同じプロジェクトで別の関数を作成するときは必ず、**Create Function** コマンドを **[Azure:Functions]** エクスプローラーで使用するか、コマンド パレット (**F1**) を開き、**Azure Functions: Create Function** コマンドを探して選択します。 いずれのコマンドでも、関数名 (エンドポイントの名前) が求められ、既定のファイルと共にサブフォルダーが作成されます。
>
> ![Azure Functions エクスプローラーで新規関数を使用して関数を作成する](media/tutorial-vs-code-serverless-python/create-new-functions-in-azure-functions-explorer.png)

> [!div class="nextstepaction"]
> [関数を作成しました - 手順 3 に進む >>>](tutorial-vs-code-serverless-python-03.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=02-create-function)
