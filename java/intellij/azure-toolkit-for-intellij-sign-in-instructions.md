---
title: Azure Toolkit for IntelliJ のサインイン手順
description: Azure Toolkit for IntelliJ を使用して Microsoft Azure にサインインする方法について説明します。
services: ''
documentationcenter: java
author: bmitchell287
manager: douge
editor: ''
ms.assetid: ''
ms.author: brendm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 7c2cec950c8a8bd51a7e1c6f9e5de390e1799c15
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2019
ms.locfileid: "68278863"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ のサインイン手順

[Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) では、[インストール](https://www.jetbrains.com/help/idea/managing-plugins.html)後に、Azure アカウントにサインインするための方法が 2 つ提供されます。

  - [[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-device-login)
  - [[サービス プリンシパル] によって Azure アカウントにサインインする](#sign-in-to-your-azure-account-by-service-principal)

[**サインアウト**](#sign-out-of-your-azure-account)方法も用意されています。

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>[Device Login]\(デバイスのログイン\) によって Azure アカウントにサインインする

[Device Login]\(デバイスのログイン\) によって Azure にサインインするには、以下を実行します。

1. IntelliJ IDEA でプロジェクトを開きます。

2. サイドバーの **Azure Explorer** を開き、上部のバーにある **[Azure サインイン]** アイコンをクリックします (または IDEA のメニューで **[ツール]、[Azure]、[Azure サインイン]** の順にクリックします)。

   ![IntelliJ Azure サインイン コマンド][I01]

3. **[Azure サインイン]** ウィンドウで、 **[Device Login]\(デバイスのログイン\)** を選択し、 **[サインイン]** をクリックします。

   ![[デバイスのログイン] が選択されている [Azure サインイン] ウィンドウ][I02]

4. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

   ![[Azure ログイン] ダイアログ ウィンドウ][I03]

5. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

   ![デバイスのログイン ブラウザー][I04]

6. **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>[サービス プリンシパル] によって Azure アカウントにサインインする

このセクションでは、サービス プリンシパル データを格納する資格情報ファイルを作成します。 このプロセスが完了すると、IntelliJ によって資格情報ファイルが使用され、ユーザーは自分のプロジェクトを開いたときに自動的に Azure にサインインします。

1. IntelliJ IDEA でプロジェクトを開きます。

1. サイドバーの **Azure Explorer** を開き、上部のバーにある **[Azure サインイン]** アイコンをクリックします (または IDEA のメニューで **[ツール]、[Azure]、[Azure サインイン]** の順にクリックします)。
   ![IntelliJ Azure サインイン コマンド][A01]

1. **[Azure サインイン]** ウィンドウで、 **[サービス プリンシパル]** を選択し、 **[新規]** をクリックします。

   ![[サービス プリンシパル] が選択されている [Azure サインイン] ウィンドウ][A02]

1. **[Azure Device Login]\(Azure デバイスのログイ\)** ダイアログで **[Copy&Open]\(コピーして開く\)** をクリックします。

   ![[Azure ログイン] ダイアログ ウィンドウ][A03]

1. ブラウザーで、該当のデバイス コード (前の手順で **[Copy&Open]\(コピーして開く\)** をクリックしたときにコピーされたもの) を貼り付け、 **[次へ]** をクリックします。

   ![デバイスのログイン ブラウザー][A04]

1. **[Create authentication files]\(認証ファイルの作成\)** ダイアログ ボックスが表示されたら、使用するサブスクリプションを選び、宛先ディレクトリを選択し、 **[開始]** をクリックします。

   ![[Create authentication files]\(認証ファイルの作成\) ウィンドウ][A05]

1. **[Service Principal Creatation Status]\(サービス プリンシパル作成状態\)** ダイアログ ボックスで、ファイルが正常に作成されたら **[OK]** をクリックします。

   ![[Service Principal Creatation Status]\(サービス プリンシパル作成ステータス\) ダイアログ ボックス][A06]

1. **[Azure サインイン]** ウィンドウで、 **[サインイン]** をクリックします。 

   ![[Azure ログイン] ダイアログ ボックス][A07]

1. **[サブスクリプションの選択]** ダイアログ ボックスで、使用するサブスクリプションを選択し、 **[OK]** をクリックします。

   ![[サブスクリプションの選択] ダイアログ ボックス][A08]

> サービス プリンシパルの認証ファイルを作成したら、手順 8 から開始し、自分の認証ファイルを選択してサインインできます。

## <a name="sign-out-of-your-azure-account"></a>Azureアカウントからサインアウトする

前の手順で自分のアカウントを構成した後は、IntelliJ IDEA を起動するたびに自動的にサインインが行われます。 ただし、自分の Azure アカウントからサインアウトしたい場合は、次の手順を実行してください。

* IntelliJ IDEA で Azure Explorer のサイド バーを開き、 **[Azure サインアウト]** アイコンをクリックして確認します (または、IDEA のメニューで **[ツール]、[Azure]、[Azure サインアウト]** の順にクリックします)。

   ![IntelliJ Azure サインアウト コマンド][L01]

## <a name="next-steps"></a>次の手順

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png
[A09]: media/azure-toolkit-for-intellij-sign-in-instructions/A09.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
