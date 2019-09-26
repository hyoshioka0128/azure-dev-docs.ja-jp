---
title: Visual Studio Code を使用して Azure Functions 用に Python 関数を作成する
description: チュートリアルの手順 2、VS Code 用の Azure Functions 拡張機能の使用方法を紹介する。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: 4427137000e7fca7706b782a73a45db03782872f
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019560"
---
# <a name="create-the-python-function"></a>Python 関数を作成する

[前の手順: 前提条件](tutorial-vs-code-serverless-python-01.md)

1. Azure Functions のコードは、コードの作成前に最初に作成する Functions "_プロジェクト_" 内で管理されます。 **[Azure:Functions]** エクスプローラーで (左側の Azure アイコンを使用して開きます)、 **[新しいプロジェクト]** コマンド アイコンを選択するか、コマンド パレット (F1) を開き、 **[Azure Functions:新しいプロジェクトの作成]** を選択します。

    ![Functions エクスプローラーの [新しいプロジェクトの作成] ボタン](media/tutorial-vs-code-serverless-python/project-create-new.png)

1. 後続のプロンプトの内容:

    | Prompt | 値 | 説明 |
    | --- | --- | --- |
    | Specify a folder for the project (プロジェクトのフォルダーを指定してください) | 現在開いているフォルダー | プロジェクトを作成するフォルダー。 サブフォルダーにプロジェクトを作成することもできます。 |
    | Select a language for your function app project (関数アプリ プロジェクトの言語を選択してください) | **Python** | 関数に使用する言語。これによりコードに使用するテンプレートが決定されます。 |
    | Select a template for your project's first function (プロジェクトの最初の関数のテンプレートを選択してください) | **HTTP トリガー** | HTTP トリガーを使用する関数は、関数のエンドポイントに HTTP 要求が行われるたびに実行されます。 (Azure Functions には他にもさまざまなトリガーがあります。 詳細は、「[Functions でできること](/azure/azure-functions/functions-overview.md#what-can-i-do-with-functions)」を参照してください。) |
    | Provide a function name (関数名を指定してください) | HttpExample | この名前は、構成データと共に関数のコードが含まれるサブフォルダーに使用されます。また、HTTP エンドポイントの名前がこの名前で定義されます。 関数自体とトリガーを区別する目的で、既定の "HTTPTrigger" をそのまま使用せず、"HttpExample" を使用します。 |
    | 承認レベル | **匿名** | 匿名認証の場合、関数は公開され、誰でもアクセスできます。 |
    | Select how you would like to open your project (プロジェクトを開く方法を選択してください) | **現在のウィンドウで開く** | 現在の Visual Studio Code ウィンドウでプロジェクトを開きます。 |

1. 短い時間の経過後、新しいプロジェクトが作成されたことを示すメッセージが表示されます。 **エクスプローラー**には、関数のために作成されたサブフォルダーがあります。Visual Studio Code によって *\_\_init\_\_.py* ファイルが開きますが、このファイルには既定の関数コードが含まれます。

    [![新しい Python 関数プロジェクトを作成した結果](media/tutorial-vs-code-serverless-python/project-create-results.png)](media/tutorial-vs-code-serverless-python/project-create-results.png)

    > [!NOTE]
    > *\_\_init\_\_.py* を開いたとき、Python インタープリターが選択されていないと Visual Studio Code に表示されたら、コマンド パレット (**F1**) を開き、**Python: Select Interpreter** コマンドを選択します。次に、(プロジェクトの一部として作成された) ローカルの `.env` フォルダーで仮想環境を選択します。 前の記事の「[前提条件](tutorial-vs-code-serverless-python-01.md#prerequisites)」で説明したように、この環境は厳密に Python 3.6x をベースにする必要があります。
    >
    > ![プロジェクトで作成された仮想環境を選択する](media/tutorial-vs-code-serverless-python/select-venv-interpreter.png)

> [!TIP]
> 同じプロジェクトで別の関数を作成するときは必ず、**Create Function** コマンドを **[Azure:Functions]** エクスプローラーで使用するか、コマンド パレット (**F1**) を開き、**Azure Functions: Create Function** コマンドを探して選択します。 いずれのコマンドでも、関数名 (エンドポイントの名前) が求められ、既定のファイルと共にサブフォルダーが作成されます。
>
> ![[Azure:Functions] エクスプローラー内の新しい関数を作成するコマンド](media/tutorial-vs-code-serverless-python/function-create-new.png)

> [!div class="nextstepaction"]
> [関数を作成しました](tutorial-vs-code-serverless-python-03.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=02-create-function)
