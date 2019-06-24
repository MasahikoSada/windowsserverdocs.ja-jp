---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: SQL Server を使用するフェデレーション サーバー ファーム
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c6aa91956f4a90b32b82e6c970e68b3164c732f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191712"
---
# <a name="ad-fs-requirements"></a>AD FS の要件

AD FS を展開するときに準拠する必要があるさまざまな要件を次に示します。  
  
-   5. コマンド プロンプトで、「[ipconfig /all](AD-FS-Requirements.md#BKMK_1)」と入力して Enter キーを押します。  
  
-   [ハードウェア要件](AD-FS-Requirements.md#BKMK_2)  
  
-   [ソフトウェア要件](AD-FS-Requirements.md#BKMK_3)  
  
-   [AD DS の要件](AD-FS-Requirements.md#BKMK_4)  
  
-   [構成データベースの要件](AD-FS-Requirements.md#BKMK_5)  
  
-   [ブラウザーの要件](AD-FS-Requirements.md#BKMK_6)  
  
-   [エクストラネットの要件](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [ネットワークの要件](AD-FS-Requirements.md#BKMK_7)  
  
-   [属性ストアの要件](AD-FS-Requirements.md#BKMK_8)  
  
-   [アプリケーションの要件](AD-FS-Requirements.md#BKMK_9)  
  
-   [認証の要件](AD-FS-Requirements.md#BKMK_10)  
  
-   [ワークプ レース ジョインの要件](AD-FS-Requirements.md#BKMK_11)  
  
-   [暗号化要件](AD-FS-Requirements.md#BKMK_12)  
  
-   [アクセス許可の要件](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>証明書の要件  
証明書は、セキュリティで保護するフェデレーションの間の通信サーバー、Web アプリケーション プロキシでは、要求で最も重要な役割を果たします\-対応のアプリケーションと Web クライアントです。 証明書の要件は、かどうかを設定する、フェデレーション サーバーまたはプロキシ コンピューターでは、このセクションで説明したによって異なります。  
  
**フェデレーション サーバーの証明書**  
  
|||  
|-|-|  
|**証明書の種類**|**要件、サポートと注意事項**|  
|**Secure Sockets Layer \(SSL\)証明書。** これは、フェデレーション サーバーとクライアント間の通信を保護するために使用される標準の SSL 証明書です。|-この証明書は、公的に信頼されたである必要があります\*X509 v3 証明書。<br />-すべてのクライアントの AD FS のエンドポイントにアクセスするには、この証明書を信頼する必要があります。 パブリックに発行される証明書を使用する強くお勧めします\(3 番目\-パーティ\)証明機関\(CA\)します。 自己を使用する\-SSL 証明書を正常にフェデレーション サーバー、テスト ラボ環境での署名します。 ただし、運用環境は、パブリック CA から証明書を入手することを勧めします。<br />-SSL 証明書を Windows Server 2012 R2 でサポートされる任意のキー サイズをサポートしています。<br />-は CNG キーを使用するサポート証明書されません。<br />-ワークプ レース ジョインと併用する場合\/デバイス登録サービスでは、AD FS サービスの SSL 証明書のサブジェクト代替名はユーザープリンシパル名が続く値enterpriseregistrationを含める必要があります\(UPN\) enterpriseregistration.contoso.com など、組織のサフィックス。<br />のワイルドカード証明書がサポートされています。 AD FS ファームを作成するときに求め、AD FS サービスのサービスの名前を指定する\(など**adfs.contoso.com**します。<br />の Web アプリケーション プロキシに同じ SSL 証明書を使用することを強くお勧めします。 ただしこれは**必要**Web アプリケーション プロキシを経由し、拡張保護認証をオンにすると、Windows 統合認証エンドポイントをサポートするときに同じである\(既定の設定\).<br />-この証明書のサブジェクト名を使用してを展開する AD FS の各インスタンスのフェデレーション サービス名を表します。 このため、新しい CA でサブジェクト名の選択を検討することがあります\-最も的確に表す、自社または自組織のパートナーの名前の証明書を発行します。<br />    証明書の id は、フェデレーション サービス名と一致する必要があります\(fs.contoso.com など\)します。Id は、dNSName タイプのいずれか、サブジェクト代替名拡張をまたは、サブジェクト名は、共通名として指定されたサブジェクト代替名のエントリがない場合。 サブジェクト代替名の複数のエントリは、フェデレーション サービス名と一致する 1 つ提供される、証明書に存在することができます。<br />-   **重要:** が強く、AD FS ファーム内のすべての Web アプリケーション プロキシと AD FS ファームのすべてのノードの間で同じ SSL 証明書を使用するお勧めします。|  
|**サービス通信証明書:** この証明書は、フェデレーション サーバー間の通信のセキュリティを確保するための WCF メッセージ セキュリティに必要です。|-既定では、SSL 証明書は、サービス通信証明書として使用されます。  サービス通信証明書として別の証明書を構成するオプションもあります。<br />-   **重要:** SSL 証明書の有効期限が切れるときに、SSL 証明書をサービス通信証明書として使用されている場合は、サービス通信証明書として更新された SSL 証明書を構成することを確認してください。 これは自動的に発生するされません。<br />-この証明書は、WCF メッセージ セキュリティを使用する AD FS のクライアントによって信頼する必要があります。<br />パブリックによって発行されたサーバー認証証明書を使用することをお勧めします- \(3 番目\-パーティ\)証明機関\(CA\)します。<br />-サービス通信証明書は、CNG キーを使用する証明書にすることはできません。<br />-この証明書は、AD FS 管理コンソールを使用して管理できます。|  
|**トークン\-署名証明書。** これは標準の X509 証明書であり、フェデレーション サーバーが発行するすべてのトークンについて署名のセキュリティを確保するために使用されます。|-既定では、AD FS は自己を作成します\-2048 ビット キーを持つ証明書に署名します。<br />CA の発行された証明書もサポートされ、AD FS 管理スナップインを使用して変更できます\-で<br />CA の証明書の発行は、格納されている & CSP 暗号化プロバイダーを使用してアクセスする必要があります。<br />-トークン署名証明書は、CNG キーを使用する証明書にすることはできません。<br />AD FS では、トークンの署名証明書が登録されている外部は必要ありません。<br />    AD FS を自動的に更新これらセルフ\-署名証明書の有効期限が切れる前に、最初に新しい証明書を構成するパートナーを使用するためにセカンダリ証明書としてし自動と呼ばれるプロセスでプライマリに切り替える証明書のロール オーバーします。トークンの署名証明書を自動的に生成された、既定値を使用することをお勧めします。<br />    Powershell を使用して、インストール時に、証明書を指定するには、組織にトークンの署名を構成するさまざまな証明書を必要とするポリシーがある場合は、\(インストールの – SigningCertificateThumbprint パラメーターを使用\-AdfsFarm コマンドレット\)します。  インストール後に、表示および AD FS 管理コンソールまたは Powershell コマンドレットのセットを使用して、トークン署名証明書を管理することができます\-AdfsCertificate と Get\-AdfsCertificate します。<br />    トークン署名の外部で登録されている証明書を使用している場合、証明書の自動更新またはロール オーバー AD FS は実行しません。  このプロセスは、管理者によって実行する必要があります。<br />    1 つの証明書が期限切れ間近の近くにあるときに、証明書のロール オーバーできるように、AD FS でセカンダリ トークン署名証明書を構成できます。 既定では、すべてのトークン署名証明書は、フェデレーション メタデータがプライマリのトークンだけに公開された\-実際にトークンの署名に AD FS によって使用されるが署名証明書。|  
|**トークン\-復号化\/暗号化証明書。** これは、標準の X509 証明書を復号化に使用される\/受信トークンを暗号化します。 これは、フェデレーション メタデータでも公開されています。|-既定では、AD FS は自己を作成します\-2048 ビット キーを持つ証明書に署名します。<br />CA の発行された証明書もサポートされ、AD FS 管理スナップインを使用して変更できます\-で<br />CA の証明書の発行は、格納されている & CSP 暗号化プロバイダーを使用してアクセスする必要があります。<br />-トークン\-復号化\/暗号化証明書は、CNG キーを使用する証明書をすることはできません。<br />-既定では、AD FS 生成し、は、独自生成された内部や self\-トークン復号化証明書を署名します。  AD FS では、この目的の外部で登録されている証明書は必要ありません。<br />    さらに、AD FS を自動的に更新これらセルフ\-署名証明書の有効期限が切れる前にします。<br />    **トークン復号化証明書を自動的に生成された、既定値を使用することをお勧めします。**<br />    Powershell を使用して、インストール時に、証明書を指定するには、組織は、トークン復号化を構成するさまざまな証明書を必要とするポリシーが、\(の – DecryptionCertificateThumbprint パラメーターを使用して、インストール\-AdfsFarm コマンドレット\)します。  インストール後に、表示および AD FS 管理コンソールまたは Powershell コマンドレットのセットを使用してトークン暗号化解除証明書の管理\-AdfsCertificate と Get\-AdfsCertificate します。<br />    **トークン復号化の外部で登録されている証明書を使用している場合、AD FS は、証明書の自動更新を行いません。管理者はこのプロセスを実行する必要があります**します。<br />-AD FS サービス アカウントは、トークンへのアクセスが必要\-ローカル コンピューターの個人用ストアに証明書の秘密キーを署名します。 これは隠メ諶セットアップします。 AD FS 管理スナップインを使用することもできます。\-の後に、トークンを変更した場合、このアクセスを確保する\-署名証明書。|  
  
> [!CAUTION]  
> トークンに使用される証明書\-署名とトークン\-復号化\/暗号化は、フェデレーション サービスの安定性に重要です。 お客様は、独自のトークンを管理\-署名 & トークン\-復号化\/暗号化の証明書をこれらの証明書がバックアップと利用できない個別に復旧イベント中を確認してください。  
  
> [!NOTE]  
> AD FS では、セキュリティで保護されたハッシュ アルゴリズムを変更できます\(SHA\)か SHA にデジタル署名に使用されるレベル\-1 または SHA\-256\(より安全な\)します。 AD FS が MD5 などの他のハッシュ方式と一緒に証明書の使用をサポートしていません\(Makecert.exe コマンドで使用される既定のハッシュ アルゴリズム\-ライン ツール\)します。 セキュリティのベスト プラクティスとして SHA を使用することをお勧めします。\-256\(既定で設定されている\)のすべての署名。 SHA\-SHA を使用して通信をサポートしない製品と相互運用する必要があるシナリオでのみ使用する 1 をお勧め\-非などの 256\-Microsoft 製品またはレガシ バージョンの AD FS します。  
  
> [!NOTE]  
> CA から証明書を受け取ったら、すべての証明書がローカル コンピューターの個人証明書ストアにインポートされていることを確認してください。 証明書 MMC スナップインを使用して個人用ストアに証明書をインポートできる\-でします。  
  
## <a name="BKMK_2"></a>ハードウェア要件  
次の最小および推奨ハードウェア要件は、Windows Server 2012 R2 で AD FS フェデレーション サーバーに適用されます。  
  
||||  
|-|-|-|  
|**ハードウェア要件**|**最小要件**|**推奨要件**|  
|CPU 速度|1.4 GHz 64\-ビットのプロセッサ|クアッド\-コア、2 GHz|  
|RAM|512 MB|4 GB|  
|ディスク領域|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>ソフトウェアの要件  
次の AD FS 要件は、Windows Server® 2012 R2 オペレーティング システムに組み込まれているサーバー機能のためです。  
  
-   エクストラネット アクセスの場合、Web アプリケーション プロキシ役割サービスをデプロイする必要があります\-Windows Server® 2012 R2 のリモート アクセス サーバーの役割の一部です。 Windows Server® 2012 R2 で AD FS では、フェデレーション サーバー プロキシの以前のバージョンはサポートされていません。  
  
-   フェデレーション サーバーと Web アプリケーション プロキシ役割サービスは、同じコンピューターにインストールすることはできません。  
  
## <a name="BKMK_4"></a>AD DS の要件  
**ドメイン コント ローラーの要件**  
  
2008 またはそれ以降に、ユーザーのすべてのドメインと AD FS サーバーが参加しているドメインのドメイン コント ローラーが Windows Server を実行する必要があります。  
  
> [!NOTE]  
> Windows Server 2003 ドメイン コント ローラーと環境のすべてのサポートは、拡張サポート終了日の Windows Server 2003 の後に終了します。 お客様が、そのドメイン コント ローラーをできるだけ早くアップグレードする強くお勧めします。 参照してください[このページ](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)マイクロソフト サポート ライフ サイクルの詳細についてはします。 問題が見つかった Windows Server 2003 ドメイン コント ローラー環境を特定、修正プログラムが発行されますセキュリティの問題に関してのみ、拡張サポートを Windows Server 2003 の有効期限の前に修正プログラムが実行できるかどうかは。  
  
**ドメインの機能\-レベルの要件**  
  
すべてのユーザー アカウントのドメインと AD FS サーバーが参加しているドメインは、Windows Server 2003 以降のドメイン機能レベルで動作する必要があります。  
  
AD FS のほとんどの機能には、AD DS の機能は不要\-レベルが正常に動作を変更します。 ただし、クライアント証明書認証の証明書が明示的に AD DS 内のユーザー アカウントにマッピングされている場合に認証を正しく動作させるには、ドメイン機能レベルが Windows Server 2008 以上であることが必要です。  
  
**スキーマの要件**  
  
-   AD FS にスキーマ変更が必要としない機能または\-レベルの AD DS に変更します。  
  
-   ワークプ レース ジョインの機能を使用するには、Windows Server 2012 R2 に参加している AD FS サーバーのフォレストのスキーマを設定する必要があります。  
  
**サービス アカウントの要件**  
  
-   任意の標準のサービス アカウントは、AD FS のサービス アカウントとして使用できます。 グループ管理サービス アカウントもサポートされます。 これにより、少なくとも 1 つのドメイン コント ローラーが必要です。 \(2 つ以上デプロイすることをお勧め\)Windows Server 2012 またはそれ以上実行します。  
  
-   Kerberos 認証のドメイン間で関数を\-クライアントと AD FS では、参加している、' ホスト\/< adfs\_サービス\_名 >' としてサービス アカウントの SPN を登録する必要があります。 既定では、AD FS はこれを構成、新しい AD FS ファームを作成する場合、この操作を実行するための十分なアクセス許可があります。  
  
-   AD FS サービスを認証するユーザーを含むすべてのユーザー ドメインに、AD FS サービス アカウントを信頼する必要があります。  
  
**ドメインの要件**  
  
-   すべての AD FS サーバーは、AD DS ドメインに参加する必要があります。  
  
-   ファーム内のすべての AD FS サーバーは、1 つのドメインにデプロイする必要があります。  
  
-   ドメインに参加している AD FS サーバーでは、AD FS サービスを認証するユーザーを含むすべてのユーザー アカウントのドメインを信頼する必要があります。  
  
**マルチ フォレストの要件**  
  
-   ドメインに参加している AD FS サーバーには、すべてのユーザー アカウントのドメインまたは AD FS サービスを認証するユーザーを含むフォレストを信頼する必要があります。  
  
-   AD FS サービスを認証するユーザーを含むすべてのユーザー ドメインに、AD FS サービス アカウントを信頼する必要があります。  
  
## <a name="BKMK_5"></a>構成データベースの要件  
要件を次に、構成ストアの種類に基づいて適用される制限事項。  
  
**WID**  
  
-   WID ファームでは、100 以下の証明書利用者のパーティ信頼がある場合、30 台のフェデレーション サーバーの制限があります。  
  
-   WID の構成データベースでは、SAML 2.0 の成果物の解像度のプロファイルはサポートされていません。  トークン リプレイ検出は、WID の構成データベースでサポートされていません。 この機能は AD FS のフェデレーション プロバイダーとして機能する方法と外部の要求プロバイダーからセキュリティ トークンの使用場所のシナリオでのみ使用のみ。  
  
-   フェールオーバーの中央に個別のデータでの AD FS サーバーを展開またはサーバーの数が 30 を超えない限り、地理的な負荷分散がサポートされています。  
  
次の表は、WID ファームの使用に関する概要を示します。  実装を計画するのにには、これを使用します。  
  
||||  
|-|-|-|  
||1 \- 100 の RP 信頼|100 を超える RP 信頼|  
|1 \- 30年の AD FS ノード|WID のサポート|WID を使用してサポートされていません\-のために必要な SQL|  
|複数の 30年の AD FS ノード|WID を使用してサポートされていません\-のために必要な SQL|WID を使用してサポートされていません\-のために必要な SQL|  
  
**SQL Server**  
  
Windows Server 2012 R2 で AD fs、SQL Server 2008 以降を使用することができます。  
  
## <a name="BKMK_6"></a>ブラウザーの要件  
ブラウザーまたはブラウザー コントロールを使用して AD FS の認証が実行されると、お使いのブラウザーは、次の要件に従う必要があります。  
  
-   JavaScript を有効にする必要があります。  
  
-   Cookie をオンする必要があります。  
  
-   Server Name Indication \(SNI\)サポートする必要があります  
  
-   ユーザーの証明書とデバイスの証明書認証\(ワークプ レース ジョイン機能\)ブラウザーは SSL クライアント証明書認証をサポートする必要があります  
  
いくつかの主要ブラウザーとプラットフォームでのレンダリングや機能の詳細については以下の検証が行われています。 ブラウザーと次の表で説明していないデバイスは上記の要件を満たしている場合に引き続きサポートします。  
  
|||  
|-|-|  
|**ブラウザー**|**プラットフォーム**|  
|IE 10.0|Windows 7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2|  
|IE 11.0|Windows 7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2|  
|Windows Web Authentication Broker|Windows 8.1|  
|Firefox \[v21\]|Windows 7、Windows 8.1|  
|Safari \[v7\]|iOS 6、Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7、Windows 8.1、Windows Server 2012、Windows Server 2012 R2 では、Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> 既知の問題\-Firefox:デバイスの証明書を使用してデバイスを識別するワークプ レース ジョイン機能では、Windows プラットフォームで機能しません。 Firefox は、SSL クライアント証明書認証を実行する Windows クライアントでユーザー証明書ストアにプロビジョニング証明書の使用を現在サポートしていません。  
  
**Cookie**  
  
AD FS は、セッションを作成します。\-サインインを提供するクライアント コンピューターに保存する必要がありますベースおよび永続の cookie\-では、署名\-、シングル サインオン\-で\(SSO\)、およびその他の機能。 したがって、クライアント ブラウザーは Cookie を受け入れるように設定されている必要があります。 認証のために使用される cookie は、常に Secure Hypertext Transfer Protocol \(HTTPS\)元のサーバー向けに作成されたセッション cookie です。 クライアント ブラウザーがこの Cookie を受け入れるように設定されていない場合は、AD FS が正しく動作することができません。 永続的 Cookie は、ユーザーが選択したクレーム プロバイダーを記憶するために使用されます。 AD FS のサインオンの構成ファイルで構成設定を使用して無効にできます\-ページ。 TLS のサポート\/SSL がセキュリティ上の理由が必要です。  
  
## <a name="BKMK_extranet"></a>エクストラネットの要件  
AD FS サービスへのエクストラネット アクセスを提供するには、必要がありますとして展開する、Web アプリケーション プロキシ役割サービス、エクストラネットに公開されたロール認証要求をプロキシする AD FS サービスのセキュリティで保護された方法で。 これにより、AD FS サービスのエンドポイントの分離とセキュリティ キーのすべての分離\(トークン署名証明書など\)をインターネットからの要求。 さらに、ソフトのエクストラネット アカウント ロックアウトなどの機能には、Web アプリケーション プロキシの使用が必要です。 Web アプリケーション プロキシの詳細については、次を参照してください。 [Web アプリケーション プロキシ](https://technet.microsoft.com/library/dn584107.aspx)します。  
  
3 つ目を使用したい場合\-、エクストラネット アクセス用のプロキシを第 3 にこのパーティ\-製のプロキシで定義されているプロトコルをサポートする必要があります[http:\/\/download.microsoft.com\/のダウンロード\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bms\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>ネットワークの要件  
次のネットワーク サービスを適切に構成するは、お客様の組織で AD FS の展開を成功させるため重要です。  
  
**企業のファイアウォールを構成します。**  
  
Web アプリケーション プロキシとフェデレーション サーバー ファームと、クライアントと Web アプリケーション プロキシの間のファイアウォール間にある、両方のファイアウォールで TCP ポート 443 を有効になっている必要があります受信します。  
  
さらに、クライアント ユーザー証明書認証で\(X509 を使用し、clientTLS 認証ユーザー証明書\)は Windows Server 2012 R2 で AD FS 必要、必要な TCP ポート 49443 を有効にする間のファイアウォールで受信用に、クライアントと Web アプリケーション プロキシ。 これは、Web アプリケーション プロキシとフェデレーション サーバーの間のファイアウォールでは必要ありません\)します。  
  
**DNS の構成**  
  
-   AD FS にアクセスするすべてのクライアントが企業内部ネットワーク内でサービスをイントラネット アクセスで\(イントラネット\)AD FS サービス名を解決できる必要があります\(SSL 証明書が指定した名前\)負荷AD FS サーバーまたは AD FS サーバーのバランサーです。  
  
-   エクストラネット アクセスは、AD FS にアクセスするすべてのクライアント サービスから企業ネットワークの外部\(エクストラネット\/インターネット\)AD FS サービス名を解決できる必要があります\(のSSL証明書が指定した名前\) Web アプリケーション プロキシ サーバーまたは Web アプリケーション プロキシ サーバーのロード バランサーにします。  
  
-   正常に機能するエクストラネット アクセスは、DMZ 内の各 Web アプリケーション プロキシ サーバーで AD FS サービス名を解決できる必要があります\(SSL 証明書が指定した名前\)AD FS サーバーまたは AD FS サーバーのロード バランサーにします。 これは、DMZ ネットワークまたはホスト ファイルを使用してローカル サーバーの解像度を変更することで、代替の DNS サーバーを使用して実現できます。  
  
-   Web アプリケーション プロキシを通じて公開されるエンドポイントのサブセットのネットワークの外部ネットワーク内で動作する Windows 統合認証の場合は、A レコードを使用する必要があります\(CNAME ではない\)ロード バランサーを指すようにします。  
  
フェデレーション サービスの会社の DNS とデバイス登録サービスを構成する方法の詳細については、次を参照してください。[フェデレーション サービスおよび DRS 企業 DNS を構成する](https://technet.microsoft.com/library/dn486786.aspx)します。  
  
Web アプリケーション プロキシの会社の DNS を構成する方法の詳細については、「DNS を構成する」セクションを参照してください。[手順 1。Web アプリケーション プロキシ インフラストラクチャを構成する](https://technet.microsoft.com/library/dn383644.aspx)します。  
  
クラスターの IP アドレスを構成またはクラスターの FQDN NLB を使用する方法についてでのクラスター パラメーターの指定を参照してください[http:\/\/go.microsoft.com\/fwlink\/でしょうか。LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282)します。  
  
## <a name="BKMK_8"></a>属性ストアの要件  
AD FS には、ユーザーを認証し、そのユーザーのセキュリティ要求を抽出するために少なくとも 1 つの属性ストアが必要です。 AD FS をサポートしているストアは、「属性の一覧で、参照してください[The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)します。  
  
> [!NOTE]  
> AD FS は、既定では、"Active Directory"属性ストアを自動的に作成されます。 属性ストアの要件は、組織がアカウント パートナーとして機能するかどうかに依存\(フェデレーション ユーザーをホストしている\)またはリソース パートナー\(フェデレーション アプリケーションをホストしている\)します。  
  
**LDAP 属性ストア**  
  
その他のライトウェイト ディレクトリ アクセス プロトコルを使用するときに\(LDAP\)\-ベースの属性ストア、Windows 統合認証をサポートする LDAP サーバーに接続する必要があります。 LDAP 接続文字列は、RFC 2255 での説明に従い、LDAP URL の形式で記述されている必要があります。  
  
また、AD FS サービスのサービス アカウントに、LDAP 属性ストアでユーザー情報を取得する権限が必要です。  
  
**SQL Server 属性ストア**  
  
適切に動作する Windows Server 2012 R2 で AD fs、コンピューター、SQL Server 属性ストアをホストする必要があります実行する Microsoft SQL Server 2008 またはそれ以降。 SQL を使用するときに\-ベースの属性ストアもに、接続文字列を構成する必要があります。  
  
**カスタム属性ストア**  
  
複雑なシナリオを実現するために、独自の属性ストアを開発することができます。  
  
-   AD FS 内蔵のポリシー言語からカスタム属性ストアを参照できるので、次に示すようなシナリオの拡張が可能になります。  
  
    -   ローカルで認証されるユーザーのクレームを作成する  
  
    -   外部で認証されるユーザーのクレームを補足する  
  
    -   トークンを取得することをユーザーに許可する  
  
    -   ユーザーの行動に基づいてトークンを取得することをサービスに許可する  
  
    -   証明書利用者に AD FS によって発行されたセキュリティ トークンの追加データを発行します。  
  
-   .NET 4.0 の上に、またはそれ以降、すべてのカスタム属性ストアを構築する必要があります。  
  
カスタム属性ストアを使用する場合は、接続文字列を構成する必要がありますもします。 その場合は、カスタム属性ストアに接続できる任意のカスタム コードを入力することができます。 このような状況で接続文字列は、一連の名前を\/値のペアは、カスタム属性ストアの開発者によって実装されるように解釈されます。開発と、カスタム属性ストアを使用する方法の詳細については、次を参照してください。[属性ストアの概要](https://go.microsoft.com/fwlink/?LinkId=190782)します。  
  
## <a name="BKMK_9"></a>アプリケーションの要件  
AD FS の要求をサポートしている\-次のプロトコルを使用するアプリケーションに注意してください。  
  
-   WS\-フェデレーション  
  
-   WS\-信頼  
  
-   SAML 2.0 プロトコルが IDPLite、SPLite & eGov1.5 プロファイルを使用します。  
  
-   OAuth 2.0 Authorization Grant プロファイル  
  
AD FS もサポートしている認証と承認のいずれかの非\-クレーム\-Web アプリケーション プロキシでサポートされている対応するアプリケーション。  
  
## <a name="BKMK_10"></a>認証の要件  
**AD DS の認証\(プライマリ認証\)**  
  
イントラネット アクセスでは、次の AD DS の標準的な認証メカニズムがサポートされています。  
  
-   Kerberos と NTLM のネゴシエートを使用した Windows 統合認証  
  
-   フォーム認証のユーザー名を使用して\/パスワード  
  
-   AD DS 内のユーザー アカウントにマップされている証明書を使用して証明書認証  
  
エクストラネット アクセスの場合、次の認証メカニズムがサポートされています。  
  
-   フォーム認証のユーザー名を使用して\/パスワード  
  
-   AD DS 内のユーザー アカウントにマップされている証明書を使用して証明書認証  
  
-   ネゴシエートを使用して Windows 統合認証\(NTLM のみ\)WS の\-Windows 統合認証を受け入れるエンドポイントを信頼します。  
  
証明書の認証。  
  
-   Pin が保護されていることができるスマート カードを拡張します。  
  
-   ユーザーは pin を入力する GUI では、AD FS によって提供されていないと、クライアント TLS を使用するときに表示されるクライアント オペレーティング システムの一部である必要です。  
  
-   リーダーと暗号化サービス プロバイダー \(CSP\)スマート カードは、ブラウザーが配置されているコンピューターで作業する必要があります。  
  
-   スマート カード証明書は、すべての AD FS サーバーと Web アプリケーション プロキシ サーバーの信頼されたルートまでチェーンする必要があります。  
  
-   証明書は、AD DS 内のユーザー アカウントに、次のいずれかの方法でマッピングされている必要があります。  
  
    -   証明書サブジェクト名が、AD DS 内のユーザー アカウントの LDAP 識別名に対応している。  
  
    -   証明書のサブジェクト別名拡張は、ユーザー プリンシパル名を持つ\(UPN\)の AD DS 内のユーザー アカウント。  
  
Windows 統合認証をシームレスに Kerberos を使用して、イントラネット内  
  
-   信頼済みサイトまたはローカル イントラネット サイトの一部としてサービス名の必要があります。  
  
-   さらに、ホスト\/< adfs\_サービス\_名 > で、AD FS ファームが実行されているサービス アカウントの SPN を設定する必要があります。  
  
**マルチ\-要素認証**  
  
AD FS は、追加の認証をサポートしている\(AD DS でサポートされているプライマリ認証を超える\)というベンダーのプロバイダー モデルを使用\/構築できる、独自のマルチ\-factor authentication アダプター管理者は登録し、ログイン時に使用できます。  
  
すべての MFA アダプターは、.NET 4.5 の上に構築する必要があります。  
  
MFA の詳細については、次を参照してください。[機密性の高いアプリケーションの追加の多要素認証によるリスク管理](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)します。  
  
**デバイスの認証**  
  
AD FS では、自分のデバイスを参加させる、エンド ユーザー ワークプ レースの act の中に、デバイス登録サービスによってプロビジョニング証明書を使用してデバイス認証をサポートします。  
  
## <a name="BKMK_11"></a>ワークプ レース ジョインの要件  
エンドユーザーには、AD FS を使用して、組織に自分のデバイスのワークプ レース ジョインことができます。 これは、AD FS でデバイス登録サービスによってサポートされます。 その結果、エンドユーザーは、AD FS でサポートされているアプリケーション間での SSO の追加特典を取得します。 さらに、管理者は、社内組織に参加しているデバイスのみにアプリケーションへのアクセスを制限することでリスクを管理することができます。 このシナリオを有効にするのには、次の要件を以下に示します。  
  
-   AD FS は、Windows 8.1 と iOS 5 ワークプ レース ジョインをサポートしている\+デバイス  
  
-   ワークプ レース ジョインの機能を使用するには、フォレストに参加している AD FS サーバーのスキーマは、Windows Server 2012 R2 をある必要があります。  
  
-   AD FS サービスの SSL 証明書のサブジェクト代替名は、ユーザー プリンシパル名が続く値 enterpriseregistration を含める必要があります\(UPN\)組織のサフィックスenterpriseregistration.corp.contoso.com します。  
  
## <a name="BKMK_12"></a>暗号化要件  
次の表は、AD FS トークン署名、トークンの暗号化に追加の暗号化のサポート情報を提供します\/復号化の機能。  
  
||||  
|-|-|-|  
|**アルゴリズム**|**キーの長さ**|**プロトコル\/アプリケーション\/コメント**|  
|TripleDES – 既定 192 \(192 ~ 256 のサポート\) \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|セキュリティ トークンの復号化のサポートされているアルゴリズムです。 このアルゴリズムでセキュリティ トークンを暗号化することはサポートされていません。|  
|AES128 \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes128\-cbc|128|セキュリティ トークンの復号化のサポートされているアルゴリズムです。 このアルゴリズムでセキュリティ トークンを暗号化することはサポートされていません。|  
|AES192 \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes192\-cbc|192|セキュリティ トークンの復号化のサポートされているアルゴリズムです。 このアルゴリズムでセキュリティ トークンを暗号化することはサポートされていません。|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**[既定]** 。 セキュリティ トークンを暗号化するためのサポートされているアルゴリズムです。|  
|(Tripledeskeywrap) \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-tripledes|.NET 4.0 でサポートされるすべてのキー サイズ\+|セキュリティ トークンを暗号化する対称キーの暗号化アルゴリズムがサポートされています。|  
|(Aes128keywrap) \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|セキュリティ トークンを暗号化する対称キーの暗号化アルゴリズムがサポートされています。|  
|AES192KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|セキュリティ トークンを暗号化する対称キーの暗号化アルゴリズムがサポートされています。|  
|AES256KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|セキュリティ トークンを暗号化する対称キーの暗号化アルゴリズムがサポートされています。|  
|RsaV15KeyWrap \- http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-1\_5|1024|セキュリティ トークンを暗号化する対称キーの暗号化アルゴリズムがサポートされています。|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|既定。 セキュリティ トークンを暗号化する対称キーの暗号化アルゴリズムがサポートされています。|  
|SHA1\-http:\/\/www.w3.org\/PICS\/DSig\/SHA1\_1\_0 html|N\/A|成果物ソースの生成では、AD FS サーバーによって使用されます。このシナリオでは、STS は SHA1 を使用。 \(SAML 2.0 標準で推奨事項ごと\)成果物ソースの短い 160 ビットの値を作成します。<br /><br />また、ADFS web エージェントで使用される\(WS2003 期間からのレガシ コンポーネント\)「最終更新」に変更を識別するために値を STS からの情報を更新するタイミングを認識できるようにします。|  
|SHA1withRSA\-<br /><br />http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0 html|N\/A|AD FS サーバーは、SAML AuthenticationRequest の署名を検証するときに、ケースで使用される、成果物の解決の要求または応答の署名、トークン作成\-署名証明書。<br /><br />このような場合は、SHA256 が既定値、および場合にのみ、SHA1 が使用パートナー\(証明書利用者\)SHA256 をサポートすることはできませんし、SHA1 を使用する必要があります。|  
  
## <a name="BKMK_13"></a>アクセス許可の要件  
インストールと AD FS の初期構成を実行する管理者では、ローカル ドメインのドメイン管理者のアクセス許可が必要\(に、フェデレーション サーバーが参加しているドメインつまり、します。\)  
  
## <a name="see-also"></a>関連項目  
[Windows Server 2012 R2 での AD FS 設計ガイド](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  
