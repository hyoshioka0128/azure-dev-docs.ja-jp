---
title: チュートリアル:Visual Studio Code を使用して変更を加えた後に Azure App Service にコンテナーを再デプロイする
description: チュートリアルの手順 3、コンテナー イメージをリビルドして再デプロイするための簡単な手順。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: a8ce5de518ef4eeec2140ec82ef560f3b487783e
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278695"
---
# <a name="tutorial-redeploy-a-container-to-azure-app-service-after-making-changes"></a>チュートリアル:変更を加えた後に Azure App Service にコンテナーを再デプロイする

[前の手順: Azure にイメージをデプロイする](tutorial-deploy-containers-02.md)

この記事では、Visual Studio Code を使用して変更を加えた後に Azure App Service にコンテナーを再デプロイする方法について説明します。

アプリには必然的に変更を加えるため、最終的にコンテナーのリビルドと再デプロイは何回も実施することになります。 さいわい、このプロセスは単純です。

1. アプリケーションに変更を加えて、ローカルでテストします (この手順と後続の 2 つの手順については、[VS Code での Python コンテナーの作成](https://code.visualstudio.com/docs/python/tutorial-create-container)に関するチュートリアルで説明しています)。

1. Docker イメージをリビルドします。 アプリのコードのみを変更する場合は、ビルドにかかる時間はわずか数秒です。

1. レジストリにイメージをプッシュします。 ここでも、アプリのコードのみを変更する場合は、この小さいレイヤーだけをプッシュすればよいので、通常このプロセスは数秒で完了します。

1. **[Azure: App Service]** エクスプローラーで、適切な App Service を右クリックし、 **[再起動]** を選択します。 App Service を再起動すると、最新のコンテナー イメージがレジストリから自動的にプルされます。

1. 約 15 から 20 秒後に、App Service の URL にもう一度アクセスして、更新プログラムを確認します。

> [!div class="nextstepaction"]
> [変更を加えて再デプロイしました](tutorial-deploy-containers-04.md)

[問題が発生しました](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)
