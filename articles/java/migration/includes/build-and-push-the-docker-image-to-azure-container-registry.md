---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 59e277b0ae4a05566e2dbcd0d7a0a4a15c22eed9
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81672988"
---
### <a name="build-and-push-the-docker-image-to-azure-container-registry"></a>Docker イメージをビルドし、Azure Container Registry にプッシュする

Dockerfile を作成したら、Docker イメージをビルドし、自身の Azure コンテナー レジストリに発行する必要があります。

[WildFly コンテナーのクイック スタート GitHub リポジトリ](https://github.com/Azure/wildfly-container-quickstart)を使用した場合、イメージをビルドして自身の Azure コンテナー レジストリにプッシュするプロセスは、次の 3 つのコマンドを呼び出すことと同等です。

これらの例では、`MY_ACR` 環境変数は自身の Azure コンテナー レジストリの名前を保持し、`MY_APP_NAME` 変数は、Azure コンテナー レジストリで使用する Web アプリケーションの名前を保持します。

WAR ファイルをビルドします。

```shell
mvn package
```

自身の Azure コンテナー レジストリにログインします。

```shell
az acr login -n ${MY_ACR}
```

イメージをビルドしてプッシュします。

```shell
az acr build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME} -f src/main/docker/Dockerfile .
```

また、次のコマンドに示すように、Docker CLI を使用して最初にイメージをビルドしてローカルでテストすることもできます。 この方法を使用すると、ACR への初期デプロイの前に、イメージのテストと調整を簡単に行えます。 ただし、Docker CLI をインストールし、Docker デーモンが実行されていることを確認する必要があります。

イメージをビルドします。

```shell
docker build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

イメージをローカルで実行します。

```shell
docker run -it -p 8080:8080 ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

[http://localhost:8080](http://localhost:8080) でアプリケーショにアクセスできるようになりました。

自身の Azure コンテナー レジストリにログインします。

```shell
az acr login -n ${MY_ACR}
```

自身の Azure コンテナー レジストリにイメージをプッシュします。

```shell
docker push ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

Azure 内でコンテナー イメージをビルドして保存する方法の詳細については、「[Azure Container Registry を使用してコンテナー イメージをビルドして保存する](/learn/modules/build-and-store-container-images/)」を参照してください。
