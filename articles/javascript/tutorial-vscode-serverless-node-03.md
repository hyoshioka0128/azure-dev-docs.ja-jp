---
title: Visual Studio Code で Azure Functions アプリケーションをローカルで実行する
description: サーバーレス チュートリアル パート 3、アプリをローカルで実行してテストする。
ms.topic: tutorial
ms.date: 09/23/2019
ms.custom: devx-track-js
ms.openlocfilehash: 94b646d29b7459b2a74a1dbdaafde4a98a348f6b
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364585"
---
# <a name="test-the-function-locally"></a>関数をローカルでテストする

[前の手順:Functions アプリを作成する](tutorial-vscode-serverless-node-02.md)

この手順では、Azure にデプロイする前に、Azure Functions プロジェクトをローカルで実行してテストします。

Functions アプリを作成したときに、Azure Functions 拡張機能によって、VS Code 起動構成がプロジェクトに自動的に追加されました。これは、 *.vscode/launch.json* ファイルにあります。 この構成では、Azure で実行されるのと同じランタイムが使用されるため、クラウドにデプロイする前にソース コードが動作することを確認できます。

1. Visual Studio Code で、**F5** を押して (または **[デバッグ]**  >  **[Start Debugging]\(デバッグの開始\)** メニュー コマンドを使用して) デバッガーを起動し、Azure Functions ホストに接続します。 (このコマンドでは、Azure Functions が作成した単一のデバッグ構成が自動的に使用されます。)

1. Functions Core ツールの出力は、VS Code の **[Terminal]\(ターミナル\)** パネルに表示されます。 ホストが起動したら、出力に表示されるローカル URL を **Alt** を押しながらクリックしてブラウザーを開き、関数を実行します。

    ![ローカルでデバッグをしているときに VS Code の [Terminal]\(ターミナル\) パネルに表示される出力](media/functions-extension/local-test-output.png)

1. 既定の HTTP トリガーによって作成されたコードは、`name` クエリ パラメーターを解析して、応答をカスタマイズします。 ブラウザーで、`?name=<yourname>` をブラウザーの URL に追加して、応答が正しく出力されることを確認します。

    ![URL パラメーターを解析する HTTP トリガー関数](media/functions-extension/local-test-browser.png)

1. 関数をローカルで実行すると、コードのさまざまな部分にブレークポイントを設定できます。 (VS Code でのブレークポイントとデバッグの詳細については、「[デバッグ](https://code.visualstudio.com/docs/editor/debugging)」を参照してください。)*index.js* を開き、エディター ウィンドウで 11 行目の左側の余白をクリックします。 ブレークポイントを示す小さな赤い点が表示されます。 次に、ブラウザーの URL から `?name=` 引数を削除します。 ブラウザーがその要求を行うと、VS Code はそのブレークポイントの関数コードを停止します。

    ![VS Code がブレークポイントで停止](media/functions-extension/debugging-breakpoint.png)

> [!Note]
>
> このプロセスで実行ポリシー エラーが発生した場合は、npm を使用して `azure-functions-core-tools@3` をアンインストールしてから、管理者特権のターミナルで Chocolatey を使用してパッケージを再インストールします。

> [!div class="nextstepaction"]
> [Function アプリをローカルで実行しました](tutorial-vscode-serverless-node-04.md) [問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)
