---
title: Azure のルート証明書を Java CA ストアに追加する
description: Microsoft Azure で使用する Java CA 証明書 (cacerts) ストアに証明機関 (CA) のルート証明書を追加する方法について説明します。
documentationcenter: java
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/13/2018
ms.custom: devx-track-java
ms.openlocfilehash: 6807374dac927d8a93e1ecf1ac2b1e3fa54877ac
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379546"
---
# <a name="adding-a-root-certificate-to-the-java-ca-certificates-store"></a>ルート証明書を Java CA 証明書ストアに追加する方法

Azure のサービス (Azure Service Bus など) を使用するアプリケーションでは、Baltimore CyberTrust Root 証明書を信頼する必要があります。 この証明書はご使用のシステムに既にインストールされている可能性があります。そうでない場合、このチュートリアルの手順では、Oracle の**keytool** を使用して、必要な証明書機関 (CA) のルート証明書を Azure サービスで使用する Java CA 証明書 (cacerts) ストアに追加する方法を示します。

Oracle の keytool ユーティリティは、"_キーと証明書管理ツール_" であり、開発者はこれを使用することで Java で使用する信頼された証明書の一覧を管理できます。 JDK を圧縮して Azure プロジェクトの *approot* フォルダーに追加する前に、keytool を使用して CA 証明書を追加できます。または、keytool を使用して証明書を追加する Azure スタートアップ タスクを実行することもできます。

2013 年 4 月 15 日より、Azure は GTE CyberTrust Global ルート証明書から Baltimore CyberTrust ルート証明書への移行を開始しました。 次の手順では、keytool を使用して Baltimore CyberTrust ルート証明書を、お使いの Java CA 証明書 (cacerts) ストアに追加する方法について説明します。

> [!NOTE]
> この記事の手順を使用すると、他の信頼された証明機関のルート証明書を信頼するように Java SDK を構成することができます。 たとえば、「[GeoTrust Root Certificates (GeoTrust ルート証明書)](https://www.geotrust.com/resources/root-certificates/)」に記載されている証明書の一覧からルート証明書を選択できます。

## <a name="determining-which-root-certificates-are-installed"></a>インストールされているルート証明書を確認する

Baltimore 証明書が cacerts ストアに既にインストールされている場合があるため、次の手順に従ってこれが既にインストールされているかどうかを確認する必要があります。

1. 管理者コマンド プロンプトで、JDK の *jdk\jre\lib\security* フォルダーに移動し、次のコマンドを実行して、お使いのシステムにインストールされている証明書を一覧表示します。

   ```shell
   keytool -list -keystore cacerts
   ```

1. ストアのパスワードを入力するように求められた場合、既定のパスワードは `changeit` です。

   > [!NOTE]
   > ストアのパスワードを変更する場合は、<https://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html> にある keytool のドキュメントを参照してください。

1. 拇印 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` のある証明書が表示されない場合は、次のセクションの手順を使用して証明書をダウンロードし、インストールします。

## <a name="to-add-a-root-certificate-to-the-cacerts-store"></a>ルート証明書を cacerts ストアに追加するには

1. Baltimore CyberTrust ルート証明書を <https://cacert.omniroot.com/bc2025.crt> からダウンロードし、*jdk \jre\lib\security* フォルダー内のローカル ファイルとしてファイル名拡張子 *.crt* を付けて保存でします。 この例では、Baltimore CyberTrust ルート証明書ファイルを *bc2025.crt* としてダウンロードしたものと想定しています。

   > [!NOTE]
   > Baltimore CyberTrust ルート証明書には、シリアル番号 `02:00:00:b9` と SHA1 拇印 `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` があります。

2. 次のコマンドを使用して、この証明書を cacerts ストアにインポートします。

   ```shell
   keytool -keystore cacerts -importcert -alias bc2025ca -file bc2025.crt
   ```

   各値の説明:

   |  パラメーター   |                              説明                               |
   |--------------|------------------------------------------------------------------------|
   | `keystore`   | 証明書ストアを指定します。                                       |
   | `importcert` | 証明書をインポートすることを指定します。                        |
   | `alias`      | 証明書のエイリアスを指定します。                                |
   | `file`       | インポートするルート証明書のファイル名を指定します。 |

3. 証明書を信頼するよう求めるメッセージが表示された場合は、拇印が `d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` であることを確認し、拇印が正しければ `y` キーを押します。

4. 次のコマンドを実行して CA 証明書が正常にインポートされたことを確認します。

   ```shell
   keytool -list -keystore cacerts
   ```

ルート証明書を JDK に正常に追加したら、JDK の内容を zip 圧縮し、Azure プロジェクトの *approot* フォルダーに追加します。

## <a name="next-steps"></a>次のステップ

keytool ユーティリティの詳細については、<https://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html> を参照してください。

Java の詳細については、「[Azure for Java developers (Java 開発者向けの Azure)](/azure/developer/java)」をご覧ください。

Azure での開発時に使用可能なサポート対象 JDK の詳細については、<https://aka.ms/azure-jdks> を参照してください。
