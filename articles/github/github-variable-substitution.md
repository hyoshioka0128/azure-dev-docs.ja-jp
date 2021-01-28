---
title: Azure 向けの GitHub Actions の variable substitution を使用する
description: パラメーター化されたファイル内の変数を置換するための GitHub Actions
author: juliakm
ms.author: jukullam
ms.topic: conceptual
ms.service: azure
ms.date: 01/25/2021
ms.custom: github-actions-azure
ms.openlocfilehash: e2a82fbcbe48269339dc672d46aca4cc3601ae12
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759501"
---
# <a name="use-variable-substitution-with-github-actions"></a>GitHub Actions の variable substitution を使用する

[variable substitution アクション](https://github.com/marketplace/actions/variable-substitution)を使用して、XML、JSON、および YAML ベースの構成およびパラメーターのファイル内の値を置換する方法について説明します。

variable substitution を使用すると、ワークフローの実行中にリポジトリ内のファイルに [GitHub シークレット](https://docs.github.com/en/actions/reference/encrypted-secrets)を含む値を挿入できます。 たとえば、ワークフローの実行中に、API ログインとパスワードを JSON ファイルに挿入できます。

variable substitution は、オブジェクト階層で事前に定義されているキーに対してのみ機能します。 variable substitution を使用して新しいキーを作成することはできません。 また、ワークフロー内の[環境変数](https://docs.github.com/en/actions/reference/environment-variables)として定義されている変数や、既に使用可能なシステム変数のみを置換に使用できます。

## <a name="prerequisites"></a>前提条件

- GitHub アカウント。 お持ちでない場合は、[無料](https://github.com/join)でサインアップしてください。  

## <a name="use-the-variable-substitution-action"></a>variable substitution アクションを使用する

この例では、[variable substitution アクション](https://github.com/marketplace/actions/variable-substitution)を使用して `employee.json` の値を置換する手順を説明します。

1. リポジトリのルート レベルで `employee.json` を作成します。

    ```json
    {
        "first-name": "Toni",
        "last-name": "Cranz",
        "username": "",
        "password": "",
        "url": ""
    }
    ```

2. GitHub リポジトリを開き、 **[Settings]\(設定\)** に移動します。

    :::image type="content" source="media/github-repo-settings.png" alt-text="ナビゲーションで [Settings]\(設定\) を選択する":::

3. **[Settings]\(設定\)** 、 **[New Secret]\([新しいシークレット\)]** の順に選択します。

    :::image type="content" source="media/select-secrets.png" alt-text="シークレットの追加を選択する":::

4. 値 `5v{W<$2B<GR2=t4#` (または選択したパスワード) を指定して新しいシークレット `PASSWORD` を追加します。 このシークレットを保存します。 

5. **[Actions]\(アクション\)** に移動し、 **[set up a workflow yourself]\(ワークフローを自分でセットアップする\)** を選択します。

6. ワークフロー ファイルを追加します。 json ファイル内の username 値は `tcranz` に置換されます。 password はお使いの GitHub シークレットに置換されます。 url フィールドには、GitHub 変数 `github.repository` を含む URL が入力されます。

    ```yaml
    on: [push]
    name: variable substitution in json

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - uses: microsoft/variable-substitution@v1 
        with:
            files: 'employee.json'
        env:
            username: tcranz
            password: ${{ secrets.PASSWORD }}
            url: https://github.com/${{github.repository}}

    ```

7. **[Actions]\(アクション\)** に移動して、ワークフローの実行を確認します。 variable substitution アクションを開きます。 各変数が置換されたことがわかります。

    ```text
    SubstitutingValueonKeyWithString username tcranz
    SubstitutingValueonKeyWithString password ***
    SubstitutingValueonKeyWithString url https://github.com/account/variable-sub
    Successfully updated file: employee.json
    ```

## <a name="clean-up-resources"></a>リソースをクリーンアップする

GitHub リポジトリは、不要になったら削除してください。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [GitHub Actions を使用して Azure Web Apps にデプロイする](/azure/app-service/deploy-github-actions)
