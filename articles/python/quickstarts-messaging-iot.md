---
title: Azure でのメッセージングと IoT for Python アプリの概要
description: メッセージングと IoT for Python アプリに関する Azure ドキュメント内の概要資料のインデックス。
ms.date: 05/28/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: b9c3746d5f6dab9cca74d9493e0e68380018db19
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87983014"
---
# <a name="messaging-and-iot-for-python-apps-on-azure"></a>Azure でのメッセージングと IoT for Python アプリ

次の記事は、Azure でさまざまなメッセージング オプションの使用を開始する際に役立ちます。

## <a name="messaging"></a>メッセージング

- **Notifications**:
  - [Python で Notification Hubs を使用する方法](/azure/notification-hubs/notification-hubs-python-push-notification-tutorial)

- **キュー**:
  - [Python から Azure Queue storage v2.1 を使用する方法](/azure/storage/queues/storage-python-how-to-use-queue-storage)
  - [Python 用 Azure Queue storage クライアント ライブラリ v12](/azure/storage/queues/storage-quickstart-queues-python)
  - [Python で Azure Service Bus キューを使用する](/azure/service-bus-messaging/service-bus-python-how-to-use-queues)
  - [Python で Service Bus のトピックとサブスクリプションを使用する](/azure/service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions)

- **リアルタイム Web 機能 (SignalR)** :
  - [Python を使用した Azure Functions と SignalR Service によるチャット ルームの作成](/azure/azure-signalr/signalr-quickstart-azure-functions-python)

## <a name="event-ingestion"></a>イベントの取り込み

- **イベントの取り込み**:
  - [Python を使用して Event Hubs でリアルタイム データを取り込む](/azure/event-hubs/event-hubs-python)
  - [Event Hubs データを Azure Storage にキャプチャし、Python を使用してそれを読み取る](/azure/event-hubs/get-started-capture-python-v2)
  - [Azure CLI と Event Grid を使ったカスタム イベントの Web エンドポイントへのルーティング](/azure/event-grid/custom-event-quickstart)

## <a name="internet-of-things-iot"></a>モノのインターネット(IoT)

- **IoT Hub**:
  - [デバイスから IoT ハブに利用統計情報を送信してバックエンド アプリケーションで読み取る](/azure/iot-hub/quickstart-send-telemetry-python)
  - [IoT Hub を使用したクラウドからデバイスへのメッセージの送信](/azure/iot-hub/iot-hub-python-python-c2d)
  - [Upload files from your device to the cloud with IoT Hub](/azure/iot-hub/iot-hub-python-python-file-upload) (IoT Hub を使用してデバイスからクラウドにファイルをアップロードする)
  - [ジョブのスケジュールとブロードキャスト](/azure/iot-hub/iot-hub-python-python-schedule-jobs)
  - [IoT ハブに接続されたデバイスを制御する](/azure/iot-hub/quickstart-control-device-python)

- **デバイス プロビジョニング**:
  - [シミュレートされた TPM デバイスを作成してプロビジョニングする](/azure/iot-dps/quick-create-simulated-device-tpm-python)
  - [TPM デバイスを IoT Hub Device Provisioning Service に登録する](/azure/iot-dps/quick-enroll-device-tpm-python)
  - [シミュレートされた X.509 デバイスを作成してプロビジョニングする](/azure/iot-dps/quick-create-simulated-device-x509-python)
  - [X.509 デバイスを Device Provisioning Service に登録する](/azure/iot-dps/quick-enroll-device-x509-python)

- **IoT Central/IoT Edge**:
  - [チュートリアル:クライアント アプリケーションを作成して Azure IoT Central アプリケーションに接続する (Python)](/azure/iot-central/core/tutorial-connect-device-python)
  - [チュートリアル:Linux デバイス用の Python IoT Edge モジュールを開発してデプロイする](/azure/iot-edge/tutorial-python-module)
