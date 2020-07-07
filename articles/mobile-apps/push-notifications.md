---
title: Visual Studio App Center と Azure Notification Hubs によるモバイル アプリのプッシュ通知の重要性
description: モバイル アプリケーション ユーザーとの連携に使用できる Visual Studio App Center などのサービスについて説明します。
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 6dab56319c04fbf7864094fbaf254621cac82b36
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632444"
---
# <a name="engage-with-your-application-users-by-sending-push-notifications"></a>プッシュ通知の送信でアプリケーション ユーザーとつながる

消費者向けでも法人向けでも、アプリケーションを開発するなら、ユーザーにはそのアプリケーションを積極的に利用して欲しいものです。 ニュース速報、ゲームや製品のアップデート、うれしい特価セールなど、他のあらゆる関連情報をユーザーと共有したい状況がしばしばあります。 ユーザーとのつながりを増やし、アプリケーションを繰り返し利用してもらうには、リアルタイムでユーザーに情報を伝える方法が必要になります。

プッシュ通知は、アプリケーション ユーザーに情報を伝える速くて効率的な手法です。 ユーザーがモバイル デバイスでそのとき何をしているのかに関係なく、通常、ポップアップ項目やダイアログ ボックスで、時機を逸せずにユーザーが望む情報を届けることができます。

## <a name="value-of-push-notifications"></a>プッシュ通知の価値

プッシュ通知により、ユーザーと企業に価値が提供されます。

企業は次のことができます。

- サポートしているプラットフォームに関するメッセージを、時機を逸せずにユーザーに直接伝えます。
- アップデートやリマインダーをユーザーにリアルタイムで送信し、アプリケーションを定期的に使用するように奨励することで、ユーザー エンゲージメントを高めます。
- ユーザーを維持し、アプリケーションをダウンロードしたが使用していない非アクティブ ユーザーには再び積極的に使用してもらいます。
- ユーザーの行動を分析し、セグメントに分け、モバイル プッシュ通知を基盤とするキャンペーンを活用することで転換率を上げます。
- プッシュ メッセージやタイミングの良いアップデートをユーザーに送信することで、製品やサービスを宣伝します。
- 個々のユーザーに合わせたプッシュ メッセージを送信することで、ユーザーをターゲットに定めます。

アプリケーション ユーザーの場合のプッシュ通知:

- リアルタイムで価値や情報を届けます。
- ユーザーにリマインダーを送信し、アプリケーションを使用してもらいます。

次のサービスを使用し、モバイル アプリでプッシュ通知を有効にします。

## <a name="azure-notification-hubs"></a>Azure 通知ハブ

[Notification Hubs](/azure/notification-hubs/notification-hubs-push-notification-overview) では、スケールアウトされた使いやすいプッシュ エンジンが提供されています。 このエンジンを使用して、クラウドまたはオンプレミスで、あらゆるプラットフォームからあらゆるバックエンドに通知を送信できます。

### <a name="azure-notification-hub-features"></a>Azure Notification Hub の機能

- クラウドまたはオンプレミスで、あらゆるプラットフォームからあらゆるバックエンドにプッシュ通知を送信します。
- 1 回の API 呼び出しで、数百万単位のモバイル デバイスにプッシュを高速配信します。
- 顧客、言語、場所に合わせてプッシュ通知を作成します。
- 顧客セグメントを動的に定義し、それに通知します。
- 数百万単位のモバイル デバイスに合わせて一瞬でスケーリングします。
- iOS、Android、Windows、Kindle、および Baidu のプラットフォーム サポートを利用できます。

### <a name="azure-notification-hub-references"></a>Azure Notification Hub の参考資料

- [Azure Portal](https://portal.azure.com) 
- [Azure Notification Hubs の使用](/azure/notification-hubs/)
- [クイック スタート](/azure/notification-hubs/create-notification-hub-portal)
- [サンプル](/azure/notification-hubs/samples)
