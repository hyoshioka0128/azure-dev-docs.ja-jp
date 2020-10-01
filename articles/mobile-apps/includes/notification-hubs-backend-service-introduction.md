---
author: mikeparker104
ms.author: miparker
ms.date: 07/27/2020
ms.service: mobile-services
ms.topic: include
ms.openlocfilehash: 98e39efca4ddaacb0e46bf5e42bd2df496529beb
ms.sourcegitcommit: e97cb81a245ce7dcabeac3260abc3db7c30edd79
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91493159"
---
[ASP.NET Core Web API](https://dotnet.microsoft.com/apps/aspnet/apis) バックエンドを使用して、最新かつ最適な[インストール](/azure/notification-hubs/notification-hubs-push-notification-registration-management#installations)方法を使用しているクライアントの[デバイス登録](/azure/notification-hubs/notification-hubs-push-notification-registration-management#what-is-device-registration)を処理します。 また、サービスは、クロスプラットフォーム方式でプッシュ通知を送信します。 

これらの操作は[バックエンド操作用の Notification Hubs SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) を使用して処理されます。 全体的なアプローチの詳細については、[アプリ バックエンドからの登録](/azure/notification-hubs/notification-hubs-push-notification-registration-management#registration-management-from-a-backend)に関するドキュメントを参照してください。