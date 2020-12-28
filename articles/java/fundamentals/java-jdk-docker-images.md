---
title: Azure Java 開発用の JDK で Docker イメージを使用する
description: コマンド ライン インターフェイスを使用して Azure 用の Java Development Kit (JDK) で Docker イメージを使用する方法について説明します。
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-august2019, seo-java-september2019, devx-track-java
ms.openlocfilehash: d1e66ac7e00f23fa4376d9dccddd301bbbe07bfd
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379826"
---
# <a name="use-docker-with-a-java-development-kit-jdk-for-azure"></a>Docker を Azure 用 Java Development Kit (JDK) と共に使用する

この記事では、Docker を Azure 用 Java Development Kit (JDK) と共に使用する方法について説明します。 Java 7、8、11 用のビルド済み Docker イメージは [Docker Hub](https://hub.docker.com/_/microsoft-java-se) から利用できます。

複数のベース OS イメージ上の Zulu JDK、JRE、および JRE ヘッドレス用の認定済み Docker コンテナー イメージを Docker Hub で利用できます。

* [JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [JRE](https://hub.docker.com/_/microsoft-java-jre)
* [JRE-headless](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a>Docker イメージの実行

次の例に示すように、Docker イメージは構文 `$ docker run mcr.microsoft.com/java/jdk:tag java` を使用して実行できます。

```cli
docker run mcr.microsoft.com/java/jdk:8-zulu-alpine java -version
```

## <a name="creating-a-docker-image"></a>Docker イメージの作成

次の例に示すように、Microsoft の公式 Docker Hub イメージを使用してイメージを作成できます。

### <a name="create-a-docker-file"></a>Docker ファイルを作成する

```cli
FROM mcr.microsoft.com/java/jdk:8-zulu-alpine
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a>Docker イメージをビルドする

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a>新しいイメージを実行する

```cli
docker run hello-world
```

次の出力が表示されます。

```output
Hello World - From Microsoft Azure !!!
```
