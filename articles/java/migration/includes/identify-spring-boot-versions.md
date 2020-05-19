---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 34701a7d3fd85fea412be859dc21823df2dde053
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990194"
---
#### <a name="identify-spring-boot-versions"></a>Spring Boot のバージョンを特定する

移行する各アプリケーションの依存関係を調べて、Spring Boot のバージョンを確認します。

##### <a name="maven"></a>Maven

Maven プロジェクトでは、Spring Boot バージョンは通常、POM ファイルの `<parent>` 要素にあります。

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

##### <a name="gradle"></a>Gradle

Gradle プロジェクトでは、Spring Boot バージョンは通常、`org.springframework.boot` プラグインのバージョンとして `plugins` セクションにあります。

```gradle
plugins {
  id 'org.springframework.boot' version '2.2.6.RELEASE'
  id 'io.spring.dependency-management' version '1.0.9.RELEASE'
  id 'java'
}
```
