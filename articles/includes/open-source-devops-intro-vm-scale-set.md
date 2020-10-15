---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 04/22/2019
ms.author: tarcher
ms.openlocfilehash: b77da5c9ba9ee1426a9ba2e7877fc27351e1dc17
ms.sourcegitcommit: 723441eda0eb4ff893123201a9e029b7becf5ecc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/08/2020
ms.locfileid: "91859662"
---
[Azure Virtual Machine Scale Sets](/azure/virtual-machine-scale-sets/overview) は、負荷分散が行われる同一の VM のグループを構成するための Azure 機能です。 スケール セットに追加コストはなく、仮想マシンから構築されます。 ユーザーは、VM インスタンス、ロード バランサー、マネージド ディスク ストレージなど、基本的なコンピューティング リソースに対してのみ支払います。 スケール セットには、アプリケーションの実行とスケーリングを行うための管理レイヤーと自動化レイヤーがあります。 代わりに手動で個々 の VM を作成し管理できます。 ただし、スケール セットの使用には、2 つの主な利点があります。 それらは Azure に組み込まれ、アプリケーションのニーズを満たすように自動的に仮想マシンを拡大縮小します。