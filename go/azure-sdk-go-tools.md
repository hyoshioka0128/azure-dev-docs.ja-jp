---
title: Azure SDK for Go を使用する開発者向けのツール
description: Azure SDK for Go および Azure サービスを操作するためのツール
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.devlang: go
ms.openlocfilehash: 9db908f6da697e4e522064cff830d87278b70b8e
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2019
ms.locfileid: "68291804"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Azure SDK for Go を使用する開発者向けのツール

Go コードを効果的に記述し、Azure サービスでシームレスに動作させるための推奨ツールを紹介します。

## <a name="azure-cli"></a>Azure CLI

Azure CLI は、サブスクリプションで Azure リソースを作成して構成するためのコマンド ライン インターフェイスを提供します。 CLI を使用すると、一般的な共有 Azure リソースの作成を速やかに開始できるため、サービスのより複雑な用途に集中できます。 CLI はクエリとフィルターの機能を備えているので、好みのコマンド ライン ツールに出力を直接パイプ処理できます。 CLI はローカル システムにインストールすることも、Docker イメージとしてインストールすることもできます。また、[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を介してインストールすることも可能です。

> [!div class="nextstepaction"]
> [Azure CLI のインストール](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code は、Go をサポートする軽量なエディターです。 この拡張機能により、オートコンプリート、`impl` テンプレート、リファクタリング、デバッグなどの機能が提供されます。 Visual Studio Code では、エディター内からソース管理にアクセスすることが可能で、Azure サービスを操作するための拡張機能も提供されています。

* [Visual Studio Code をインストールする](https://code.visualstudio.com/Download)
* [Visual Studio Code Go 拡張機能を入手する](https://code.visualstudio.com/docs/languages/go)
* [Visual Studio Code の Azure ツール拡張機能を入手する](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>Azure DevOps プロジェクトによる CI/CD

Azure DevOps プロジェクトのパイプラインを使用すると、Go アプリケーションの継続的インテグレーション システムを設定できます。 Git リポジトリだけで、Azure 上で直接デプロイとテストを実行できます。

> [!div class="nextstepaction"]
> [Azure DevOps Projects で CI/CD パイプラインを作成する方法を確認する](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>dep を使用した依存関係の管理

Azure SDK for Go では、依存関係の管理に dep を使用します。 dep コマンドを使用すると、Go アプリケーションのベンダー要件をプルし、バージョン間の競合を回避し、プロジェクトが適切に動作することを確認できます。

> [!div class="nextstepaction"]
> [dep 依存関係マネージャーを入手する](https://github.com/golang/dep)
