---
title: Azure Toolkit for Eclipse のサインイン手順
description: Azure Toolkit for Eclipse を使用して Microsoft Azure にサインインする方法について説明します。
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: d9e3b0de0c6ea20996c54b6ab5cb03083ed25a65
ms.sourcegitcommit: d5dabc6dde727ed167a9dc8a4eaaf21025b3efa8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2020
ms.locfileid: "91947547"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Azure Toolkit for Eclipse のサインイン手順

Azure Toolkit for Eclipse には、Azure アカウントにサインインするための 2 つの方法が用意されています。

  - [[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-device-login)
  - [[サービス プリンシパル] によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-service-principal)

[**サインアウト**](#sign-out-of-your-azure-account)方法も用意されています。

[!INCLUDE [prerequisites](includes/prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする

このセクションでは、デバイス ログインによる Azure サインイン プロセスについて説明します。

1. Eclipse でプロジェクトを開きます。

1. **[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。

      :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="Eclipse IDE で Azure にサインインします。":::

1. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、 **[サインイン]** をクリックします。

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

1. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

> [!NOTE]
>
> ブラウザーが開かない場合は、Internet Explorer、Firefox、Chrome などの外部ブラウザーを使用するように Eclipse を構成します。
>
> 1. [Preferences]\(設定\) - > [General]\(全般\) - > [Web Browser ]\(Web ブラウザー\) - > [Use external web browser in Eclipse]\(Eclipse で外部 Web ブラウザーを使用する\) の順に開きます。
>
> 2. 使用するブラウザーを選択します
>

1. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

1. Azure アカウントを選択し、サインインするために必要な認証手順を完了します。

1. サインインしたら、ブラウザーを閉じて Eclipse IDE に戻ります。 **[Select Subscriptions]\(サブスクリプションの選択\)** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>[サービス プリンシパル] によって Azure アカウントにサインインする

このセクションでは、サービス プリンシパル データを格納する資格情報ファイルを作成します。 このプロセスが完了すると、プロジェクトを開いたときに、Eclipse によって資格情報ファイルが使用されて、自動的に Azure にサインインします。

1. Eclipse でプロジェクトを開きます。

2. **[ツール]** 、 **[Azure]** 、 **[サインイン]** の順にクリックします。

      :::image type="content" source="media/sign-in-instructions/eclipse-azure-signin.png" alt-text="Eclipse IDE で Azure にサインインします。":::

3. **[Azure サインイン]** ウィンドウで、 **[サービス プリンシパル]** を選択します。 サービス プリンシパル認証ファイルがまだない場合は、 **[新規]** をクリックして作成します。 そうでない場合は、 **[参照]** をクリックしてそれを開き、ステップ 8 に進むことができます。

   ![[サービス プリンシパル] が選択されている [Azure サインイン] ウィンドウ][A02]

4. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

> [!NOTE]
>
> ブラウザーが開かない場合は、IE や Chrome などの外部ブラウザーを使用するように Eclipse を構成します。
>
> 1. [Preferences]\(設定\) - > [General]\(全般\) - > [Web Browser ]\(Web ブラウザー\) - > [Use external web browser in Eclipse]\(Eclipse で外部 Web ブラウザーを使用する\) の順に開きます。
>
> 2. 使用するブラウザーを選択します
>

5. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

6. **[Create authentication files]\(認証ファイルの作成\)** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選択し、 **[開始]** をクリックします。

7. **[Service Principal Creatation Status]\(サービス プリンシパル作成状態\)** ダイアログ ボックスで、ファイルが正常に作成されたら **[OK]** をクリックします。

8. 作成されたファイルのアドレスは **[Azure サインイン]** ウィンドウに自動的に入力されるため、ここで **[サインイン]** をクリックします。

   ![Azure のログイン ダイアログ ボックス][A06]

9. 最後に、 **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。


## <a name="sign-out-of-your-azure-account"></a>Azureアカウントからサインアウトする

前の手順によってアカウントを構成すると、Eclipse を起動するたびに自動的にサインインします。 ただし、Azure アカウントからサインアウトするには、次の手順を使用します。

1. Eclipse で、 **[ツール]** 、 **[Azure]** 、 **[サインアウト]** の順にクリックします。

2. **[Azure Sign Out (Azure サインアウト)]** ダイアログ ボックスが表示されたら、 **[はい]** をクリックします。

## <a name="next-steps"></a>次のステップ

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->


<!-- IMG List -->

[I02]: media/sign-in-instructions/I02.png

[A02]: media/sign-in-instructions/A02.png
[A06]: media/sign-in-instructions/A06.png
