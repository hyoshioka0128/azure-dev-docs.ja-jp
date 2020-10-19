---
title: Azure Toolkit for IntelliJ のサインイン手順
description: Azure Toolkit for IntelliJ を使用して Microsoft Azure にサインインする方法について説明します。
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 27442efda23ba24abe6b40f20f7685f6ac7a524e
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010234"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ のサインイン手順

[Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) では、[インストール](https://www.jetbrains.com/help/idea/managing-plugins.html)後に、Azure アカウントにサインインするための方法が 2 つ提供されます。

  - [[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-device-login)
  - [[サービス プリンシパル] によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-service-principal)

[**サインアウト**](#sign-out-of-your-azure-account)方法も用意されています。

[!INCLUDE [basic-prerequisites](includes/basic-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする

[Device Login]\(デバイスのログイン\) によって Azure にサインインするには、以下を実行します。

1. IntelliJ IDEA でプロジェクトを開きます。

1. サイドバーの **Azure Explorer** を開き、上部のバーにある **[Azure サインイン]** アイコンをクリックします (または IntelliJ メニューで **[ツール] > [Azure] > [Azure サインイン]** の順にクリックします)。

   ![IntelliJ Azure サインイン コマンド][I01]

1. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、 **[サインイン]** をクリックします。

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

1. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

1. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

1. Azure アカウントを選択し、サインインするために必要な認証手順を完了します。

1. **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。


## <a name="sign-in-to-your-azure-account-by-service-principal"></a>[サービス プリンシパル] によって Azure アカウントにサインインする

このセクションでは、サービス プリンシパル データを格納する資格情報ファイルを作成します。 このプロセスが完了すると、ユーザーが自分のプロジェクトを開いたときに、IntelliJ によって資格情報ファイルが使用されて自動的に Azure にサインインします。

1. IntelliJ IDEA でプロジェクトを開きます。

1. サイドバーの **Azure Explorer** を開き、上部のバーにある **[Azure サインイン]** アイコンをクリックします (または IntelliJ メニューで **[ツール] > [Azure] > [Azure サインイン]** の順にクリックします)。

   ![IntelliJ Azure サインイン コマンド][I01]

1. **[Azure サインイン]** ウィンドウで、**[サービス プリンシパル]** を選択し、**[新規]** をクリックします。

   ![[サービス プリンシパル] が選択されている [Azure サインイン] ウィンドウ][A02]

1. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

1. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

1. Azure アカウントを選択し、サインインするために必要な認証手順を完了します。 認証後、ブラウザーを閉じて IntelliJ に戻ります。

1. **[Create authentication files]\(認証ファイルの作成\)** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選択し、 **[開始]** をクリックします。

1. **[Service Principal Creatation Status]\(サービス プリンシパル作成状態\)** ダイアログ ボックスで、ファイルが正常に作成されたら **[OK]** をクリックします。

1. **[Azure サインイン]** ウィンドウで、**[サインイン]** をクリックします。 

1. **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   > [!TIP]
   > サービス プリンシパルの認証ファイルを作成した後、手順 3 から開始し、自分の認証ファイルを選択してサインインできます。

## <a name="sign-out-of-your-azure-account"></a>Azureアカウントからサインアウトする

前の手順で自分のアカウントを構成した後は、IntelliJ IDEA を起動するたびに自動的にサインインが行われます。 

ただし、Azure アカウントからサインアウトする場合は、Azure Explorer のサイドバーに移動し、 **[Azure サインアウト]** アイコンをクリックするか、IntelliJ メニューで **[ツール] > [Azure] > [Azure サインアウト]** の順にクリックします。


## <a name="next-steps"></a>次のステップ

[!INCLUDE [additional-resources](includes/additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/sign-in-instructions/I01.png
[I02]: media/sign-in-instructions/I02.png

[A02]: media/sign-in-instructions/A02.png

