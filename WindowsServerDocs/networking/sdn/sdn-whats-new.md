---
title: Windows Server 用 SDN の新機能
description: このトピックでは、Windows Server 1709 のソフトウェア定義ネットワークの新機能について説明します。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: 803ca27ca138281cbea1a93aca7e5a8b799bd862
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870160"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Windows Server 2019 の SDN の新機能

>適用対象:Windows Server (半期チャネル)


|                         **機能**                          |                                                                                                                                                                                         **[説明]**                                                                                                                                                                                         | **新規/更新** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [暗号化されたネットワーク](vnet-encryption/sdn-vnet-encryption.md) | 仮想ネットワーク暗号化を使用すると、"暗号化が有効になっている" とマークされているサブネット内で相互に通信する仮想マシン間で仮想ネットワークトラフィックを暗号化できます。 また、この機能は、仮想サブネットのデータグラム トランスポート層セキュリティ (DTLS) を利用して、パケットを暗号化します。 DTLS は、物理ネットワークへのアクセスを持つユーザーによる盗聴、改ざん、偽造に対する保護を提供します。 |       新規作成       |
|    [ファイアウォールの監査](security/sdn-firewall-auditing.md)    |                                                                                            ファイアウォール監査は、Windows Server 2019 の SDN ファイアウォールの新機能です。 SDN ファイアウォールを有効にすると、ログが有効になっている SDN ファイアウォール規則 (Acl) によって処理されるすべてのフローが記録されます。                                                                                            |       新規作成       |
| [仮想ネットワーク ピアリング](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      仮想ネットワークピアリングを使用すると、2つの仮想ネットワークをシームレスに接続できます。 ピアリングされた接続のために、仮想ネットワークは1つとして表示されます。                                                                                                                      |       新規作成       |
|           [送信の測定](manage/sdn-egress.md)            |                  Windows Server 2019 のこの新機能により、SDN は送信データ転送の使用量メーターを提供できます。 この機能を追加すると、ネットワークコントローラーは SDN 内で使用されているすべての IP 範囲の Virtual Network ごとにホワイトリストを保持し、送信データ転送の料金を請求するために、これらの範囲のいずれかに含まれていない宛先のパケットバインドを検討します。                   |       新規作成       |

---



