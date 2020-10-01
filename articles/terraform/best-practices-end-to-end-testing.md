---
title: チュートリアル - Terraform プロジェクトでエンド ツー エンドの Terratest テストを設定する
description: Terraform プロジェクトでの Terratest を使用したエンド ツー エンド テストの詳細について説明します。
ms.topic: tutorial
ms.date: 07/31/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: b760908bf1950751b93ba1787f444ca37ee8bf83
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401712"
---
# <a name="tutorial-setup-end-to-end-terratest-testing-on-terraform-projects"></a>チュートリアル:Terraform プロジェクトでエンド ツー エンドの Terratest テストを設定する

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

この記事では、次のタスクを行う方法について説明します。

> [!div class="checklist"]
> * [Terratest](https://github.com/gruntwork-io/terratest) を使用したエンド ツー エンドのテストの基本を理解する
> * Golang を使用してエンド ツー エンド テストを記述する方法について学ぶ
> * コードがリポジトリにコミットされたときに、Azure DevOps を使用して自動的にエンド ツー エンド テストをトリガーする方法を学ぶ

## <a name="prerequisites"></a>前提条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Terraform のインストール**:ご使用の環境に応じて、[Terraform をダウンロードしてインストール](https://www.terraform.io/downloads.html)します。
- **テスト サンプルをフォークする**: すぐに開始できるように、[このリポジトリ](https://github.com/Azure/terraform)をご自身の GitHub 組織にフォークすることをお勧めします。
- **Go プログラミング言語**: Terraform テスト ケースは [Go](https://golang.org/dl/) で記述します。 この記事のサンプルでは、[Go モジュール](https://blog.golang.org/using-go-modules)を使用します。 この記事では、Go 1.13 (またはそれ以降) をお勧めします。

## <a name="what-is-end-to-end-testing"></a>エンド ツー エンド テストとは

エンド ツー エンド テストでは、システムが 1 つのまとまりとして機能しているかどうかを検証します。 この種類のテストは、特定のモジュールのテストとは対照的です。 Terraform プロジェクトの場合、エンド ツー エンド テストによって、何がデプロイ済みであるかを検証できます。 この種類のテストは、デプロイ前のシナリオをテストする他の多くの種類とは異なります。 エンド ツー エンド テストは、複数のモジュールを含み、複数のリソースに作用する複雑なシステムをテストする場合に不可欠です。 このようなシナリオの場合、エンド ツー エンド テストは、さまざまなモジュールが正しく連携しているかどうかを判定する唯一の方法です。

この記事では、[Terratest](https://github.com/gruntwork-io/terratest) を使用したエンド ツー エンド テストの実装に焦点を当てています。 Terratest は、次のタスクを実行するために必要なすべての機能を備えています。

- Terraform の構成をデプロイする
- 何がデプロイ済みであるかを検証するためのテストを、Go 言語を使用して記述できるようにする
- テストを複数の段階に調整する
- デプロイされたインフラストラクチャを破棄する

## <a name="tutorial-scenario"></a>チュートリアルのシナリオ

このチュートリアルでは、[Azure/terraform サンプル リポジトリ](https://github.com/Azure/terraform/blob/master/samples/end-to-end-testing/README.md)で入手できるサンプルを使用します。

このサンプルでは、2 つの Linux 仮想マシンを同じ仮想ネットワークにデプロイする Terraform の構成を定義します。 1 つの VM (`vm-linux-1` という名前) にはパブリック IP アドレスがあります。 SSH 接続を許可するためにポート 22 だけが開かれています。 2番目の VM (`vm-linux-2`) には、パブリック IP アドレスは定義されていません。

このテストでは、次のシナリオを検証する必要があります。

- インフラストラクチャが正しくデプロイされている
- ポート 22 を使用して、`vm-linux-1` への SSH セッションを開くことができる
- `vm-linux-1` の SSH セッションを使用して、`vm-linux-2` を ping できる

![エンド ツー エンド テストのサンプル シナリオ](media/best-practices-end-to-end-testing/scenario.png)

[サンプルをダウンロード](#prerequisites)した場合、このシナリオの Terraform の構成は `src/main.tf` ファイルにあります。 このファイルには、前の図に示されている Azure インフラストラクチャをデプロイするために必要なものがすべて含まれています。

仮想マシンの作成に慣れていない場合は、「[Terraform を使用して Azure に Linux VM とインフラストラクチャを作成する](create-linux-virtual-machine-with-infrastructure.md)」を参照してください。

> [!CAUTION]
> この記事に記載されているサンプル シナリオは、例示のみを目的としています。 エンド ツー エンド テストの手順に集中するために、内容を意図的に単純化しています。 運用仮想マシンでパブリック IP アドレスを介して SSH ポートを公開することはお勧めしません。

## <a name="end-to-end-test"></a>エンド ツー エンド テスト

エンド ツー エンド テストは Go 言語で記述し、Terratest フレームワークを使用します。 [サンプルをダウンロード](#prerequisites)した場合、`src/test/end2end_test.go` ファイルにそれが定義されています。

次のソース コードは、Terratest を使用した Golang テストの標準構造を示しています。

```Go
package test

import (
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    test_structure "github.com/gruntwork-io/terratest/modules/test-structure"
)

func TestEndToEndDeploymentScenario(t *testing.T) {
    t.Parallel()

    fixtureFolder := "../"

    // User Terratest to deploy the infrastructure
    test_structure.RunTestStage(t, "setup", func() {
        terraformOptions := &terraform.Options{
            // Indicate the directory that contains the Terraform configuration to deploy
            TerraformDir: fixtureFolder,
        }

        // Save options for later test stages
        test_structure.SaveTerraformOptions(t, fixtureFolder, terraformOptions)

        // Triggers the terraform init and terraform apply command
        terraform.InitAndApply(t, terraformOptions)
    })

    test_structure.RunTestStage(t, "validate", func() {
        // run validation checks here
        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
            publicIpAddress := terraform.Output(t, terraformOptions, "public_ip_address")
    })

    // When the test is completed, teardown the infrastructure by calling terraform destroy
    test_structure.RunTestStage(t, "teardown", func() {
        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
        terraform.Destroy(t, terraformOptions)
    })
}
```

前のコード スニペットにあるように、テストは次の 3 つのステージで構成されています。

- **setup**:Terraform を実行して構成をデプロイします。
- **validate**:検証チェックとアサーションを行います。
- **teardown**:テストの実行後にインフラストラクチャをクリーンアップします。

次の一覧は、Terratest フレームワークによって提供される主な関数の一部を示しています。

- **terraform.InitAndApply**:Go コードから `terraform init` と `terraform apply` を実行できるようにします。
- **terraform.Output**:デプロイ出力変数の値を取得します。
- **terraform.Destroy**:Go コードから `terraform destroy` コマンドを実行します。
- **test_structure.LoadTerraformOptions**:構成や変数などの Terraform オプションを状態から読み込みます。
- **test_structure.SaveTerraformOptions**:構成や変数などの Terraform オプションを状態に保存します。

## <a name="run-the-end-to-end-test"></a>エンド ツー エンド テストを実行する

このセクションでは、サンプルの構成とデプロイに対してテストを実行します。 

1. Bash/ターミナル ウィンドウを開きます。

1. Azure アカウントにログインします。 Azure サブスクリプションへのログインについては、[Azure への認証に関するセクション](get-started-cloud-shell.md#authenticate-to-azure)を参照してください。

1. このサンプル テストを実行するには、ホーム ディレクトリに、SSH 秘密キー/公開キーのペアの名前 `id_rsa` と `id_rsa.pub` が必要です。 `USER` はホーム ディレクトリの名前に置き換えます。

```bash
export TEST_SSH_KEY_PATH="/home/USER/.ssh/id_rsa"
```

1. ディレクトリを `test` ディレクトリに変更します。

1. テストを実行します。

```go
go test -v ./ -timeout 10m
```

テストにより、次の出力のような結果が表示されます。

```output
--- PASS: TestEndToEndDeploymentScenario (390.99s)
PASS
ok      test    391.052s
```

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Terraform のテストの概要](best-practices-testing-overview.md)