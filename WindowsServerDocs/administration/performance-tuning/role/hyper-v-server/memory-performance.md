---
title: Hyper-v メモリのパフォーマンス
description: パフォーマンスチューニングに関するメモリの考慮事項 Hyper-v
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ddb336e0d6e16342dd60f2f61e50afeda61837e9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866574"
---
# <a name="hyper-v-memory-performance"></a>Hyper-v メモリのパフォーマンス


ハイパーバイザーは、仮想マシンを相互に分離し、仮想化されていないシステムと同様に、ゲストオペレーティングシステムごとに0から始まる連続したメモリ領域を提供するために、ゲスト物理メモリを仮想化します。

## <a name="correct-memory-sizing-for-child-partitions"></a>子パーティションのメモリのサイズを修正します

物理コンピューター上のサーバーアプリケーションの場合と同じように、仮想マシンのメモリサイズを指定する必要があります。 メモリ不足によって応答時間が大幅に長くなり、CPU または i/o の使用量が大幅に増加する可能性があるため、通常とピーク時に予想される負荷を適度に処理するようにサイズ変更する必要があります。

動的メモリを有効にすると、Windows が仮想マシンのメモリのサイズを動的に変更できるようになります。 動的メモリでは、仮想マシンのアプリケーションで大きな急激なメモリ割り当てが発生しているという問題が発生した場合は、仮想マシンのページファイルサイズを増やして、動的メモリがメモリ不足に応答するまで一時的なバックアップを確保することができます。

動的メモリの詳細については、「 [hyper-v 動的メモリの概要]( https://go.microsoft.com/fwlink/?linkid=834434)」および「 [Hyper-v 動的メモリ構成ガイド](https://go.microsoft.com/fwlink/?linkid=834435)」を参照してください。

子パーティションで Windows を実行している場合は、子パーティション内で次のパフォーマンスカウンターを使用して、子パーティションにメモリ不足が発生しているかどうかを特定し、仮想マシンのメモリサイズを大きくするとパフォーマンスが向上する可能性があります。

| パフォーマンス カウンター                                                         | 推奨されるしきい値                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| メモリ-スタンバイキャッシュ予約バイト数                                        | スタンバイキャッシュ予約バイト数と空きページリストバイト数の合計は、1 GB のシステムでは 200 MB 以上、表示される RAM が 2 GB 以上のシステムでは 300 MB 以上である必要があります。 |
| メモリ-空き & ゼロページリストバイト数                                        | スタンバイキャッシュ予約バイト数と空きページリストバイト数の合計は、1 GB のシステムでは 200 MB 以上、表示される RAM が 2 GB 以上のシステムでは 300 MB 以上である必要があります。 |
| メモリ-1 秒あたりの入力ページ数                                                    | 1時間の平均は10未満です。                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>ルートパーティションのメモリのサイズを修正します

ルートパーティションには、子パーティションをサポートするための i/o 仮想化、仮想マシンのスナップショット、管理などのサービスを提供するのに十分なメモリが必要です。

Windows Server 2016 の hyper-v は、ルートパーティションの管理オペレーティングシステムの実行時の正常性を監視し、ルートパーティションの高パフォーマンスと信頼性を確保しながら、子パーティションに安全に割り当てることができるメモリの量を判断します。

## <a name="see-also"></a>関連項目

-   [Hyper-V の用語](terminology.md)

-   [Hyper-V のアーキテクチャ](architecture.md)

-   [Hyper-V サーバーの構成](configuration.md)

-   [Hyper-V プロセッサのパフォーマンス](processor-performance.md)

-   [Hyper-V 記憶域の I/O のパフォーマンス](storage-io-performance.md)

-   [Hyper-V ネットワークの I/O のパフォーマンス](network-io-performance.md)

-   [仮想化環境のボトルネックの検出](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 仮想マシン](linux-virtual-machine-considerations.md)
