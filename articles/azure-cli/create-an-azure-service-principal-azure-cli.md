---
title: Azure CLI で Azure サービス プリンシパルを使用する
description: Azure CLI でサービス プリンシパルを作成および使用する方法について説明します。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 02/15/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c18adbee84fd3e5c73367b07bbd0b03ac61008cd
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030734"
---
# <a name="create-an-azure-service-principal-with-the-azure-cli"></a>Azure CLI で Azure サービス プリンシパルを作成する

Azure サービスを使用する自動化ツールのアクセス許可は、常に制限されている必要があります。 完全な特権を持つユーザーとしてアプリケーションをサインインさせる代わりに、Azure にはサービス プリンシパルが用意されています。

Azure サービス プリンシパルは、Azure リソースにアクセスするアプリケーション、ホステッド サービス、および自動化ツールで使用するために作成される ID です。 このアクセスはサービス プリンシパルに割り当てられているロールによって制限されるため、どのリソースに、どのレベルでアクセスできるかを制御することができます。 セキュリティ上の理由から、自動化ツールにはユーザー ID でのログインを許可するのではなく、常にサービス プリンシパルを使用することを推奨します。

この記事では、Azure CLI を使用して、サービス プリンシパルの作成、情報取得、およびリセットを行うための手順について説明します。

## <a name="create-a-service-principal"></a>サービス プリンシパルの作成

サービス プリンシパルは、[az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) コマンドを使用して作成します。 サービス プリンシパルを作成する際に、サービス プリンシパルが使用するサインイン認証の種類を選択します。

> [!NOTE]
>
> ご利用のアカウントにサービス プリンシパルを作成するためのアクセス許可がない場合は、`az ad sp create-for-rbac` から "この操作を完了するのに十分な特権がありません" というエラー メッセージが返されます。 サービス プリンシパルを作成するには、Azure Active Directory 管理者にお問い合わせください。

サービス プリンシパルで使用できる認証には、パスワードベースの認証と証明書ベースの認証の 2 種類があります。

### <a name="password-based-authentication"></a>パスワードベースの認証

パスワード ベースの認証では、認証パラメーターは使用せず、自動的に作成されるランダム パスワードと共に使用されます。

  ```azurecli-interactive
  az ad sp create-for-rbac --name ServicePrincipalName
  ```

> [!IMPORTANT]
> Azure CLI 2.0.68 の時点で、ユーザーが定義したパスワードを使ってサービス プリンシパルを作成する `--password` パラメーターは、脆弱なパスワードの予期しない使用を避けるために__サポートされなくなりました__。

パスワード認証によるサービス プリンシパルの出力には、`password` キーが含まれます。 この値は__必ず__コピーしてください。取得することはできません。 パスワードを忘れた場合は、[サービス プリンシパルの資格情報をリセット](#reset-credentials)します。

`appId` キーと `tenant` キーは `az ad sp create-for-rbac` の出力に表示され、サービス プリンシパルの認証で使用されます。
これらの値を記録してください。ただし、[az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) を使用していつでも取得できます。

### <a name="certificate-based-authentication"></a>証明書ベースの認証

証明書ベースの認証の場合は、`--cert` 引数を使用します。 この引数を使用するには、既存の証明書を保持しておく必要があります。 このサービス プリンシパルを使用する任意のツールから、証明書の秘密キーにアクセスできることを確認します。 証明書は、PEM、CER、または DER などの ASCII 形式でなければなりません。 文字列として証明書を渡すか、`@path` 形式を使用してファイルから証明書を読み込みます。

> [!NOTE]
> PEM ファイルを使用する場合は、ファイル内で **PRIVATE KEY** (秘密キー) に **CERTIFICATE** (証明書) を追加する必要があります。

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert "-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----"
```

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert @/path/to/cert.pem
```

`--keyvault` 引数を追加すると、Azure Key Vault 内の証明書を使用できます。 ここでは、`--cert` 値は証明書の名前を表します。

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --cert CertName --keyvault VaultName
```

認証用の "_自己署名_" 証明書を作成するには、`--create-cert` 引数を使用します。

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert
```

コンソール出力:

```
Creating a role assignment under the scope of "/subscriptions/myId"
Please copy C:\myPath\myNewFile.pem to a safe place.
When you run 'az login', provide the file path in the --password argument
{
  "appId": "myAppId",
  "displayName": "myDisplayName",
  "fileWithCertAndPrivateKey": "C:\\myPath\\myNewFile.pem",
  "name": "http://myName",
  "password": null,
  "tenant": "myTenantId"
}
```

新しい PEM ファイルの内容:
```
-----BEGIN PRIVATE KEY-----
myPrivateKeyValue
-----END PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
myCertificateValue
-----END CERTIFICATE-----
```

> [!NOTE]
> `az ad sp create-for-rbac --create-cert` コマンドはサービス プリンシパルと PEM ファイルを作成します。 PEM ファイルには、正しく書式設定された **PRIVATE KEY** (秘密キー) と **CERTIFICATE** (証明書) が含まれます。

`--keyvault` 引数を追加すると、証明書を Azure Key Vault に格納できます。 `--keyvault` を使用する場合は、`--cert` 引数が__必要__です。

```azurecli-interactive
az ad sp create-for-rbac --name ServicePrincipalName --create-cert --cert CertName --keyvault VaultName
```

証明書を Key Vault に格納しない場合は、出力に `fileWithCertAndPrivateKey` キーが含まれています。 このキーの値を見ると、生成された証明書の格納場所がわかります。
証明書は__必ず__安全な場所にコピーしてください。そうしないと、このサービス プリンシパルでサインインすることができません。

Key Vault に格納されている証明書の場合、[az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show) を使用して証明書の秘密キーを取得します。 Key Vault では、証明書のシークレットの名前は、証明書の名前と同じです。 証明書の秘密キーにアクセスできない場合は、[サービス プリンシパルの資格情報をリセット](#reset-credentials)します。

`appId` キーと `tenant` キーは `az ad sp create-for-rbac` の出力に表示され、サービス プリンシパルの認証で使用されます。
これらの値を記録してください。ただし、[az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) を使用していつでも取得できます。

## <a name="get-an-existing-service-principal"></a>既存のサービス プリンシパルの取得

[az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) を使用して、テナント内のサービス プリンシパルの一覧を取得できます。 既定では、このコマンドによって、テナントの最初の 100 個のサービス プリンシパルが返されます。 テナントのサービス プリンシパルをすべて取得するには、`--all` 引数を使用します。 この一覧の取得には時間がかかる場合があるため、次の引数のいずれかを使用して一覧をフィルター処理することをお勧めします。

* `--display-name` は、指定した名前に一致する "_プレフィックス_" を持つサービス プリンシパルを要求します。 サービス プリンシパルの表示名は、作成時に `--name` パラメーターで設定した値です。 サービス プリンシパルの作成時に `--name` を設定していない場合、名前のプレフィックスは `azure-cli-` になります。
* `--spn` は、正確に一致するサービス プリンシパル名でフィルター処理します。 サービス プリンシパル名は常に `https://` で始まります。
  `--name` に使用した値が URI ではない場合、この値は `https://` とこれに続く表示名となります。
* `--show-mine` は、サインインしているユーザーによって作成されたサービス プリンシパルのみを要求します。
* `--filter` は OData フィルターを使用して、"_サーバー側_" のフィルター処理を実行します。 この方法は、CLI の `--query` 引数を使用したクライアント側のフィルター処理よりも推奨されます。 OData フィルターの詳細については、[フィルターのための OData 式の構文](/rest/api/searchservice/odata-expression-syntax-for-azure-search)に関するページをご覧ください。

サービス プリンシパル オブジェクトについて返される情報は、詳細情報です。 サインインに必要な情報のみを取得するには、クエリ文字列 `[].{id:appId, tenant:appOwnerTenantId}` を使用します。 たとえば、現在ログインしているユーザーによって作成されたすべてのサービス プリンシパルのサインイン情報を取得するには、次のコマンドを使用します。

```azurecli-interactive
az ad sp list --show-mine --query "[].{id:appId, tenant:appOwnerTenantId}"
```

> [!IMPORTANT]
>
> `az ad sp list` または [az ad sp show](/cli/azure/ad/sp#az-ad-sp-show) を使用するとユーザーとテナントは取得されますが、認証シークレット "_も_" 認証方法も取得されません。
> Key Vault 内の証明書のシークレットは [az keyvault secret show](/cli/azure/keyvault/secret#az-keyvault-secret-show) を使用して取得できますが、既定では、それ以外のシークレットは保存されません。
> 認証方法やシークレットを忘れた場合は、[サービス プリンシパルの資格情報をリセット](#reset-credentials)します。

## <a name="manage-service-principal-roles"></a>サービス プリンシパル ロールを管理する

Azure CLI には、ロールの割り当てを管理するために、次のコマンドが用意されています。

* [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list)
* [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)
* [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete)

サービス プリンシパルの既定のロールは**共同作成者**です。 このロールには、Azure アカウントの読み取りと書き込みを行うための完全なアクセス許可が付与されます。 **閲覧者**ロールは制限がより厳しく、読み取り専用アクセスが提供されます。  ロールベースのアクセス制御 (RBAC) とロールの詳細については、[RBAC の組み込みのロール](/azure/active-directory/role-based-access-built-in-roles)に関するページをご覧ください。

この例では、**閲覧者**ロールを追加し、**共同作成者**ロールを削除します。

```azurecli-interactive
az role assignment create --assignee APP_ID --role Reader
az role assignment delete --assignee APP_ID --role Contributor
```

> [!NOTE]
> ロールを割り当てるためのアクセス許可がアカウントにない場合は、アカウントに "'Microsoft.Authorization/roleAssignments/write' のアクションを実行するためのアクセス権限がありません" というエラー メッセージが表示されます。ロールを管理するには、Azure Active Directory 管理者にお問い合わせください。

ロールを追加しても、以前に割り当てられたアクセス許可は制限され "_ません_"。 サービス プリンシパルのアクセス許可を制限するときは、__共同作成者__ロールを削除する必要があります。

変更を確認するには、割り当てられているロールの一覧を表示します。

```azurecli-interactive
az role assignment list --assignee APP_ID
```

## <a name="sign-in-using-a-service-principal"></a>サービス プリンシパルを使用したサインイン

サインインして、新しいサービス プリンシパルの資格情報とアクセス許可をテストします。 サービス プリンシパルでサインインするには、`appId`、`tenant`、および資格情報が必要です。

パスワードを使用してサービス プリンシパルでサインインするには、次のコマンドを使用します。

```azurecli-interactive
az login --service-principal --username APP_ID --password PASSWORD --tenant TENANT_ID
```

証明書を使用してサインインする場合、その証明書は、ASCII 形式の PEM または DER ファイルとしてローカルで使用できる必要があります。 PEM ファイルを使用する場合は、ファイル内に **PRIVATE KEY** (秘密キー) と **CERTIFICATE** (証明書) を共に追加する必要があります。

```azurecli-interactive
az login --service-principal --username APP_ID --tenant TENANT_ID --password /path/to/cert
```

サービス プリンシパルを使用したサインインの詳細については、「[Azure CLI を使用してサインインする](authenticate-azure-cli.md)」を参照してください。

## <a name="reset-credentials"></a>資格情報をリセットする

サービス プリンシパルの資格情報を忘れた場合は、[az ad sp credential reset](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) を使用します。 リセット コマンドでは、`az ad sp create-for-rbac` と同じ引数を使用します。

```azurecli-interactive
az ad sp credential reset --name APP_ID
```
