---
title: Azure SDK アプリケーションの開発用に Node.js をインストールする
description: Azure での一般的な開発シナリオ用に Node.js をインストールする方法について説明します。
ms.topic: how-to
ms.date: 12/04/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5fdaca001f82afca942a22ec9ad971f4af18c8b8
ms.sourcegitcommit: 525c4b41d85aae9c3026a070b07e00c2241ea716
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2020
ms.locfileid: "97394038"
---
# <a name="install-and-manage-nodejs-for-common-azure-development-scenarios"></a>Azure の一般的な開発シナリオ用に Node.js をインストールして管理する

Azure 開発用に Node.js をインストールするときは、ローカルの開発環境と、デプロイ先に予定しているホスティング環境の両方を考慮する必要があります。 Azure により、Windows と Linux の両方で、長期サポート (LTS) バージョンでの Node.js のホスティングが提供されています。 

## <a name="minimum-version-of-nodejs-8"></a>最小バージョンは Node.js 8 以降

Azure SDK でサポートされる最小バージョンは、Node.js 8 です。 

## <a name="download-and-install-nodejs-based-on-your-intended-use"></a>使用目的に応じて Node.js をダウンロードしてインストールする

要件に基づいて、Node.js をダウンロードしてインストールできます。
 
* [Node.js ダウンロード ページ](https://nodejs.org/en/download/) 
* [公式の Docker イメージ](https://hub.docker.com/_/node/)

## <a name="managing-versions-of-nodejs"></a>Node.js のバージョンの管理

Azure 開発用に複数のバージョンの Node.js を管理する必要がある場合は、nvm を使用します。

* OSX、*nix - [nvm](https://github.com/creationix/nvm)
* Windows - [nvm for Windows](https://github.com/marcelklehr/nodist)

## <a name="next-steps"></a>次の手順

* Azure SDK を使用できるように[ローカル開発環境を構成する](configure-local-development-environment.md)
