---
ms.custom: devx-track-js
ms.topic: include
ms.date: 09/30/2020
ms.openlocfilehash: 527ff2264481a134955b87dec8ebae8be72090c8
ms.sourcegitcommit: c3a1c9051b89870f6bfdb3176463564963b97ba4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2020
ms.locfileid: "92438067"
---
## <a name="types-of-sdk-client-libraries"></a>SDK クライアント ライブラリの種類

リソース管理を支援する Azure サービスの管理パッケージには、2 つのセットがあります。

|特徴量|Azure SDK for JavaScript<br>(推奨、最新)|Azure SDK for Node.js<br>(古い)|
|--|--|--|
|アクティブな開発|✔️|❌|
|Node.js での使用|✔️|✔️|
|ブラウザーでの使用|✔️|❌|
|JavaScript での使用|✔️|✔️|
|TypeScript での使用<br>(.d.ts を含む)|✔️|❌|
|認証ライブラリ<br>[サンプル](../core/node-sdk-azure-authenticate.md)|ブラウザー以外: [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth)<br>ブラウザー: [@azure/ms-rest-browserauth](https://www.npmjs.com/package/@azure/ms-rest-browserauth)|ブラウザー以外: : [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure)|
|GitHub リポジトリ|[Azure/azure-sdk-for-js](https://github.com/Azure/azure-sdk-for-js)|[Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node)|
