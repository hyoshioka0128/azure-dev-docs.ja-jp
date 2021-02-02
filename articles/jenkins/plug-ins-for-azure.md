---
title: Azure 用の Jenkins プラグイン
description: Azure で使用できる Jenkins プラグインについて説明します
keywords: jenkins, plugis, azure, devops
ms.date: 04/25/2020
ms.topic: article
ms.custom: devx-track-jenkins
ms.openlocfilehash: 3051492f4278c2b5702fdc20932fa47928dea7a5
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699850"
---
# <a name="jenkins-plug-ins-for-azure"></a>Azure 用の Jenkins プラグイン

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

Azure の多くのサービスと機能に、Jenkins プラグインを介してアクセスできます。これらのサービスでは、DevOps 環境の継続的インテグレーションと継続的配置 (CI/CD) に関する一連の可能性がサポートされます。 任意の Jenkins プラグインをクリックして、そのプラグインの詳細を確認してください。

| Jenkins プラグイン | 説明                                   |
|------------------------------------------------------------------------------|
| [Azure App Service プラグイン](https://plugins.jenkins.io/azure-app-service)     | Azure アプリ サービスをデプロイするための Jenkins プラグイン (現在は Web アプリのみをサポート)。 |
| [Azure AD プラグイン](https://plugins.jenkins.io/azure-ad)                       | Azure Active Directory を使用して認証と認可をサポートする Jenkins プラグイン。 |
| [Azure Artifact Manager プラグイン](https://plugins.jenkins.io/azure-artifact-manager) | Azure Artifact Manager プラグインは、成果物を Azure BLOB ストレージに格納するために使用できる成果物マネージャーです。 Azure Artifact Manager プラグインは、既定の成果物マネージャーと同様に、Jenkins やユーザー ジョブに対して透過的に動作します。 |
| [Azure コンテナー エージェント プラグイン](https://plugins.jenkins.io/azure-container-agents) | Azure Container Agents プラグインは、Jenkins でコンテナーをエージェントとして実行するために役立ちます。 |
| [Azure Container Registry タスク プラグイン](https://plugins.jenkins.io/azure-container-registry-tasks)       | docker-build 要求を [Azure コンテナー レジストリ](/azure/container-registry/container-registry-tasks-overview)に送信するための Jenkins プラグイン。 |
| [Azure Container Service プラグイン](https://plugins.jenkins.io/azure-acs)       | 構成を Azure Container Service (AKS) にデプロイするための Jenkins プラグイン。 |
| [Azure 資格情報プラグイン](https://plugins.jenkins.io/azure-credentials)      | Azure 資格情報を管理するための Jenkins プラグイン。 |
| [Azure 関数プラグイン](https://plugins.jenkins.io/azure-function)           | Azure 関数をデプロイするための Jenkins プラグイン。 |
| [Azure IoT Edge プラグイン](https://plugins.jenkins.io/azure-iot-edge)           | Azure IoT Edge プラグインを使用すると、Jenkins で IoT Edge ソリューションを開発するための CI/CD パイプラインを簡単に設定できます。 |
| [Azure Service Fabric プラグイン](https://plugins.jenkins.io/service-fabric)     | Linux Azure Service Fabric プロジェクト用の Jenkins プラグイン。 |
| [Azure Storage プラグイン](https://plugins.jenkins.io/windows-azure-storage)     | Microsoft Azure BLOB ストレージとの間でビルド成果物のアップロードかビルド依存関係のダウンロードを行うためのプラグイン。 |
| [Azure VM エージェント プラグイン](https://plugins.jenkins.io/azure-vm-agents)         | (Azure Resource Manager テンプレートを使用して) Azure 仮想マシンに Jenkins エージェントを作成するための Jenkins プラグイン。 |
| [Azure 仮想マシン スケール セット プラグイン](https://plugins.jenkins.io/azure-vmss)           | VM イメージを Azure 仮想マシン スケール セットにデプロイするための Jenkins プラグイン。 |