---
title: Azure portal でログを表示する
description: Azure Monitor と Application Insights を使用してログを確認する方法について説明します。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: fd0741cd58e8d2e882a352035cd3c4f9bca956d1
ms.sourcegitcommit: a2a51e0c6530eb5794a2fe667cf4c9a60b2a7470
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625071"
---
# <a name="6-view-logs-in-azure-portal"></a>6.Azure portal でログを表示する

チュートリアルのこのセクションでは、Azure Monitor と Application Insights を使用してログを確認する方法について説明します。 

## <a name="count-of-traces-in-logs-with-azure-cli"></a>Azure CLI を使用してログのトレースをカウントする

Azure CLI を使用すると、ログの重要な部分をすばやく確認できます。 たとえば、次のコマンドを使用すると、ログに記録されているトレースの数を確認できます。 

トレースは `/trace` ルートにのみ追加されていることに注意してください。 Web アプリのルートを呼び出しても、トレース ログは生成されません。 

```azurecli
az monitor app-insights metrics show \
    --resource-group rg-demo-vm-eastus \
    --app demoWebAppMonitor \
    --metric traces/count
```

応答は次のようになります。この例では、合計で 2 つのトレースがあります。 

```console
{
  "value": {
    "end": "2020-11-11T21:33:40.311000+00:00",
    "interval": null,
    "segments": null,
    "start": "2020-11-11T20:33:40.311000+00:00",
    "traces/count": {
      "sum": 2
    }
  }
}
```

## <a name="view-application-traces-in-azure-portal"></a>Azure portal でアプリケーション トレースを表示する

トレースを一覧として表示する場合、最も簡単な方法は Azure portal を使用することです。 

1. Web ブラウザーで [Azure portal](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAll) を開きます。
1. リソース グループ `rg-demo-vm-eastus` によってリソースの一覧をフィルター処理します。 
1. `demoWebAppMonitor` リソースを選択します。 
1. **[監視]** セクションの **[ログ]** 項目を選択します。 ポップアップで選択可能なクエリが表示された場合は、隅の **[X]** をクリックしてポップアップを無視します。
1. **[traces]** という名前の **Application Insights** 項目をダブルクリックして選択します。 これにより、クエリ ウィンドウにその名前が追加されます。 
1. **[実行]** ボタンを選択してクエリを実行します。
1. Web アプリからの Azure Monitor の Application Insights カスタム トレースが一覧に表示されます。

    :::image type="content" source="../../media/tutorial-vm/azure-portal-application-insights-custom-trace.png" alt-text="Web アプリからの Azure Monitor の Application Insights カスタム トレースが一覧に表示されます。":::

## <a name="next-step"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure リソースをクリーンアップする](clean-up-resources.md) 