---
title: Java 8 から Java 11 への移行
titleSuffix: Azure
description: Java 8 から Java 11 への移行を管理するためのガイドです。
author: dsgrieve
manager: maverbur
tags: java
ms.service: azure
ms.devlang: java
ms.topic: article
ms.date: 11/19/2019
ms.author: dagrieve
ms.openlocfilehash: 528b111e945bb68bd18c849847522a070259c0f3
ms.sourcegitcommit: f1e3c72c38376b15f5313d4bfe5fefdbfc022dc9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "79022299"
---
# <a name="transition-from-java-8-to-java-11"></a>Java 8 から Java 11 への移行

Java 8 から Java 11 にコードを移行するための万能のソリューションはありません。
複雑なアプリケーションの場合、Java 8 から Java 11 への移行はかなりの作業量になる可能性があります。 潜在的な問題として、削除された API、非推奨のパッケージ、内部 API の使用、クラス ローダーの変更、ガベージ コレクションの変更などがあります。 

一般に、再コンパイルせずに Java 11 上で実行してみる方法と、まず JDK 11 を使用してコンパイルする方法があります。 できるだけ早くアプリケーションを起動して実行することが目標であれば、多くの場合、Java 11 上で実行してみることが最適な方法です。 ライブラリの目標は、JDK 11 を使用してコンパイルおよびテストされた成果物を発行することです。

Java 11 への移行は苦労する価値があります。 Java 8 以降、新機能が追加され、機能強化が行われてきました。 これらの機能と機能強化により、起動、パフォーマンス、メモリ使用量が向上し、コンテナーとの統合が強化されます。 また、開発者の生産性を向上させる API に追加や変更が加えられています。 

このドキュメントでは、コードを検査するツールについて説明します。 また、発生する可能性のある問題とその解決に関する推奨事項についても説明します。 「[Oracle JDK Migration Guide (Oracle JDK 移行ガイド)](https://docs.oracle.com/en/java/javase/11/migrate/index.html)」など、他のガイドも参照することをお勧めします。 既存のコード [モジュラー](http://openjdk.java.net/projects/jigsaw)の作成方法については、ここでは説明しません。  


## <a name="the-toolbox"></a>ツールボックス

Java 11 には、*jdeprscan* と *jdeps* という 2 つのツールがあり、潜在的な問題をスニッフィングするために役立ちます。 これらのツールは、既存のクラスまたは jar ファイルに対して実行できます。 再コンパイルしなくても、移行作業を評価することができます。 

[jdeprscan](https://docs.oracle.com/en/java/javase/11/tools/jdeprscan.html) を使うと、非推奨の API または削除された API を検索できます。
非推奨の API の使用は障害となる問題ではありませんが、注意すべき点があります。 更新された jar ファイルはありますか。 非推奨の API の使用に対処するために問題をログに記録する必要はありますか。 削除された API の使用は、Java 11 上で実行を試みる前に対処する必要がある障害となる問題です。


[jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html)。Java クラスの依存関係アナライザーです。 *jdeps* を `--jdk-internals` オプションと共に使用すると、どのクラスがどの内部 API に依存しているかがわかります。 Java 11 では引き続き内部 API を使用できますが、そのような使用方法は置き換えることを優先してください。 OpenJDK の wiki ページ「[Java Dependency Analysis Tool (Java 依存関係分析ツール)](https://wiki.openjdk.java.net/display/JDK8/Java+Dependency+Analysis+Tool)」では、一般的に使用されるいくつかの JDK 内部 API の置き換えが推奨されています。 

Gradle と Maven の両方に *jdeps* および *jdeprscan* プラグインがあります。 これらのツールをビルド スクリプトに追加することをお勧めします。 

> [!div class="mx-tdBreakAll"]
> |ツール|Gradle プラグイン|Maven プラグイン|
> |-|-|-|
> |jdeps|[jdeps-gradle-plugin](https://github.com/kordamp/jdeps-gradle-plugin)|[Apache Maven JDeps プラグイン](https://maven.apache.org/plugins/maven-jdeps-plugin/index.html)|
> |jdeprscan|[jdeprscan-gradle-plugin](https://github.com/kordamp/jdeprscan-gradle-plugin)|[Apache Maven JDeprScan プラグイン](https://maven.apache.org/plugins/maven-jdeprscan-plugin/index.html)|

Java コンパイラ自体 (*javac*) は、ツールボックス内の別のツールです。 *jdeprscan* と *jdeps* から生成される警告とエラーは、コンパイラから得られます。  *jdeprscan* および *jdeps* を使用する利点は、サードパーティ製ライブラリを含む既存の jar とクラス ファイルに対してこれらのツールを実行できることです。

*jdeprscan* と *jdeps* を使って実行できないことは、カプセル化された API にアクセスするためのリフレクションの使用に関する警告です。 反射アクセスは実行時に確認されます。 確実に確認するには、最終的に、Java 11 上でコードを実行する必要があります。

### <a name="using-jdeprscan"></a>jdeprscan の使用

[jdeprscan](https://docs.oracle.com/en/java/javase/11/tools/jdeprscan.html) を使用する最も簡単な方法は、既存のビルドから jar ファイルを指定することです。 コンパイラの出力ディレクトリなどのディレクトリ、または個々のクラス名を付けることもできます。 非推奨の API の最も詳細な一覧を取得するには、`--release 11` オプションを使用します。 非推奨の API に優先順位を付ける場合は、設定を `--release 8` に戻します。 Java 8 で非推奨になった API は、最近非推奨になった API よりも早く削除される可能性があります。 

```console
jdeprscan --release 11 my-application.jar
```

*jdeprscan* ツールを使うと、依存クラスの解決時に問題が発生した場合にエラー メッセージを生成できます。
たとえば、「 `error: cannot find class org/apache/logging/log4j/Logger` 」のように入力します。 依存クラスを `--class-path` に追加するか、アプリケーションのクラスパスを使用することをお勧めしますが、使用しなくても、ツールのスキャンは続行されます。
引数は *&#8209;&#8209;class&#8209;path* です。 class-path 引数のその他のバリエーションは機能しません。

```console
jdeprscan --release 11 --class-path log4j-api-2.13.0.jar my-application.jar
error: cannot find class sun/misc/BASE64Encoder
class com/company/Util uses deprecated method java/lang/Double::<init>(D)V
```
この出力は、`com.company.Util` クラスが `java.lang.Double` クラスの非推奨のコンストラクターを呼び出していることを示しています。 javadoc から、非推奨の API の代わりに使用する API が推奨されます。 API は削除されているため、`error: cannot find class sun/misc/BASE64Encoder` を解決する作業はありません。 Java 8 以降では、`java.util.Base64` を使用することをお勧めします。 

Java 8 以降に非推奨とされた API の意味を理解するには、`jdeprscan --release 11 --list` を実行します。 削除された API の一覧を取得するには、`jdeprscan --release 11 --list --for-removal` を実行します。

### <a name="using-jdeps"></a>jdeps の使用

[jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html) を `--jdk-internals` オプションと共に使用して、JDK 内部 API の依存関係を見つけます。 *log4j-core-2.13.0.jar* は[マルチリリース jar ファイル](https://docs.oracle.com/en/java/javase/11/docs/specs/jar/jar.html#multi-release-jar-files)なので、この例ではコマンド ライン オプション `--multi-release 11` が必要です。 このオプションを指定しないと、マルチリリース jar ファイルが見つかった場合に *jdeps* からエラーが生成されます。 このオプションには、検査するクラス ファイルのバージョンを指定します。 

```console
jdeps --jdk-internals --multi-release 11 --class-path log4j-core-2.13.0.jar my-application.jar
Util.class -> JDK removed internal API
Util.class -> jdk.base
Util.class -> jdk.unsupported
   com.company.Util        -> sun.misc.BASE64Encoder        JDK internal API (JDK removed internal API)
   com.company.Util        -> sun.misc.Unsafe               JDK internal API (jdk.unsupported)
   com.company.Util        -> sun.nio.ch.Util               JDK internal API (java.base)

Warning: JDK internal APIs are unsupported and private to JDK implementation that are
subject to be removed or changed incompatibly and could break your application.
Please modify your code to eliminate dependence on any JDK internal APIs.
For the most recent update on JDK internal API replacements, please check:
https://wiki.openjdk.java.net/display/JDK8/Java+Dependency+Analysis+Tool

JDK Internal API                         Suggested Replacement
----------------                         ---------------------
sun.misc.BASE64Encoder                   Use java.util.Base64 @since 1.8
sun.misc.Unsafe                          See http://openjdk.java.net/jeps/260   

```

出力には、JDK 内部 API の使用を排除するための適切なアドバイスが含まれます。 可能な場合は、代替 API が提案されます。 パッケージがカプセル化されたモジュールの名前は、かっこで囲まれています。 明示的に[カプセル化を解除する](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-2F61F3A9-0979-46A4-8B49-325BA0EE8B66)必要がある場合は、モジュール名を `--add-exports` または `--add-opens` と共に使用できます。 

*sun.misc.BASE64Encoder* または *sun.misc.BASE64Decoder* を使用すると、Java 11 で *java.lang.NoClassDefFoundError* が生成されます。 これらの API を使用するコードは、*java.util.Base64* を使用するように変更する必要があります。 

モジュール *jdk.unsupported* に由来する API の使用をなくすようにしてください。 このモジュールの API からは、提案された代替として [JDK エンハンスメント提案 (JEP) 260](http://openjdk.java.net/jeps/260) が参照されます。
簡単に言うと、JEP 260 には、代替 API が使用可能になるまで、内部 API の使用がサポートされると記載されています。 コードで JDK 内部 API を使用している場合でも、少なくともしばらくは引き続き実行されます。 一部の内部 API については代替がポイントされるため、JEP 260 を確認してください。 
[変数ハンドル](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/invoke/VarHandle.html)は、たとえば何らかの *sun.misc.Unsafe* API の代わりに使用できます。 

*jdeps* の機能は、JDK 内部の使用をスキャンするだけではありません。 依存関係を分析したり、モジュール情報ファイルを生成したりする場合に便利なツールです。 詳細については、[ドキュメント](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html)を参照してください。

### <a name="using-javac"></a>javac の使用

JDK 11 を使用してコンパイルする場合は、ビルド スクリプト、ツール、テスト フレームワーク、および付属ライブラリを更新する必要があります。 *javac* に `-Xlint:unchecked` オプションを使用すると、JDK 内部 API とその他の警告の使用方法の詳細を取得できます。 また、カプセル化されたパッケージをコンパイラに公開するために `--add-opens` または `--add-reads` を使用する必要がある場合もあります ([JEP 261](http://openjdk.java.net/jeps/261) を参照してください)。 

ライブラリは、[マルチリリース jar ファイル](https://docs.oracle.com/en/java/javase/11/docs/specs/jar/jar.html#multi-release-jar-files)としてパッケージ化することを検討できます。 マルチリリース jar ファイルを使用すると、同じ jar ファイルから Java 8 と Java 11 の両方のランタイムをサポートできます。 ビルドに複雑さを追加されます。 マルチリリース jar を構築する方法については、このドキュメントでは説明しません。 

## <a name="running-on-java-11"></a>Java 11 上での実行

ほとんどのアプリケーションは、変更せずに Java 11 上で実行する必要があります。 まず、コードを再コンパイルせずに、Java 11 上で実行する方法を試します。 実行することのポイントは、実行からどの警告とエラーが生成されるかを確認できることです。 この方法では、  
実行する必要のある最小限のものに焦点が当てられているので、Java 11 上でアプリケーションをより迅速に実行できます。 

発生する可能性のあるほとんどの問題は、コードを再コンパイルしなくても解決できます。
コードの問題を修正する必要がある場合は、修正を行いますが、JDK 8 を使用してコンパイルが続行されます。 可能であれば、JDK 11 を使用して "*コンパイル*" する前に、`java` バージョン 11 を使用してアプリケーションを "*実行*" してみてください。 

### <a name="check-command-line-options"></a>コマンドライン オプションを確認する

Java 11 上で実行する前に、コマンドライン オプションのクイック スキャンを実行します。 
[削除されたオプション](#unrecognized-options)を選択すると、Java 仮想マシン (JVM) が終了します。 GC ログ オプションを使用する場合は、Java 8 から大幅に変更されたため、このチェックは特に重要です。 [JaCoLine](https://jacoline.dev/about) ツールは、コマンドライン オプションを使用して問題を検出するために適しています。 

### <a name="check-third-party-libraries"></a>サードパーティ製ライブラリを確認する

問題の原因は、自分では制御できないサードパーティ製ライブラリである可能性があります。 サードパーティ製ライブラリは、事前により新しいバージョンに更新できます。 または、アプリケーションの実行されない機能を確認し、必要なライブラリのみを更新することができます。 すべてのライブラリを最新バージョンに更新する場合の問題は、アプリケーションに何らかのエラーがある場合、根本原因の特定が難しくなることです。 更新されたライブラリが原因でエラーが発生しましたか。 または、ランタイムの何らかの変更が原因でエラーが発生しましたか。 必要なものだけを更新する場合の問題は、解決するために複数回の反復処理が必要になる場合があることです。

ここでの推奨事項は、可能な限り少ない変更を行い、[別の作業](#next-steps)としてサードパーティ製ライブラリを更新することです。 サードパーティ製ライブラリを更新する場合は、Java 11 と互換性のある最新かつ最大バージョンを使用するのが一般的です。 現在のバージョンがどの程度遅れたものかに応じて、より慎重な方法を採り、最初の Java 9 以降の互換バージョンにアップグレードすることをお勧めします。 

リリース ノートを確認するだけでなく、*jdeps* および *jdeprscan* を使用して jar ファイルを評価できます。 また、OpenJDK Quality Group は [Quality Outreach](https://wiki.openjdk.java.net/display/quality/Quality+Outreach) wiki ページを保守しており、OpenJDK のバージョンに対する多くの無料オープン ソース ソフトウェア (FOSS) プロジェクトのテストの状態が掲載されています。 

### <a name="explicitly-set-garbage-collection"></a>ガベージ コレクションを明示的に設定する

並列ガベージ コレクター (Parallel GC) は、Java 8 の既定の GC です。 アプリケーションでこの既定が使用されている場合は、コマンドライン オプション `-XX:+UseParallelGC` を使用して GC を明示的に設定する必要があります。
既定値は、Java 9 で Garbage First ガベージ コレクター (G1GC) に変更されました。 Java 8 上と Java 11 上で実行されているアプリケーションを公平に比較するには、GC 設定を同じにする必要があります。 GC 設定の実験は、アプリケーションが Java 11 で検証されるまで延期することをお勧めします。 

### <a name="explicitly-set-default-options"></a>既定のオプションを明示的に設定する

HotSpot VM 上で実行している場合、コマンド ライン オプション `-XX:+PrintCommandLineFlags` を設定すると、VM によって設定されたオプションの値、特に GC によって設定された既定値がダンプされます。
Java 8 上でこのフラグを指定して実行し、Java 11 上で実行する場合は出力されたオプションを使用します。 ほとんどの場合、8 と 11 の既定値は同じです。 ただし、8 の設定を使用すると、パリティが確実になります。

コマンド ライン オプション `--illegal-access=warn` を設定することをお勧めします。
Java 11 では、リフレクションを使用して JDK 内部 API にアクセスすると、[不正なリフレクト アクセス警告](#warning-an-illegal-reflective-access-operation-has-occurred)が発生します。
既定では、最初の不正なアクセスに対してのみ警告が発行されます。 `--illegal-access=warn` を設定すると、不正な反射アクセス "*ごとに*" 警告が生成されます。 オプションが *warn* に設定された不正アクセスの場合、より多くのケースが見つかります。 ただし、重複する警告も多数表示されます。  
アプリケーションが Java 11 上で動作したら、Java ランタイムの今後の動作を模倣するように `--illegal-access=deny` を設定します。 Java 16 以降では、既定値は `--illegal-access=deny` です。 

### <a name="classloader-cautions"></a>クラスローダーの注意事項

Java 8 では、システム クラス ローダーを `URLClassLoader` にキャストできます。 通常、この処理は、実行時にクラスをクラスパスに挿入するアプリケーションとライブラリによって実行されます。 Java 11 では、クラス ローダーの階層が変更されています。 システム クラス ローダー (アプリケーション クラス ローダーとも呼ばれます) は、内部クラスになりました。 `URLClassLoader` にキャストすると、実行時に `ClassCastException` がスローされます。 Java 11 には、実行時にクラスパスを動的に拡張する API がありませんが、リフレクションを介して実行できます。ただし、内部 API の使用に関する明確な警告を受け取ります。 

Java 11 では、ブート クラス ローダーによってコア モジュールのみが読み込まれます。 null の親を持つクラス ローダーを作成した場合、すべてのプラットフォーム クラスが見つからない可能性があります。 このような場合、Java 11 では、親クラス ローダーとして `null` ではなく `ClassLoader.getPlatformClassLoader()` を渡す必要があります。 

### <a name="locale-data-changes"></a>ロケール データの変更

Java 11 のロケール データの既定のソースは、[JEP 252](http://openjdk.java.net/jeps/252) で、Unicode コンソーシアムの共通ロケール データ リポジトリに変更されました。 これはローカライズされた書式設定に影響する可能性があります。 必要に応じて、システム プロパティ `java.locale.providers=COMPAT,SPI` を設定して、Java 8 のロケール動作に戻します。 

### <a name="potential-issues"></a>潜在的な問題

ここでは、発生する可能性のある一般的な問題をいくつか紹介します。 これらの問題の詳細については、リンクを選択してください。

- [認識されない VM オプション](#unrecognized-options)
- [認識されないオプション](#unrecognized-options)
- [VM の警告:オプションを無視する](#vm-warnings)
- [VM の警告:オプション&lt;*オプション*&gt;は非推奨になりました](#vm-warnings)
- [警告: 不正な反射アクセス操作が発生しました](#warning-an-illegal-reflective-access-operation-has-occurred)
- [java.lang.reflect.InaccessibleObjectException](#javalangreflectinaccessibleobjectexception)
- [java.lang.NoClassDefFoundError](#javalangnoclassdeffounderror)
- [-Xbootclasspath/p はサポートされるオプションではなくなりました](#-xbootclasspathp-is-no-longer-a-supported-option)
- [java.lang.UnsupportedClassVersionError](#unsupportedclassversionerror)

#### <a name="unrecognized-options"></a>認識されないオプション

コマンドライン オプションが削除されている場合、アプリケーションからは `Unrecognized option:` または `Unrecognized VM option` の後に問題のあるオプションの名前を付けて出力します。 認識されないオプションを指定すると、VM が終了します。
非推奨ですが削除されていないオプションを使うと、[VM 警告](#vm-warnings)が生成されます。

一般に、削除されたオプションに代替はありません。唯一の修正は、コマンド ラインからオプションを削除することです。 例外は、ガベージ コレクション ログのオプションです。 GC ログ記録は、[統合 JVM ログ記録フレームワーク](http://openjdk.java.net/jeps/158)を使用するために Java 9 で[再実装](http://openjdk.java.net/jeps/271)されました。 詳細については、Java SE 11 ツール リファレンスのセクション「[Enable Logging with the JVM Unified Logging Framework (JVM 統合ログ記録フレームワークでログ記録を有効にする)](https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5)」の「Table 2-2 Mapping Legacy Garbage Collection Logging Flags to the Xlog Configuration (表 2-2 従来のガベージ コレクション ログ記録フラグから Xlog 構成へのマッピング)」を参照してください。 

#### <a name="vm-warnings"></a>VM の警告

非推奨のオプションを使用すると、警告が生成されます。 オプションは、置き換えられた場合、または不要になった場合は非推奨になります。 [削除されたオプション](#unrecognized-options)と同様に、これらのオプションはコマンドラインから削除することをお勧めします。
警告 `VM Warning: Option <option> was deprecated` は、オプションが引き続きサポートされていることを意味しますが、将来的にサポートされなくなる可能性があります。 サポートされなくなったオプションの場合は警告 `VM Warning: Ignoring option` が生成されます。
サポートされなくなったオプションは、ランタイムに影響しません。

Web ページの [VM オプション エクスプローラー](https://chriswhocodes.com/hotspot_option_differences.html)には、JDK 7 以降に Java から追加または削除されたオプションの詳細な一覧が表示されます。 

#### <a name="error-could-not-create-the-java-virtual-machine"></a>エラー:Could not create the Java Virtual Machine (Java 仮想マシンを作成できませんでした)

このエラー メッセージは、JVM で[認識されないオプション](#unrecognized-options)が検出されたときに出力されます。

#### <a name="warning-an-illegal-reflective-access-operation-has-occurred"></a>警告:An illegal reflective access operation has occurred (不正な反射アクセス操作が発生しました)

Java コードがリフレクションを使用して JDK 内部 API にアクセスする場合、ランタイムでは不正な反射アクセスの警告が発行されます。

```console
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by my.sample.Main (file:/C:/sample/) to method sun.nio.ch.Util.getTemporaryDirectBuffer(int)
WARNING: Please consider reporting this to the maintainers of com.company.Main
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
```

これは、モジュールから、リフレクションを介してアクセスされているパッケージがエクスポートされていないことを意味します。 パッケージはモジュール内で*カプセル化*され、基本的には内部 API です。 Java 11 上で起動し、実行するための最初の作業段階では、この警告は無視できます。
Java 11 ランタイムでは、反射アクセスが許可されているため、レガシ コードは引き続き動作します。  

この警告に対処するには、内部 API を使用しない更新されたコードを探します。 更新されたコードで問題が解決しない場合は、`--add-exports` または `--add-opens` コマンドライン オプションを使用してパッケージへのアクセスを開くことができます。
これらのオプションを使用すると、あるモジュールのエクスポートされていない型に別のモジュールからアクセスできます。

[`--add-exports`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-2F61F3A9-0979-46A4-8B49-325BA0EE8B66) オプションを使うと、ターゲット モジュールから、ソース モジュールの名前付きパッケージの*パブリック*型にアクセスできます。 コードで `setAccessible(true)` を使用してパブリックではないメンバーと API にアクセスする場合があります。 これは "*ディープ リフレクション*" と呼ばれます。 この場合は、[`--add-opens`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-12F945EB-71D6-46AF-8C3D-D354FD0B1781) を使用して、パッケージのパブリックではないメンバーへのアクセス権をコードに付与します。 *--add-exports* または *--add-opens* のどちらを使用するかわからない場合は、まず *--add-exports* から始めます。 

`--add-exports` または `--add-opens` オプションは、長期的な解決策ではなく、回避策として考えるようにします。
これらのオプションを使用すると、モジュール システムのカプセル化が解除されます。これは、JDK 内部 API が使用されないようにするためのものです。  内部 API が削除または変更されると、アプリケーションは失敗します。  Java 16 では、`--add-opens` などのコマンド ライン オプションによってアクセスが有効にされている場合を除き、反射アクセスは拒否されます。
今後の動作を模倣するには、コマンド ラインで `--illegal-access=deny` を設定します。

`sun.nio.ch` パッケージは `java.base` モジュールによってエクスポートされないため、上記の例の警告が発行されます。 つまり、モジュール `java.base` の `module-info.java` ファイルには `exports sun.nio.ch;` がありません。 これは `--add-exports=java.base/sun.nio.ch=ALL-UNNAMED` を使用して解決できます。 モジュールで定義されていないクラスは、暗黙的に `ALL-UNNAMED` という名前の "*名前のない*" モジュールに暗黙的に属します。

#### <a name="javalangreflectinaccessibleobjectexception"></a>java.lang.reflect.InaccessibleObjectException

この例外は、カプセル化されたクラスのフィールドまたはメソッドで `setAccessible(true)` を呼び出そうとしていることを示します。 また、[不正な反射アクセス警告](#warning-an-illegal-reflective-access-operation-has-occurred)を受け取る場合もあります。 [`--add-opens`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-12F945EB-71D6-46AF-8C3D-D354FD0B1781) オプションを使用して、パッケージの非パブリック メンバーへのアクセス権をコードに付与します。 例外メッセージによって、*setAccessible* を呼び出そうとしているモジュールに対してパッケージを "開かない" ことが通知されます。 モジュールが "名前のないモジュール" の場合は、 *--add-opens* オプションでターゲット モジュールとして `UNNAMED-MODULE` を使用します。

```shell
java.lang.reflect.InaccessibleObjectException: Unable to make field private final java.util.ArrayList jdk.internal.loader.URLClassPath.loaders accessible: 
module java.base does not "opens jdk.internal.loader" to unnamed module @6442b0a6

$ java --add-opens=java.base/jdk.internal.loader=UNNAMED-MODULE example.Main
```

#### <a name="javalangnoclassdeffounderror"></a>java.lang.NoClassDefFoundError

*NoClassDefFoundError* の原因は、おそらく分割パッケージ、または削除されたモジュールの参照です。 

##### <a name="noclassdeffounderror-caused-by-split-packages"></a>分割パッケージによって引き起こされる NoClassDefFoundError

分割パッケージとは、1 つのパッケージが複数のライブラリで見つかる場合です。 分割パッケージの問題の症状は、クラスパスにあることがわかっているクラスが見つからないことです。 

この問題は、モジュールパスを使用した場合にのみ発生します。 Java モジュール システムでは、パッケージを 1 つの "*名前付き*" モジュールに制限することにより、クラス ルックアップを最適化します。 ランタイムでは、クラス参照を行うときに、クラスパスよりもモジュールパスが優先されます。 パッケージがモジュールパスとクラスパスに分割されている場合、モジュールのみがクラス参照の実行に使用されます。 これにより、`NoClassDefFound` エラーが発生する可能性があります。 

分割パッケージを確認するには、モジュール パスとクラス パスを [jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html) に接続し、アプリケーション クラス ファイルへのパスを &lt;path&gt; として使用する方法が簡単です。 分割パッケージがある場合は、jdeps から警告 `Warning: split package: <package-name> <module-path> <split-path>` が出力されます。 

この問題を解決するには、`--patch-module <module-name>=<path>[,<path>]` を使用して名前付きモジュールに分割パッケージを追加します。 

##### <a name="noclassdeffounderror-caused-by-using-java-ee-or-corba-modules"></a>Java EE または CORBA モジュールを使用して発生した NoClassDefFoundError

アプリケーションは Java 8 上で動作しますが `java.lang.NoClassDefFoundError` または `java.lang.ClassNotFoundException` がスローされる場合、アプリケーションで Java EE または CORBA モジュールのパッケージが使用されている可能性があります。 これらのモジュールは Java 9 で非推奨になり、[Java 11 で削除されました](https://openjdk.java.net/jeps/320)。 

この問題を解決するには、ランタイム依存関係をプロジェクトに追加します。

> [!div class="mx-tdBreakAll"]
> |削除されたモジュール|影響を受けるパッケージ|推奨される依存関係|
> |-|-|-|
> |Java API for XML Web Services (JAX-WS) |java.xml.ws |[JAX WS RI Runtime](https://mvnrepository.com/artifact/com.sun.xml.ws/jaxws-rt) |
> |Java Architecture for XML Binding (JAXB) |java.xml.bind |[JAXB Runtime](https://mvnrepository.com/artifact/org.glassfish.jaxb/jaxb-runtime)|
> |JavaBeans Activation Framework (JAV) |java.activation |[JavaBeans (TM) Activation Framework](https://mvnrepository.com/artifact/javax.activation/activation) |
> |一般的な注釈 |java.xml.ws.annotation |[Javax Annotation API](https://mvnrepository.com/artifact/javax.annotation/javax.annotation-api)|
> |Common Object Request Broker Architecture (CORBA) |java.corba | [GlassFish CORBA ORB](https://mvnrepository.com/artifact/org.glassfish.corba/glassfish-corba-orb) |
> |Java Transaction API (JTA) |java.transaction | [Java Transaction API](https://mvnrepository.com/artifact/javax.transaction/jta)|

#### <a name="-xbootclasspathp-is-no-longer-a-supported-option"></a>-Xbootclasspath/p はサポートされるオプションではなくなりました

`-Xbootclasspath/p` のサポートは削除されました。 代わりに `--patch-module` を使用してください *--patch-module* オプションについては [JEP 261](http://openjdk.java.net/jeps/261) を参照してください。 「Patching module content (モジュール コンテンツの修正)」というセクションを探してください。 *--patch-module* を *javac* および *java* と共に使用して、モジュール内のクラスをオーバーライドまたは拡張できます。 

*--patch-module* によって実際に行われる処理は、モジュール システムのクラス参照にパッチ モジュールを挿入することです。 モジュール システムによって、まずパッチ モジュールからクラスが取得されます。 これは、Java 8 で bootclasspath を事前に保留する処理と同じ効果です。 

#### <a name="unsupportedclassversionerror"></a>UnsupportedClassVersionError

この例外は、新しいバージョンの Java を使用してコンパイルされたコードを、以前のバージョンの Java 上で実行しようとしていることを意味します。 たとえば、JDK 13 を使用してコンパイルされた jar を、Java 11 上で実行しています。 

| Java バージョン | クラス ファイル形式のバージョン |
|-|-|
| 8  | 52 |
| 9  | 53 |
| 10 | 54 |
| 11 | 55 |
| 12 | 56 |
| 13 | 57 |

## <a name="next-steps"></a>次のステップ

アプリケーションが Java 11 上で動作したら、ライブラリをクラスパスからモジュールパスに移動することを検討します。 アプリケーションが依存しているライブラリの更新されたバージョンを探します。 使用可能な場合は、モジュール ライブラリを選択します。 アプリケーションでモジュールを使用する予定がない場合でも、可能な限りモジュールパスを使用します。 モジュールパスを使用すると、クラスパスよりもクラス読み込みのパフォーマンスが向上します。 
