---
title: Azure に Node.js Web アプリをデプロイする
description: Azure App Service をはじめとする、Web アプリのホスティング方法について説明します。プログレッシブ Web アプリ (PWA) についても取り上げます。
ms.topic: article
ms.date: 08/20/2019
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: e7cb374b645738140ead924296ef7cef9b1b61d7
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2020
ms.locfileid: "85792342"
---
# <a name="deploy-nodejs-web-apps-to-azure-app-service"></a>Azure App Service に Node.js Web アプリをデプロイする

Web アプリのデプロイとホスティングに関して、Azure にはいくつかの方法が用意されています。

- Web アプリの最適なホスティング方法は、PaaS (サービスとしてのプラットフォーム) プランである Azure App Service です。 その足がかりとして、次のリソースが参考になります。

  - [Azure で Node.js Web アプリを作成する](/azure/app-service/app-service-web-get-started-nodejs)
  - [Azure App Service を試す - テンプレートから Express アプリを作成する](https://code.visualstudio.com/tryappservice/?utm_source=msftdocs&utm_medium=microsoft&utm_campaign=tryappservice)
  - [Azure App Service で Web アプリをホストする - Learn モジュール](/learn/modules/host-a-web-app-with-azure-app-service/index)
  - [Azure で Node.js と MongoDB アプリを構築する](/azure/app-service/app-service-web-tutorial-nodejs-mongodb-app)
  - [App Service サンプル](/samples/browse/?languages=javascript%2Cnodejs&products=azure-app-service)

- Azure Container Registry と Azure Kubernetes Service を使用すれば、独自のコンテナーを作成して Azure にデプロイすることができます。 詳細については、「[Node.js コンテナーを Azure にデプロイする方法](node-howto-deploy-containers.md)」を参照してください。

- 主にサーバーレス コードを扱う場合は、「[Azure でサーバーレス Node.js コードを作成する方法](node-howto-write-serverless-code.md)」を参照してください。

- JAMstack (静的) サイトの作成について詳しくは、「[Azure で JAMstack (静的サイト) Web アプリを作成する方法](node-howto-create-static-site-jamstack.md)」を参照してください。

- インフラストラクチャを制御したい場合は、どうしても仮想マシンを利用することになるでしょう。 まず、Microsoft Learn の「[Azure Virtual Machines を使用して Web サイトをデプロイする](/learn/paths/deploy-a-website-with-azure-virtual-machines/)」というラーニング パスに従ってください。

さまざまなホスティング方法を包括的に把握するには、「[Azure コンピューティング サービスのデシジョン ツリー](/azure/architecture/guide/technology-choices/compute-decision-tree)」のほか、Microsoft Learn の「[コア Cloud Services - Azure コンピューティング オプション](/learn/modules/intro-to-azure-compute/)」モジュールを参照してください。
