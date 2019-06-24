---
title: エントリ ポイント追加のトラブルシューティング
description: このトピックは、ガイドの一部複数リモート アクセス サーバーの展開で Windows Server 2016 の Multisite 展開します。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 51f49364aa4e7a6da6c51b1d8b7da7e37f842190
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282562"
---
# <a name="troubleshooting-adding-entry-points"></a>エントリ ポイント追加のトラブルシューティング

>適用先:Windows Server 2016 の Windows Server (半期チャネル)

このトピックでは、`Add-DAEntryPoint` コマンドに関連する問題のトラブルシューティング情報を示します。 表示されたエラーがエントリ ポイントの追加に関連していることを確認するには、Windows イベント ログにイベント ID 10067 があることを確認してください。  
  
## <a name="missing-remoteaccessserver-parameter"></a>RemoteAccessServer パラメーターが見つからない  
**表示されるエラー**します。 パラメーター RemoteAccessServer の値を指定する必要があります。  
  
**原因**  
  
新しいエントリ ポイントをマルチサイト展開に追加するときに、パラメーター *RemoteAccessServer* を指定する必要があります。このパラメーターは、新しいエントリ ポイントとして追加するサーバーの名前です。  
  
**ソリューション**  
  
コマンドを実行し、*RemoteAccessServer* パラメーターにエントリ ポイントとして追加するサーバーの名前を指定します。  
  
## <a name="remote-access-is-not-configured"></a>リモート アクセスが構成されていない  
**表示されるエラー**します。 リモート アクセスは < server_name > で構成されていません。 マルチサイト展開に属するサーバーの名前を指定してください。  
  
**原因**  
  
リモート アクセスが *ComputerName* パラメーターで指定されたコンピューター、またはコマンドを実行したコンピューターに構成されていません。  
  
新しいエントリを追加するマルチサイト展開にポイントし、2 つのパラメーターを指定する必要があります。*ComputerName*と*RemoteAccessServer*します。 *ComputerName* は、既にマルチサイト展開に含まれているサーバーの名前で、*RemoteAccessServer* は新しいエントリ ポイントとして追加するサーバーの名前です。 マルチサイト展開の一部であるコンピューターから実行する場合、ComputerName パラメーターは不要です。  
  
**ソリューション**  
  
コマンドを実行し、*ComputerName* パラメーターと、マルチサイト展開の一部として既に構成されているサーバーの名前を指定するか、マルチサイト展開の一部となっているコンピューターからコマンドを実行します。  
  
## <a name="multisite-not-enabled"></a>マルチサイトが有効でない  
**表示されるエラー**します。 この操作を実行する前にマルチサイト展開を有効にする必要があります。 有効にするには、`Enable-DAMultiSite` コマンドレットを使用してください。  
  
**原因**  
  
*ComputerName* パラメーターで指定されたサーバーでマルチサイトが有効になっていません。 新しいエントリ ポイントをリモート アクセスの展開に追加するには、最初にマルチサイトを有効にする必要があります。  
  
**ソリューション**  
  
`Enable-DaMultiSite` コマンドレットを使用してマルチサイトを有効にします。 詳細については、次を参照してください。[マルチサイトのリモート アクセスの展開](https://technet.microsoft.com/library/hh831664.aspx)します。  
  
## <a name="ipv6-prefix-issues"></a>IPv6 プレフィックスの問題  
  
-   **問題 1**  
  
    **表示されるエラー**します。 内部のネットワークで IPv6 が展開されているが、クライアントの IPv6 プレフィックスが指定されていません。  
  
    **原因**  
  
    IPv6 が企業ネットワークに展開されていて、IP-HTTPS プレフィックスが必要ですが、 新しいエントリ ポイントの *ClientIPv6Prefix* パラメーターでプレフィックスが指定されていません。  
  
    **ソリューション**  
  
    1.  一意の IP-HTTPS プレフィックスを新しいエントリ ポイントに割り当て、このプレフィックスの IP アドレスを対象としているパケットが、追加しているサーバーにルーティングされるようにします。  
  
    2.  `Add-DAEntryPoint` コマンドレットを実行し、*ClientIPv6Prefix* パラメーターで IP-HTTPS プレフィックスを指定します。  
  
-   **問題 2**  
  
    **表示されるエラー**します。 クライアントの IPv6 プレフィックスは、既に別のエントリ ポイントで使用されています。 別の値を指定してください。  
  
    **原因**  
  
    *ClientIPv6Prefix* パラメーターで指定された IP-HTTPS プレフィックスは、別のエントリ ポイントで既に使用されています。  
  
    **ソリューション**  
  
    1.  一意の IP-HTTPS プレフィックスを新しいエントリ ポイントに割り当て、このプレフィックスの IP アドレスを対象としているパケットが、追加しているサーバーにルーティングされるようにします。  
  
    2.  `Add-DAEntryPoint` コマンドレットを実行し、*ClientIPv6Prefix* パラメーターで IP-HTTPS プレフィックスを指定します。  
  
## <a name="connectto-address"></a>ConnectTo アドレス  
**表示されるエラー**します。 DirectAccess クライアントがリモート アクセス サーバーに接続するアドレス (< 接続するアドレス >) では、ネットワーク ロケーション サーバーのアドレスと同じです。 別の値を指定してください。  
  
**原因**  
  
ConnectTo アドレスとネットワーク ロケーション サーバーのアドレスが同じです。  
  
**ソリューション**  
  
ConnectTo アドレスをインターネット経由で解決可能にして、クライアント コンピューターが IP-HTTPS 経由で接続できるようにする必要があります。 ネットワーク ロケーション サーバーのアドレスは企業ネットワーク経由で解決可能にし、インターネット経由では解決できないようにします。 ネットワーク ロケーション サーバーと ConnectTo アドレスが同じではないこを確認します。 別のアドレスを選択してやり直します。  
  
## <a name="directaccess-or-vpn-already-installed"></a>DirectAccess または VPN が既にインストールされている  
**表示されるエラー**します。 VPN のインストールは、< server_name > サーバーで検出されました。 リモート アクセスがインストールされていない別のサーバーを指定するか、サーバーから VPN の構成を削除してください。  
  
または  
  
リモート アクセスは < server_name > サーバーに既にインストールされています。 DirectAccess を実行していない別のサーバーを指定するか、サーバーから既存の DirectAccess の構成を削除してください。  
  
**原因**  
  
DirectAccess または VPN は新しいエントリ ポイントに既に構成されています。 構成されたエントリ ポイントはマルチサイト展開に追加できません。  
  
**ソリューション**  
  
サーバーをマルチサイト展開に追加するには、リモート アクセスの役割をサーバーにインストールする必要がありますが、DirectAccess および VPN は構成しないでください。  
  
コマンドを実行し、*RemoteAccessServer* パラメーターで指定したサーバーに DirectAccess または VPN が構成されていないことを確認します。  
  
## <a name="ipsec-root-certificate"></a>IPsec ルート証明書  
**表示されるエラー**します。 サーバー < server_name > には、構成された IPsec ルート証明書を配置することはできません。  
  
**原因**  
  
コンピューター証明書を発行するルート証明機関または中間証明機関 (CA) の証明書は、展開に追加しようとしているサーバー上に見つかりませんでした。  
  
**ソリューション**  
  
リモート アクセス管理コンソールの **[手順 2 リモート アクセス サーバー]** で **[編集]** をクリックし、 **[認証]** ページの **[コンピューターの証明書を使用する]** で、選択された証明書が有効であることを確認します。 証明書が有効な場合は、その証明書が追加するサーバーの信頼されたルート CA にあることを確認してやり直します。  
  
> [!NOTE]  
> 証明書は、同じ拇印の同じ証明書であることが必要です。  
  
証明書が無効な場合は、すべてのリモート アクセス サーバー上の信頼されたルート CA として構成されている有効な証明書を選択します。  
  
## <a name="mixing-ipv6-and-ipv4-entry-points"></a>IPv6 および IPv4 エントリ ポイントが混在している  
DirectAccess が初めてインストールされるときに、内部ネットワーク アダプターが検査されて、ネットワークに含まれているのが IPv4 アドレスのみ (IPv4 専用ネットワーク)、IPv6 アドレスと IPv4 アドレス、または IPv6 アドレスのみ (IPv6 専用ネットワーク) のいずれであるか確認されます。 この情報を使用して、展開の種類 (IPv4 専用、IPv6+IPv4、または IPv6 専用) を確認します。  
  
-   **問題 1**  
  
    **表示される警告**します。 追加するリモート アクセス サーバーは、IPv4 と IPv6 の両方のアドレスで構成されます。 これは IPv4 専用の展開であるため、リモート アクセスでは IPv6 アドレスは無視されます。  
  
    **原因**  
  
    この展開が最初にインストールされたときに、内部ネットワークは IPv4 専用ネットワークとして検出されました。 マルチサイト展開では、各エントリ ポイントがそれぞれ特性の異なるさまざまなサブネットにあると見なされます。 したがって、展開が IPv4 専用の展開として構成されていても、IPv6+IPv4 サブネットにあるエントリ ポイントを含めることができます。 ただし、展開に追加するエントリ ポイントが DirectAccess には、新しいエントリ ポイントの内部インターフェイスに構成された IPv6 アドレスは無視します。  
  
    **ソリューション**  
  
    内部ネットワーク全体に IPv6 アドレスと IPv4 アドレスが構成されている場合は、IPv6+IPv4 展開に移行して IPv6 テクノロジを活用することを検討してください。 「純粋な IPv4 から ipv 6 + IPv4 企業ネットワークに移行」を参照してください[手順 3。マルチサイト展開の計画](assetId:///19d49dbf-1786-47bb-ab97-f0458c53d91d)します。  
  
-   **問題 2**  
  
    **表示されるエラー**します。 このマルチサイト展開でリモート アクセス サーバーの内部ネットワーク アダプターは、IPv4 アドレスで構成されます。 追加するエントリ ポイントも、内部ネットワーク アダプターの IPv4 アドレスで構成する必要があります。  
  
    **原因**  
  
    この展開が最初にインストールされたときに、内部ネットワークは IPv4 専用ネットワークとして検出されました。 リモート アクセスは、追加しようとしているエントリ ポイントの内部ネットワークに IPv6 アドレスのみが構成されていることを検出しました。 これは、IPv4 専用の展開では許可されません。  
  
    **ソリューション**  
  
    ネットワーク全体に IPv6 アドレスが既に構成されている場合は、IPv6+IPv4 または IPv6 専用の展開に移行する必要があります。 「マルチサイトのリモート アクセスが展開されている場合は IPv6 への移行を計画する」を参照してください。  
  
-   **問題 3**  
  
    **表示されるエラー**します。 このエントリ ポイントは、IPv4 ネットワーク内にあるが、前のエントリ ポイントは、IPv6 ネットワークに配置されます。 このエントリ ポイントを同じマルチサイト展開に追加する前に、このエントリ ポイントを IPv6 ネットワークに接続してください。  
  
    **原因**  
  
    この展開が最初にインストールされたときに、内部ネットワークが IPv6+IPv4 または IPv6 専用であることが検出されました。 追加しようとしている新しいエントリ ポイント上の内部ネットワークに IPv4 アドレスのみが構成されていることが検出されました。 これは、IPv6+IPv4 または IPv6 専用の展開では許可されません。  
  
    **ソリューション**  
  
    IPv6 アドレスで新しいエントリ ポイントを構成してから、エントリ ポイントをマルチサイト展開に追加します。  
  
-   **問題 4**  
  
    **表示される警告**します。 リモート アクセス サーバーで内部ネットワーク アダプターが IPv4 アドレスで構成されていません。 このサーバーでは DNS64 と NAT64 は構成されません。 DirectAccess クライアントは IPv6 内部サーバーにのみアクセスできます。  
  
    **原因**  
  
    この展開が最初にインストールされたときに、内部ネットワークが IPv6+IPv4 であることが検出されました。 この展開モードでは、DNS64 と NAT64 が有効になり、クライアント コンピューターは、IPv4 アドレスのみが構成されている内部ネットワーク上のコンピューターにアクセスできます。  
  
    新しいエントリ ポイントを追加するときに、リモート アクセスは、新しいコンピューター上の内部インターフェイスに IPv6 アドレスだけがあることを検出しました。 DNS64 および NAT64 を構成するには、リモート アクセス サーバーまたは IPv4 専用コンピューターへパケットをルーティングするために IPv4 アドレスが必要です。 この IP が新しいコンピューターに存在しないため、NAT64 と DNS64 はリモート アクセス サーバーに構成されません。 したがって、このエントリ ポイントを使用して DirectAccess 経由で企業ネットワークにアクセスしているクライアント コンピューターは、内部ネットワーク上の IPv4 専用サーバーにアクセスできません。 Ipv 6 + IPv4 ネットワークでは、または IPv6 専用ネットワークへ移行する方法については、「マルチサイトのリモート アクセスが展開されている場合は IPv6 への移行を計画する」を参照してください。  
  
    **ソリューション**  
  
    IPv4 アドレスを新しいリモート アクセス サーバーに追加して、DNS64 および NAT64 が正しく動作することを確認します。  
  
## <a name="domain-issues-with-the-servergponame"></a>ServerGpoName でのドメインの問題  
  
-   **問題 1**  
  
    **表示されるエラー**します。 < サーバー > gpo ServerGpoName パラメーターで指定されたドメインが存在しません。 代わりに < domain_name > ドメインを指定します。  
  
    **原因**  
  
    管理者によって送信されたサーバーの GPO 名のドメイン名部分が見つかりませんでした。  
  
    **ソリューション**  
  
    ドメイン名を正しく入力しているか確認します。 ドメイン名が正しく入力されていない場合は、完全修飾ドメイン名 (FQDN) を使用してやり直します。  
  
-   **問題 2**  
  
    **表示されるエラー**します。 サーバー GPO は、リモート アクセス サーバー ドメイン内になければなりません。 ServerGpoName パラメーターでドメイン < domain_name > を指定します。  
  
    **原因**  
  
    サーバーの GPO のドメインが、リモート アクセス サーバーが属しているドメインと異なります。  
  
    **ソリューション**  
  
    サーバーの GPO はリモート アクセス サーバーと同じドメインに存在する必要があります。 サーバーの GPO にサーバーのドメイン名を使用してやり直します。  
  
## <a name="split-brain-dns"></a>スプリット ブレイン DNS  
**表示される警告**します。 < DNS_suffix > DNS サフィックスに NRPT エントリには、リモート アクセス サーバーに接続するクライアント コンピューターで使用されるパブリック名が含まれています。 Nrpt 除外として接続する < アドレス > 名を追加します。  
  
**原因**  
  
スプリット ブレイン DNS を使用しています。 IP-HTTPS を使用してクライアントに接続できるようにするには、選択されている ConnectTo アドレスが NRPT 規則で除外されていることを確認する必要があります。  
  
**ソリューション**  
  
マルチサイト展開がある場合は、異なるエントリ ポイントのアドレスへのすべての接続が NRPT 規則から除外されていることを確認します。  
  
NRPT 規則でアドレスを除外するには  
  
1.  リモート アクセス管理コンソールの **[手順 3 インフラストラクチャ サーバー]** で、 **[編集]** をクリックします。  
  
2.  **インフラストラクチャ サーバーのセットアップ** ウィザードの **[DNS]** ページで、テーブルをダブルクリックし、新しい名前サフィックスを入力します。  
  
3.  **[DNS サーバー アドレス]** ダイアログ ボックスの [DNS サフィックス] に、エントリ ポイントの ConnectTo アドレスを入力し、 **[適用]** をクリックします。  
  
サーバーのアドレスを指定せずに、名前サフィックスを追加すると、サフィックスは NRPT 除外として扱われます。  
  
## <a name="saving-server-gpo-settings"></a>サーバーの GPO 設定の保存  
**表示されるエラー**します。 リモート アクセスの設定を GPO < gpo 名 > に保存中にエラーが発生しました。  
  
このエラーをトラブルシューティングするでのサーバー GPO 設定の保存をご覧ください。[を有効にするマルチサイトのトラブルシューティング](https://technet.microsoft.com/library/jj591658.aspx)します。  
  
## <a name="gpo-updates-cannot-be-applied"></a>GPO の更新が適用されない  
**表示される警告**します。 < Server_name > では、GPO の更新を適用できません。 次のポリシーの更新まで、変更は有効になりません。  
  
**原因**  
  
指定されたコンピューターでポリシーを更新しようとしているときにエラーが発生しました。 したがって、次のポリシーの更新まで、変更は有効になりません。  
  
**ソリューション**  
  
ポリシーを強制的に更新するには、指定されたコンピューターで `gpupdate /force` を実行します。  
  

