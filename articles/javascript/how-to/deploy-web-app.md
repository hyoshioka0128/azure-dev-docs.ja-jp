---
title: Azure に Node.js Web アプリをデプロイする
description: Azure App Service をはじめとする、Web アプリのホスティング方法について説明します。プログレッシブ Web アプリ (PWA) についても取り上げます。
ms.topic: how-to
ms.date: 08/20/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-js
ms.openlocfilehash: ee2d9a58baa306309df28f1a4bf6515481fccf3e
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2020
ms.locfileid: "92437272"
---
# <a name="deploy-nodejs-web-apps-to-azure-app-service"></a>Azure App Service に Node.js Web アプリをデプロイする

Web アプリのデプロイとホスティングに関して、Azure にはいくつかの方法が用意されています。

- Web アプリの最適なホスティング方法は、PaaS (サービスとしてのプラットフォーム) プランである Azure App Service です。 その足がかりとして、次のリソースが参考になります。

  - [Azure で Node.js Web アプリを作成する](/azure/app-service/app-service-web-get-started-nodejs)
  - [Azure App Service を試す - テンプレートから Express アプリを作成する](https://code.visualstudio.com/tryappservice/?utm_source=msftdocs&utm_medium=microsoft&utm_campaign=tryappservice)
  - [Azure App Service で Web アプリをホストする - Learn モジュール](/learn/modules/host-a-web-app-with-azure-app-service/index)
  - [Azure で Node.js と MongoDB のアプリを構築する](/azure/app-service/app-service-web-tutorial-nodejs-mongodb-app)
  - [App Service サンプル](/samples/browse/?languages=javascript%2Cnodejs&products=azure-app-service)

- Azure Container Registry と Azure Kubernetes Service を使用すれば、独自のコンテナーを作成して Azure にデプロイすることができます。 詳細については、「[Node.js コンテナーを Azure にデプロイする方法](deploy-containers.md)」を参照してください。

- 主にサーバーレス コードを扱う場合は、「[Azure でサーバーレス Node.js コードを作成する方法](develop-serverless-apps.md)」を参照してください。

- JAMstack (静的) サイトの作成について詳しくは、「[Azure で JAMstack (静的サイト) Web アプリを作成する方法](create-static-site.md)」を参照してください。

- インフラストラクチャを制御したい場合は、どうしても仮想マシンを利用することになるでしょう。 まず、Microsoft Learn の「[Azure Virtual Machines を使用して Web サイトをデプロイする](/learn/paths/deploy-a-website-with-azure-virtual-machines/)」というラーニング パスに従ってください。

さまざまなホスティング方法を包括的に把握するには、「[Azure コンピューティング サービスのデシジョン ツリー](/azure/architecture/guide/technology-choices/compute-decision-tree)」のほか、Microsoft Learn の「[コア Cloud Services - Azure コンピューティング オプション](/learn/modules/intro-to-azure-compute/)」モジュールを参照してください。
