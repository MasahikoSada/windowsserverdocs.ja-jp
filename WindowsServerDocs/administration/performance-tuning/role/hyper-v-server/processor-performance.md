---
title: Hyper-v プロセッサのパフォーマンス
description: Hyper-v のパフォーマンスチューニングにおけるプロセッサのパフォーマンスに関する考慮事項
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 232141758032a8e21eca50ddb73ac9bc3cf6af56
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866550"
---
# <a name="hyper-v-processor-performance"></a>Hyper-v プロセッサのパフォーマンス


## <a name="virtual-machine-integration-services"></a>仮想マシン統合サービス

バーチャルマシン Integration Services には、Hyper-v 固有 i/o デバイス用の対応ドライバーが含まれています。これにより、エミュレートされたデバイスと比較した場合の i/o の CPU オーバーヘッドが大幅に減少します。 サポートされているすべての仮想マシンに、Integration Services の仮想マシンの最新バージョンをインストールする必要があります。 このサービスにより、ゲストの CPU 使用率が、アイドル状態のゲストから頻繁に使用されるゲストに減り、i/o スループットが向上します。 これは、Hyper-v を実行しているサーバーのパフォーマンスをチューニングするための最初の手順です。 サポートされているゲストオペレーティングシステムの一覧については、「 [hyper-v の概要](https://technet.microsoft.com/library/hh831531.aspx)」を参照してください。

## <a name="virtual-processors"></a>仮想プロセッサの数

Windows Server 2016 の hyper-v は、仮想マシンあたり最大240の仮想プロセッサをサポートしています。 CPU 負荷の高い負荷がある仮想マシンは、1つの仮想プロセッサを使用するように構成する必要があります。 これは、ゲストオペレーティングシステムの追加の同期コストなど、複数の仮想プロセッサに関連する追加のオーバーヘッドが原因です。

仮想マシンがピーク時の負荷下で複数の CPU の処理を必要とする場合は、仮想プロセッサの数を増やします。

## <a name="background-activity"></a>バックグラウンドアクティビティ

アイドル状態の仮想マシンのバックグラウンドアクティビティを最小化すると、他の仮想マシンの他の場所で使用できる CPU サイクルが解放されます。 Windows ゲストは、通常、CPU がアイドル状態のときに1パーセント未満の CPU を使用します。 次に、仮想マシンのバックグラウンド CPU 使用率を最小限に抑えるためのベストプラクティスをいくつか示します。

-   Integration Services の仮想マシンの最新バージョンをインストールします。

-   [バーチャルマシンの設定] ダイアログボックスで、エミュレートされたネットワークアダプタを削除します (Microsoft Hyper-V 固有のアダプタを使用します)。

-   CD-ROM や COM ポートなどの未使用のデバイスを削除するか、メディアを切断します。

-   Windows ゲストオペレーティングシステムが使用されていないときにサインイン画面に表示され、スクリーンセーバーを無効にします。

-   既定で有効になっている、スケジュールされたタスクとサービスを確認します。

-   Logman クエリを実行して、既定でオンになっている ETW トレースプロバイダーを確認し**ます。**

-   サーバーアプリケーションを改善して、周期的なアクティビティ (タイマーなど) を減らします。

-   ホストとゲストの両方のオペレーティングシステムでサーバーマネージャーを閉じます。

-   仮想マシンのサムネイルは常に更新されるため、Hyper-v マネージャーを実行したままにしないでください。

次に、仮想マシンで*クライアントバージョン*の Windows を構成して全体的な CPU 使用率を削減するための追加のベストプラクティスを示します。

-   SuperFetch や Windows Search などのバックグラウンドサービスを無効にします。

-   スケジュールされたデフラグなどのスケジュールされたタスクを無効にします。

## <a name="virtual-numa"></a>仮想 NUMA

大規模なスケールアップされたワークロードを仮想化できるようにするために、Windows Server 2016 の Hyper-v では仮想マシンのスケール制限が拡張されています。 1つの仮想マシンに最大240の仮想プロセッサと 12 TB のメモリを割り当てることができます。 このような大規模な仮想マシンを作成する場合、ホストシステム上の複数の NUMA ノードのメモリが使用される可能性があります。 このような仮想マシン構成では、仮想プロセッサとメモリが同じ NUMA ノードから割り当てられていない場合、NUMA の最適化を利用できないため、ワークロードのパフォーマンスが低下する可能性があります。

Windows Server 2016 では、Hyper-v は仮想 NUMA トポロジを仮想マシンに提示します。 既定では、この仮想 NUMA トポロジは基になっているホスト コンピューターの NUMA トポロジと一致するように最適化されます。 仮想 NUMA トポロジを仮想マシンに公開すると、仮想マシンで実行しているゲスト オペレーティング システムおよび NUMA 対応のアプリケーションは、物理コンピューターで実行しているときと同じように、NUMA パフォーマンスの最適化を利用できます。

ワークロードの観点からは、仮想 NUMA と物理 NUMA の違いはありません。 仮想マシン内では、ワークロードがローカル メモリをデータに割り当てて、同じ NUMA ノード内でそのデータにアクセスすると、基になっている物理システムで高速のローカル メモリ アクセスが行われます。 リモート メモリ アクセスによるパフォーマンスの低下は回避されます。 VNUMA の恩恵を受けることができるのは、NUMA 対応のアプリケーションだけです。

Microsoft SQL Server は、NUMA 対応アプリケーションの例です。 詳細については、「 [Non-uniform Memory Access につい](https://technet.microsoft.com/library/ms178144.aspx)て」を参照してください。

仮想 NUMA 機能と動的メモリ機能を同時に使用することはできません。 動的メモリを有効にしている仮想マシンの仮想 NUMA ノードは、実質、1 つだけです。仮想 NUMA の設定に関係なく、NUMA トポロジは仮想マシンに提示されません。

仮想 NUMA の詳細については、「 [Hyper-v 仮想 numa の概要](https://technet.microsoft.com/library/dn282282.aspx)」を参照してください。

## <a name="see-also"></a>関連項目

-   [Hyper-V の用語](terminology.md)

-   [Hyper-V のアーキテクチャ](architecture.md)

-   [Hyper-V サーバーの構成](configuration.md)

-   [Hyper-V メモリのパフォーマンス](memory-performance.md)

-   [Hyper-V 記憶域の I/O のパフォーマンス](storage-io-performance.md)

-   [Hyper-V ネットワークの I/O のパフォーマンス](network-io-performance.md)

-   [仮想化環境のボトルネックの検出](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 仮想マシン](linux-virtual-machine-considerations.md)
