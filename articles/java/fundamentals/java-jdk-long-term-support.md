---
title: Azure 開発のための Java JDK と中期および長期サポート
description: この記事では、Java アプリケーションを開発して実行するためのダウンロードおよび Azure サポートに関する声明を示します。
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-september2019, devx-track-java
ms.openlocfilehash: 7df92d62afa57677e4985231a621257bd6b9031f
ms.sourcegitcommit: 12f80b1e0fe08db707c198271d0c399c3aba343a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2020
ms.locfileid: "94515138"
---
# <a name="java-long-term-support-and-medium-term-support-on-azure-and-azure-stack"></a>Azure および Azure Stack に対する Java の長期サポートと中期サポート

Azure と Azure Stack の Java 開発者は、[Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) JDK ビルドを使用し、運用 Java アプリケーションを構築して実行できます。追加のサポート コストは発生しません。 Azure では、任意の Java ランタイムを使用することができます。 しかし、Zulu を使用する場合、無料のメンテナンス更新プログラムが提供され、Microsoft のサポート チケットを作成できます。

長期サポート (LTS) として指定されているリリースは、Oracle と OpenJDK コミュニティによって指定されている LTS リリースと同じものです。 LTS リリースでは、少なくとも 8 年間、バグ修正、セキュリティ更新プログラム、および必要に応じてその他 (実稼働サポート) の修正プログラムへのアクセスが提供されます。 また、新しい JDK バージョンに移行するユーザーに助言し、支援することを目的とした 2 年間の追加サポート (拡張サポート) が提供されます。

中期サポート (MTS) として指定されたリリースについては、次回の LTS リリースの一般提供から少なくとも 1.5 年間は実稼働サポートが提供されます。 また、1 年間の拡張サポートが提供されます。

> [!div class="nextstepaction"]
> [Java のダウンロードとインストール](java-jdk-install.md)

## <a name="long-term-support-lts"></a>長期サポート (LTS)

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/?version=java-11-lts)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/?version=java-8-lts)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/?version=java-7-lts)

## <a name="medium-term-support-mts"></a>中期サポート (MTS)

* [Java 15](https://www.azul.com/downloads/azure-only/zulu/?version=java-15)
* [Java 13](https://www.azul.com/downloads/azure-only/zulu/?version=java-13)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>Azure の Zulu OpenJDK とは何ですか?

OpenJDK の Azul Zulu for Azure - Enterprise Edition ビルドは、無料でマルチプラットフォームの実稼働可能な、Azure と Azure Stack 用の OpenJDK のディストリビューションです。 これらは、Microsoft および Azul Systems によってサポートされます。 これらのディストリビューションは次のような特長があります。

* Java Development Kit (JDK)、Java Runtime Environment (JRE)、およびヘッドレス JRE としてパッケージ化された、100% オープンソースの OpenJDK のビルド。 これらのバイナリは、Java Standard Edition (SE) の完全に互換性のある、準拠している商用ビルドであり、Azure と Azure Stack 上の Java アプリケーションまたはコンポーネントで使用できます。
* バグの修正、パフォーマンス強化、セキュリティ パッチを含む、長期サポートが付属。
* Windows、Linux、macOS での Java アプリケーションの開発および実行で使用可能。
* Docker Hub のコンテナー イメージおよび Azure Marketplace の仮想マシン (Windows および Linux) として使用可能。
* 次のような多くの Azure サービスを稼働させるために Microsoft Azure で使用。
  * Windows での App Service
  * App Service on Linux
  * Azure Functions
  * Azure Service Fabric
  * Azure HDInsight
  * Azure Cognitive Search
  * Azure DevOps
  * Azure Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>サポートされている Java バージョンと更新スケジュール

Azul Systems では、Java SE 7、8、11、13、および 15 を含む、Java の長期サポート (LTS) と中期サポート (MTS) のすべてのバージョンに対して、完全にサポートされた [Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) ビルドが提供されます。 詳細については、[Azul プレス リリース](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)と [Azul 製品サポート ライフサイクル](https://www.azul.com/products/azul_support_roadmap/)のロードマップを参照してください。

|Java SE バージョン  |サポート対象  |
|---------|----------|
|[![Java 7 のロゴ](media/supported-java-versions-java-7.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-7-lts) |2023 年 7 月 (LTS)|
|[![Java 8 のロゴ](media/supported-java-versions-java-8.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-8-lts) |2030 年 12 月 (LTS)|
|[![Java 11 のロゴ](media/supported-java-versions-java-11.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-11-lts) |2027 年 9 月 (LTS)|
|[![Java 13 のロゴ](media/supported-java-versions-java-13.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-13) |2023 年 3 月 (MTS)|
|[![Java 15 のロゴ](media/supported-java-versions-java-15.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-15) |2023 年 3 月 (MTS)|

LTS と MTS の JDK リリースには、四半期ごとのセキュリティ更新プログラムとバグ修正が含まれ、必要に応じて、重要なアウトオブバンドの更新プログラムと修正プログラムも含まれます。 このサポートには、Java 11 などの新しいバージョンの Java で報告された、セキュリティ更新プログラムとバグ修正の Java 7 および 8 へのバックポートが含まれています。 このバックポートにより、古いバージョンの Java の継続的な安定性とセキュリティが確保されます。 Azure のお客様は、予定外の Java SE サブスクリプション料金を負担することなく、これらのセキュリティ更新プログラムとプラットフォームのバグ修正を入手できます。

現在、Azure Functions には Java 8 が必要であり、Java 11 のサポートはまだ開発中です。

## <a name="benefits-for-developers"></a>開発者にとっての利点

Azul Zulu for Azure - Enterprise Edition JDK リリース:

- Microsoft と Azul Systems の両方でサポートされます。

   * Zulu のバイナリは実稼働可能であり、Microsoft と Azul Systems でサポートされます。
   * Zulu には、Java 7、8、および 11 の長期サポート (LTS) と、Java 13 および 15 の中期サポート (MTS) が無料で付属します (LTS は Java 17 にも提供されます)。Java のバージョンは、必要なときにのみアップグレードできます。
   * Java 7 は 2023 年 7 月までサポートされます。 Java 8 は 2030 年 12 月までサポートされます。 Java 11 は 2027 年 9 月までサポートされます。 Java 13 および 15 は 2023 年 3 月までサポートされます。
   * Microsoft は、多くの Azure サービスを稼働するマシンで Zulu を内部的に実行することに取り組んでいます。

- 実稼働可能です。

   * OpenJDK のビルドで 100% オープン ソース。
   * 多くの Java SE ディストリビューションに対する簡単な置き換え。
   * JDK、JRE、およびヘッドレス JRE。
   * Java 7、8、11、13、および 15。
   * OpenJDK Community Technology Compatibility Kit (TCK) を使用して Java SE 仕様に準拠していることを確認済みです。
   * Java SE 7、8、11、13、および 15 のバグ修正、パフォーマンス強化、およびセキュリティ更新プログラムを含む、Java SE 用の運用更新プログラムが含まれます。

- マルチプラットフォームでサポートされます。 Zulu では、次のような複数のプラットフォームとバージョンのバイナリがサポートされています。

   * Windows
     * 10
     * 8.1
     * 8
     * 7
   * Windows Server
     * 2016 R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * Mac OS X
   * 以下の複数の種類のパッケージで配信されます。
     * MSI、ZIP、tar、deb、RPM、DMG

     複数のベース OS イメージ上の Zulu JDK、JRE、およびヘッドレス JRE 用の認定済み Docker コンテナー イメージを Docker Hub で利用できます。

     * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
     * [JRE](https://hub.docker.com/_/microsoft-java-jre)
     * [Headless JRE](https://hub.docker.com/_/microsoft-java-jre-headless)

- 無料です。

   * Microsoft では、Azure 上の Java アプリをビルドおよび拡張するために必要なものをすべて無料で提供しています。 Zulu を通じて、Java アプリ用のセキュリティ更新プログラムとプラットフォームのバグ修正が無料で提供されます。
   * [Java Flight Recorder および Zulu Mission Control](java-jdk-flight-recorder-and-mission-control.md) は、Zulu Java 8、11、およびそれ以降で利用できます。

- LTS と MTS 以外のバージョンのテクニカル プレビューが含まれます。

   * テクニカル プレビューでは、新機能が最終的に Java 17 LTS となる短期バージョンで提供されるため、それらを段階的にテストできます。

- OpenJDK に対するアップストリームされた変更が含まれます。
   * Azure Systems のコミッターによって、Zulu の変更が OpenJDK にプッシュされます。 これらのコミットによって、アップストリームのリポジトリが包括的になります。

通常どおり、Java 開発者は、Oracle JDK や Red Hat JDK などの独自の Java ランタイムを Azure に取り込んで、セキュリティで保護されたインフラストラクチャと機能豊富なサービスを利用できます。 Java 開発者は、Azure 上の Windows または Linux 仮想マシンで Java ワークロードを実行するために Oracle Java SE の運用エディションも利用できます。

## <a name="use-for-local-development"></a>ローカル開発に使用

開発者は、[Azure および Azure Stack 用の Java JDK をダウンロード](https://www.azul.com/downloads/azure-only/zulu/)して、ローカル開発環境で使用できます。 ダウンロードは、Windows、Linux、macOS で利用できます。 Linux を使用している開発者は、[yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) および [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) パッケージ マネージャーを使ってパッケージを入手することもできます。

詳細については、[Azure の Docker イメージ](java-jdk-docker-images.md)に関するページを参照してください。
