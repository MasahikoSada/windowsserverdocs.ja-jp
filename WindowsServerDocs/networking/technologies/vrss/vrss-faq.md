---
title: vRSS に関してよく寄せられる質問
description: このトピックでは、vRSS の使用に関してよく寄せられる質問とその回答を紹介します。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0f3cb2bebf5aed31be0dda0c8e33a78aae94367
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871820"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS に関してよく寄せられる質問

このトピックでは、vRSS の使用に関してよく寄せられる質問とその回答を紹介します。

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>VRSS で使用している物理ネットワークアダプターの要件はどのようなものですか。

ネットワークアダプターは仮想マシンキュー \(VMQ\)と互換性があり、10 Gbps 以上のリンク速度を備えている必要があります。

詳細については、「 [vRSS の使用を計画する](vrss-plan.md)」を参照してください。

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS はハイパー\-スレッドプロセッサコアで動作しますか。

No. VRSS と VMQ は、両方\-ともハイパースレッドプロセッサコアを無視します。

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS はホスト仮想 nic \(vnics\)に対して機能しますか。

可能。 **VMNetworkAdapter** Windows PowerShell コマンドで、仮想マシン\(VM\)名の代わりに **-ManagementOS**パラメーターを使用し、ホスト vNIC で **-set-netadapterrss を有効**にします。

詳細については、「 [RSS および vRSS 用の Windows PowerShell コマンド](vrss-wps.md)」を参照してください。

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>VM では、vRSS を使用するために必要な論理プロセッサの数を確認できます。

VRSS を使用できるよう\(\)にするには、vm に2つ以上の論理プロセッサが必要です。

詳細については、「 [vRSS の使用を計画する](vrss-plan.md)」を参照してください。

## <a name="is-vrss-compatible-with-nic-teaming"></a>VRSS は、NIC チーミングと互換性がありますか。

可能。 NIC チーミングを使用している場合は、NIC チーミング設定と連動するように VMQ を正しく構成することが重要です。 NIC チーミングの展開と管理の詳細については、「 [Nic チーミング](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)」を参照してください。

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>vRSS は有効になっていますが、動作しているかどうかを確認するにはどうすればよいですか。 

VM でタスクマネージャーを開き、仮想プロセッサの使用率を表示することによって、vRSS が機能していることを通知できます。 VM に対して複数の接続が確立されている場合は、0% の使用率を上回るコアが1つ以上表示されます。

1つの TCP セッションを複数の論理プロセッサコア間で負荷分散することはできません。そのため、vRSS が動作しているかどうかを確認する前に、VM で複数の TCP セッションを受信する必要があります。

VM が複数の TCP セッションを受信しているにもかかわらず、使用率が 0% を超える LP コアが1つ以上表示されない場合は、「 [vRSS の使用を計画](vrss-plan.md)する」のすべての準備手順を完了していることを確認してください。

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>ホストを見ていますが、すべてのプロセッサが使用されているわけではありません。 1 つおきにスキップされていると思われます。
  
ハイパー スレッディングが有効であるかどうかをチェックします。 VMQ と vRSS はどちらも、ハイパー\-スレッドコアをスキップするように設計されています。

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>RSS および vRSS 用のさまざまな Windows PowerShell コマンドはありますか。

さあ何とも言えません。 Vm ではネイティブホストと RSS の両方に同じコマンドを使用しますが、vRSS では物理 NIC で VMQ を有効にする必要があります。また、VM と vRSS をスイッチポートで有効にする必要もあります。

詳細については、「 [RSS および vRSS 用の Windows PowerShell コマンド](vrss-wps.md)」を参照してください。

---
