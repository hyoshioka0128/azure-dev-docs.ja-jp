---
title: 'チュートリアル: ロード バランサーとして Azure Application Gateway を使用して、Azure に WebLogic Server クラスターを移行する'
description: このチュートリアルでは、ロード バランサーとしての Azure Application Gateway と共に WebLogic Server を Azure にデプロイする手順について説明します
author: edburns
ms.author: edburns
ms.topic: tutorial
ms.date: 10/15/2020
ms.openlocfilehash: 7a4f130e82338dea7c4fd344cfbedb2ed7f88c67
ms.sourcegitcommit: 050c898df76a1af5feffe99e392a073b8ac9c19c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2020
ms.locfileid: "92137091"
---
# <a name="tutorial-migrate-a-weblogic-server-cluster-to-azure-with-azure-application-gateway-as-a-load-balancer"></a>チュートリアル:ロード バランサーとして Azure Application Gateway を使用して、Azure に WebLogic Server クラスターを移行する

このチュートリアルでは、Azure Application Gateway と共に WebLogic Server (WLS) をデプロイするプロセスについて説明します。  Key Vault を作成して、SSL 証明書を Key Vault に格納し、SSL 終端にその証明書を使用する具体的な手順について説明します。  これらの要素はすべて個別に詳しく文書化されていますが、このチュートリアルでは、これらのすべての要素を連携させて、Azure 上の WLS 向けのシンプルだが強力な負荷分散ソリューションを作成する具体的な方法について説明します。

<!-- Diagram source at https://github.com/wls-eng/arm-oraclelinux-wls/blob/master/src/main/resources/weblogic-app-gateway-key-vault.vsdx -->

:::image type="content" border="false" source="media/migrate-weblogic-with-app-gateway/weblogic-app-gateway-key-vault.png" alt-text="WLS、App Gateway、および Key Vault の間の関係を示す図。":::

負荷分散は、Oracle WebLogic Server クラスターの Azure への移行において不可欠な部分です。  最も簡単な解決策は、[Azure Application Gateway](/azure/application-gateway/overview) の組み込みサポートを使用することです。  App Gateway は、Azure での WebLogic クラスターのサポートの一部として含まれています。  Azure での WebLogic クラスターのサポートの概要については、「[Azure の Oracle WebLogic Server とは](/azure/virtual-machines/workloads/oracle/oracle-weblogic)」を参照してください。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
> * Azure Key Vault を作成する
> * SSL 証明書を作成する
> * SSL 証明書を Key Vault に格納する
> * Azure Application Gateway と共に WebLogic Server を Azure にデプロイする
> * WLS と App Gateway が正常にデプロイされたことを確認する

## <a name="prerequisites"></a>前提条件

* UNIX のようなコマンド ライン環境を実行しているコンピューター上の [OpenSSL](https://www.openssl.org/)。

   証明書の管理に使用できるツールは他にもありますが、このチュートリアルでは OpenSSL を使用します。 OpenSSL は、Ubuntu など、多くの GNU または Linux ディストリビューションにバンドルされています。
* 有効な Azure サブスクリプション
  * Azure サブスクリプションをお持ちでない場合は、[無料アカウントを作成](https://azure.microsoft.com/free/)してください。
* 「[Oracle WebLogic Server Azure アプリケーション](/azure/virtual-machines/workloads/oracle/oracle-weblogic)」に記載されている、いずれかの WLS Azure アプリケーションをデプロイできること。

## <a name="migration-context"></a>移行のコンテキスト

オンプレミスの WLS インストールと Azure Application Gateway の移行について考慮すべきことをいくつか次に示します。  このチュートリアルの手順は、Azure 上の WebLogic Server クラスターの前にロードバランサーを配置する最も簡単な方法ですが、他にも多くの方法があります。  次の一覧に、その他の考慮事項をいくつか示します。

* 既存の負荷分散ソリューションがある場合は、Azure Application Gateway がその機能を満たしているか、上回ることを確認します。  その他の Azure 負荷分散ソリューションと比較した Azure Application Gateway の機能の概要については、「[Azure の負荷分散オプションの概要](/azure/architecture/guide/technology-choices/load-balancing-overview)」を参照してください。
* 既存の負荷分散ソリューションによって一般的な悪用と脆弱性に対するセキュリティ保護が実現されている場合は、Application Gateway で対応できます。 Application Gateway の組み込み Web アプリケーション ファイアウォール (WAF) は、[OWASP (Open Web Application Security Project) のコア ルールセット](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project)を実装しています。  Application Gateway での WAF サポートの詳細については、「[Application Gateway の機能](/azure/application-gateway/features#web-application-firewall)」を参照してください。
* 既存の負荷分散ソリューションにエンド ツー エンドの SSL 暗号化が必要な場合は、このガイドの手順に従った後、追加の構成を行う必要があります。  「[Application Gateway での TLS 終了とエンド ツー エンド TLS の概要](/azure/application-gateway/ssl-overview#end-to-end-tls-encryption)」と、[Oracle Fusion Middleware での SSL の構成](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.3/asadm/configuring-ssl1.html#GUID-623906C0-B1FD-423F-AE51-061B5800E927)に関する Oracle のドキュメントを参照してください。
* クラウド向けに最適化する場合は、Azure App Gateway と WLS を使用してゼロから始める方法について、このガイドで説明します。
* WebLogic Server の Azure Virtual Machines への移行の包括的な調査については、「[WebLogic Server のアプリケーションを Azure Virtual Machines に移行する](migrate-weblogic-to-virtual-machines.md)」を参照してください。

## <a name="create-an-azure-key-vault"></a>Azure Key Vault を作成する

このセクションでは、Azure portal を使用して Azure Key Vault を作成する方法について説明します。

1. Azure portal メニューまたは **[ホーム]** ページで、 **[リソースの作成]** を選択します。
1. 検索ボックスに「**Key Vault**」と入力します。
1. 結果の一覧の **[Key Vault]** を選択します。
1. Key Vault のセクションで、 **[作成]** を選択します。
1. **[キー コンテナーの作成]** セクションで、次の情報を入力します。
    * **サブスクリプション**:サブスクリプションを選択します。
    * **[リソース グループ]** で、 **[新規作成]** を選択し、リソース グループ名を入力します。  Key Vault 名を書き留めておきます。  "*これは、後で WLS をデプロイするときに必要になります。* "
    * **Key Vault 名**:一意の名前が必要です。  Key Vault 名を書き留めておきます。  "*これは、後で WLS をデプロイするときに必要になります。* "
    > [!NOTE]
    > **[リソース グループ]** と **[Key Vault 名]** の両方に同じ名前を使用できます。
    * **[場所]** プルダウン メニューで場所を選択します。
    * 他のオプションは既定値のままにしておきます。
1. **[Next:アクセス ポリシー]** を選択します。
1. **[アクセスの有効化]** で **[Azure Resource Manager (テンプレートの展開用)]** を選択します。
1. **[確認および作成]** を選択します。
1. **［作成］** を選択します

Key Vault の作成は非常に負荷の低い操作であり、通常は 2 分未満で完了します。  デプロイが完了したら、 **[リソースに移動]** を選択し、次のセクションに進みます。

## <a name="create-an-ssl-certificate"></a>SSL 証明書を作成する

このセクションでは、Azure 上に WebLogic と共にデプロイされた Application Gateway での使用に適した形式で自己署名 SSL 証明書を作成する方法について説明します。  この証明書には、空でないパスワードが必要です。  空でないパスワードがある *.pfx* 形式の有効な SSL 証明書が既にある場合は、このセクションをスキップし、次のセクションに進むことができます。  空でないパスワードがある既存の有効な SSL 証明書が *.pfx* 形式でない場合は、次のセクションに進む前に、まずそれを *.pfx* ファイルに変換します。  それ以外の場合は、コマンド シェルを開いて、以下のコマンドを入力します。

> [!NOTE]
> このセクションでは、証明書をシークレットとして Key Vault に格納する前に Base 64 でエンコードする方法について説明します。  これは、WebLogic Server と Application Gateway を作成する、基になる Azure デプロイで必要となります。

次の手順に従って、証明書を作成して Base 64 でエンコードします。

1. `RSA PRIVATE KEY` を作成します

   ```bash
   openssl genrsa 2048 > private.pem
   ```
1. 対応する公開キーを作成します。

   ```bash
   openssl req -x509 -new -key private.pem -out public.pem
   ```

   OpenSSL ツールからプロンプトが表示されたら、いくつかの質問に答える必要があります。  これらの値は証明書に含まれます。  このチュートリアルでは自己署名証明書を使用するため、これらの値は無関係です。  次のリテラル値で構いません。
     1. **[Country Name]\(国名\)** には、2 文字のコードを入力します。
     1. **[State or Province Name]\(州または都道府県\)** には、「WA」と入力します。
     1. **[Organization Name]\(組織名\)** には、「Contoso」と入力します。  [Organizational Unit Name]\(組織単位名\) には、「billing」と入力します。
     1. **[Common Name]\(共通名\)** には、「Contoso」と入力します。
     1. **[Email Address]\(メール アドレス\)** には、「billing@contoso.com」と入力します。

1. *.pfx* ファイルとして証明書をエクスポートします

   ```bash
   openssl pkcs12 -export -in public.pem -inkey private.pem -out mycert.pfx
   ```

   パスワードを 2 回入力します。  パスワードを書き留めておきます。  "*これは、後で WLS をデプロイするときに必要になります。* "

1. *mycert.pfx* ファイルを Base 64 でエンコードします

   ```bash
   base64 mycert.pfx > mycert.txt
   ```

これで、Key Vault を作成し、空でないパスワードのある有効な SSL 証明書を用意できたので、Key Vault に証明書を格納できます。

## <a name="store-the-ssl-certificate-in-the-key-vault"></a>SSL 証明書を Key Vault に格納する

このセクションでは、前のセクションで作成した Key Vault に証明書とそのパスワードを格納する方法について説明します。

証明書を格納するには、次の手順に従います。

1. Azure portal から、ページの上部にある検索バーにカーソルを置き、このチュートリアルで先ほど作成した Key Vault の名前を入力します。
1. 自分の Key Vault が **[リソース]** 見出しの下に表示されます。  それを選択します。
1. **[設定]** セクションで **[シークレット]** を選択します。
1. **[Generate/Import]\(生成/インポート\)** を選択します。
1. **[アップロード オプション]** は、既定値のままにします。
1. **[名前]** に、「`myCertSecretData`」または任意の名前を入力します。
1. **[値]** に、*mycert.txt* ファイルの内容を入力します。  値の長さと改行の有無は、このテキスト フィールドでは問題になりません。
1. 残りの値は既定値のままにし、 **[作成]** を選択します。

証明書のパスワードを格納するには、次の手順に従います。

1. **[シークレット]** ページに戻ります。  **[Generate/Import]\(生成/インポート\)** を選択します。
1. **[アップロード オプション]** は、既定値のままにします。
1. **[名前]** に、「`myCertSecretPassword`」または任意の名前を入力します。
1. **[値]** に、証明書のパスワードを入力します。
1. 残りの値は既定値のままにし、 **[作成]** を選択します。
1. **[シークレット]** ページに戻ります。

## <a name="deploy-weblogic-server-with-application-gateway-to-azure"></a>Application Gateway と共に WebLogic Server を Azure にデプロイする

このセクションでは、前のセクションで作成した Key Vault、SSL 証明書、およびパスワードの使用方法について説明します。  クラスター ノードのロード バランサーとして自動的に作成される Azure Application Gateway と共に WLS クラスターをプロビジョニングします。  この Application Gateway は、指定された SSL 証明書を SSL 終端に使用します。  Application Gateway での SSL 終端の詳細については、「[Application Gateway での TLS 終了とエンド ツー エンド TLS の概要](/azure/application-gateway/ssl-overview)」を参照してください。

WLS クラスターと Application Gateway を作成するには、次の手順を実行します。

1. [Oracle のドキュメント](https://aka.ms/arm-oraclelinux-wls-cluster-oracle-docs)の説明に従って、[Azure portal の WebLogic Server Cluster オファー](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster)を使用して WebLogic Server Cluster をプロビジョニングしますが、ここに示す **[Azure Application Gateway]** ブレードに到達したら、このページに戻ります。

   :::image type="content" source="media/migrate-weblogic-with-app-gateway/weblogic-app-gateway-blade.png" alt-text="WLS、App Gateway、および Key Vault の間の関係を示す図。":::

1. **[Azure Application Gateway]** ブレードで、 **[はい]** を選択します。
1. **[Resource group name in current subscription containing the KeyVault]\(KeyVault を含む現在のサブスクリプションのリソース グループ名\)** に、先ほど作成した Key Vault を含むリソース グループの名前を入力します。
1. **[Name of the Azure KeyVault containing secrets for the Certificate for SSL Termination]\(SSL 終端用の証明書のシークレットを含む Azure KeyVault の名前\)** に、Key Vault の名前を入力します。
1. **[The name of the secret in the specified KeyVault whose value is the SSL Certificate Data]\(値が SSL 証明書データである、指定した KeyVault 内のシークレットの名前\)** に、「`myCertSecretData`」または以前に入力した名前を入力します。
1. **[The name of the secret in the specified KeyVault whose value is the password for the SSL Certificate]\(値が SSL 証明書のパスワードである、指定した KeyVault 内のシークレットの名前\)** に、「`myCertSecretData`」または以前に入力した名前を入力します。
1. **[確認および作成]** を選択します。
1. **［作成］** を選択します  これにより、Key Vault 証明書を取得できることと、そのパスワードが Key Vault のパスワードとして格納した値と一致することが検証されます。  この検証手順が失敗した場合は、Key Vault のプロパティを見直して、証明書が正しく入力されたことと、パスワードが正しく入力されたことを確認します。
1. **[検証に成功しました]** が表示されたら、 **[作成]** を選択します。

これにより、WLS クラスターとそのフロントエンド Application Gateway の作成プロセスが開始されます。これには約 15 分かかる場合があります。  デプロイが完了したら、 **[リソース グループに移動]** を選択します。 リソース グループ内のリソースの一覧から、**myAppGateway** を選択します。

## <a name="validate-successful-deployment-of-wls-and-app-gateway"></a>WLS と App Gateway が正常にデプロイされたことを確認する

このセクションでは、WLS クラスターと Application Gateway が正常にデプロイされたことを迅速に確認する方法について説明します。

前のセクションの最後で **[リソース グループに移動]** と **myAppGateway** を選択した場合は、その Application Gateway の概要ページが表示されています。  そうでない場合は、Azure portal の上部にあるテキスト ボックスに「`myAppGateway`」と入力し、表示された適切なものを選択して、このページを見つけることができます。  必ず、WLS クラスター用に作成したリソース グループ内のものを選択してください。  その後、次の手順を完了します。

1. **myAppGateway** の概要ページの左ペインで、 **[監視]** セクションまで下にスクロールし、 **[バックエンド正常性]** を選択します。
1. **[読み込み中]** メッセージが表示されなくなったら、バックエンド プール内のノードとして構成されているクラスターのノードを示すテーブルが画面の中央に表示されます。
1. 各ノードの状態が **[正常]** と表示されていることを確認します。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

WLS クラスターを引き続き使用する予定がない場合は、次の手順で Key Vault と WLS クラスターを削除します。

1. 前のセクションで示したように、**myAppGateway** の概要ページにアクセスします。
1. ページの上部にある **[リソース グループ]** テキストの下で、リソース グループを選択します。
1. **[リソース グループの削除]** を選択します。
1. 入力フォーカスは、 **[リソース グループ名を入力してください]** というラベルのフィールドに設定されます。  要求に従ってリソース グループ名を入力します。
1. これにより、 **[削除]** ボタンが有効になります。  **[削除]** ボタンを選択します。  この操作にはしばらく時間がかかりますが、削除の処理中に次の手順に進むことができます。
1. 「[SSL 証明書を Key Vault に格納する]()」セクションの最初の手順に従って、Key Vault を見つけます。
1. **[削除]** を選択します。
1. 表示されるペインで、 **[削除]** を選択します。

## <a name="next-steps"></a>次のステップ

Azure 上で WLS を実行するためのオプションについて引き続き調べます。
> [!div class="nextstepaction"]
> [Azure 上の Oracle WebLogic の詳細を確認する](/azure/virtual-machines/workloads/oracle/oracle-weblogic)
