---
title: Web アプリのホスティング - 構成設定
description: Web アプリの一般的な構成を設定する方法について説明します。
ms.topic: conceptual
ms.date: 12/08/2020
ms.custom: devx-track-js
ms.openlocfilehash: 271c2b916062d9cbd2b905fb937c9fb216eb43e5
ms.sourcegitcommit: 1901759f41adfac3c3f2ff135bcf72206543b639
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2020
ms.locfileid: "96934296"
---
# <a name="hosting-web-apps-on-azure"></a>Azure での Web アプリのホスティング

Web アプリの一般的な構成を設定する方法について説明します。 共通の設定がない場合は、フィードバックで問題を提出し、それについてお知らせください。 

リソースを作成するときに、すべての **必須設定** が要求されます。 設定がその時点で要求されていない場合は、既定値が存在します。これは、リソースの作成後に変更できます。 

## <a name="what-is-a-web-app"></a>Web アプリとは

Web アプリとは、インターネット URL を使用して到達できるあらゆるものです。 Web アプリと見なすことができる Azure サービスは多数あります。 Web アプリで一般的に使用される上位のサービスは次のとおりです。

* App Service。これには以下も含まれます。
    * [静的 Web アプリ](/azure/static-web-apps/)
    * [関数](/azure/azure-functions/)
    * [Web アプリ](/azure/app-service/)
    * [Containers](/azure/app-service/configure-custom-container?pivots=container-linux)
* Containers - [Kubernetes](/azure/aks/) および単一の[コンテナー](/azure/container-instances/)
* Virtual Machines - [Windows](/azure/virtual-machines/windows) および [Linux](/azure/virtual-machines/linux)

## <a name="how-to-configure-web-app-settings"></a>Web アプリの設定を構成する方法

ほとんどの Azure サービスには、設定を構成する 4 つの方法があります。

* [Azure Portal](https://portal.azure.com)
* サービス用の [Azure SDK](https://github.com/Azure/azure-sdk) (通常は管理として示される)
* [Azure CLI](/cli/azure/)
* [Azure PowerShell](/powershell/azure/)

多くの設定は、[拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)を使用して Visual Studio Code 内で構成することもできます。 

## <a name="use-default-domain-name-provided-by-azure"></a>Azure によって提供される既定のドメイン名を使用する

ほとんどの Azure サービスでは、リソースの URL が提供されます。 サービス名によって、サブドメインが決定され、ドメインの残りの部分は Azure から取得されます。 

次に例を示します。

* Azure Functions - `https://my-function-app.azurewebsites.net`
* Azure Web アプリ - `https://my-web-app.azurewebsites.net`
* Azure Storage Blob - `https://mystorage.blob.core.windows.net/`

Static Web Apps などの一部のサービスでは、比較的一意なサブドメインが提供されるため、運用環境ですぐに使用することができます。

* Azure Static Web Apps = `https://gentle-tree-0b08aaf12.azurestaticapps.net`

## <a name="configure-custom-domain-name"></a>カスタム ドメイン名を構成する 

各サービスには、カスタム ドメインを追加するための独自のメカニズムが用意されています。 

* [Azure Static Web Apps](/azure/static-web-apps/custom-domain)
* [Azure Functions](/azure/app-service/app-service-web-tutorial-custom-domain) & [Azure Web アプリ](/azure/app-service/app-service-web-tutorial-custom-domain) - 機能はWeb アプリ上に構築されているため、同じメカニズムが使用されます。
* [Azure Storage Blobs](/azure/storage/blobs/storage-custom-domain-name?tabs=azure-portal)

## <a name="configure-port-forwarding"></a>ポート転送の構成

アプリのポート番号が既定のポート `8080` でない場合は、[それをマップする](/azure/app-service/configure-language-nodejs?pivots=platform-windows#get-port-number)必要があります。 これにより、App Service は正しいポートに要求を転送できます。 

## <a name="configure-certificates"></a>証明書の構成

アプリで証明書がすぐに必要な場合は、[証明書の提供](/azure/app-service/configure-ssl-certificate#import-an-app-service-certificate)方法に関していくつかの選択肢があります。

* 独自の証明書をアップロードする
* App Service 内で証明書を管理する
* Azure Key Vault から証明書をインポートする
* [コード内](/azure/app-service/configure-ssl-certificate-in-code)

## <a name="configure-secrets"></a>シークレットの構成

シークレットは通常、次の方法で提供されます。

* Azure Key Vault - このサービスのリソースを作成します。これにより[アプリ シークレット](/azure/app-service/app-service-key-vault-references)が提供されます。 
* アプリの設定 - 軽量のソリューションを必要としている場合は、アプリ設定としてシークレットを提供し、一般的な `process.env.VARNAME` を使用して参照できます。 

## <a name="configure-logging"></a>ログの構成

ログには次が含まれます。

* プラットフォームのログ - アプリの外部で起こっていること
* アプリのログ - アプリの内部で起こっていること

プラットフォームのログは、以下のために提供されます。
* 環境の正常性を把握する。
* 異なる価格レベルへ、またはリージョン間でスケーリングする。 

アプリケーション ログは自動的には提供されません。 内部のアプリの動作について独自のログを追加できます。
* [Azure Monitor](/azure/azure-monitor/overview) では、ログとログがアーカイブされるストレージ リソースを提供するために [Application Insights](/azure/azure-monitor/app/app-insights-overview) 用の npm ライブラリが提供されています。 

## <a name="configure-database-and-storage"></a>データベースとストレージを構成する

通常、データベースまたはデータ ストレージへの接続は、接続文字列を使用して開始されます。 

データ接続に関する考慮事項:
* 現在の接続を取得する
* 新しいデータ ストレージ - アプリで新しいストレージ メカニズムが必要な場合、Azure には[さまざまなデータベース](integrate-database.md)の選択肢が用意されています。 接続は安全に保管する必要があります。 

## <a name="missing-something"></a>何か足りないものがありますか? 

この一覧に含まれていない項目がある場合は、フィードバックを入力してお知らせください。 

## <a name="next-steps"></a>次のステップ

* [エンドツーエンドの Node.js アプリ](/azure/developer/javascript/how-to/develop-nodejs-on-azure)開発フローで、これらの手順の多くを確認してください。 