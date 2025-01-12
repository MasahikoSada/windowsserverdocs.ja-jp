---
title: ワイヤレス アクセスの展開計画
description: このトピックは、Windows Server 2016 ネットワークガイド「パスワードベースの 802.1 X 認証ワイヤレスアクセスの展開」に含まれています。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8c632d02-2270-4a82-8fc4-74ea3747f079
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4606e1cb418426623aca9e199ddde575a826a064
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865330"
---
# <a name="wireless-access-deployment-planning"></a>ワイヤレス アクセスの展開計画

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

ワイヤレスアクセスを展開する前に、次の項目を計画する必要があります。

- ネットワークへのワイヤレスアクセス\(ポイント\) ap のインストール

- ワイヤレスクライアントの構成とアクセス

以下のセクションでは、これらの計画手順について詳しく説明します。

## <a name="planning-wireless-ap-installations"></a>ワイヤレス AP インストールの計画
ワイヤレスネットワークアクセスソリューションを設計する場合は、次の操作を行う必要があります。

1. ワイヤレス Ap がサポートする必要がある標準を決定する
2. ワイヤレスサービスを提供する範囲を決定する
3. ワイヤレス Ap をどこに配置するかを決定する

また、ワイヤレス AP とワイヤレスクライアントの IP アドレススキームを計画する必要があります。 関連情報については、「 **NPS でワイヤレス AP の構成を計画**する」を参照してください。

### <a name="verify-wireless-ap-support-for-standards"></a>ワイヤレス AP による標準のサポートを確認する
一貫性を確保し、展開と AP の管理を容易にするために、同じブランドおよびモデルのワイヤレス Ap を展開することをお勧めします。

展開するワイヤレス Ap は、次の機能をサポートしている必要があります。

- **IEEE 802.1 X**

- **RADIUS 認証**

- **ワイヤレス認証と暗号。** 最も優先順位の高い順に一覧表示されます。

    1.  WPA2\-Enterprise と AES

    2.  WPA2\-Enterprise と TKIP

    3.  AES\-を使用した WPA Enterprise

    4.  WPA\-Enterprise と TKIP

>[!NOTE]
>WPA2 を展開するには、ワイヤレスネットワークアダプターと、WPA2 もサポートするワイヤレス Ap を使用する必要があります。 それ以外の場合\-は、WPA Enterprise を使用します。

さらに、ネットワークのセキュリティを強化するために、ワイヤレス Ap は次のセキュリティオプションをサポートする必要があります。

- **DHCP フィルタリング。** ワイヤレス AP は、ワイヤレスクライアントが DHCP サーバーとして構成されている場合に DHCP ブロードキャストメッセージが送信されないように、IP ポートをフィルター処理する必要があります。 ワイヤレス AP は、クライアントが UDP ポート68からネットワークに IP パケットを送信することをブロックする必要があります。

- **DNS フィルター。** ワイヤレス AP は、クライアントが DNS サーバーとして実行されないように、IP ポートをフィルター処理する必要があります。 ワイヤレス AP は、クライアントが TCP または UDP ポート53からネットワークに IP パケットを送信することをブロックする必要があります。

- **クライアントの分離**ワイヤレスアクセスポイントがクライアントの分離機能を提供する場合は、アドレス解決プロトコル\(の ARP\)スプーフィング攻撃の可能性を防ぐために、この機能を有効にする必要があります。

### <a name="identify-areas-of-coverage-for-wireless-users"></a>ワイヤレスユーザーの対象範囲の特定
建物ごとに各フロアのアーキテクチャ図面を使用して、ワイヤレスカバレッジを提供する領域を特定します。 たとえば、適切なオフィス、会議室、ロビー、cafeterias、または courtyards を特定します。

図には、医療機器、ワイヤレスビデオカメラ、2.4 から 2.5 GHz の産業、科学的、医療\(ISM\)で動作するコードレス電話など、ワイヤレス信号に干渉するデバイスがあることを示しています。範囲、および Bluetooth\-対応デバイス。

描画で、ワイヤレス信号に干渉する可能性がある建物の側面をマークします。建物の構築で使用される金属オブジェクトは、ワイヤレス信号に影響を与える可能性があります。 たとえば、次の一般的なオブジェクトは、シグナル伝達に干渉する可能性があります。エレベーター、暖房および空調\-装置ダクト、具象サポート girders います。

ワイヤレス AP の無線周波数の減衰を引き起こす可能性のあるソースの詳細については、AP の製造元に問い合わせてください。 ほとんどの Ap は、信号の強さ、エラー率、データスループットを確認するために使用できるテストソフトウェアを提供します。

### <a name="determine-where-to-install-wireless-aps"></a>ワイヤレス Ap のインストール場所を決定する
アーキテクチャの図では、十分に近いワイヤレス Ap を見つけて、十分なワイヤレスカバレッジを提供しますが、互いに干渉しないようにします。

APs 間に必要な距離は、AP と AP アンテナの種類、ワイヤレス信号をブロックする建物の側面、およびその他の干渉源によって異なります。 ワイヤレス ap の配置を設定して、各ワイヤレス ap が隣接するワイヤレス AP の300フィートを超えないようにすることができます。 AP 仕様と配置のガイドラインについては、ワイヤレス AP の製造元のドキュメントを参照してください。

アーキテクチャの図に指定されている場所にワイヤレス Ap を一時的にインストールします。 次に、802.11 ワイヤレスアダプターが搭載されているラップトップと、ワイヤレスアダプターに付属しているサイト調査ソフトウェアを使用して、各カバレッジエリア内の信号の強度を決定します。

信号強度が低いカバレッジ領域では、AP を設置して、範囲内の信号強度を向上させ、追加のワイヤレス Ap をインストールして、必要なカバレッジを提供し、再配置したり、シグナルの干渉源を削除したりします。  

すべてのワイヤレス Ap の最終的な配置を示すように、アーキテクチャ図面を更新します。 正確な AP 配置マップを使用すると、トラブルシューティングの際、または APs をアップグレードまたは交換するときに、後で支援を受けることができます。

### <a name="plan-wireless-ap-and-nps-radius-client-configuration"></a>ワイヤレス AP と NPS RADIUS クライアントの構成を計画する
NPS を使用して、ワイヤレス Ap を個別またはグループで構成できます。

多数の Ap を含む大規模なワイヤレスネットワークを展開する場合は、グループに APs を構成する方がはるかに簡単です。 NPS の RADIUS クライアントグループとして APs を追加するには、これらのプロパティを使用して Ap を構成する必要があります。

- ワイヤレス Ap は、同じ IP アドレス範囲の IP アドレスで構成されます。

- ワイヤレス Ap はすべて同じ共有シークレットを使用して構成されます。

### <a name="plan-the-use-of-peap-fast-reconnect"></a>PEAP の高速再接続の使用を計画する
802.1 X インフラストラクチャでは、ワイヤレスアクセスポイントは radius サーバーの RADIUS クライアントとして構成されます。 PEAP の高速再接続が展開されている場合、2つ以上のアクセスポイント間をローミングするワイヤレスクライアントは、新しい各関連付けで認証される必要はありません。

PEAP の高速再接続では、クライアントと認証システム間の認証の応答時間が短縮されます。これは、認証要求は、最初にクライアントの認証と承認を実行した NPS に新しいアクセスポイントから転送されるためです。接続要求。

PEAP クライアントと NPS の両方で、以前にキャッシュされた\(トランスポート\)層セキュリティ\(TLS 接続プロパティが使用されるため、\)tls ハンドルという名前のコレクションが nps によってすぐにクライアントは再接続を承認されています。

>[!IMPORTANT]
>高速再接続を正常に機能させるには、Ap を同じ NPS の RADIUS クライアントとして構成する必要があります。

元の NPS が使用できなくなった場合、または別の RADIUS サーバーの RADIUS クライアントとして構成されているアクセスポイントにクライアントが移動した場合は、クライアントと新しい認証システムの間で完全な認証を行う必要があります。

### <a name="wireless-ap-configuration"></a>ワイヤレス AP の構成
次の一覧は、802.1 x\-対応ワイヤレス ap で一般的に構成されている項目をまとめたものです。

> [!NOTE]
> 項目名は、ブランドとモデルによって異なる場合があり、次の一覧の項目とは異なる場合があります。 構成\-固有の詳細については、ワイヤレス AP のドキュメントを参照してください。

- **サービスセット識別子\(の\)SSID**。 これは、ワイヤレスネットワーク\(の名前 (例、wlan\)、ワイヤレスクライアントに提供される名前) です。 混乱を減らすために、提供することを選択した SSID は、ワイヤレスネットワークの受信範囲内にあるワイヤレスネットワークによってブロードキャストされている SSID と一致しないようにする必要があります。

    同じワイヤレスネットワークの一部として複数のワイヤレス Ap が展開されている場合は、各ワイヤレス AP を同じ SSID で構成します。 同じワイヤレスネットワークの一部として複数のワイヤレス Ap が展開されている場合は、各ワイヤレス AP を同じ SSID で構成します。  

    特定のビジネスニーズを満たすためにさまざまなワイヤレスネットワークを展開する必要がある場合は、1つのネットワーク上のワイヤレス AP が、他のネットワーク\(\)の ssid とは異なる ssid をブロードキャストする必要があります。 たとえば、従業員とゲストに別のワイヤレスネットワークが必要な場合は、SSID**をブロードキャストの**例に設定して、ビジネスネットワーク用のワイヤレス ap を構成できます。 ゲストネットワークの場合、各ワイヤレス AP の SSID を設定して、 **Guestwlan**をブロードキャストすることができます。 このようにして、従業員とゲストは不要な混乱を招くことなく、目的のネットワークに接続できます。  

    > [!TIP]  
    > ワイヤレス AP によっては、複数の SSID をブロードキャストして複数\-のネットワーク展開に対応できる場合があります。 複数の SSID をブロードキャストできるワイヤレス AP は、展開と運用保守のコストを削減できます。  

- **ワイヤレス認証と暗号化**。

    ワイヤレス認証は、ワイヤレスクライアントがワイヤレスアクセスポイントに関連付けられている場合に使用されるセキュリティ認証です。  

    ワイヤレス暗号化は、ワイヤレス AP とワイヤレスクライアントの間で送信される通信を保護するためにワイヤレス認証で使用されるセキュリティ暗号化暗号です。  

- **ワイヤレス AP IP アドレス\(が\)静的**です。 各ワイヤレス AP で、一意の静的 IP アドレスを構成します。 サブネットが DHCP サーバーによって処理されている場合は、dhcp サーバーが別のコンピューターまたはデバイスに同じ IP アドレスを発行しないように、すべての AP IP アドレスが DHCP 除外範囲内にあることを確認します。 除外範囲については、[コアネットワークガイド](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)の「新しい DHCP スコープを作成してアクティブ化するには」の手順に記載されています。 NPS のグループごとに Ap を RADIUS クライアントとして構成する予定の場合、グループ内の各 AP は、同じ IP アドレス範囲の IP アドレスを持つ必要があります。

- **DNS 名**。 一部のワイヤレス Ap は、DNS 名を使用して構成できます。 各ワイヤレス AP を一意の名前で構成します。 たとえば、複数\-のストーリーの建物にワイヤレス ap が展開されている場合、3番目のフロア AP3\-01、AP3\-02、および AP3\-03 にデプロイされている最初の3つのワイヤレス ap に名前を付けることができます。

- **ワイヤレス AP サブネットマスク**。 マスクを構成して、IP アドレスのどの部分がネットワーク ID であり、IP アドレスのどの部分がホストであるかを指定します。

- **AP DHCP サービス**。 ワイヤレス AP に DHCP サービスが組み込ま\-れている場合は、無効にします。

- **RADIUS 共有シークレット**。 [グループに NPS RADIUS クライアントを構成する] を計画している場合を除き、ワイヤレス AP ごとに一意の RADIUS 共有シークレットを使用します。この場合、グループ内のすべての Ap を同じ共有シークレットで構成する必要があります。 共有シークレットは、長さが22文字以上で、大文字と小文字、数字、句読点の両方を持つランダムシーケンスである必要があります。 ランダムな文字生成プログラムを使用して、共有シークレットを作成できます。 各ワイヤレス AP の共有シークレットを記録し、安全な場所 (オフィスの安全な場所) に保管することをお勧めします。 NPS コンソールで RADIUS クライアントを構成する場合は、各 AP の仮想バージョンを作成します。 NPS の各仮想 AP で構成する共有シークレットは、実際の物理 AP の共有シークレットと一致している必要があります。

- **RADIUS サーバーの IP アドレス**。 このアクセスポイントへの接続要求を認証および承認するために使用する NPS の IP アドレスを入力します。

- **UDP ポート\(s\)** 。 既定では、NPS は radius 認証メッセージに UDP ポート1812および1645を、RADIUS アカウンティングメッセージに UDP ポート1813および1646を使用します。 既定の RADIUS UDP ポートの設定は変更しないことをお勧めします。

- **Vsa**。 一部のワイヤレス ap で\-は、 \(完全なワイヤレス AP 機能を提供するために、ベンダー固有の属性 vsa\)が必要です。

- **DHCP フィルタリング**。 ワイヤレスクライアントが UDP ポート68からネットワークに IP パケットを送信するのをブロックするようにワイヤレス Ap を構成します。 ワイヤレス AP のドキュメントを参照して、DHCP フィルタリングを構成します。

- **DNS フィルター**。 ワイヤレスクライアントが TCP または UDP ポート53からネットワークに IP パケットを送信するのをブロックするようにワイヤレス Ap を構成します。 DNS フィルタリングを構成するには、ワイヤレス AP のドキュメントを参照してください。

## <a name="planning-wireless-client-configuration-and-access"></a>ワイヤレスクライアントの構成とアクセスの計画

802.1 x\-で認証されたワイヤレスアクセスの展開を計画するときは\-、いくつかのクライアント固有の要因を考慮する必要があります。

- **複数の標準のサポートを計画して**います。

    ワイヤレスコンピューターがすべて同じバージョンの Windows を使用しているかどうか、または異なるオペレーティングシステムを実行しているコンピューターが混在しているかどうかを確認します。 これらが異なる場合は、オペレーティングシステムでサポートされている標準の違いについて理解しておいてください。

    すべてのワイヤレスクライアントコンピューター上のすべてのワイヤレスネットワークアダプターで同じワイヤレス標準がサポートされているかどうか、またはさまざまな標準をサポートする必要があるかどうかを確認します。 たとえば、一部のネットワークアダプターハードウェアドライバーで WPA2\-enterprise と AES がサポートされているかどうかを確認し、WPA\-enterprise と TKIP だけをサポートします。

- **クライアント認証モードを計画**します。 認証モードは、Windows クライアントがドメイン資格情報を処理する方法を定義します。 ワイヤレスネットワークポリシーでは、次の3つのネットワーク認証モードから選択できます。  

    1. **ユーザーの\-再認証**。 このモードでは、コンピューターの現在の状態に基づいて、常にセキュリティ資格情報を使用して認証が実行されることを指定します。 コンピューターにログオンしているユーザーがいない場合は、コンピューターの資格情報を使用して認証が実行されます。 ユーザーがコンピューターにログオンしている場合、認証は常にユーザーの資格情報を使用して実行されます。  

    2. **[コンピューターのみ]** 。 コンピューターのみのモードでは、常にコンピューターの資格情報のみを使用して認証が実行されることを指定します。  

    3.  **[ユーザー認証]** : ユーザー認証モードは、ユーザーがコンピューターにログオンしたときにのみ認証を実行することを指定します。 コンピューターにログオンしているユーザーがいない場合、認証の試行は行われません。  

- **ワイヤレス制限を計画**する。 ワイヤレスネットワークに同じレベルのアクセス権を持つすべてのワイヤレスユーザーを提供するかどうか、または一部のワイヤレスユーザーのアクセスを制限するかどうかを決定します。 NPS には、特定のワイヤレスユーザーのグループに対して制限を適用できます。  たとえば、特定のグループにワイヤレスネットワークへのアクセスが許可されている特定の日と時間を定義できます。  

- **新しいワイヤレスコンピューターを追加するための計画方法**。 ワイヤレスネットワーク\-を展開する前に、ドメインに参加しているワイヤレス対応コンピューターの場合、コンピューターが 802.1 x によって保護されていないワイヤード (有線) ネットワークのセグメントに接続されていると、ワイヤレス構成設定はワイヤレスネットワーク\(の IEEE 802.11\)ポリシーをドメインコントローラーで構成した後、ワイヤレスクライアントでグループポリシーが更新された後に自動的に適用されます。  

    ただし、まだドメインに参加していないコンピューターの場合は、802.1 x\-認証アクセスに必要な設定を適用する方法を計画する必要があります。 たとえば、次のいずれかの方法を使用して、コンピューターをドメインに参加させるかどうかを決定します。

    1.  コンピューターを 802.1 X で保護されていないワイヤード (有線) ネットワークのセグメントに接続し、コンピューターをドメインに参加させます。

    2.  ワイヤレスユーザーに、独自のワイヤレスブートストラッププロファイルを追加するために必要な手順と設定を指定します。これにより、コンピューターをドメインに参加させることができます。

    3.  ワイヤレスクライアントをドメインに参加させるよう IT スタッフに依頼します。

### <a name="planning-support-for-multiple-standards"></a>複数の標準のサポートの計画

グループポリシーのワイヤレス\(ネットワーク IEEE\) 802.11 ポリシー拡張機能には、さまざまな展開オプションをサポートするためのさまざまな構成オプションが用意されています。

サポートする標準で構成されているワイヤレス ap を展開し、ワイヤレスネットワーク\(の IEEE 802.11\)ポリシーで複数のワイヤレスプロファイルを構成することができます。各プロファイルでは、標準のセットを1つ指定します。必要です。

たとえば、ネットワークに WPA2\-エンタープライズと aes をサポートするワイヤレスコンピューターがある場合、wpa\-enterprise と aes をサポートする他のコンピューター、wpa\-enterprise と TKIP のみをサポートするその他のコンピューターがある場合は、次のようにするかどうかを決定します。

- すべてのコンピューターでサポートされている最も弱い暗号化方法 (この場合は WPA\-Enterprise と TKIP) を使用して、すべてのワイヤレスコンピューターをサポートするように1つのプロファイルを構成します。  
- 2つのプロファイルを構成して、各ワイヤレスコンピューターでサポートされる最適なセキュリティを提供します。 この例では、1つのプロファイルを構成して\(、\-最も強力な\)暗号化 WPA2 enterprise と AES を指定し、\-もう1つのプロファイルで、弱くなる WPA enterprise と TKIP 暗号化を使用します。 この例では、優先順位に従って、WPA2\-Enterprise と AES を使用するプロファイルを配置することが重要です。 WPA2\-enterprise および AES を使用できないコンピューターは、優先順位に従って次のプロファイルに自動的にスキップされ、WPA\-enterprise と TKIP を指定するプロファイルを処理します。

> [!IMPORTANT]
> 接続しているコンピューターは、使用可能な最初のプロファイルを使用するので、プロファイルの順序付きリストで最も安全な標準を持つプロファイルを配置する必要があります。

### <a name="planning-restricted-access-to-the-wireless-network"></a>ワイヤレスネットワークへの制限付きアクセスの計画

多くの場合、ワイヤレスネットワークへのさまざまなレベルのアクセスをワイヤレスユーザーに提供することが必要になる場合があります。 たとえば、一部のユーザーに対して無制限のアクセスを許可したり、1時間ごとに曜日ごとにアクセスを許可したりすることができます。 他のユーザーについては、月曜日から金曜日までのコア時間中にのみアクセスを許可し、土曜日と日曜日のアクセスを拒否することをお勧めします。

このガイドでは、ワイヤレスリソースへの一般的なアクセス権を持つすべてのワイヤレスユーザーをグループに配置するアクセス環境を作成する手順について説明します。 [Active Directory ユーザーとコンピューター] スナップ\-インで1つのワイヤレスユーザーのセキュリティグループを作成し、ワイヤレスアクセスを許可するすべてのユーザーに対して、そのグループのメンバーへのアクセスを許可します。

NPS ネットワークポリシーを構成する場合は、認証を決定するときに NPS が処理するオブジェクトとしてワイヤレスユーザーセキュリティグループを指定します。

ただし、展開でさまざまなレベルのアクセスをサポートする必要がある場合は、次の操作のみを実行する必要があります。  

1. 複数のワイヤレスユーザーセキュリティグループを作成して Active Directory ユーザーとコンピューターに追加のワイヤレスセキュリティグループを作成します。 たとえば、フルアクセス権を持つユーザーと、通常の作業時間にのみアクセス権を持つユーザーのグループ、および要件に一致する他の条件に適合するその他のグループを作成できます。

2. 作成した適切なセキュリティグループにユーザーを追加します。

3. 追加のワイヤレスセキュリティグループごとに追加の NPS ネットワークポリシーを構成し、各グループに必要な条件と制約を適用するようにポリシーを構成します。

### <a name="planning-methods-for-adding-new-wireless-computers"></a>新しいワイヤレスコンピューターを追加するための計画方法

新しいワイヤレスコンピューターをドメインに参加させてからドメインにログオンする場合は、ドメインコントローラーへのアクセス権を持つ LAN セグメントへのワイヤード (有線) 接続を使用して、802.1 X 認証イーサネットスイッチによって保護されていない方法をお勧めします。

ただし、場合によっては、ワイヤード (有線) 接続を使用してコンピューターをドメインに参加させることは実用的でない場合があります。また、ユーザーは、既にドメインに参加しているコンピューターを使用して最初のログオン試行にワイヤード (有線) 接続を使用することもありません。

ワイヤレス接続を使用してコンピューターをドメインに参加させる場合、またはユーザーがドメイン\-に参加しているコンピューターとワイヤレス接続を使用して初めてドメインにログオンする場合は、ワイヤレスクライアントはまず、ワイヤレスネットワークへの接続を確立する必要があります。次のいずれかの方法を使用してネットワークドメインコントローラーにアクセスできるセグメント。

1. **IT スタッフのメンバーは、ワイヤレスコンピューターをドメインに参加させ、シングルサインオンのブートストラップワイヤレスプロファイルを構成します。** この方法では、IT 管理者はワイヤレスコンピューターをワイヤード (有線) イーサネットネットワークに接続し、コンピューターをドメインに参加させます。 その後、管理者がコンピューターをユーザーに配布します。 ユーザーがコンピューターを起動すると、ユーザーログオンプロセス用に手動で指定したドメイン資格情報が、ワイヤレスネットワークへの接続を確立してドメインにログオンするために使用されます。  

2. **ユーザーは、ブートストラップワイヤレスプロファイルを使用してワイヤレスコンピューターを手動で構成し、ドメインに参加します。** この方法では、ユーザーは、IT 管理者からの指示に基づいて、ブートストラップワイヤレスプロファイルを使用してワイヤレスコンピューターを手動で構成します。 ブートストラップワイヤレスプロファイルを使用すると、ユーザーはワイヤレス接続を確立し、コンピューターをドメインに参加させることができます。 コンピューターをドメインに参加させ、コンピューターを再起動した後、ユーザーはワイヤレス接続とそのドメインアカウントの資格情報を使用してドメインにログオンできます。

ワイヤレスアクセスを展開するには、「[ワイヤレスアクセスの展開](e-wireless-access-deployment.md)」を参照してください。
