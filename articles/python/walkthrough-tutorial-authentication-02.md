---
title: 'チュートリアル、パート 2: Azure サービスで Python アプリを認証する'
description: このサンプル シナリオにおけるさまざまな認証のニーズと課題、および Azure 統合認証を使用してこれらの課題にどのように対応するかについて説明します。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 2e3755f7049fd091c05cd2aca5ddf8276cebff95
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764426"
---
# <a name="part-2-authentication-needs-in-the-scenario"></a>パート 2: シナリオにおける認証のニーズ

[前のパート:概要と背景](walkthrough-tutorial-authentication-01.md)

このサンプル シナリオ内で、メイン アプリには次の認証要件があります。

- Azure Key Vault で認証して、格納されているサードパーティの API キーにアクセスする。
- API キーを使用してサードパーティ API で認証する。
- ストレージ アカウントの必要な資格情報を使用して、Azure Queue Storage で認証する。

このように 3 つの異なる要件があるため、アプリケーションは 3 つの資格情報セットを管理する必要があります。2 つは Azure リソース (Key Vault と Queue Storage) 用で、1 つは外部リソース (サードパーティ API) 用です。

既に説明したように、Key Vault 自体に必要な資格情報を除き、すべての資格情報は Key Vault で安全に管理できます。 Key Vault で一度認証されると、アプリケーションは実行時に他のキーを取得して、Queue Storage のようなサービスで認証できるようになります。

ただし、この方法でも、アプリは Key Vault 用の資格情報を別個に管理する必要があります。 では、その資格情報を安全に管理し、ローカル開発とクラウドでの運用環境デプロイの両方でそれを機能させるにはどうすればよいでしょうか。

部分的な解決策としては、キーをサーバー側の環境変数に格納することです (つまり、Azure App Service と Azure Functions のアプリケーション設定を使用します)。これにより、少なくともキーはソース管理の外で保持されます。 ただし、開発者ワークステーションでコードを実行するには、その環境変数をローカルでレプリケートする必要があります。これにより、資格情報が漏洩したり、ソース管理に誤って含まれたりするリスクがあります。 この問題は、コードの開発バージョンで特別な手順を実装することで、ある程度回避できますが、それにより開発プロセスの複雑さが増します。

幸いなことに、Azure Active Directory (AD) を使用した統合認証を用いると、アプリはどの Azure の資格情報もまったく処理しないで済みます。

## <a name="integrated-authentication-with-managed-identity"></a>マネージド ID を使用した統合認証

Storage や Key Vault などの多くの Azure サービスは、Azure Active Directory (Azure AD) と統合されています。これにより、[マネージド ID](/azure/active-directory/managed-identities-azure-resources/overview) を使用してアプリケーションを Azure AD で認証すると、他の接続されたリソースで自動的に認証されます。 ID の認可の処理は、[ロールベースのアクセス制御 (RBAC)](/azure/role-based-access-control/role-assignments-steps) を使用して行われます。また、場合によっては、他のアクセス ポリシーが使用されることもあります。

この統合は、Azure に関連した資格情報をアプリ コードで処理する必要がなく、これらの資格情報が決して開発者用ワークステーションやソース管理に表示されないことを意味します。 さらに、サードパーティ API とサービスのキーの処理はすべて実行時に行われるため、これらのキーを安全に保つことができます。

マネージド ID は、Azure にデプロイされているアプリで特に機能します。 ローカル開発では、ローカルで実行する際のアプリ ID として機能する別のサービス プリンシパルを作成します。 このサービス プリンシパルを Azure ライブラリで使用できるようにするには、[ローカル開発環境の構成 - 認証の構成](configure-local-development-environment.md#configure-authentication)に関する記事で説明されているように、環境変数を使用します。 また、クラウドで使用されるマネージド ID に加え、このサービス プリンシパルにはロールのアクセス許可を割り当てます。

ローカル サービス プリンシパルに対してこれらの手順を一度実行すると、同じコードがローカルとクラウドの両方で機能し、Azure リソースでアプリを認証します。 これらの詳細については、[アプリの認証および認可の方法](azure-sdk-authenticate.md)に関する記事に記載されていますが、簡単に説明すると以下のようになります。

1. コードで、Azure で実行している場合はマネージド ID を、ローカルで実行している場合は別個のサービス プリンシパルを自動的に使用する `DefaultAzureCredential` オブジェクトを作成します。

1. この資格情報は、アクセスしたいリソース (Key Vault、Queue Storage など) に適したクライアント オブジェクトを作成するときに使用します。

1. 認証は、そのクライアント オブジェクトを介して操作メソッドを呼び出すときに実行され、リソースへの REST API 呼び出しが生成されます。

1. アプリ ID が有効な場合、Azure は、その ID も特定の操作に対して認可されているかどうかも確認します。

このチュートリアルの残りの部分では、サンプル シナリオとそれに付随するサンプル コードのコンテキストで、プロセスのすべての詳細を示します。

このサンプルのプロビジョニング スクリプトでは、すべてのリソースは、`auth-scenario-rg` という名前のリソース グループの下に作成されます。 このグループは、Azure CLI の [`az group create`](/cli/azure/group#az-group-create) コマンドを使用して作成されます。

> [!div class="nextstepaction"]
> [パート 3 - サードパーティ API の実装例 >>>](walkthrough-tutorial-authentication-03.md)
