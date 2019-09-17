---
title: Azure Java 開発用の JDK で Docker イメージを使用する
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 04/09/2019
ms.devlang: java
ms.topic: conceptual
ms.service: azure
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 618766a6aafbafea3036b9c4abff51bd069e2786
ms.sourcegitcommit: cf5881235569c754c829a8f819642a6f5aed30be
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2019
ms.locfileid: "70904881"
---
# <a name="use-docker-with-a-java-development-kit-jdk-for-azure"></a>Docker を Azure 用 Java Development Kit (JDK) と共に使用する 

この記事では、Docker を Azure 用 Java Development Kit (JDK) と共に使用する方法について説明します。 Java 7、8、11 用のビルド済み Docker イメージは [Docker Hub](https://hub.docker.com/_/microsoft-java-se) から利用できます。

複数のベース OS イメージ上の Zulu JDK、JRE、および JRE ヘッドレス用の認定済み Docker コンテナー イメージを Docker Hub で利用できます。

* [JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [JRE](https://hub.docker.com/_/microsoft-java-jre)
* [JRE ヘッドレス](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a>Docker イメージの実行

次の例に示すように、Docker イメージは構文 `$ docker run mcr.microsoft.com/java/jdk:tag java` を使用して実行できます。

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a>Docker イメージの作成

次の例に示すように、Microsoft の公式 Docker Hub イメージを使用してイメージを作成できます。

### <a name="create-a-docker-file"></a>Docker ファイルを作成する

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
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
