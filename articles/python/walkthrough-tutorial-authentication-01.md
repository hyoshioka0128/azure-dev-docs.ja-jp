---
title: チュートリアル:Azure サービスで Python アプリを認証する
description: Azure Python SDK azure-identity ライブラリを使用して、Azure Active Directory、Azure Key Vault、Azure Queue Storage で Python アプリを認証する方法についての詳細なチュートリアル。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 7f716f3276c0b93ec37ba0941f4029b2ee817fa0
ms.sourcegitcommit: 324da872a9dfd4c55b34739824fc6a6598f2ae12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379544"
---
# <a name="walkthrough-integrated-authentication-for-python-apps-with-azure-services"></a>チュートリアル:Azure サービスでの Python アプリの統合認証

Azure Active Directory (Azure AD) を Azure Key Vault と共に使用すると、アクセス キーが関係する Azure サービスおよびサードパーティのサービスでアプリケーションの認証を行うための包括的かつ便利な手段が提供されます。

このチュートリアルでは、背景を説明した後、サンプルの [github.com/Azure-Samples/python-integrated-authentication](https://github.com/Azure-Samples/python-integrated-authentication) のコンテキストでこれらの認証機能について説明します。

便宜上、このサンプルは既に Azure にデプロイされているので、動作を確認することができます。 このサンプルを自分の Azure サブスクリプションにデプロイする場合、リポジトリには Azure CLI デプロイ スクリプトも含まれています。

## <a name="part-1-background"></a>パート 1: バックグラウンド

多くの Azure サービスは、ロールベースのアクセス制御のみに依存して認可を行いますが、特定のサービスは、シークレットまたはキーを使用して、それぞれのリソースへのアクセスを制御しています。 このようなサービスとしては、Azure Storage、データベース、Cognitive Services、Key Vault、Event Hubs などがあります。

これらのサービス内のリソースを使用するクラウド アプリを作成する場合は、Azure portal、Azure CLI、または Azure PowerShell を使用して、特定のアクセス ポリシーに関連付けられているアプリのキーを作成して構成します。 これらのキーを使用すると、認可されていない他のコードが、それらのアプリ固有のリソースにアクセスするのを防ぐことができます。

この一般的な設計では、通常、クラウド アプリがこれらのキーを管理し、各サービスで個別に認証する必要があります。これは、面倒で間違いが生じやすいプロセスです。 アプリ コード内で直接キーを管理する場合は、ソース管理でそれらのキーが漏洩するリスクもあり、セキュリティで保護されていない開発者ワークステーションにキーが格納される可能性があります。

幸いなことに、Azure には、このプロセスを簡略化し、セキュリティを強化するための 2 つの特定のサービスが用意されています。

- Azure Key Vault は、アクセス キーに対してセキュリティで保護されたクラウドベースのストレージを提供します (この記事では説明していませんが、暗号化キーと証明書も含まれます)。 Key Vault を使用すると、アプリがこのようなキーにアクセスするのは実行時のみとなるため、ソース コードに直接表示されることはありません。

- Azure Active Directory (Azure AD) のマネージド ID では、アプリが Active Directory で認証を行う必要があるのは 1 回のみです。 その後、アプリは、Key Vault を含む他の Azure サービスで自動的に認証されます。 その結果、コードでそれらの Azure サービスのキーや他の資格情報を考慮する必要はありません。 さらに良い点として、最小限の構成要件で、同じコードをローカルとクラウドの両方で実行できます。

Azure AD と Key Vault を組み合わせて使用することで、アプリは個々の Azure サービスで自身を認証する必要がなくなり、サードパーティのサービスに必要なすべてのキーに簡単かつ安全にアクセスできます。

> [!IMPORTANT]
> この記事では、REST API のアクセス キーなど、Azure Key Vault に "シークレット" として格納されているものを参照するために、一般的な用語である "キー" を使用しています。 この使用法を、Key Vault の "*暗号*" キーの管理と混同しないようにしてください。それは、Key Vault の "*シークレット*" とは別の機能です。

## <a name="example-cloud-app-scenario"></a>クラウド アプリのシナリオ例

Azure の認証プロセスをより深く理解するには、次のシナリオを検討してください。

- メイン アプリは、JSON データを使用して HTTP 要求に応答するパブリック (認証されていない) API エンドポイントを公開します。 この記事で示されているエンドポイント例は、Azure App Service にデプロイされた単純な Flask アプリとして実装されています。

- API は、応答を生成するために、アクセス キーを必要とするサードパーティ API を呼び出します。 アプリは、実行時に Azure Key Vault からそのアクセス キーを取得します。

- API は、応答を返す前に、後で処理するためにメッセージを Azure Storage キューに書き込みます。 (これらのメッセージの具体的な処理は、メイン シナリオには関係ありません。)

![アプリケーション シナリオの図](media/walkthrough-tutorial-authentication/scenario-diagram.png)

> [!NOTE]
> パブリック API エンドポイントは、通常、独自のアクセス キーによって保護されていますが、この記事では、エンドポイントはオープンであり、認証されていないものと仮定しています。 この仮定により、アプリと、このエンドポイントの "*外部*" 呼び出し元との間で、認証のニーズに関して混乱が生じるのを防ぐことができます。 このシナリオでは、そのような外部呼び出し元については説明しません。

> [!div class="nextstepaction"]
> [パート 2 - 認証のニーズ >>>](walkthrough-tutorial-authentication-02.md)
