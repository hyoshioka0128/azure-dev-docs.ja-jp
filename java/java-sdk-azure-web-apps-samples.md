---
title: Azure Management Libraries for Java Web アプリ サンプル
description: Azure Management Libraries for Java を使用して、App Service でホストされる Azure Web アプリの作成と更新を行うサンプル コードを入手しましょう。
keywords: Azure, Java, SDK, API, Maven, Gradle, Web アプリ, App Service
author: rloutlaw
ms.author: brendm
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.devlang: java
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.custom: seo-java-august2019
ms.openlocfilehash: 1a21ca6e11cf66a603509bd5414d1b5caf9e8a30
ms.sourcegitcommit: f519a1ee8017850b2fa37049af3bac1ea5ca5516
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889854"
---
# <a name="azure-management-libraries-for-java---web-app-samples"></a>Azure Management Libraries for Java - Web アプリ サンプル 

| **アプリの作成** ||
|---|---|
| [Web アプリを作成して FTP または GitHub からデプロイする][1] | ローカル Git、FTP、GitHub の継続的インテグレーションから Web アプリをデプロイします。 |
| [Web アプリを作成してデプロイ スロットを管理する][2] | Web アプリを作成してステージング スロットにデプロイした後、スロット間でデプロイをスワップします。 |
| **アプリケーションの構成** ||
| [Web アプリを作成してカスタム ドメインを構成する][3] | カスタム ドメインと自己署名 SSL 証明書を使った Web アプリを作成します。 |
| **アプリのスケール** ||
| [複数のリージョンにわたる高可用性によって Web アプリをスケーリングする][4] | 3 つの異なる地理的リージョンに Web アプリをスケーリングし、Azure Traffic Manager を使用して、1 つのエンドポイントを介して利用できるようにします。 | 
| **アプリのリソースへの接続** ||
| [Web アプリをストレージ アカウントに接続する][5] | Azure ストレージ アカウントを作成し、そのストレージ アカウントの接続文字列をアプリケーション設定に追加します。 |
| [Web アプリを SQL データベースに接続する][6] | Web アプリと SQL データベースを作成し、SQL データベースの接続文字列をアプリケーション設定に追加します。 |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://azure.microsoft.com/resources/samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://azure.microsoft.com/resources/samples/app-service-java-manage-data-connections-for-web-apps/