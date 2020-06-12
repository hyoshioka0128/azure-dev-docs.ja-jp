---
title: Azure 開発のための Java JDK と長期サポート
description: Java アプリケーションを開発して実行するためのダウンロードと Azure のサポートに関する声明。
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-september2019
ms.openlocfilehash: 4722c64e3b328cb5b63b6b976bd25e5b91341117
ms.sourcegitcommit: 367217792f3b16c769e2c39372358bc6b9c9c044
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292795"
---
# <a name="java-long-term-support-and-medium-term-support-for-azure-and-azure-stack"></a>Azure および Azure Stack に対する Java の長期サポートと中期サポート

Azure と Azure Stack を使用する Java 開発者は、[Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) JDK ビルドを使用して、運用 Java アプリケーションを構築して実行できます。追加のサポート コストは発生しません。 Azure 上で希望の Java ランタイムを使用できますが、Zulu を使用すると、無料のメンテナンス更新プログラムが提供され、Microsoft でサポート案件を作成できます。

長期サポート (LTS) として指定されているリリースは、Oracle と OpenJDK コミュニティによって指定されている LTS リリースと同じものです。 LTS リリースでは、少なくとも 8 年間、バグ修正、セキュリティ更新プログラム、および必要に応じてその他 ("実稼働サポート") の修正へのアクセスが提供されます。それに加え、新しい JDK バージョン に移行するユーザーに助言し支援することを目的とした 2 年間の追加サポート ("拡張サポート") が提供されます。

中期サポート (MTS) として設計されたリリースについては、次回の LTS リリースの一般提供から少なくとも 1.5 年間は実稼働サポートが提供されます。それに加え、1 年間の拡張サポートが提供されます。

> [!div class="nextstepaction"]
> [Java のダウンロードとインストール](java-jdk-install.md)

## <a name="long-term-support-lts"></a>長期サポート (LTS)

* [Java 11](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts)
* [Java 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts)
* [Java 7](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts)

## <a name="medium-term-support-mts"></a>中期サポート (MTS)

* [Java 13](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13)

## <a name="technical-preview"></a>テクニカル プレビュー

* [Java 14](https://www.azul.com/downloads/azure-only/zulu/?version=java-14)

## <a name="what-is-the-zulu-openjdk-for-azure"></a>Azure の Zulu OpenJDK とは何ですか?

OpenJDK の Azul Zulu for Azure - Enterprise Edition ビルドは、Microsoft および Azul Systems によってサポートされる、無料でマルチプラットフォーム対応かつ実稼働可能な、Azure と Azure Stack 用の OpenJDK のディストリビューションです。 これらのディストリビューションは次のような特長があります。

* Java Development Kit (JDK)、Java Runtime Environment (JRE) およびヘッドレス JRE としてパッケージ化された、100% オープンソースの OpenJDK のビルド。 これらのバイナリは、Java Standard Edition (SE) の完全に互換性のある、準拠している商用ビルドであり、Azure と Azure Stack 上の Java アプリケーションまたはコンポーネントで使用できます。
* バグの修正、パフォーマンス強化、セキュリティ パッチを含む、長期サポートが付属。
* Windows、Linux、macOS での Java アプリケーションの開発および実行で使用可能。
* Docker Hub のコンテナー イメージおよび Azure Marketplace の仮想マシン (Windows および Linux) として使用可能。
* 次のような多くの Azure サービスを稼働させるために Microsoft Azure で使用。
  * App Service Windows
  * App Service Linux
  * 関数
  * Service Fabric
  * HDInsight
  * 検索
  * Azure DevOps
  * Cloud Shell  

## <a name="supported-java-versions-and-update-schedule"></a>サポートされている Java バージョンと更新スケジュール

Azul Systems は、Java SE 7、8、11、および 13 を含む Java の長期サポート (LTS) と中期サポート (MTS) の全バージョンに対して、完全にサポートされた [Azul Zulu for Azure - Enterprise Edition](https://www.azul.com/downloads/azure-only/zulu/) ビルドを提供します。 詳細については、[Azul プレス リリース](https://www.azul.com/press_release/free-java-production-support-for-microsoft-azure-azure-stack)と [Azul 製品サポート ライフサイクル](https://www.azul.com/products/azul_support_roadmap/)のロードマップを参照してください。

|Java SE バージョン  |サポート期限  |
|---------|----------|
|[![サポートされている Java バージョン - LTS - Java 7](media/supported-java-versions-java-7.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-7-lts) |2023 年 7 月 - LTS|
|[![サポートされている Java バージョン - LTS - Java 8](media/supported-java-versions-java-8.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts) |2030 年 12 月 - LTS|
|[![サポートされている Java バージョン - LTS - Java 11](media/supported-java-versions-java-11.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts) |2027 年 9 月 - LTS|
|[![サポートされている Java バージョン - MTS - Java 13](media/supported-java-versions-java-13.png)](https://www.azul.com/downloads/azure-only/zulu/?&version=java-13) |2023 年 3 月 - MTS|
|[![サポートされている Java バージョン - プレビュー - Java 14](media/supported-java-versions-java-14.png)](https://www.azul.com/downloads/azure-only/zulu/?version=java-14) |**プレビュー**|

これらの LTS と MTS の JDK リリースには、四半期ごとのセキュリティ更新プログラムとバグ修正が含まれ、必要に応じて、重要なアウトオブバンドの更新プログラムと修正プログラムも含まれます。  このサポートには、Java 11 などの新しいバージョンの Java で報告されたセキュリティ更新プログラムとバグ修正の Java 7 および 8 へのバック ポーティングが含まれており、古いバージョンの Java の継続的な安定性とセキュリティが確保されます。  Azure のお客様は、予定外の Java SE サブスクリプション料金を負担することなく、これらのセキュリティ更新プログラムとプラットフォームのバグ修正を入手できます。

## <a name="benefits-for-developers"></a>開発者にとっての利点

Azul Zulu for Azure - Enterprise Edition JDK リリース:

1. Microsoft と Azul Systems の両方でサポート

   * Zulu のバイナリは実稼働可能であり、Microsoft と Azul Systems でサポートされます。
   * Zulu には、Java 7、8、および 11 の長期サポート (LTS) と Java 13 の中期サポート (MTS) が無料で 付属します。 (LTS は Java 17 にも提供されます)。 Java のバージョンは、必要なときにのみアップグレードできます。
   * Java 7 は 2023 年 7 月までサポートされます。 Java 8 および 11 は 2027 年 9 月までサポートされます。 Java 13 は 2023 年 3 月までサポートされます。
   * Microsoft は、多くの Azure サービスを稼働するマシンで Zulu を内部的に実行することに取り組んでいます。

2. 実稼働可能

   * OpenJDK のビルドで 100% オープンソース。
   * 多くの Java SE ディストリビューションに対する簡単な置き換え。
   * JDK、JRE および JRE ヘッドレス
   * Java 7、8、11、および 13。
   * OpenJDK Community Technology Compatibility Kit (TCK) を使用して Java SE 仕様に準拠していることを確認済み。
   * 開発者には、Java SE 向けの運用更新プログラムの提供が継続され、これには Java SE 7、8、11、および 13 のバグ修正、パフォーマンス強化、およびセキュリティ修正プログラムが含まれます。

3. マルチプラット フォームのサポート。 Zulu では、次のような複数のプラットフォームとバージョンのバイナリがサポートされています。

   * Windows クライアント
     * 10
     * 8.1
     * 8、7
   * Windows Server
     * 2016R2
     * 2016
     * 2012 R2
     * 2012
     * 2008 R2
   * Linux (次を含む)
     * RHEL
     * CentOS
     * Ubuntu
     * SLES
     * Debian
     * Oracle Linux
   * Mac OS X
   * 複数のパッケージ タイプで配信:
     * MSI、ZIP、TAR、DEB、RPM、および DMG

    複数のベース OS イメージ上の Zulu JDK、JRE および JRE ヘッドレス用の認定済み Docker コンテナー イメージを Docker で利用できます。

    ハブ:

    * [JDK](https://hub.docker.com/_/microsoft-java-jdk)
    * [JRE](https://hub.docker.com/_/microsoft-java-jre)
    * [JRE ヘッドレス](https://hub.docker.com/_/microsoft-java-jre-headless)

4. 無料

   * Microsoft は、Azure 上の Java アプリをビルドおよび拡張するために必要なものをすべて無料で提供します。 Zulu を通じて、Java アプリ用のセキュリティ更新プログラムとプラットフォームのバグ修正が無料で提供されます。
   * [Java Flight Recorder および Mission Control](java-jdk-flight-recorder-and-mission-control.md) は、Zulu Java 8、11、およびそれ以降で利用できます。

5. 非 LTS/MTS バージョンの技術プレビュー

   * 技術プレビューでは、新機能が最終的に Java 17 LTS となる短期的なバージョンで提供される中で、それらを斬新的にテストする機会を得られます。

6. OpenJDK に対する変更を上流に反映

   * Azul Systems のコミッターが Zulu の変更を OpenJDK にプッシュするため、上流のリポジトリが包括的に維持されます。

Java 開発者は、Oracle JDK や Red Hat JDK などの独自の Java ランタイムを Azure に取り込んで、セキュリティ保護されたインフラストラクチャと機能豊富なサービスを利用できます。 Java 開発者は、Azure 上の Windows または Linux 仮想マシンで Java ワークロードを実行するために Oracle Java SE の運用エディションも利用できます。

## <a name="use-for-local-development"></a>ローカル開発に使用

開発者は、Azure および Azure Stack 用の Java JDK を[ダウンロード](https://www.azul.com/downloads/azure-only/zulu/)して、ローカル開発環境で使用できます。 ダウンロードは、Windows、Linux、macOS で利用できます。 Linux を使用している開発者は、[yum](https://www.azul.com/downloads/azure-only/zulu/#yum-repo) および [apt](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) パッケージ マネージャーを使ってパッケージを入手することもできます。

追加のガイダンスについては、[Azure 用の Docker イメージ](java-jdk-docker-images.md)に関する記事を参照してください。
