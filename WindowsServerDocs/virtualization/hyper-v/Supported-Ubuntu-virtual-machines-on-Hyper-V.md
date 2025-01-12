---
title: HYPER-V でサポートされている Ubuntu 仮想マシン
description: 各バージョンに含まれる Linux integration services と機能の一覧を示します。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 06/13/2019
ms.openlocfilehash: ad0f79767310595244d0d57876c20b9548a81a96
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2019
ms.locfileid: "69584809"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>HYPER-V でサポートされている Ubuntu 仮想マシン

>適用先:Windows Server 2019、2016、Hyper-v Server 2019、2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7。

Ubuntu 12.04 以降は「仮想 linux」のパッケージの読み込み、ゲスト仮想マシンとしての使用に適したカーネルをインストールします。 このパッケージは、常に最新の最小限の一般的なカーネル イメージとバーチャル マシン用に使用されるヘッダーに依存します。 使用は省略可能ですが、linux 仮想カーネルが少ないドライバーを読み込むと可能性があります起動高速化して汎用のイメージよりも少ないメモリのオーバーヘッドが発生します。

HYPER-V の使用を増加させるには、適切な linux ツールと仮想マシンでのツールおよび使用するためのデーモンをインストールする linux クラウド ツール パッケージをインストールします。 Linux 仮想カーネルを使用する場合は、linux マシン ツールから仮想マシンと linux-クラウド ツール-バーチャルを読み込みます。

次の機能の配布マップでは、各バージョンの機能を示します。 既知の問題と各配布の回避策は、表の下に一覧表示されます。

## <a name="table-legend"></a>表の凡例

* **組み込まれている** に LIS がこの Linux ディストリビューションの一部として含まれています。 インストールされないように、この配布では、Microsoft 提供の LIS のダウンロード パッケージは機能しません。 組み込みの LIS のカーネル モジュールのバージョン番号 (に示すように **lsmod**, 、たとえば) マイクロソフト提供の LIS のダウンロード パッケージにバージョン番号とは異なります。 組み込みの LIS の項目が古いことに不一致が示されません。

* &#10004; です。の機能使用

* (*空白*) の機能は使用できません

|**機能**|**Windows Server オペレーティングシステムのバージョン**|**18.10/19.04**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**可用性**||組み込み|組み込み|組み込み|組み込み|組み込み|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 の正確な時刻|2019、2016|&#10004;|&#10004;|&#10004;|||
|**[機能](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Jumbo Frame|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN のタグ付けとトランキング|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|ライブ マイグレーション|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静的 IP インジェクション|2019、2016、2012 R2、2012|&#10004 です。注 1|&#10004 です。注 1|&#10004 です。注 1|&#10004 です。注 1|&#10004 です。注 1|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|TCP セグメント化とチェックサムのオフロード|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;|||
|**[・](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|VHDX のサイズ変更|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|仮想ファイバー チャネル|2019、2016、2012 R2|&#10004 です。注 2|&#10004 です。注 2|&#10004 です。注 2|&#10004 です。注 2||
|仮想マシンのライブバックアップ|2019、2016、2012 R2|&#10004;3、4、6 に注意してください。|&#10004 です。3, 4, 5 に注意してください。|&#10004 です。3, 4, 5 に注意してください。|&#10004 です。3, 4, 5 に注意してください。||
|トリムのサポート|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[量](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|PAE カーネルサポート|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|MMIO ギャップの構成|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|動的メモリでホット アド|2019、2016、2012 R2、2012|&#10004 です。7, 8, 9 に注意してください。|&#10004 です。7, 8, 9 に注意してください。|&#10004 です。7, 8, 9 に注意してください。|&#10004 です。7, 8, 9 に注意してください。||
|動的メモリ - バルーニング|2019、2016、2012 R2、2012|&#10004 です。7, 8, 9 に注意してください。|&#10004 です。7, 8, 9 に注意してください。|&#10004 です。7, 8, 9 に注意してください。|&#10004 です。7, 8, 9 に注意してください。||
|ランタイムのメモリのサイズ変更|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|HYPER-V で特定のビデオ デバイス|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[な](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|キー/値ペア|2019、2016、2012 R2、2012、2008 R2|&#10004;注 6「10|&#10004 です。注 5、10|&#10004 です。注 5、10|&#10004 です。注 5、10|&#10004 です。注 5、10|
|マスクなしの割り込み|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|ホストからゲストへのファイルのコピー|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus コマンド|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Hyper V ソケット|2019、2016||||||
|PCI パススルー/DDA|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[第2世代仮想マシン](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|UEFI を使用したブート|2019、2016、2012 R2|&#10004 です。注 11、12|&#10004 です。注 11、12|&#10004 です。注 11、12|&#10004 です。注 11、12||
|セキュアブート|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>メモ

1. 静的 IP インジェクションが機能しなくなる **ネットワーク マネージャー** が仮想マシン上の指定したハイパー V 固有ネットワーク アダプターに対して構成されています。 静的 IP の円滑に機能することを確認するインジェクションを確認してくださいネットワーク マネージャーが完全にオフや、オフになっているを使って特定のネットワーク アダプターにその **ifcfg ethX** ファイルです。

2. 仮想ファイバー チャネル デバイスを使用している論理ユニット番号 (LUN 0) を 0 に設定されていることを確認します。 LUN 0 が設定されていません Linux 仮想マシンは、ファイバー チャネル デバイスをネイティブにマウントすることができません。

3. 開いている場合に、バーチャル マシンのバックアップ操作中にファイルを処理し、いくつかのコーナー ケースでバックアップされる Vhd は、ファイル システムの整合性チェックを行う必要があります (`fsck`) のサイズに戻す。

4. ライブ バックアップ操作がエラーに何も行わずに、仮想マシンに接続された iSCSI デバイスまたは直接接続ストレージ (パススルー ディスクとも呼ばれます) がある場合。

5. 長期的なサポートでは、(LTS) のリリースは、最新の Linux Integration Services の最新バージョンの仮想ハードウェア支援 (HWE) カーネルを使用します。

   14.04、16.04、および18.04 に Azure でチューニングされたカーネルをインストールするには、root (または sudo) として次のコマンドを実行します。

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   12.04 には、個別の仮想カーネルがありません。 Generic HWE カーネルを12.04 にインストールするには、root (または sudo) として次のコマンドを実行します。

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty
   ```

   Ubuntu 12.04 では、次の Hyper-v デーモンが個別にインストールされたパッケージに含まれています。

   * **VSS スナップショット デーモン** -ライブの Linux 仮想マシンのバックアップを作成するこのデーモンが必要です。
   * **KVP デーモン** -このデーモンは、設定および組み込みおよび外部キー値のペアを照会します。
   * **fcopy デーモン** -このデーモンは、ファイル サービス、ホストとゲスト間のコピーを実装します。

   12.04 に KVP デーモンをインストールするには、root (または sudo) として次のコマンドを実行します。

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty
   ```

   カーネルが更新されるたびに、それを使用する、仮想マシンを再起動する必要があります。

6. Ubuntu 18.10 または19.04 では、最新の仮想カーネルを使用して最新の Hyper-v 機能を利用できます。

   18.10 または19.04 に仮想カーネルをインストールするには、ルート (または sudo) として次のコマンドを実行します。

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   カーネルが更新されるたびに、それを使用する、仮想マシンを再起動する必要があります。

7. 動的メモリのサポートは、64 ビット仮想マシンで使用できるのみです。

8. 動的メモリの操作は、ゲスト オペレーティング システムのメモリ不足実行に失敗します。 ベスト プラクティスを次に示します。

   * 起動メモリと最小限のメモリは、ディストリビューションのベンダーが推奨されているメモリ量以上にする必要があります。

   * システム全体の利用可能なメモリを消費する傾向があるアプリケーションは、使用可能なメモリの最大 80% を消費してに制限されます。

9. Windows Server 2019、Windows Server 2016、または Windows Server 2012/2012 R2 のいずれかのオペレーティングシステムで動的メモリを使用している場合は、**起動メモリ**、**最小メモリ**、および**最大メモリ**パラメーターを128メガバイト (mb) の倍数で指定します。 そのためにはエラーはエラーにつながるホット アドとゲスト オペレーティング システムに大きくなるメモリが表示されない場合があります。

10. Windows Server 2019、Windows Server 2016、または Windows Server 2012 R2 では、Linux ソフトウェア更新プログラムがないと、キー/値ペアのインフラストラクチャが正しく機能しない可能性があります。 この機能の問題が確認された場合に、ソフトウェア更新プログラムを取得するディストリビューションのベンダーに問い合わせてください。

11. Windows Server 2012 R2 では、第 2 世代仮想マシンは、セキュア ブート オプションが無効にしない限り、仮想マシンは起動しません Linux 既定で有効になり、セキュア ブートがあります。 セキュア ブートを無効にすることができます、 **ファームウェア** でバーチャル マシンの設定のセクション **HYPER-V マネージャーで** または Powershell を使用して無効にすることができます。

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. 新しい第 2 世代バーチャル マシンを作成するための既存の世代 2 の VHD の仮想マシンの VHD をコピーする前に、次の手順に従います。

    1. 既存の第 2 世代バーチャル マシンにログインします。

    2. ブート EFI ディレクトリにディレクトリを変更します。

       ```bash
       # cd /boot/efi/EFI
       ```

    3. Ubuntu ディレクトリを boot という名前の新しいディレクトリにコピーします。

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. 新しく作成したブート ディレクトリにディレクトリを変更します。

       ```bash
       # cd boot
       ```

    5. Shimx64.efi ファイルの名前を変更します。

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>関連項目

* [サポートされている CentOS と Hyper-v 上の仮想マシンの Red Hat Enterprise Linux](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V でサポートされている Debian 仮想マシン](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v でサポートされている Oracle Linux の仮想マシン](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v でサポートされている SUSE 仮想マシン](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上の Linux および FreeBSD 仮想マシンの機能の説明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v で Linux を実行するためのベストプラクティス](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [第2世代 VM の Ubuntu 14.04-Ben Armstrong の仮想化に関するブログ](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
