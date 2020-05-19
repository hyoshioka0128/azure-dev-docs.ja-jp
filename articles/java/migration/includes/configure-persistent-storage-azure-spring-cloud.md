---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 6fdf908458eec428f4e01d1b5f20fcbf4e039dbe
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990224"
---
### <a name="configure-persistent-storage"></a>永続ストレージを構成する

アプリケーションのいずれかの部分でローカル ファイル システムに対する読み取りまたは書き込みが行われる場合は、永続ストレージを構成してローカル ファイル システムを置き換える必要があります。 詳細については、「[Azure Spring Cloud で永続ストレージを使用する](/azure/spring-cloud/spring-cloud-howto-persistent-storage)」を参照してください。

一時ファイルは、`/tmp` ディレクトリに書き込む必要があります。 OS 非依存の場合は、`System.getProperty("java.io.tmpdir")` を使用してこのディレクトリを取得できます。 また、[`java.nio.Files::createTempFile`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/nio/file/Files.html#createTempFile(java.lang.String,java.lang.String,java.nio.file.attribute.FileAttribute...)) を使用して、一時ファイルを作成することもできます。
