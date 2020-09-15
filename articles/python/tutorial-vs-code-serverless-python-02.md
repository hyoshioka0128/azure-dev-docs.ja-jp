---
title: 手順 2:VS Code を使用して Azure Functions 用に Python 関数を作成する
description: チュートリアルの手順 2、VS Code 用の Azure Functions 拡張機能の使用方法を紹介する。
ms.topic: conceptual
ms.date: 05/19/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 08f301ef4596ae8aac96051a13fd680917bd79be
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473567"
---
# <a name="2-create-a-python-function-for-azure-functions"></a>2:Azure Functions 用に Python 関数を作成する

[前の手順: 前提条件](tutorial-vs-code-serverless-python-01.md)

この記事では、Visual Studio Code を使用して Azure Functions 用に Python 関数を作成します。 Azure Functions のコードは、コードの作成前に最初に作成する Functions "_プロジェクト_" 内で管理されます。

1. **Azure:Functions** エクスプローラーで (左側の Azure アイコンを使用して開きます)、**新しいプロジェクトの作成** コマンド アイコンを選択するか、コマンド パレット (F1) を開き、**Azure Functions:新しいプロジェクトの作成** を選択します。

    ![Azure Functions エクスプローラーで新しいプロジェクトを作成する](media/tutorial-vs-code-serverless-python/create-a-new-project-in-azure-functions-explorer.png)

1. 後続のプロンプトの内容:

    | Prompt | 値 | 説明 |
    | --- | --- | --- |
    | Specify a folder for the project (プロジェクトのフォルダーを指定してください) | 現在開いているフォルダー | プロジェクトを作成するフォルダー。 サブフォルダーにプロジェクトを作成することもできます。 |
    | Select a language for your function app project (関数アプリ プロジェクトの言語を選択してください) | **Python** | 関数に使用する言語。これによりコードに使用するテンプレートが決定されます。 |
    | Select Python interpreter (Python インタープリターを選択してください) | (指定された既定のパスを使用します。) | 仮想環境に使用する Python インタープリター。 |
    | Select a template for your project's first function (プロジェクトの最初の関数のテンプレートを選択してください) | **HTTP トリガー** | HTTP トリガーを使用する関数は、関数のエンドポイントに HTTP 要求が行われるたびに実行されます。 (Azure Functions には他にもさまざまなトリガーがあります。 詳細は、「[Functions でできること](/azure/azure-functions/functions-overview#what-can-i-do-with-functions)」を参照してください。) |
    | Provide a function name (関数名を指定してください) | HttpExample | この名前は、構成データと共に関数のコードが含まれるサブフォルダーに使用されます。また、HTTP エンドポイントの名前がこの名前で定義されます。 関数自体とトリガーを区別する目的で、既定の "HTTPTrigger1" をそのまま使用せず、"HttpExample" を使用します。 |
    | 承認レベル | **Anonymous** | 匿名認証の場合、関数は公開され、誰でもアクセスできます。 |

1. 短い時間の経過後、新しいプロジェクトが作成されたことを示すメッセージが表示されます。 **エクスプローラー**には、関数のために作成されたサブフォルダーがあります。Visual Studio Code によって *\_\_init\_\_.py* ファイルが開きますが、このファイルには既定の関数コードが含まれます。

    ![Azure Functions で新しい Python プロジェクトを作成した結果](media/tutorial-vs-code-serverless-python/display-results-of-new-python-project-in-azure-functions.png)

    *\_\_init\_\_.py* を開いたとき、Python インタープリターが選択されていないと Visual Studio Code に表示されたら、コマンド パレット (**F1**) を開き、**Python: Select Interpreter** コマンドを選択します。次に、(プロジェクトの一部として作成された) ローカルの `.env` フォルダーで仮想環境を選択します。

    ![Python プロジェクトで作成された仮想環境を選択する](media/tutorial-vs-code-serverless-python/select-virtual-environment-created-with-the-python-project.png)

> [!TIP]
> 同じプロジェクトで別の関数を作成するときは必ず、**Create Function** コマンドを **[Azure:Functions]** エクスプローラーで使用するか、コマンド パレット (**F1**) を開き、**Azure Functions: Create Function** コマンドを探して選択します。 いずれのコマンドでも、関数名 (エンドポイントの名前) が求められ、既定のファイルと共にサブフォルダーが作成されます。
>
> ![Azure Functions エクスプローラーで新規関数を使用して関数を作成する](media/tutorial-vs-code-serverless-python/create-new-functions-in-azure-functions-explorer.png)

> [!div class="nextstepaction"]
> [関数を作成しました - 手順 3 に進む >>>](tutorial-vs-code-serverless-python-03.md)

問題がある場合 ページの下部にある [このページ] へのフィードバックを使用して、GitHub の問題を送信します。
