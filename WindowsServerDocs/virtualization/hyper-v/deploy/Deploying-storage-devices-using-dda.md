---
title: 個別のデバイスの割り当てを使用して NVMe 記憶装置を展開する
description: DDA を使用して記憶装置を展開する方法について説明します。
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: 001d8addba2cdb8cbbd5d72ffa224d16f62d1245
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872124"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>個別のデバイスの割り当てを使用して NVMe 記憶装置を展開する

>適用先:Microsoft Hyper-V Server 2016、Windows Server 2016

Windows Server 2016 以降では、個別のデバイス割り当て (DDA) を使用して、PCIe デバイス全体を VM に渡すことができます。  これにより、デバイスのネイティブドライバーを利用できるのに対して、VM 内から NVMe ストレージやグラフィックスカードなどのデバイスに高パフォーマンスでアクセスできるようになります。  デバイスの展開については、デバイス[の個別割り当てを使用したデバイスの展開計画](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)に関するページを参照してください。DDA でデバイスを使用するには、次の3つの手順を実行します。
-   DDA 用に VM を構成する
-   ホストパーティションからデバイスのマウントを解除する
-   ゲスト VM へのデバイスの割り当て

すべてのコマンドは、Windows PowerShell コンソールのホストで管理者として実行できます。

## <a name="configure-the-vm-for-dda"></a>DDA 用に VM を構成する
個別のデバイスの割り当てによって、Vm にいくつかの制限が課され、次の手順を実行する必要があります。

1.  を実行して、VM の "自動停止アクション" を TurnOff に構成します。

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>ホストパーティションからデバイスのマウントを解除する

### <a name="locating-the-devices-location-path"></a>デバイスの場所のパスを特定する
ホストからデバイスをマウント解除してマウントするには、PCI ロケーションパスが必要です。  ロケーションパスの例は次`"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`のようになります。   ロケーションパスの詳細については、こちらを参照してください。[個別のデバイスの割り当てを使用してデバイスを展開](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)することを計画します。

### <a name="disable-the-device"></a>デバイスを無効にする
デバイスマネージャーまたは PowerShell を使用して、デバイスが "無効" になっていることを確認します。  

### <a name="dismount-the-device"></a>デバイスのマウントを解除する
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>ゲスト VM へのデバイスの割り当て
最後の手順では、VM がデバイスにアクセスできる必要があることを Hyper-v に指示します。  上に示した場所のパスに加えて、vm の名前を把握しておく必要があります。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>次の内容
デバイスが VM に正常にマウントされると、その VM を起動して、ベアメタルシステムで実行されていた場合と同じように、通常どおりにデバイスと対話できるようになります。  これを確認するには、ゲスト VM でデバイスマネージャーを開き、ハードウェアが表示されていることを確認します。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>デバイスを削除してホストに戻す
デバイスを元の状態に戻す場合は、VM を停止し、次のものを発行する必要があります。
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
デバイスマネージャーでデバイスを再度有効にすると、ホストオペレーティングシステムがデバイスと再び対話できるようになります。
