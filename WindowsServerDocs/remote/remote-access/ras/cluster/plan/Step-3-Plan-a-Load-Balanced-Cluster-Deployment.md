---
title: 手順 3. 負荷分散クラスターの展開を計画します。
description: このトピックでは、Windows Server 2016 でのクラスターでのリモート アクセスの展開ガイドの一部です。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8efeb29bf07b572d5e2faef677349690d4282b4d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281179"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>手順 3. 負荷分散クラスターの展開を計画します。

>適用先:Windows Server 2016 の Windows Server (半期チャネル)

次の手順では、負荷分散の構成とクラスターの展開を計画します。  
  
|タスク|説明|  
|----|--------|  
|3.1 負荷分散を計画します。|Windows ネットワーク負荷分散 (NLB)、または外部ロード バランサー (ELB) を使用するかどうかを決定します。|  
|3.2、IP-HTTPS を計画します。|自己署名証明書を使用しない場合、リモート アクセス サーバーは、IP-HTTPS 接続を認証するために、クラスター内の各サーバーで SSL 証明書が必要です。|  
|3.3 が VPN クライアント接続を計画します。|VPN クライアント接続のための要件に注意してください。|  
|3.4 ネットワーク ロケーション サーバーを計画します。|ネットワーク ロケーション サーバー web サイトがリモート アクセス サーバーでホストされている自己署名証明書を使用しない場合は、クラスター内の各サーバーが、web サイトへの接続を認証するサーバー証明書を持つことを確認します。|  
  
## <a name="bkmk_2_1_Plan_LB"></a>3.1 負荷分散を計画します。  
リモート アクセスは、1 台のサーバーまたはリモート アクセス サーバーのクラスターに展開できます。 クラスターへのトラフィックは負荷分散する DirectAccess クライアントの高可用性とスケーラビリティを提供できます。 2 つの負荷分散オプションがあります。  
  
-   **Windows NLB**-Windows NLB は、Windows server の機能です。 これを使用するには、必要としない追加のハードウェア、クラスター内のすべてのサーバーが、トラフィックの負荷を管理するためです。 Windows NLB には、リモート アクセス クラスター内の最大 8 台のサーバーがサポートしています。  
  
-   **外部ロード バランサー**-外部ロード バランサーを使用するには、クラスターのリモート アクセス サーバーの間でトラフィックの負荷を管理する外部のハードウェアが必要です。 さらに、外部ロード バランサーを使用すると、クラスターで最大 32 台のリモート アクセス サーバーをサポートします。 いくつかの点を考慮して外部の負荷分散の構成は次のとおりです。  
  
    -   管理者は、(f5 キーを押して Big-ip ローカル Traffic Manager のシステム) のような外部のロード バランサーでリモート アクセスの負荷分散ウィザードで構成されている仮想 Ip が使用されていることを確認する必要があります。 外部の負荷分散を有効にすると外部および内部インターフェイス上の IP アドレスは仮想 IP アドレスに昇格するロード バランサーに組み込まする必要があります。 これにより、管理者が、クラスターの展開のパブリック名の DNS エントリを変更する必要があるないようにします。 また、IPsec トンネル エンドポイントは、サーバーの ip アドレスから派生します。 管理者は、別の仮想 ip アドレスを提供する場合、クライアントはできません、サーバーに接続できません。 3\.1.1 で外部の負荷分散と DirectAccess を構成するための使用例を参照してください。 外部ロード バランサーの構成の例です。  
  
    -   (F5 キーを含む) 多くの外部のロード バランサーは、6to4 と ISATAP の負荷分散をサポートしていません。 リモート アクセス サーバーが ISATAP ルーターの場合は、ISATAP 関数を別のコンピューターに移動してください。 また、ISATAP 関数が別のコンピューター上にある場合は、DirectAccess サーバーは、ISATAP ルーターにネイティブ IPv6 接続が必要です。 ノートがこの接続が DirectAccess を構成する前に存在する必要があります。  
  
    -   外部の負荷分散 Teredo が使用する必要がある場合、すべてのリモート アクセス サーバーが必要 2 つの連続するパブリック IPv4 アドレス専用 IP アドレスとしてです。 クラスターの仮想 ip アドレスも 2 つの連続するパブリック IPv4 アドレスが必要です。 これについては該当しません Windows NLB クラスターの仮想 ip アドレスのみが 2 つの連続するパブリック IPv4 アドレスを持つ必要があります。 Teredo を使用しない場合は 2 つの連続した IP アドレスは必要ありません。  
  
    -   管理者は、外部ロード バランサーまたはその逆 Windows NLB を切り替えることができます。 管理者できませんに切り替えること外部ロード バランサーから Windows NLB 8 個を超えるサーバーが外部ロード バランサーの展開で彼は場合に注意してください。  
  
### <a name="ELBConfigEx"></a>3.1.1 外部ロード バランサー構成の例  
このセクションでは、新しいリモート アクセスの展開で外部ロード バランサーを有効にするための構成手順について説明します。 外部ロード バランサーを使用する場合、リモート アクセス クラスターは次の図のようになります可能性があります、企業ネットワーク内部ネットワーク上のロード バランサーを経由して外部ネットワークに接続されているロード バランサーを経由してインターネットにリモート アクセス サーバーが接続されています。  
  
![外部ロード バランサーの構成例](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)  
  
##### <a name="planning-information"></a>計画に関する情報  
  
1.  外部 Vip (クライアントがリモート アクセスへの接続に使用する Ip) されたことを 131.107.0.102、131.107.0.103  
  
2.  ロード バランサーが外部ネットワーク self-Ip での 131.107.0.245 (インターネット)、131.107.1.245  
  
    境界ネットワーク (非武装地帯および DMZ とも呼ばれます) は、ロード バランサーの外部ネットワークとリモート アクセス サーバーの間です。  
  
3.  IP アドレスを境界ネットワーク上のリモート アクセス サーバー 131.107.1.102、131.107.1.103  
  
4.  30.11.1.101 2006:2005:11:1::101 (つまり、リモート アクセス サーバーと内部ネットワーク上のロード バランサー) - 間 ELB ネットワーク上でリモート アクセス サーバーの IP アドレスします。  
  
5.  ロード バランサーが内部ネットワーク self-Ip - 30.11.1.245 で 2006:2005:11:1::245 (ELB) 30.1.1.245 2006:2005:1:1::245 (Corpnet)  
  
6.  30.1.1.10 2006:2005:1:1::10 であると決定した内部 Vip (IP アドレスが、リモート アクセス サーバーにインストールされている場合、クライアントの web プローブおよびネットワーク ロケーション サーバーを使用)  
  
##### <a name="steps"></a>手順  
  
1.  アドレス 131.107.0.102、131.107.0.103 で (つまり、境界ネットワークに接続されている)、リモート アクセス サーバーの外部ネットワーク アダプターを構成します。 この手順は、正しい IPsec トンネル エンドポイントを検出するために DirectAccess の構成に必要です。  
  
2.  Web プローブ/ネットワーク ロケーション サーバーの IP アドレス (30.1.1.10、2006:2005:1:1::10) と (つまり ELB ネットワークに接続されている)、リモート アクセス サーバーの内部ネットワーク アダプターを構成します。 この手順は、クライアントは、network connectivity assistant は正しく DirectAccess への接続状態を示すように、web プローブ IP にアクセスできるようにするために必要です。 この手順では、DirectAccess サーバーで構成されている場合、ネットワーク ロケーション サーバーへのアクセスもできます。  
  
    > [!NOTE]  
    > ドメイン コント ローラーがこの構成でのリモート アクセス サーバーから到達できることを確認します。  
  
3.  リモート アクセス サーバーでの DirectAccess 単一サーバーを構成します。  
  
4.  外部の負荷分散は DirectAccess の構成で有効にします。 外部の専用 IP アドレス (DIP) と 131.107.1.102 を使用して (131.107.1.103 は自動的に選択)、30.11.1.101、内部 Dip として 2006:2005:11:1::101 を使用します。  
  
5.  アドレス 131.107.0.102 と 131.107.0.103 と外部ロード バランサーの外部の仮想 Ip (VIP) を構成します。 また、アドレス 30.1.1.10 と外部ロード バランサー上の内部の Vip と 2006:2005:1:1::10 を構成します。  
  
6.  計画的な IP アドレスを使用してリモート アクセス サーバーの情報が構成され、計画的な IP アドレスに従って、クラスターの外部および内部の IP アドレスが構成します。  
  
## <a name="bkmk_2_2_NLB"></a>3.2、IP-HTTPS を計画します。  
  
1.  **証明書の要件**-パブリックまたは内部の証明機関 (CA) によって発行された、IP-HTTPS 証明書または自己署名証明書を使用して選択した単一のリモート アクセス サーバーの展開時にします。 クラスター展開用と同じ種類の証明書を使用して、リモート アクセス クラスターのメンバーごとにする必要があります。 つまり、(推奨)、公共の CA によって発行された証明書を使用している場合は、クラスターのメンバーごとにパブリック CA によって発行された証明書をインストールする必要があります。 新しい証明書のサブジェクト名は、展開で使用されて、IP-HTTPS 証明書のサブジェクト名と同じにする必要があります。 自己署名証明書を使用している場合は、これらを構成することに自動的に各サーバーでクラスターの展開時に注意してください。  
  
2.  **要件のプレフィックス**-リモート アクセスにより、SSL ベースのトラフィックと DirectAccess トラフィックの両方の負荷を分散します。 負荷を分散するすべての IPv6 ベースの DirectAccess トラフィック、リモート アクセスはすべての移行テクノロジの IPv4 トンネリングを調べる必要があります。 IP-HTTPS のトラフィックが暗号化されているために、IPv4 トンネルの内容を確認することはできません。 別の/64 の IPv6 プレフィックスは、各クラスター メンバーに割り当てることができるように、IP-HTTPS のトラフィックを負荷分散を有効にするのに十分な IPv6 プレフィックスを割り当てる必要があります。 負荷分散されたクラスター; で最大 32 台のサーバーを構成します。このため、/59 を指定する必要がありますプレフィックス。 このプレフィックスは、リモート アクセス クラスターの内部 IPv6 アドレスにルーティング可能である必要がありがリモート アクセス サーバーのセットアップ ウィザードで構成されています。  
  
    > [!NOTE]  
    > プレフィックスの要件は次の IPv6 を有効になっている内部ネットワークにのみ関係 (IPv6 専用または ipv 4 + IPv6)。 IPv4 のみの企業ネットワークでは、クライアントのプレフィックスが自動的に構成されている、管理者が変更ことはできません。  
  
## <a name="BKMK_3.3"></a>3.3 が VPN クライアント接続を計画します。  
さまざまな VPN クライアントの接続に関する考慮事項があります。  
  
-   VPN クライアント トラフィックを負荷分散の場合は DHCP を使用して VPN クライアント アドレスが割り当てられるをすることはできません。 静的アドレス プールが必要です。  
  
-   RRAS で有効にするのみ、DirectAccess の展開されている負荷分散クラスターを使用して **VPN を有効にする** リモート アクセス管理コンソールの [タスク] ウィンドウにします。  
  
-   VPN の変更が、ルーティングとリモート アクセス管理コンソール (rrasmgmt.msc) で完了には、クラスター内のすべてのリモート アクセス サーバー上で手動でレプリケートする必要があります。  
  
-   VPN IPv6 クライアント トラフィックを負荷分散を有効にするには 59 ビットの IPv6 プレフィックスを指定する必要があります。  
  
## <a name="BKMK_nls"></a>3.4 ネットワーク ロケーション サーバーを計画します。  
単一のリモート アクセス サーバーで、ネットワーク ロケーション サーバー web サイトを実行している展開時に選択した場合、内部証明機関 (CA) によって発行された証明書または自己署名証明書を使用します。  次のことを考慮してください。  
  
1.  リモート アクセス クラスターの各メンバーは、ネットワーク ロケーション サーバー web サイトの DNS エントリに対応するネットワーク ロケーション サーバー用の証明書が必要です。  
  
2.  各クラスター サーバーの証明書は、上、単一のリモート アクセス サーバー現在ネットワーク ロケーション サーバー証明書の証明書と同じ方法で発行する必要があります。 たとえば、内部 CA によって発行された証明書を使用している場合は、クラスターの各メンバーの内部 CA によって発行された証明書をインストールする必要があります。  
  
3.  自己署名証明書を使用している場合自己署名証明書が自動的にサーバーに対して構成する各クラスターの展開時にします。  
  
4.  証明書のサブジェクト名の任意のリモート アクセス展開でサーバー名と同じにすることはできません。  