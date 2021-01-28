---
title: JavaScript 開発者にとって Azure とは
description: JavaScript、TypeScript、Node.js 開発者のための Azure の概念。
ms.topic: conceptual
ms.date: 12/15/2020
ms.custom: devx-track-js
ms.openlocfilehash: b6d0e54bdaf1b0ea9adba9800de58fe664df8324
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699800"
---
# <a name="what-is-azure-for-javascript-developers"></a>JavaScript 開発者にとって Azure とは

Azure は、あらゆるホスティング オプションとクラウドベースのサービスが提供されているクラウド プラットフォームです。 初めてクラウド開発を行う場合は、Azure について詳しく学習してください。

* [Azure アーキテクチャ センター](/azure/architecture/) 
* [Azure の用語](/azure/cloud-adoption-framework/ready/considerations/fundamental-concepts)
* [Azure アプリケーションの 10 の設計原則](/azure/architecture/guide/design-principles/)
* [クラウド設計パターン](/azure/architecture/patterns/)

## <a name="javascript-typescript-and-other-languages"></a>JavaScript、TypeScript、その他の言語

Azure ランタイムによる JavaScript のサポートでは、TypeScript や、JavaScript にトランスパイルされるその他のフレーバーもサポートされています。 

## <a name="run-javascript-with-hosted-runtime"></a>ホストされたランタイムで JavaScript を実行する 

* Azure [App Service](/azure/app-service/) では、Node.js ランタイム エンジンが使用されます。 サポートされているすべての Node.js バージョンを表示するには、[Cloud Shell](https://shell.azure.com) で次のコマンドを実行します。

    ```azurecli-interactive
    az webapp list-runtimes | grep node
    ```

* Azure [Functions によってサポートされる Node.js のバージョン](/azure/azure-functions/functions-reference-node?tabs=v2#node-version)は、使用している Functions のバージョンに基づきます。 

* カスタム ランタイム - カスタム ランタイムは、次の方法でサポートされます。

    * [仮想マシン](/azure/virtual-machines/)
    * コンテナー - [単一](/azure/container-instances/)、[Web アプリ](/azure/app-service/)、[Kubernetes](/azure/aks/)
    * (サーバーレス) Functions - [カスタム ハンドラー](/azure/azure-functions/functions-custom-handlers)を使用する

## <a name="run-javascript-with-azure-sdks"></a>Azure SDK を使用して JavaScript を実行する

すべての Azure SDK は JavaScript で実行され、他のツールは使用されません。 最新の SDK は TypeScript で記述されており、型チェック用に `*.d.ts` ファイルが提供されますが、Azure SDK または Azure クラウド サービスを使用するために TypeScript は必要ありません。 

ホストされている場所 (ローカル、ハイブリッド、クラウド) に関係なく、JavaScript コードで Azure サービスを使用できます。 JavaScript を使用してプログラムで Azure サービスを使用するための推奨される方法は、Azure SDK です。 これらの SDK には、少なくとも Node.js ランタイム バージョン 8 以降が必要です。 

## <a name="azure-concepts-and-terminology"></a>Azure の概念と用語

Azure を使い始めるには、次のことを行う必要があります。
* [無料のサブスクリプション](https://azure.microsoft.com/en-us/free/)を作成します
* リソースを作成します。以下のことが含まれます。
    * [Azure portal](https://portal.azure.com/) にログインします
    * リソース グループを作成します - Azure リソースの論理コレクションを保持するため
    * リソース グループにリソースを作成します
* リソースに応じてリソースを使用します。これには次のものが含まれます。
    * Azure Portal の場合
    * Azure CLI の場合
    * PowerShell の場合
    * Azure SDK

[Azure の概念と用語の概要](/azure/cloud-adoption-framework/ready/considerations/fundamental-concepts)について、さらに学習してください。 

Azure からは、リソースを見つけやすくするための[名前付け規則とリソースのタグ付け](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)についてのガイダンスが提供されています。 

## <a name="select-development-ide-and-set-up-environment"></a>開発 IDE を選択して環境を設定する

任意の統合開発環境で JavaScript アプリケーションを開発します。 JavaScript の開発には、**Visual Studio Code** をお勧めします。 

[JavaScript 開発者に推奨されるツール](../node-azure-tools.md)について、さらに学習してください。 

## <a name="select-azure-account-subscription-resource-and-tag"></a>Azure アカウント、サブスクリプション、リソース、タグを選択する

* **Azure アカウント** は、メール アドレスによってサブスクリプションに関連付けられます。 1 人で 1 つまたは複数の Azure アカウントを使用できます。

* **サブスクリプション** は、リソースの編成と請求の単位です。 1 人で 1 つ以上のサブスクリプションを使用できます。 [最初の運用サブスクリプションの作成](/azure/cloud-adoption-framework/ready/azure-best-practices/initial-subscriptions)について、さらに学習してください。

* **リソース グループ** は、サブスクリプション内での 1 つ下の編成レベルです。 プロジェクトごと、または一緒に使用する予定の Azure サービスのグループごとに、新しいリソース グループを作成します。 

* **リソース** の名前を選択するときは、どのようなときにそのリソース名がリソースの URL エンドポイントに対して使用されるかを把握しておくことが重要です。 また、リソースを作成した後で、多くのリソースにカスタム ドメイン名を割り当てることもできます。 

* リソースをさらに流動的にグループ化するには、プロパティ名と値を使用して **リソースにタグを付けます**。 タグの一般的な使用方法は、作成するすべてのリソースのプロジェクト所有者として、名前またはメールを追加することです。 サブスクリプション管理者は、ユーザーが所有しているリソースを簡単に見つけたり、使用や請求で問題が発生したときに所有権を決定したりできます。 

[体系的な名前付け規則](/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming)は、運用レベルでの Azure の使用の重要な部分です。

## <a name="create-azure-resources"></a>Azure リソースを作成する

Azure の新規ユーザー用にリソースを作成する最も一般的な方法は、Azure portal を使用することです。 作成手順で、指定する必要があるすべての選択肢が、推奨される選択と共に表示されます。

### <a name="pricing-tiers"></a>価格レベル

価格レベルは、リソースの課金方法です。 リソースの課金について理解するには、[Azure 料金計算ツール](https://azure.microsoft.com/en-us/pricing/calculator)を使用します。 

### <a name="free-tier-resources"></a>Free レベルのリソース

Free (F0) 価格レベルを選択するときは、そのプランに伴う制限を理解しておくことが重要です。

Free レベルが提供されている場合:

* サブスクリプションは、そのサービスの 1 つの無料リソースに制限されることがあります。 無料リソースを作成できない場合は、そのサブスクリプションに無料リソースが既に存在することを示します。
* 価格レベルのクォータ (1 秒あたりのトランザクション数 (TPS) または 1 か月あたりのトランザクション数 (TPS) のいずれか) を超過すると、HTTP エラーとクォータ超過を示すメッセージをアプリケーションで受け取ります。 

### <a name="resource-creation-time"></a>リソースの作成時間

ほとんどのリソースの作成には数分かかりますが、数分かかるものもあります。 リソースが使用できるようになると、リソースへのリンクと共に Azure portal の通知ウィンドウに表示されます。 

## <a name="install-azure-sdk-for-resources"></a>リソース用の Azure SDK をインストールする

JavaScript 開発者がプログラムでリソースを使用する場合は、[**Azure SDK**](../azure-sdk-library-package-index.md) を利用することをお勧めします。 

Azure リソースは、次のものからも利用できます。
* [Azure CLI](/cli/azure/install-azure-cli) と [Azure Cloud Shell](https://shell.azure.com/)
* [Azure PowerShell](/powershell/azure/)
* [REST API](/rest/api/azure/)

## <a name="deploy-web-apps-to-hosting-options"></a>ホスティング オプションに Web アプリをデプロイする

ホスティング オプションを使用すると、Azure をアプリケーション用にすばやく使用できます。 Azure の最も一般的な初期エクスペリエンスについては、ホスティングに関する次のクイックスタートとチュートリアルを参照してください。

* クライアントおよび静的アプリケーション
    * [Vanilla JS](/azure/static-web-apps/getting-started?tabs=vanilla-javascript)
    * [React](/azure/static-web-apps/getting-started?tabs=react)
    * [Angular](/azure/static-web-apps/getting-started?tabs=angular)
    * [Vue](/azure/static-web-apps/getting-started?tabs=vue)
* サーバー アプリケーション 
    * [Visual Studio Code から App Service に Express.js MongoDB アプリをデプロイする](../tutorial/deploy-nodejs-mongodb-app-service-from-visual-studio-code.md)
* コンテナー アプリケーション 
    * [Visual Studio Code を使用して、Express.js のコンテナー化されたアプリをプライベート コンテナー レジストリから App Service にデプロイする](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md?tabs=bash)
* 仮想マシン アプリケーション
    * [Azure CLI と GitHub Actions を使用して、Express.js アプリで Linux 仮想マシンを作成してデプロイする](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md)

[ホスティング オプション](../how-to/deploy-web-app.md)の詳細を確認する。

## <a name="next-steps"></a>次の手順

* [Node.js をインストールする](install-nodejs-develop-azure-sdk-project.md)
* [Azure JavaScript 開発者向けに推奨されるツールについて学習する](../node-azure-tools.md)
