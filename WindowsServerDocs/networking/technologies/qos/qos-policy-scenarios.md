---
title: QoS ポリシーのシナリオ
description: このトピックでは、サービスの品質 (QoS) ポリシーのシナリオについて説明します。このシナリオでは、グループポリシーを使用して、Windows Server 2016 の特定のアプリケーションとサービスのネットワークトラフィックに優先順位を付ける方法を示します。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0968157532c0b3bd926acbaff4291e27a71de31
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871868"
---
# <a name="qos-policy-scenarios"></a>QoS ポリシーのシナリオ

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

このトピックを使用して、QoS ポリシーを使用する方法、タイミング、および理由を示す仮想的なシナリオを確認できます。

このトピックの2つのシナリオは次のとおりです。

1. 基幹業務アプリケーションのネットワークトラフィックの優先順位を設定する
2. HTTP サーバーアプリケーションのネットワークトラフィックの優先順位を設定する

>[!NOTE]
>このトピックのいくつかのセクションでは、説明されている操作を実行するために実行できる一般的な手順について説明します。 QoS ポリシーを管理する方法の詳細については、 [Qos ポリシーの管理](qos-policy-manage.md)に関する説明を参照してください。

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>シナリオ 1:基幹業務アプリケーションのネットワークトラフィックの優先順位を設定する

このシナリオでは、IT 部門が QoS ポリシーを使用して達成できるいくつかの目標があります。

- ミッション\-クリティカルなアプリケーションのネットワークパフォーマンスを向上させます。
- 特定のアプリケーションを使用しているときに、ユーザーのキーセットのネットワークパフォーマンスを向上させます。
- 企業\-全体のデータバックアップアプリケーションで、一度に使用する帯域幅が多すぎると、ネットワークパフォーマンスが低下しないようにしてください。

IT 部門は、差別化されたサービスコードポイント\(の DSCP\)値を使用してネットワークトラフィックを分類し、優先するようにルーターを構成することにより、特定のアプリケーションの優先順位を決定するように QoS ポリシーを構成します。優先順位の高いトラフィックを処理します。 

>[!NOTE]
>DSCP の詳細については、「[サービス品質 (qos) ポリシー](qos-policy-top.md)」の「**差別化サービスコードポイントを使用して QoS の優先順位を定義**する」を参照してください。

QoS ポリシーでは、DSCP 値に加えて、スロットル率を指定できます。 スロットルには、QoS ポリシーに一致するすべての送信トラフィックを特定の送信レートに制限する効果があります。

### <a name="qos-policy-configuration"></a>QoS ポリシーの構成

2つの目標を達成するために、IT 管理者は3つの異なる QoS ポリシーを作成することを決定します。

#### <a name="qos-policy-for-lob-app-servers"></a>LOB アプリサーバーの QoS ポリシー

IT 部門が\-QoS ポリシーを作成する最初のミッションクリティカルなアプリケーションは、企業\-全体のエンタープライズリソース\(計画\) ERP アプリケーションです。 ERP アプリケーションは、Windows Server 2016 を実行している複数のコンピューターでホストされています。 Active Directory Domain Services では、これらのコンピューターは、基幹業務\( \(LOB\) \)アプリケーションサーバー用に作成された組織単位 OU のメンバーです。 ERP アプリケーション\-のクライアント側コンポーネントは、Windows 10 および Windows 8.1 を実行しているコンピューターにインストールされます。

グループポリシーでは、IT 管理者は、QoS \(ポリシー\)が適用されるグループポリシーオブジェクト GPO を選択します。 IT 管理者は、qos ポリシーウィザードを使用して、"Server LOB policy" という qos ポリシーを作成\-します。このポリシーでは、すべてのアプリケーション、IP アドレス、TCP と UDP、およびポート番号について、優先度が高い DSCP 値44が指定されています。

QoS ポリシーは、グループポリシー管理コンソール\(GPMC\)ツールを使用して、これらのサーバーのみを含む OU に GPO をリンクすることで、LOB サーバーにのみ適用されます。 この最初のサーバー LOB ポリシーは、\-コンピューターがネットワークトラフィックを送信するたびに優先順位の高い DSCP 値を適用します。 この QoS ポリシーは、後で\(グループポリシーオブジェクトエディターツール\)で編集して、ERP アプリケーションのポート番号を含めることができます。これにより、指定したポート番号が使用されている場合にのみポリシーが適用されるように制限されます。

#### <a name="qos-policy-for-the-finance-group"></a>Finance グループの QoS ポリシー

企業内の多くのグループが ERP アプリケーションにアクセスしていますが、財務グループは顧客とのやり取り時にこのアプリケーションに依存しており、グループは常に高いパフォーマンスを必要とします。

財務グループが顧客をサポートできるようにするには、QoS ポリシーでこれらのユーザーのトラフィックを優先順位の高いものとして分類する必要があります。 ただし、finance グループのメンバーが ERP アプリケーション以外のアプリケーションを使用する場合、ポリシーは適用されません。 

このため、IT 部門は、finance ユーザーグループが ERP アプリケーションを実行するときに、DSCP 値60を適用する "Client LOB policy" という2番目の QoS ポリシーをグループポリシーオブジェクトエディターツールで定義します。

#### <a name="qos-policy-for-a-backup-app"></a>バックアップアプリの QoS ポリシー

すべてのコンピューターで別のバックアップアプリケーションが実行されています。 バックアップアプリケーションのトラフィックが使用可能なすべてのネットワークリソースを使用していないことを確認するために、IT 部門はバックアップデータポリシーを作成します。 このバックアップポリシーでは、バックアップアプリの実行可能ファイル名に基づいて、バックアップアプリケーションの DSCP 値1を指定します。これは、 **backup .exe**です。 

3番目の GPO が作成され、ドメイン内のすべてのクライアントコンピューターに展開されます。 バックアップアプリケーションがデータを送信するたびに、低優先度の DSCP 値が適用されます。これは、財務部門のコンピューターから発生した場合も同様です。
  
>[!NOTE]
>QoS ポリシーのないネットワークトラフィックは、DSCP 値0で送信されます。

### <a name="scenario-policies"></a>シナリオのポリシー

次の表は、このシナリオの QoS ポリシーをまとめたものです。
  
|ポリシー名|DSCP 値|スロットル率|組織単位に適用済み|説明|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[ポリシーなし]|0|なし|[配置なし]|未分類のトラフィックのベストエフォート (既定) 処理。|  
|バックアップ データ|1|なし|すべてのクライアント|この一括データの優先順位の低い DSCP 値を適用します。|  
|サーバー LOB|44|なし|ERP サーバー用のコンピューター OU|ERP サーバートラフィックに高優先度 DSCP を適用します。|  
|クライアント LOB|60|なし|Finance ユーザーグループ|ERP クライアントトラフィックの優先度の高い DSCP を適用します。|  

>[!NOTE]
>DSCP 値は、10進形式で表されます。

グループポリシーを使用して QoS ポリシーを定義して適用すると、送信ネットワークトラフィックはポリシーによって指定された DSCP 値を受け取ります。 その後、ルーターは、キューを使用して、これらの DSCP 値に基づいて差分処理を提供します。 この IT 部門では、ルーターは、高優先度、中間優先度、ベストエフォート、低優先度の4つのキューで構成されています。

"サーバー LOB ポリシー" および "クライアント LOB ポリシー" からの DSCP 値を使用してルーターでトラフィックが到着すると、優先順位の高いキューにデータが配置されます。 DSCP 値が0のトラフィックは、ベストエフォートレベルのサービスを受け取ります。 DSCP 値が 1 (バックアップアプリケーション) のパケットは、優先度の低い処理を受け取ります。  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>基幹業務アプリケーションに優先順位を適用するための前提条件

このタスクを完了するには、次の要件を満たしていることを確認します。

- 関連するコンピュータは、QoS\-互換のオペレーティングシステムを実行しています。

- 参加しているコンピューターは、グループポリシー \(を\)使用して構成できるように、Active Directory Domain Services AD DS ドメインのメンバーです。

- Tcp/ip ネットワークは、DSCP \(RFC 2474\)用に構成されたルーターで設定されます。 詳細については、 [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt)を参照してください。

- 管理資格情報の要件が満たされています。

#### <a name="administrative-credentials"></a>管理資格情報

このタスクを完了するには、グループポリシーオブジェクトを作成して展開できる必要があります。
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>基幹業務アプリケーションの優先順位を設定するためのテスト環境のセットアップ

テスト環境をセットアップするには、次のタスクを実行します。

- クライアントとユーザーが組織単位にグループ化された AD DS ドメインを作成します。 AD DS の展開手順については、[コアネットワークガイド](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide)を参照してください。

- DSCP 値に基づいて differentially queue にルーターを構成します。 たとえば、DSCP 値44は "プラチナ" キューに入り、その他はすべて重み付けされた-フェアキューに格納されます。

>[!NOTE]
> ネットワークモニターのようなツールでネットワークキャプチャを使用して、DSCP 値を表示できます。 ネットワークキャプチャを実行した後、キャプチャしたデータの TOS フィールドを確認できます。

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>基幹業務アプリケーションの優先順位を示す手順

基幹業務アプリケーションの優先順位を設定するには、次のタスクを実行します。

1. グループポリシーオブジェクト\(GPO\)を作成し、QoS ポリシーを使用してリンクします。

2. 選択した DSCP 値に基づいて、(キューを使用して) 基幹業務アプリケーションを differentially するようにルーターを構成します。 このタスクの手順は、使用しているルーターの種類によって異なります。

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>例 2:HTTP サーバーアプリケーションのネットワークトラフィックの優先順位を設定する

Windows Server 2016 では、ポリシーベースの QoS には、機能の URL ベースのポリシーが含まれています。 URL ポリシーを使用すると、HTTP サーバーの帯域幅を管理できます。

多くのエンタープライズアプリケーションはインターネットインフォメーションサービス\(IIS\) web サーバー用に開発され、ホストされています。 web アプリには、クライアントコンピューターのブラウザーからアクセスします。

このシナリオでは、すべての組織の従業員のトレーニングビデオをホストする一連の IIS サーバーを管理しているとします。 目標は、これらのビデオサーバーからのトラフィックによってネットワークが過負荷にならないようにし、ビデオトラフィックがネットワーク上の音声とデータトラフィックと区別されるようにすることです。 

このタスクは、シナリオ1のタスクに似ています。 ビデオトラフィックの DSCP 値などのトラフィック管理設定を設計および構成し、基幹業務アプリケーションの場合と同じように調整率を構成していきます。 ただし、トラフィックを指定するときは、アプリケーション名を指定する代わりに、HTTP サーバーアプリケーションが応答する URL を入力するだけです。 https://hrweb/training たとえば、のようになります。
  
> [!NOTE]
>URL ベースの QoS ポリシーを使用して、windows 7 および Windows Server 2008 R2 より前にリリースされた Windows オペレーティングシステムを実行しているコンピューターのネットワークトラフィックの優先順位を設定することはできません。

### <a name="precedence-rules-for-url-based-policies"></a>URL ベースのポリシーの優先順位規則

次の Url はすべて有効で、QoS ポリシーで指定して、コンピューターまたはユーザーに同時に適用することができます。

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

しかし、どちらの優先順位が優先されるのでしょうか。 規則は単純です。 URL ベースのポリシーは、左から右への読み取り順序で優先順位が付けられます。 そのため、優先度が最も高い優先順位から最も低い優先度までの URL フィールドは次のとおりです。
  
[1.URL スキーム](#bkmk_QoS_UrlScheme)

[2.URL ホスト](#bkmk_QoS_UrlHost)

[3.URL ポート](#bkmk_QoS_UrlPort)

[4.URL パス](#bkmk_QoS_UrlPath)

詳細は次のとおりです。

####  <a name="bkmk_QoS_UrlScheme"></a> 1.URL スキーム

 `https://`はよりも`https://`優先順位が高くなります。

####  <a name="bkmk_QoS_UrlHost"></a> 2.URL ホスト

 優先順位が最も高いものから順に、次のようになります。

1. hostname

2. IPv6 アドレス

3. IPv4 アドレス

4. ワイルドカード

ホスト名の場合、ドットの付いた要素 (より深い深さ) を持つホスト名は、ドット要素の数が多いホスト名よりも優先順位が高くなります。 たとえば、次のホスト名の中にあります。

- video.internal.training.hr.mycompany.com (深さ = 6)
  
- selfguide.training.mycompany.com (深さ = 4)
  
- トレーニング (深さ = 1)
  
- ライブラリ (深さ = 1)
  
  **video.internal.training.hr.mycompany.com**の優先順位は最も高く、 **selfguide.training.mycompany.com**の優先順位は次に高くなります。 **トレーニング**と**ライブラリ**は、同じ最低優先順位を共有します。  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3.URL ポート

特定のまたは暗黙のポート番号は、ワイルドカードポートよりも優先順位が高くなります。

####  <a name="bkmk_QoS_UrlPath"></a> 4.URL パス

ホスト名のように、URL パスは複数の要素で構成される場合があります。 より多くの要素を持つ1つの方が、より少ない優先順位を持つことになります。 たとえば、次のパスは優先順位によって一覧表示されます。  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

ユーザーが URL パスの後にすべてのサブディレクトリとファイルを含めることを選択した場合、この URL パスの優先度は、選択した場合よりも低くなります。

ユーザーは、URL ベースのポリシーで宛先 IP アドレスを指定することもできます。 宛先 IP アドレスは、前に説明した4つの URL フィールドのいずれかよりも低い優先順位を持ちます。
  
### <a name="quintuple-policy"></a>5つポリシー

5つポリシーは、プロトコル ID、発信元 IP アドレス、発信元ポート、接続先 IP アドレス、および宛先ポートによって指定されます。 5つポリシーは、常に URL ベースのポリシーより優先順位が高くなります。 

5つポリシーが既にユーザーに適用されている場合、新しい URL ベースのポリシーでは、そのユーザーのクライアントコンピューターで競合が発生することはありません。

このガイドの次のトピックについては、 [QoS ポリシーの管理](qos-policy-manage.md)に関する記事をご覧ください。

このガイドの最初のトピックについては、「[サービスの品質 (QoS) ポリシー](qos-policy-top.md)」を参照してください。
