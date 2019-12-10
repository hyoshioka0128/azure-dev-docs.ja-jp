---
title: コンテナー化された Node.js アプリを VS Code と Azure を使用してデプロイする
description: Node.js アプリの作成、Docker 化、Azure へのデプロイの方法を紹介する完全なエンド ツー エンドのチュートリアル
ms.topic: article
ms.date: 06/25/2017
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: 98b154a78702fa5c5a0808170c8227ce7910459b
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467380"
---
# <a name="develop-and-deploy-a-containerized-nodejs-app-with-visual-studio-code-and-azure"></a>コンテナー化された Node.js アプリを Visual Studio Code と Azure を使用して開発してデプロイする

このチュートリアルでは、既存の Node.js アプリを (Docker で) "コンテナー化" したうえで、Visual Studio Code を使って Azure にデプロイします。

チュートリアルでは、[Scotch.io](https://scotch.io/tutorials/creating-a-single-page-todo-app-with-node-and-angular) によって作成、公開されている単純な To Do アプリを使用します。 これは単一ページの MEAN アプリです。データベースには MongoDB が、REST API/Web サーバーには Node/Express が、フロントエンド UI には Angular.js 1.x が使用されています。 

## <a name="prerequisites"></a>前提条件

このデモに沿って作業するためには、次のソフトウェアがインストールされている必要があります。

- [Visual Studio Code](https://code.visualstudio.com/)
- [Docker](https://www.docker.com/products/docker)
- [DockerHub アカウント](https://hub.docker.com/)
- [Azure CLI 2.0](/cli/azure/install-az-cli2)
- [Azure アカウント](https://azure.microsoft.com/free/)
- [Yarn](https://yarnpkg.com/en/docs/install)
- [Chrome](https://www.google.com/chrome/browser/desktop/) - デモ アプリのフロントエンドをデバッグするために使用します。
- MongoDB - デモ アプリでは MongoDB を使用しているため、MongoDB インスタンスがローカルで実行され、標準 `27017` ポートでリッスンしている必要があります。 MongoDB をローカルで実行する最も簡単な方法は、Docker のインストール後、次の 2 つのコマンドを実行することです。`docker pull mongo` の後に `docker run -it -p 27017:27017 mongo` を実行してください。

## <a name="project-setup"></a>プロジェクトの設定

最初に、次の手順でサンプル プロジェクトをダウンロードします。

1. Visual Studio Code を開きます。

1. **F1** キーを押してコマンド パレットを表示します。

1. コマンド パレットのプロンプトに「`gitcl`」と入力し、**Git: Clone** コマンドを選択して、**Enter** キーを押します。

    ![Visual Studio Code コマンド パレットのプロンプトに gitcl コマンドを入力](./media/node-howto-e2e/visual-studio-code-git-clone.png)

1. **リポジトリの URL** の入力を求められたら、「`https://github.com/scotch-io/node-todo`」と入力し、**Enter** キーを押します。

1. プロジェクトの複製先となるローカル ディレクトリを選択 (または作成) します。

    ![Visual Studio Code エクスプローラー](./media/node-howto-e2e/visual-studio-code-explorer.png)

## <a name="integrated-terminal"></a>統合ターミナル

この Node.js プロジェクトでは、最初に npm からプロジェクトのすべての依存関係を確実にインストールしておく必要があります。  

1. **Ctrl**+ **`** キーを押して Visual Studio Code の統合ターミナルを表示します。 

1. 「`yarn`」と入力し、**Enter** キーを押します。  

     ![Visual Studio Code 内で yarn コマンドを実行](./media/node-howto-e2e/visual-studio-code-install-yarn.png)

## <a name="integrated-git-version-control"></a>Git バージョン コントロールの統合

Yarn でアプリの依存関係をインストールすると、*yarn.lock* ファイルが作成されます。このファイルによって、将来、CI (継続的インテグレーション) ビルドや運用環境のデプロイ、他の開発者のマシンに不測の事態を招くことなく、まったく同じ依存関係を予測可能な方法で再取得できます。

次の手順で、*yarn.lock* ファイルをソース管理にチェックインする方法を説明します。

1. Visual Studio Code 内で、統合された Git タブ (Git ロゴのあるタブ) に切り替えます。

1. **[メッセージ]** ボックスにコミット メッセージを入力し、**Ctrl**+**Enter** キーを押します。

    ![yarn.lock ファイルを Git に追加](./media/node-howto-e2e/visual-studio-code-add-yarn-lock.png)

## <a name="project-and-code-navigation"></a>プロジェクトとコードのナビゲーション

コードベースの操作に慣れるために、Visual Studio Code に備わっているナビゲーション機能の例をいくつか紹介します。

1. **Ctrl**+**P** キーを押します。

1. 「`.js`」と入力すると、プロジェクトに含まれているすべての JavaScript/JSON ファイルが、各ファイルの親ディレクトリと共に表示されます。 

    ![Visual Studio Code ですべての .js* ファイルを表示する](./media/node-howto-e2e/visual-studio-code-javascript-json-file-list.png)

1. *server.js* (アプリのスタートアップ スクリプト) を選択します。

1. **database** 変数 (6 行目のインポート) にマウス カーソルを合わせると、該当する型が表示されます。 ファイル内の変数/モジュール/型をすぐに調べることができるので、プロジェクトの開発時に役立ちます。 

    ![ホバー ヘルプを使用して Visual Studio Code で型を検出する](./media/node-howto-e2e/visual-studio-code-hover-help.png)

1. 変数 (**database** など) 内のどこかでマウスをクリックすると、同じファイル内でその変数が使われている箇所をすべて確認できます。 プロジェクト内である変数が使われている箇所をすべて表示するには、その変数を右クリックし、コンテキスト メニューから、 **[すべての参照の検索]** を選択します。

    ![Visual Studio Code ですべての参照を検索する](./media/node-howto-e2e/visual-studio-code-find-all-references.png)

1. マウス カーソルを変数に合わせることによってその型を調べることに加え、変数の定義を調べることもできます。別のファイルに存在していても問題ありません。 たとえば、**database.localUrl** (12 行目) を右クリックして、コンテキスト メニューから **[定義をここに表示]** を選択します。

    ![Visual Studio Code で変数の定義を表示する](./media/node-howto-e2e/visual-studio-code-peek-definition.png)

## <a name="modifying-the-code-and-using-autocompletion"></a>コードの変更とオートコンプリートの使用

`database.localUrl` プロパティの宣言には、MongoDB の接続文字列がハードコーディングされています。 このセクションでは、環境変数から接続文字列を取得するようにコードを変更します。また、Visual Studio Code のオートコンプリート機能について説明します。  

1. *server.js* ファイルを開きます

1. 次のコードを探してください。

    ```javascript
    mongoose.connect(database.localUrl);
    ```

    次のコードに置き換えます。

    ```javascript
    mongoose.connect(process.env.MONGODB_URL || database.localUrl);
    ```

コードを (コピーして貼り付けるのではなく) 手動で入力した場合、`process` の後にピリオドを入力すると、Visual Studio Code によって Node.js の process グローバル API で使用できるメンバーが表示されることに注目してください。

![オートコンプリートによって API のメンバーが自動的に表示される](./media/node-howto-e2e/visual-studio-code-process-env.png)

オートコンプリートは、Visual Studio Code がバックグラウンドで TypeScript (JavaScript にも対応) を使用し、型情報を提供することによって実現されています。その型情報が、ユーザーが何か入力したときに、入力候補一覧に伝えられます。 Visual Studio Code は、Node.js プロジェクトを認識し、[Node.js に使用される TypeScript の型指定ファイルを NPM から](https://www.npmjs.com/package/@types/node)自動的にダウンロードします。 この型指定ファイルによって、他の Node.js グローバル (`Buffer`、`setTimeout` など) やすべての組み込みモジュール (`fs`、`http` など) でオートコンプリートが利用できるようになります。

型指定の自動取得は、Node.js の組み込み API に対してだけでなく、React、Underscore、Express など、2,000 個を超えるサード パーティ モジュールに対しても機能します。 たとえば、構成されている MongoDB データベース インスタンスに Mongoose が接続できなかった場合に、サンプル アプリがクラッシュしないようにするためには、次のコード行を 13 行目に挿入します。

```javascript
mongoose.connection.on("error", () => { console.log("DB connection error"); });
```

先ほどのコードと同様、何もしなくてもオートコンプリートが作動することがわかります。

![オートコンプリートによって API のメンバーが自動的に表示される](./media/node-howto-e2e/visual-studio-code-autocomplete-mongoose.png)

このオートコンプリート機能がどのモジュールでサポートされているかについては、[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) プロジェクトを参照すると確認できます。このプロジェクトは、TypeScript のすべての型定義のソースとなるもので、コミュニティが中心となって作成しています。

## <a name="running-the-app"></a>アプリの実行

コードをひととおり確認したら、実際にアプリを実行してみましょう。 Visual Studio Code でアプリを実行するには、**F5** キーを押します。 **F5** (デバッグ モード) でコードを実行すると、Visual Studio Code によってアプリが起動され、 **[デバッグ コンソール]** ウィンドウが表示されて、アプリの StdOut が表示されます。

![デバッグ コンソールでアプリの StdOut を監視](./media/node-howto-e2e/visual-studio-code-debug-console.png)

また、**デバッグ コンソール**は最近実行したアプリにアタッチされるので、JavaScript の式を入力すれば、そのアプリ内で評価されます。オートコンプリートも機能します。 この動作を確認するには、コンソールに「`process.env`」と入力します。

![デバッグ コンソールにコードを入力](./media/node-howto-e2e/visual-studio-code-debug-console-autocomplete.png)

先ほど **F5** キーを押してアプリを実行できたのは、現在開いているファイルが JavaScript ファイル (*server.js*) であるためです。 その結果、プロジェクトが Node.js アプリであると Visual Studio Code によって認識されました。 Visual Studio Code で JavaScript ファイルをすべて閉じて、**F5** キーを押した場合は、環境を選択するように求められます。

![ランタイム環境の指定](./media/node-howto-e2e/visual-studio-code-select-environment.png)

ブラウザーを開いて `http://localhost:8080` に移動し、実行中のアプリを表示します。 テキスト ボックスにメッセージを入力し、いくつかの To Do を追加したり削除したりして、アプリの動作を確かめます。

![アプリを使用して To Do を追加または削除する](./media/node-howto-e2e/add-remove-todos-app.png)

## <a name="debugging"></a>デバッグ

Visual Studio Code では、統合コンソールでアプリを実行したり対話的に操作したりすることに加え、コード内に直接ブレークポイントを設定することができます。 たとえば、**Ctrl**+**P** キーを押して、ファイル ピッカーを表示します。 ファイル ピッカーが表示されたら、「`route`」と入力して *route.js* ファイルを選択します。

28 行目にブレークポイントを設定します。これは、アプリが To Do エントリを追加しようとしたときに呼び出される Express ルートを表します。 ブレークポイントは、次の図のようにエディター内で行番号の左側の領域をクリックするだけで設定できます。

![Visual Studio Code 内でブレークポイントを設定](./media/node-howto-e2e/visual-studio-code-set-breakpoint.png)

> [!NOTE]
> Visual Studio Code では標準的なブレークポイントに加え、条件付きブレークポイントもサポートされており、アプリの実行を一時停止するタイミングをカスタマイズすることができます。 条件付きブレークポイントを設定するには、実行を一時停止する行の左側の領域を右クリックし、 **[条件付きブレークポイントの追加]** を選択します。次に、JavaScript の式 (例: `foo = "bar"`) または実行回数を指定して、実行を一時停止する条件を定義します。

ブレークポイントを設定したら、実行中のアプリに戻って、To Do エントリを追加します。 To Do エントリを追加するとすぐに、ブレークポイントを設定した 28 行目でアプリの実行が一時停止されます。

![Visual Studio Code によりブレークポイントで実行が一時停止される](./media/node-howto-e2e/visual-studio-code-pause-breakpoint-execution.png)

アプリケーションが一時停止された後、コードの式にマウス カーソルを合わせると、現在の値を表示することができます。さらに、ローカル変数/ウォッチ式や呼び出し履歴を調べたり、デバッグ ツール バーを使ってコードをステップ実行することもできます。 **F5** キーを押すと、アプリの実行が再開されます。

## <a name="full-stack-debugging"></a>フルスタック デバッグ

このトピックの最初に述べたように、この To Do アプリは MEAN アプリです。つまり、そのフロントエンドとバックエンドはどちらも JavaScript で記述されています。 そのため現在はバックエンド (Node/Express) コードをデバッグしていますが、いずれフロントエンド (Angular) コードのデバッグが必要になることが考えられます。 そのため、Visual Studio Code には広大な拡張機能のエコシステムが存在します。その例として統合 Chrome デバッグが挙げられます。

**[拡張機能]** タブに切り替えて、検索ボックスに「`chrome`」と入力します。

![Visual Studio Code 用の Chrome デバッグ拡張機能](./media/node-howto-e2e/visual-studio-code-chrome-extension.png)

**Debugger for Chrome** という名前の拡張機能を選択し、 **[インストール]** を選択します。 Chrome デバッグ拡張機能をインストールしたら、 **[再読み込み]** を選択してください。拡張機能をアクティブ化するために、Visual Studio Code を一度閉じて開き直します。

![Chrome デバッグ拡張機能をインストールした後で Visual Studio Code を再度読み込む](./media/node-howto-e2e/visual-studio-code-reload-extension.png)

Node.js コードの実行とデバッグを行ううえで Visual Studio Code に特別な構成は必要ありませんでしたが、フロントエンド Web アプリをデバッグするためには、アプリの実行方法を Visual Studio Code に伝える *launch.json* ファイルを生成する必要があります。

*launch.json* ファイルを生成するには、 **[デバッグ]** タブに切り替えて、歯車アイコン (上部に小さな赤色の点が表示されます) をクリックし、**node.js** 環境を選択します。

![launch.json ファイルを構成するための Visual Studio Code オプション](./media/node-howto-e2e/visual-studio-code-debug-gear.png)

作成された *launch.json* ファイルは、次のようになります。アプリをデバッグするために、それをどのように起動し、どのようにアタッチするかが、このファイルによって Visual Studio Code に伝えられます。

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceRoot}/server.js"
        },
        {
            "type": "node",
            "request": "attach",
            "name": "Attach to Port",
            "address": "localhost",
            "port": 5858
        }
    ]
}
```

このアプリのスタートアップ スクリプトが *server.js* であることを Visual Studio Code が検出できたことに注目してください。

*launch.json* ファイルを開いた状態で、 **[構成の追加]** (右下) を選択し、**Chrome:Launch with userDataDir** を選択します。

![Chrome の構成を Visual Studio Code に追加](./media/node-howto-e2e/visual-studio-code-add-chrome-config.png)

Chrome 用に新しい実行構成を追加することで、フロントエンド JavaScript コードをデバッグできるようになります。 

指定されているいずれかの設定にマウス カーソルを合わせると、その設定の内容についてのドキュメントが表示されます。 さらに、アプリの URL が Visual Studio Code によって自動的に検出されることがわかります。 アプリのフロントエンド アセットの検索対象となる場所を Chrome デバッガーが認識できるように、**webRoot** プロパティを `${workspaceRoot}/public` に更新します。

```json
{
   "type": "chrome",
   "request": "launch",
   "name": "Launch Chrome",
   "url": "http://localhost:8080",
   "webRoot": "${workspaceRoot}/public",
   "userDataDir": "${workspaceRoot}/.vscode/chrome"
}
```

フロントエンドとバックエンドの両方を同時に起動/デバッグするためには、"*複合*" 実行構成を作成して、並列実行する一連の構成を Visual Studio Code に伝える必要があります。

次のスニペットを *launch.json* ファイル内の最上位のプロパティとして (既存の **configurations** プロパティの兄弟として) 追加します。

```json
"compounds": [
   {
      "name": "Full-Stack",
      "configurations": ["Launch Program", "Launch Chrome"]
   }
]
```

**compounds.configurations** 配列に指定した文字列値は、**configurations** のリストに含まれる各エントリの **name** を表します。 これらの名前を変更した場合は、配列に適宜変更を加える必要があります。 たとえば、[デバッグ] タブに切り替えて、選択構成を **[Full-Stack]** (複合構成の名前) に変更し、**F5** キーを押して実行します。

![Visual Studio Code での構成の実行](./media/node-howto-e2e/visual-studio-code-full-stack-configuration.png)

この構成を実行すると、デバッグ コンソール出力に見られるような Node.js アプリと、`http://localhost:8080` の Node.js アプリに移動するように構成された Chrome が起動します。

**Ctrl**+**P** キーを押して、*todos.js* と入力 (または選択) します。これが、アプリのフロントエンドのメイン Angular コントローラーになります。

11 行目 (作成中の新しい To Do エントリのエントリ ポイント) にブレークポイントを設定します。

実行中のアプリに戻って新しい To Do エントリを追加すると、Visual Studio Code が今度は Angular コード内で実行を一時停止させていることがわかります。

![Visual Studio Code でのフロントエンド コードのデバッグ](./media/node-howto-e2e/visual-studio-code-chrome-pause.png)

Node.js のデバッグと同様、式にマウス カーソルを置くことでローカル変数/ウォッチ式を確認したり、コンソール内で式を評価したりすることができます。 

ここで便利な機能を 2 つ紹介します。

1. **[呼び出し履歴]** ウィンドウには、2 つの異なるスタックが表示されます: **Node** と **Chrome**。これにより、現在どちらが一時停止されているのかがわかります。

1. フロントエンドとバックエンドのコードの間でステップ実行することができます。**F5** キーを押すと、Express ルートに先ほど設定したブレークポイントに到達します。

この設定によって、フロントエンド、バックエンド、またはフルスタックの JavaScript コードを Visual Studio Code 内から直接効率的にデバッグすることができます。

また、複合デバッガーの概念は、2 つのターゲット プロセスに限定されたものではなく、また JavaScript に限定されたものでもありません。 したがってマイクロサービス アプリ (多言語も含む) の開発を行う場合も、(言語/フレームワークの適切な拡張機能をインストールしたうえで) まったく同じワークフローを利用することができます。

## <a name="dockerizing-the-app"></a>アプリの Docker 化

このセクションでは、[Docker](https://www.docker.com/) を使った開発のために Visual Studio Code が備えている機能について重点的に説明します。 Node.js の開発者は、CI (継続的インテグレーション) と運用環境のどちらの開発においても、Docker を使ってポータブルなアプリのデプロイを提供することができます。 Docker は習得が容易であることから、Visual Studio Code には、アプリに Docker を使って作業を単純化する拡張機能が用意されています。

**[拡張機能]** タブに切り替え、「`docker`」を検索して、**Docker** 拡張機能を選択します。

Docker 拡張機能をインストールし、Visual Studio Code を再度読み込みます。

![Visual Studio Code 用の Docker 拡張機能をインストール](./media/node-howto-e2e/visual-studio-code-docker-extension.png)

Visual Studio Code 用 Docker 拡張機能には、既存プロジェクト用の *Dockerfile* および *docker-compose.yml* ファイルを生成するためのコマンドが含まれています。

利用可能な Docker コマンドを確認するには、コマンド パレットを表示し (**F1** キー)、「`docker`」と入力します。

![Visual Studio Code 用 Docker 拡張機能でサポートされているコマンド ](./media/node-howto-e2e/visual-studio-code-available-docker-codes.png)

**Docker: Add docker files to workspace** を選択してアプリのプラットフォームに **Node.js** を選択し、アプリで公開するポートとして `8080` を指定します。

この Docker コマンドによって、すぐに使うことができる完全な *Dockerfile* と Docker-compose ファイルが生成されます。

![Visual Studio Code で生成された Dockerfile](./media/node-howto-e2e/visual-studio-code-complete-dockerfile.png)

Docker 拡張機能では、*Dockerfile* と *docker-compose.yml* ファイルのオートコンプリートも提供されます。 たとえば、*Dockerfile* を開き、2 行目を変更します。変更前:

```docker
FROM node:latest
```

変更後:

```docker
FROM mhart
```

`mhart` の `t` の後にカーソルを置いて、**Ctrl** +**Space** キーを押すと、`mhart` が DockerHub で公開しているすべてのイメージ リポジトリが表示されます。

![DockerHub のイメージ リポジトリを表示する](./media/node-howto-e2e/visual-studio-code-dockerhub-image-repositories.png)

`mhart/alpine-node` を選択します。このアプリに必要なあらゆるものが、ここから得られます。 

通常、イメージはできるだけ小さくすることをお勧めします。アプリのビルドとデプロイにかかる時間が最小限で済み、配布とスケーリングを迅速に行うことができるためです。

*Dockerfile* を生成したら、実際の Docker イメージをビルドする必要があります。 ここでも、Docker 拡張機能によって Visual Studio Code にインストールされたコマンドを使用することができます。 **F1** キーを押し、コマンド パレットに「`dockerb`」と入力して、次を選択します: **Docker: Build Image** コマンド。 先ほど生成して変更した *Dockerfile* を選択してください。 DockerHub のユーザー名が含まれているタグを指定します (例: `lostintangent/node`)。 **Enter** キーを押して統合ターミナル ウィンドウを起動し、ビルド中の Docker イメージの出力を表示します。

![Docker イメージのビルド出力](./media/node-howto-e2e/docker-build-image-output.png)

このコマンドによって、`docker build` を実行するプロセスが自動化され、ここでも高い生産性が実現されていることに注目してください。この方法を選ばずに、単に Docker CLI を直接使用してもかまいません。

この時点で、このイメージを簡単に取得してデプロイできるようにするために、DockerHub にイメージをプッシュする必要があります。 イメージをプッシュするには、CLI から `docker login` を実行し、アカウントの資格情報を入力して、あらかじめ DockerHub に対して認証を行っておく必要があります。 そのうえで、Visual Studio Code でコマンド パレットを呼び出し、「`dockerpush`」を入力して、`Docker: Push` コマンドを選択します。 先ほどビルドしたイメージ タグ (例: `lostintangent/node`) を選択して、**Enter** キーを押します。 このコマンドによって自動的に `docker push` が呼び出され、統合ターミナルに出力結果が表示されます。

## <a name="deploying-the-app"></a>アプリのデプロイ

Docker でコンテナー化したアプリを作成して DockerHub にプッシュしたところで、それをクラウドにデプロイして公開する必要があります。 デプロイには、Azure の PaaS オファリングである Azure App Service を使用できます。 App Service には、Node.js の開発者に関係した 2 つの機能があります。

- Linux ベースの VM のサポート。ネイティブの Node モジュールを使って作成されたアプリや Windows をサポートしていない (または動作が異なる) 可能性のある各種ツールの互換性の問題が軽減されます。
- Docker ベースのデプロイのサポート。Docker イメージの名前を指定して、そのイメージを App Service で自動的にプル、デプロイ、スケーリングすることができます。

最初に、Visual Studio ターミナルを開きます。 Azure アカウントを管理したり、必要なインフラストラクチャをプロビジョニングして To Do アプリを実行したりするには、新しい Azure CLI 2.0 を使います。 「前提条件」で触れたように、この CLI で `az login` コマンドを使ってアカウントにログインした後、次の手順を実行して App Service インスタンスをプロビジョニングし、To Do アプリのコンテナーをデプロイします。

1. リソース グループを作成します。これは、Azure リソースを体系化するための "*名前空間*" または "*ディレクトリ*" と考えることができます。 グループの名前は `-n` オプションで指定します。任意の名前を使用できます。

    ```shell
    az group create -n nina-demo -l westus
    ```

    `-l` オプションは、リソース グループの場所を示します。 プレビュー段階では、Linux における App Service のサポートが一部のリージョンに限られます。 したがって、ご利用のリージョンが米国西部以外の場合に、他にどのリージョンが利用できるかを調べるには、CLI で `az appservice list-locations --linux-workers-enabled` を実行して、選択可能なデータセンターを確認してください。

1. 今後 CLI でコマンドを呼び出すときに毎回明示的にリソース グループを指定しなくて済むよう、新たに作成したリソース グループを既定のリソース グループとして設定します。

   ```shell
   az configure -d group=nina-demo
   ```

1. App Service "*プラン*" を作成します。アプリのデプロイ先となるベースの仮想マシンの作成とスケーリングは、このプランによって管理されます。 この場合も、`n` オプションには任意の値を指定してください。

    ```shell
    az appservice plan create -n nina-demo-plan --is-linux
    ```

    `--is-linux option` は、Linux ベースの仮想マシンが必要であることを示しています。 指定しなかった場合は、既定で Windows ベースの仮想マシンがプロビジョニングされます。

1. App Service Web アプリを作成します。これは、先ほど作成したプランとリソース グループ内で実行される実際の To Do アプリに相当します。 Web アプリとプランはそれぞれ、プロセス (またはコンテナー) とそれらが実行される仮想マシン (またはコンテナー ホスト) と同義と考えることができます。 さらに、Web アプリを作成する過程で、DockerHub に公開した Docker イメージを使うための構成を、その Web アプリに対して行う必要があります。

    ```shell
    az webapp create -n nina-demo-app -p nina-demo-plan -i lostintangent/node
    ```

    > [!NOTE]
    > カスタム コンテナーではなく Git デプロイを使いたい場合は、記事「[Azure で Node.js Web アプリを作成する](/azure/app-service-web/app-service-web-get-started-nodejs#configure-to-use-nodejs)」を参照してください。

1. Web アプリを既定の Web インスタンスとして設定します。

    ```shell
    az configure -d web=nina-demo-app
    ```

1. アプリを起動して、デプロイしたコンテナーを表示します。コンテナーには、`*.azurewebsites.net` という URL でアクセスできます。

    ```shell
    az webapp browse
    ```

    初回は、App Service が Docker イメージを DockerHub からプルして起動する必要があるために、アプリの読み込みに数分かかる場合があります。

    ![ブラウザーで実行中の To Do アプリ](./media/node-howto-e2e/deployed-container-app.png)

この時点で、To Do アプリのデプロイと実行は完了しています。 回転するアイコンが表示されています。これは、アプリがデータベースに接続できないことを示します。 この問題の原因は、開発中に MongoDB のローカル インスタンスを使用していたためです。当然、Azure データセンター内からそこには到達できません。 先ほどアプリに変更を加え、環境変数を介して接続文字列を取得できるようにしたので、後は MongoDB サーバーを起動し、環境変数を参照するように App Service インスタンスを再構成するだけです。 これらのアクションについては、次のセクションで説明します。

## <a name="provisioning-a-mongodb-server"></a>MongoDB サーバーのプロビジョニング

MongoDB サーバー (またはレプリカ セット) を構成し、そのインフラストラクチャを自分で管理することもできますが、Azure には、[Cosmos DB](https://azure.microsoft.com/services/documentdb/) というソリューションが用意されています。 Cosmos DB は、完全に管理された、geo レプリケーション対応のハイパフォーマンス NoSQL データベースで、MongoDB 互換のレイヤーを備えています。 結果として、接続文字列の変更だけで、既存の MEAN アプリをそれに (または [Studio 3T](https://studio3t.com/) などの MongoDB クライアント/ツールに) 指し示すことができます。 この機能を次の手順で説明します。

1. Visual Studio Code ターミナルで次のコマンドを実行して、MongoDB 互換の Cosmos DB サービス インスタンスを作成します。 **<NAME** プレースホルダーを、グローバルに一意の値に置き換えます (この名前は、Cosmos DB がデータベースのサーバー URL を生成するときに使用されます)。

   ```shell
   COSMOSDB_NAME=<NAME>
   az cosmosdb create -n $COSMOSDB_NAME --kind MongoDB
   ```

1. このインスタンスの MongoDB 接続文字列を取得します。

   ```shell
   MONGODB_URL=$(az cosmosdb list-connection-strings -n $COSMOSDB_NAME -otsv --query "connectionStrings[0].connectionString")
   ```

1. ローカルで実行されている (実際には存在しない) MongoDB サーバーに接続を試みるのではなく、新しくプロビジョニングした Cosmos DB インスタンスが接続先となるように、Web アプリの **MONGODB_URL** 環境変数を更新します。

    ```shell
    az webapp config appsettings set --settings MONGODB_URL=$MONGODB_URL
    ```

1. ブラウザーに戻って最新の情報に更新します。 To Do 項目の追加と削除を試して、何も変更しなくてもアプリが正常に動作することを実証します。 作成済みの Cosmos DB インスタンスを環境変数に設定します。MongoDB データベースは、この Cosmos DB インスタンスによって完全にエミュレートされます。

    ![データベースに接続した後のデモ アプリ](./media/node-howto-e2e/finish-demo-walkthrough.png)

必要に応じて Cosmos DB インスタンスに戻り、MongoDB インスタンスが必要とする予約済みスループットをスケールアップ (またはスケールダウン) すれば、インフラストラクチャを手動で管理することなく、トラフィックの増大に対応できるメリットを活かすことができます。

さらに、Cosmos DB では、あらゆるドキュメントおよびプロパティのインデックスが自動的に作成されます。 そのため低速クエリを自分でプロファイリングしたり、インデックスを手動で微調整したりする必要はありません。 プロビジョニングして、必要に応じてスケーリングを行うだけで、後は Cosmos DB が対処します。

## <a name="hosting-a-private-docker-registry"></a>プライベート Docker レジストリのホスティング

DockerHub は、コンテナー イメージを配布する手段としてきわめて高い利便性を備えていますが、状況によっては、独自のプライベート Docker レジストリをホスティングした方が都合がよい場合もあります (セキュリティやガバナンス、パフォーマンス上の優位性など)。 そのため Azure には、独自の Docker レジストリを立ち上げることができる [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR) が用意されています。ACR ではバッキング ストレージが Web アプリと同じデータ センターに置かれるため、プルにかかる時間が短縮されます。 また ACR では、コンテンツとアクセス管理 (イメージのプッシュまたはプルをだれに許可するかなど) を完全に自分で制御することができます。

カスタム レジストリをプロビジョニングするには、次のコマンドを実行します。 **<NAME** プレースホルダーを、グローバルに一意の値に置き換えます。指定した値は、ACR がレジストリのログイン サーバー URL を生成するときに使用されます。

```shell
ACR_NAME=<NAME>
az acr create -n $ACR_NAME -l westus --admin-enabled
```

> [!NOTE]
> このトピックの例では、単純化するために**管理者アカウント**を使っていますが、運用環境のレジストリではお勧めできません。

`az acr create` コマンドは、Docker CLI を使ってログインするときに使用するログイン サーバー URL を (`LOGIN SERVER` 列に) 表示します (例: `ninademo.azurecr.io`)。 また、このコマンドを実行すると、管理者の認証に使用するための資格情報が生成されます。 それらの資格情報を取得するには、次のコマンドを実行し、表示されたユーザー名とパスワードをメモしてください。

```shell
az acr credential show -n $ACR_NAME
```

レジストリには、前の手順で取得した資格情報と、個々に割り当てられたログイン サーバーを使い、標準的な Docker CLI ワークフローでログインすることができます。

```shell
docker login <LOGIN_SERVER> -u <USERNAME> -p <PASSWORD>
```

Docker コンテナーにタグ付けし、プライベート レジストリに関連付けられていることを示すには、次のコマンドを使用します。`lostintangent/node` には、実際のコンテナー イメージに付けた名前を指定してください。

```shell
docker tag lostintangent/node <LOGIN_SERVER>/lostintangent/node
```

最後に、タグ付けしたイメージをプライベート Docker レジストリにプッシュします。

```shell
docker push <LOGIN_SERVER>/lostintangent/node
```

これでコンテナーが独自のプライベート レジストリに格納されました。また Docker CLI は、DockerHub を使用していたときと同じように利用することができます。 プライベート レジストリからプルするよう App Service Web アプリに命令するには、次のコマンドを実行するだけです。

```shell
az appservice web config container set \
    -r <LOGIN_SERVER> \
    -c <LOGIN_SERVER>/lostintangent/node \
    -u <USERNAME> \
    -p <PASSWORD>
```

`-r` オプションの先頭には、必ず `https://` プレフィックスを追加してください。 一方、コンテナー イメージの名前にはプレフィックスを追加しないでください。

ブラウザーでアプリを最新の情報に更新しても、一見、変わったところがないように見えます。 しかし現在は、アプリがプライベート Docker レジストリを介して実行されています。 アプリを更新したら、先ほどと同じようにタグ付けして変更をプッシュし、App Service コンテナーの構成でタグを更新してください。

## <a name="configuring-a-custom-domain-name"></a>カスタム ドメイン名の構成

テスト目的であれば、`*.azurewebsites.net` という URL でまったく問題ありませんが、いずれはカスタム ドメイン名を Web アプリに追加することが必要になると考えられます。 レジストラーからドメイン名を取得したら、Web アプリの外部 IP (実際にはロード バランサー) を指す `A` レコードをそこに追加する必要があります。 この IP は、次のコマンドを実行することで取得できます。

```shell
az webapp config hostname get-external-ip
```

`A` レコードを追加することに加え、これまで使用してきた `*.azurewebsites.net` ドメインを指す `TXT` レコードをドメインに追加する必要があります。 Azure では、ユーザーがドメインを所有していることが `A` レコードと `TXT` レコードの組み合わせによって確認されます。

これらのレコードが作成されて DNS の変更が反映されたら、そのカスタム ドメインを Azure に登録して、受信トラフィックを正しく予測できるようにしてください。

```shell
az webapp config hostname add --hostname <DOMAIN>
```

> [!NOTE]
> このコマンドは、DNS の変更が反映されるまで正しく機能しません。

ブラウザーを開いてカスタム ドメインにアクセスし、Azure 上にデプロイされているアプリへと解決されることを確かめましょう。

## <a name="scaling-up-and-out"></a>スケールアップとスケールアウト

将来的に Web アプリの利用者が増えてくると、割り当て済みのリソース (CPU と RAM) では、運用上の需要やトラフィックの増大に追い付かなくなる可能性があります。 先ほど作成した App Service プラン (**B1**) で利用できるのは 1 CPU コアと 1.75 GB の RAM であり、これはたやすく過負荷状態になる可能性があります。 **B2** プランではその 2 倍の RAM と CPU が利用できるため、アプリでいずれかが不足し始めたら、次のコマンドを実行することで、基になる仮想マシンをスケールアップすることができます。

```shell
az appservice plan update -n nina-demo-plan --sku B2
```

> [!NOTE]
> Azure アプリ プランの料金の詳細と仕様については、記事「[App Service の価格](https://azure.microsoft.com/pricing/details/app-service/)」を参照してください。

少しすると、要求したハードウェアに Web アプリが移行され、そのリソースを活用できるようになります。 スケールアップだけではありません。より少ないリソースをより低価格で利用できる `--sku` オプションを指定して、先ほどと同じコマンドを実行すれば、スケールダウンすることもできます。

また、仮想マシンの仕様をスケールアップする以外にも、Web アプリがステートレスであれば、基になる仮想マシン インスタンスを増やすことによる "*スケールアウト*" を行うこともできます。 先ほど作成した App Service プランには仮想マシンが 1 つ (*worker*) しか含まれていないので、結局のところ、その 1 つのインスタンスで利用できるリソースの上限が、全受信トラフィックの限界となります。 仮想マシン インスタンスをもう 1 つ追加する必要がある場合も、先ほどと同じコマンドを使用できます。ただし、SKU をスケールアップするのではなく、worker 仮想マシンの数をスケールアウトします。

```shell
az appservice plan update -n nina-demo-plan --number-of-workers 2
```

このように Web アプリをスケールアウトすると、すべてのインスタンス間で受信トラフィックが透過的に負荷分散されるので、コードに変更を加えたり必要なインフラストラクチャについて心配したりすることなく、キャパシティをすぐに増やすことができます。

Web アプリはステートレスにすることをお勧めします。機能するうえで必要な状態が、どれか 1 つの仮想マシンまたはアプリ インスタンスに格納されることがないため、そのときの状況に合わせて自在にスケーリング (スケールアップ、スケールダウン、スケールアウト) を行えます。

> [!NOTE]
> この記事では、App Service プランで実行する Web アプリは 1 つだけですが、同じプランに複数の Web アプリを作成してデプロイできます。そうすれば、プロビジョニングと料金の支払い対象を 1 つのプランにまとめることができます。

## <a name="clean-up"></a>クリーンアップ

使用しない Azure リソースに対して料金が請求されないように、Visual Studio Code ターミナルで次のコマンドを実行して、このチュートリアルでプロビジョニングしたリソースをすべて削除します。

```shell
az group delete
```

クリーンアップ プロセスは、完了までに数分かかる場合があります。 完了後、`az group delete` コマンドを実行することで、チュートリアルを開始する前の状態に Azure アカウントを戻すことができます。 複数の Azure リソースを 1 つの単位にまとめ、デプロイし、削除できるのは、リソース グループの大きな利点です。 そのため、使用期間が同じであると考えられる自分のリソースは、グループ化することをお勧めします。
