---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/09/2020
ms.author: tarcher
ms.openlocfilehash: 4f1fbe75f974aadc9e92e518e0e84d49ee73ed3d
ms.sourcegitcommit: eabc9e3fb8ad0f067be5ed878c2eacebd461b6ce
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2020
ms.locfileid: "81755225"
---
- **Azure コレクションの構成**:ターミナル ウィンドウから次のコマンドを実行して、Azure コレクションをインストールします。 Azure コレクションが既にインストールされている場合は、`--force` フラグによって最新のバージョンに更新されます。

    ```bash
    ansible-galaxy collection install azure.azcollection --force
    ```
