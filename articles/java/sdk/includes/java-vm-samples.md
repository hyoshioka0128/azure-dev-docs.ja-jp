---
ms.openlocfilehash: fe73a2c6b6f18a5e85d342f8fed7ed547b272be9
ms.sourcegitcommit: cbcde17e91e7262a596d813243fd713ce5e97d06
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/06/2020
ms.locfileid: "93406666"
---
| サンプル  | 説明 |
|---|---|
| **仮想マシンの作成** ||
| [仮想マシンの管理][1] | 仮想マシンを作成、変更、開始、停止、削除します。 |
| [カスタム イメージからの仮想マシンの作成][2] | 仮想マシンのカスタム イメージを作成し、そのイメージを使用して新しい仮想マシンを作成します。 | 
| [スナップショットから特殊化された VHD を使って仮想マシンを作成する][3] | 仮想マシンの OS とデータ ディスクからスナップショットを作成し、そのスナップショットからマネージド ディスクを作成した後、マネージド ディスクをアタッチすることにより仮想マシンを作成します。 |  
| [同一ネットワーク内に同時に仮想マシンを作成する][4] | 2 つのサブネットがある同一仮想ネットワーク上の同一リージョンに、並行して複数の仮想マシンを作成します。 |
| [複数のリージョンに対して同時に仮想マシンを作成する][5] | 複数の Azure リージョンにわたって一連の仮想マシンを作成し、負荷分散します。 |
| **ネットワークの仮想マシン** || 
| [仮想ネットワークを管理する][6] | 2 つのサブネットがある仮想ネットワークを設定し、これらに対するインターネット アクセスを制限します。 |
| **スケール セットの作成** ||
| [ロード バランサーを使って仮想マシン スケール セットを作成する][7] | VM スケール セットを作成してロード バランサーを設定し、スケール セットの VM への SSH 接続文字列を取得します。 |

[1]: ../index.yml
[2]: https://github.com/Azure-Samples/managed-disk-java-create-virtual-machine-using-custom-image/
[3]: https://github.com/Azure-Samples/managed-disk-java-create-virtual-machine-using-specialized-disk-from-vhd/
[4]: https://github.com/Azure-Samples/compute-java-manage-virtual-machines-in-parallel/
[5]: ../index.yml
[6]: ../index.yml
[7]: ../java-sdk-manage-vm-scalesets.md