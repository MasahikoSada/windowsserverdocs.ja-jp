---
title: 記憶域レプリカについてよく寄せられる質問
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 89676ba821b99d44865bc6f45c34c05edb771d9d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865256"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>記憶域レプリカについてよく寄せられる質問

>適用対象:Windows Server 2019、Windows Server 2016、Windows Server (半期チャネル)

このトピックには、記憶域レプリカに関してよく寄せられる質問 (FAQ) に対する回答が含まれています。

## <a name="FAQ1"></a>ストレージレプリカは Azure でサポートされていますか?
可能。 Azure では、次のシナリオを使用できます。

1. Azure 内のサーバー間のレプリケーション (1 つまたは2つのデータセンターの障害ドメインにある IaaS Vm 間で同期的または非同期的に、または2つの異なるリージョン間で非同期的に)
2. Azure とオンプレミス間のサーバー間の非同期レプリケーション (VPN または Azure ExpressRoute を使用)
3. Azure 内のクラスター間のレプリケーション (1 つまたは2つのデータセンターの障害ドメインにある IaaS Vm 間で同期的または非同期的に、または2つの異なるリージョン間で非同期的に)
4. Azure とオンプレミス (VPN または Azure ExpressRoute を使用) 間のクラスター間非同期レプリケーション

Azure でのゲストクラスタリングに関するその他の注意事項については、以下を参照してください。[Microsoft Azure に IAAS VM ゲストクラスターをデプロイ](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)しています。

重要な注意事項:

1. Azure では、共有 VHDX ゲストクラスタリングがサポートされていないため、Windows フェールオーバークラスターの仮想マシンでは、従来の共有記憶域の永続的なディスク予約クラスタリングまたは記憶域スペースダイレクトに iSCSI ターゲットを使用する必要があります。
2. 記憶域スペースダイレクトベースの記憶域レプリカのクラスター化には Azure Resource Manager テンプレートがあります。「 [Create a 記憶域スペースダイレクト SOFS クラスターと Azure リージョン間でのディザスターリカバリーのための記憶域レプリカを作成する](https://aka.ms/azure-storage-replica-cluster)」をご覧ください。  
3. クラスターからクラスターへの RPC 通信 (クラスター間のアクセスを許可するためにクラスター Api によって必要) では、CNO に対してネットワークアクセスを構成する必要があります。 Tcp ポート135と TCP ポート49152を超える動的範囲を許可する必要があります。 [AZURE IAAS VM での Windows Server フェールオーバークラスターの構築に関するリファレンス-第2部のネットワークと作成](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/)。  
4. 2ノードのゲストクラスターを使用できます。各ノードでは、記憶域レプリカによってレプリケートされた非対称クラスターにループバック iSCSI を使用します。 しかし、この方法はパフォーマンスが非常に低く、ワークロードやテストが非常に限られている場合にのみ使用してください。  

## <a name="FAQ2"></a>初期同期中にレプリケーションの進行状況を操作方法確認するには  
レプリケーション先のサーバーの記憶域レプリカの管理者イベント ログに表示される Event 1237 メッセージに、コピーされたバイト数および残りのバイト数が 10 秒間隔で表示されます。 1 つまたは複数のレプリケートされたボリュームに対して **\Storage Replica Statistics\Total Bytes Received** が表示されるレプリケーション先で、記憶域レプリカのパフォーマンス カウンターを使用することもできます。 Windows PowerShell を使用して、レプリケーション グループをクエリすることもできます。 たとえば、このサンプル コマンドはレプリケーション先にあるグループの名前を取得し、**Replication 2** という名前の 1 つのグループをクエリして、10 秒間隔で進行状況を表示します。  

```  
Get-SRGroup

do{
    $r=(Get-SRGroup -Name "Replication 2").replicas
    [System.Console]::Write("Number of remaining bytes {0}`n", $r.NumOfBytesRemaining)
    Start-Sleep 10
}until($r.ReplicationStatus -eq 'ContinuouslyReplicating')
Write-Output "Replica Status: "$r.replicationstatus

```  

## <a name="FAQ3"></a>レプリケーションに使用する特定のネットワークインターフェイスを指定できますか。  

はい、`Set-SRNetworkConstraint` を使用して指定できます。 このコマンドレットはインターフェイス層で動作し、クラスター シナリオおよび非クラスター シナリオの両方で使用されます。  
たとえば、(各ノード上の) スタンドアロン サーバーでは次のコマンドを実行します。  

```  
Get-SRPartnership  

Get-NetIPConfiguration  
```  
(両方のサーバーの) ゲートウェイとインターフェイスの情報およびパートナーシップの方向に注意してください。 次に、次のコマンドを実行します。  

```  
Set-SRNetworkConstraint -SourceComputerName sr-srv06 -SourceRGName rg02 -  
SourceNWInterface 2 -DestinationComputerName sr-srv05 -DestinationNWInterface 3 -DestinationRGName rg01  

Get-SRNetworkConstraint  

Update-SmbMultichannelConnection  

```  

ストレッチ クラスター上でネットワークの制約を構成するには、次のコマンドを実行します。  

    Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"

## <a name="FAQ4"></a>1対多のレプリケーションまたは推移的 (A から B から C) へのレプリケーションを構成できますか。  
いいえ。記憶域レプリカは、サーバー、クラスター、またはストレッチクラスターノードの1対1のレプリケーションのみをサポートします。 これは、今後のリリースで変更される可能性があります。 当然ながら、各種サーバーの特定のボリューム ペアを、方向を指定してレプリケーションするよう構成することはできます。 たとえば、サーバー 1 の D ボリュームをサーバー 2 に、E ボリュームをサーバー 3 からレプリケートできます。

## <a name="FAQ5"></a>記憶域レプリカによってレプリケートされたボリュームを拡大または縮小することはできますか。  
ボリュームは、拡大 (拡張) できますが、圧縮できません。 既定では、記憶域レプリカを使うと管理者はレプリケートされたボリュームを拡張できません。サイズ変更前に、ソース グループで `Set-SRGroup -AllowVolumeResize $TRUE` オプションを使ってください。 以下に例を示します。

1. ソースコンピューターに対して使用する:`Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. 希望する方法を使ってボリュームを拡大する
3. ソースコンピューターに対して使用する:`Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>読み取り専用アクセスのために、宛先ボリュームをオンラインにすることはできますか。  
Windows Server 2016 では構成できません。 記憶域レプリカは、レプリケーションが開始されると宛先ボリュームをマウント解除します。 

ただし、バージョン1709以降の Windows Server 2019 および Windows Server 半期チャネルでは、移行先ストレージをマウントするオプションが使用できるようになりました。この機能は "テストフェールオーバー" と呼ばれています。 これを実行するには、現在、宛先でレプリケートされていない、未使用の、NTFS または ReFS でフォーマットされたボリュームが必要です。 次に、レプリケートされた記憶域のスナップショットを、テストやバックアップの目的で、一時的にマウントすることができます。 

たとえば、宛先サーバー "SRV2" のレプリケーション グループ "RG2" 内にボリューム "D:" をレプリケートしており、SRV2 にレプリケートされていない "T:" ドライブがある場合に、テスト フェールオーバーを作成するには、次の操作を実行します。

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
レプリケートされたボリューム D: は SRV2 でアクセスできるようになります。 このファイルに対して読み取りと書き込みを行ったり、ファイルをコピーしたり、安全のために別の場所に保存されているオンラインバックアップを実行したりできます。 T: ボリュームにはログデータのみが含まれます。

テスト フェールオーバーのスナップショットを削除してその変更を破棄するには:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
テスト フェールオーバー機能は、短期的な一時的な操作としてのみ使用する必要があります。 長期的に使用するための機能ではありません。 使用中の場合、レプリケーションは実際の宛先ボリュームまで継続されます。 

## <a name="FAQ7"></a>ストレッチクラスターでスケールアウトファイルサーバー (SOFS) を構成できますか。  
技術的には可能ですが、SOFS に接続するコンピューティングノードにサイト認識がないため、これは推奨される構成ではありません。 通常、待機時間がサブミリ秒になるキャンパス距離ネットワークを使用している場合、この構成は問題なく動作します。   

クラスター間のレプリケーションを構成している場合、記憶域レプリカは、2 つの記憶域クラスター間でのレプリケート中の記憶域スペース ダイレクトの使用も含め、スケールアウト ファイル サーバーを完全にサポートします。  

## <a name="FAQ7.5"></a>CSV は、ストレッチクラスター内またはクラスター間でレプリケートする必要がありますか。  
No. ファイルサーバーの役割など、クラスターリソースによって所有されている CSV または永続的なディスク予約 (民主共和国) を使用してレプリケートできます。 

クラスター間のレプリケーションを構成している場合、記憶域レプリカは、2 つの記憶域クラスター間でのレプリケート中の記憶域スペース ダイレクトの使用も含め、スケールアウト ファイル サーバーを完全にサポートします。  

## <a name="FAQ8"></a>記憶域レプリカを使用するストレッチクラスターで記憶域スペースダイレクトを構成できますか。  
これは、Windows Server でサポートされている構成ではありません。 これは、今後のリリースで変更される可能性があります。 クラスター間のレプリケーションを構成している場合、記憶域レプリカは、記憶域スペース ダイレクトの使用も含め、スケールアウト ファイル サーバーと Hyper-V Server を完全にサポートします。  

## <a name="FAQ9"></a>非同期レプリケーションを構成操作方法には  

`New-SRPartnership -ReplicationMode` を指定して引数 **Asynchronous** を入力します。 既定では、記憶域レプリカのすべてのレプリケーションは同期です。 `Set-SRPartnership -ReplicationMode` でモードを変更することもできます。  

## <a name="FAQ10"></a>ストレッチクラスターの自動フェールオーバーを操作方法できない場合は、  
自動フェールオーバーを回避するには、PowerShell を使用して `Get-ClusterNode -Name "NodeName").NodeWeight=0` を構成します。 これにより、障害復旧サイト内の各ノードの票が削除されます。 その後、プライマリ サイト内のノードの `Start-ClusterNode -PreventQuorum` および障害サイト内のノードの `Start-ClusterNode -ForceQuorum` を使用して、フェールオーバーを強制することができます。 自動フェールオーバーを防止するためのグラフィカルなオプションはなく、自動フェールオーバーの防止は推奨されません。  

## <a name="FAQ11"></a>仮想マシンの回復性を操作方法無効にしますか?
新しい Hyper-v 仮想マシンの回復性機能が実行されないようにして、障害復旧サイトにフェールオーバーするのではなく、仮想マシンを一時停止するには、を実行します。`(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a>初期同期にかかる時間を短縮するにはどうすればよいですか。

初期同期の間を短縮する 1 つの方法として、仮想プロビジョニングされた記憶域を使用できます。 記憶域レプリカは、クエリを実行し、非クラスター化記憶域スペース、Hyper-V の動的ディスク、SAN LUN を含む、仮想プロビジョニングされたストレージを自動的に使用します。  

シードされたデータボリュームを使用して、帯域幅の使用量を減らし、場合によっては、宛先ボリュームがプライマリからのデータの一部を保持し`New-SRPartnership`、フェールオーバークラスターマネージャーまたはのシードオプションを使用するようにすることもできます。 ボリュームがほぼ空の場合は、シード同期を使用すると、時間と使用帯域幅を低減できます。 さまざまな程度の意味でデータをシード処理する方法は複数あります。

1. 以前のレプリケーション-ディスクとボリュームを含むノード間で通常の初期同期をローカルにレプリケートすることにより、レプリケーションを削除し、別の場所に宛先ディスクを配布してから、シードオプションを使用してレプリケーションを追加します。 これは、記憶域レプリカがブロックコピーミラーを保証し、レプリケートするのがデルタブロックだけである場合に最も効果的な方法です。
2. スナップショットの復元またはスナップショットベースのバックアップの復元-ボリュームベースのスナップショットをターゲットボリュームに復元することにより、ブロックレイアウトの違いが最小限に抑えられます。 これは、ボリュームスナップショットがミラー化されているためにブロックが一致する可能性が高いため、次の最も効果的な方法です。
3. コピーされたファイル-転送先に使用されていない新しいボリュームを作成し、データの完全な robocopy/MIR ツリーコピーを実行することで、ブロック一致が発生する可能性があります。 Windows エクスプローラーを使用したり、ツリーの一部をコピーしたりしても、多くのブロック一致は作成されません。 ファイルの手動コピーは、シード処理の最も効果的な方法です。

## <a name="FAQ13"></a>レプリケーションを管理するユーザーを委任できますか。  

`Grant-SRDelegation`コマンドレットを使用できます。 これにより、サーバー間、クラスター間で特定のユーザーを設定して、クラスターのレプリケーション シナリオを、ローカルの管理者グループのメンバーにならずにレプリケーションを作成、変更、削除する権限が付与されているとして拡張することができます。 以下に例を示します。  

    Grant-SRDelegation -UserName contso\tonywang  

このコマンドレットは、変更が有効になるように、管理しようと計画しているサーバーからユーザーがログオフおよびログオンする必要があることを通知します。 `Get-SRDelegation` と `Revoke-SRDelegation` を使用して、さらにこれを制御できます。  

## <a name="FAQ13"></a>レプリケートされたボリュームのバックアップと復元のオプションは何ですか。
記憶域レプリカは、ソース ボリュームのバックアップと復元をサポートします。 さらに、ソース ボリュームのスナップショットの作成と復元もサポートします。 記憶域レプリカによって保護されている間はマウントもアクセスもできないため、レプリケーション先のボリュームをバックアップまたは復元することはできません。 ソース ボリュームが失われる障害が発生した場合、`Set-SRPartnership` を使用して以前のレプリケーション先ボリュームを昇格します。読み取りと書き込みが可能になったソースによって、失われたソース ボリュームをバックアップまたは復元できるようになります。 `Remove-SRPartnership` と `Remove-SRGroup` を使用して、このボリュームを読み取りと書き込みが可能なボリュームとして再マウントすることもできます。
アプリケーション整合性のスナップショットを定期的に作成するために、ソース サーバーで VSSADMIN.EXE を使用して、レプリケートされたデータ ボリュームのスナップショットを作成できます。 たとえば、F: ボリュームを記憶域レプリカでレプリケートしている場合は次のようになります。

    vssadmin create shadow /for=F:
その後、レプリケーションの方向を切り替えた後、レプリケーションを削除した後、または同じソース ボリューム上にいる場合、任意のスナップショットをその時点に復元できます。 たとえば、依然として F: を使用している場合は次のようになります。

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
スケジュールされたタスクを使用して、このツールが定期的に実行されるようにスケジュールすることもできます。 VSS の使用方法の詳細については、[Vssadmin](../../administration/windows-commands/vssadmin.md) を確認してください。 ログ ボリュームのバックアップは不要であり、行う価値もありません。 バックアップしようとすると、VSS によって無視されます。
Windows Server バックアップ、Microsoft Azure Backup、Microsoft DPM、またはその他のスナップショット、VSS、仮想マシン、またはファイルベースのテクノロジの使用は、ボリューム層内で動作する限り、記憶域レプリカでサポートされます。 記憶域レプリカは、ブロックベースのバックアップと復元はサポートしません。

## <a name="FAQ14"></a>帯域幅の使用を制限するようにレプリケーションを構成できますか。
はい、SMB 帯域幅リミッターを介して構成できます。 これは記憶域レプリカのすべてのトラフィックのグローバル設定であるため、このサーバーからのすべてのレプリケーションに影響を与えます。 これは通常、すべてのデータを転送する必要がある、記憶域レプリカの初期同期のセットアップでのみ必要です。 初期同期後に必要な場合は、ネットワーク帯域幅が IO ワークロードに対して狭すぎます。IO を削減するか、帯域幅を拡大してください。

これは非同期レプリケーションでのみ使用する必要があります (注: 初期同期は、同期を指定した場合でも常に非同期になります)。
ネットワーク QoS ポリシーを使用して、記憶域レプリカのトラフィックを形成することもできます。 一致率の高いシード記憶域レプリカのレプリケーションを使用した場合も、全体的な初期同期の帯域幅使用が大幅に削減されます。


帯域幅の制限を設定するには、次のコマンドを使用します。

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

帯域幅の制限を確認するには、次のコマンドを使用します。

    Get-SmbBandwidthLimit -Category StorageReplication

帯域幅の制限を削除するには、次のコマンドを使用します。

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>記憶域レプリカにはどのようなネットワークポートが必要ですか。
記憶域レプリカは、レプリケーションと管理において SMB と WSMAN を利用します。 つまり、次のポートが必要です。

 445 (SMB レプリケーショントランスポートプロトコル、クラスター RPC 管理プロトコル) 5445 (iwarp RDMA ネットワークを使用する場合にのみ必要) 5985 (WMI/CIM/PowerShell 用の WSManHTTP 管理プロトコル)

注:Test-srtopology コマンドレットは、ICMPv4/ICMPv6 を必要としますが、レプリケーションまたは管理には必要ありません。

## <a name="FAQ15.5"></a>ログボリュームのベストプラクティスは何ですか?
ログの最適なサイズは環境やワークロードごとに大きく異なり、ワークロードが実行する書き込み IO の量によって決まります。 

1.  ログのサイズを大きくしたり小さくしたりしても、高速または低速になることはありません。
2.  たとえば、サイズの大きいログまたは小さいログには、10 GB のデータボリュームと 10 TB データボリュームに対する影響はありません。

サイズの大きいログは、単純に、一杯になるまでに多くの書き込み IO を収集して保持できます。これにより、ログのソースと保存先コンピューターの間での、長時間にわたるサービス中断に対処できます (ネットワークの切断や保存先コンピューターのオフラインなど)。 ログが 10 時間分の書き込みを保持できる場合、ネットワークが 2 時間ダウンしても、ネットワークの復旧時にソース側から保存先に対して同期されていない変更の差分を素早く同期すれば、短時間で保護された状態に回復します。 しかし保持されるログが 10 時間分である場合に、ネットワークが 2 日間ダウンすると、ソース側はビットマップと呼ばれる別のログから再生する必要があるため、同期状態への回復に時間がかかります。同期された後は、元どおりログを使用できます。

記憶域レプリカのすべての書き込みパフォーマンスはログに依存します。 ログのパフォーマンスは、レプリケーションのパフォーマンスにとって重要です。 ログ ボリュームには、ログではすべての書き込み IO がシリアル化、シーケンス化されるため、ログ ボリュームはデータ ボリュームよりも高速に動作するデバイスを使用する必要があります。 ログ ボリュームでは、常に SSD などのフラッシュ メディアを使う必要があります。 ログ ボリューム上では、他のワークロードの実行を絶対に許可しないでください。同様に、SQL データベースのログ ボリューム上で他のワークロードの実行を許可しないでください。 

戻すMicrosoft では、ログストレージがデータストレージより高速であること、およびログボリュームを他のワークロードに使用することがないことを強くお勧めします。

テスト用のツールを実行することで、ログのサイズ変更に関する推奨事項を取得できます。 または、既存のサーバーでパフォーマンスカウンターを使用して、ログサイズを略しすることもできます。 この数式は単純です。ワークロードの下にあるデータディスクのスループット (1 秒あたりの平均書き込みバイト数) を監視し、それを使用して、さまざまなサイズのログがいっぱいになるまでの時間を計算します。 たとえば、50 MB/秒のデータディスクスループットによって、120GB のログが 120GB/50 MB 秒または2400秒または40分でラップされます。 そのため、転送されるログが40分になるまで、宛先サーバーに到達できない時間があります。 ログがラップされているにもかかわらず、宛先が再び到達可能になると、ソースはメインログではなくビットマップログを使用してブロックを再生します。 ログのサイズがパフォーマンスに影響を与えることはありません。

ソースクラスターのデータディスクのみをバックアップする必要があります。 バックアップが記憶域レプリカの操作と競合する可能性があるため、記憶域レプリカのログディスクをバックアップしないでください。

## <a name="FAQ16"></a>ストレッチクラスターをクラスター間トポロジとサーバー間トポロジのどちらで選択するのでしょうか。  
記憶域レプリカには、ストレッチクラスター、クラスターからクラスター、サーバー間の3つの主な構成があります。 それぞれの構成には、異なる利点があります。

ストレッチ クラスター トポロジは、Hyper-V プライベート クラウド クラスターや SQL Server FCI など、オーケストレーションを使用した自動フェールオーバーを必要とするワークロードに最適です。 また、フェールオーバー クラスター マネージャーを使用したグラフィカル インターフェイスが組み込まれています。 このトポロジでは、永続的予約を介して、従来の非対称クラスター共有記憶域アーキテクチャである記憶域スペース、SAN、iSCSI、および RAID が使用されます。 これは、2 ノード以上の構成で実行できます。

クラスター間トポロジでは、2 つの独立したクラスターが使用されます。特に 2 番目のサイトが障害回復用にプロビジョニングされていて、日常的な用途に使用されず、手動のフェールオーバーを必要とする場合に最適です。 オーケストレーションは手動で実行します。 ストレッチクラスターとは異なり、記憶域スペースダイレクトはこの構成で使用できます (注意点があります。記憶域レプリカに関する FAQ とクラスター間のドキュメントを参照してください)。 これは、4 ノード以上の構成で実行できます。 

サーバー間トポロジは、クラスター化できないハードウェアを実行しているユーザーに最適です。 この構成では、手動によるフェールオーバーとオーケストレーションが必要です。 ブランチオフィスと中央データセンター (特に非同期レプリケーションを使用する場合) との間の安価なデプロイに最適です。 この構成では、多くの場合、単一マスター障害回復シナリオに使用される DFSR で保護されたファイル サーバーのインスタンスを置き換えることができます。

このトポロジは、すべての場合において、物理ハードウェアでの実行と仮想マシンでの実行の両方をサポートします。 仮想マシンで使用する場合、基盤のハイパーバイザーは Hyper-V である必要はなく、VMware、KVM、Xen なども使用できます。

記憶域レプリカには、同じコンピューター上の 2 つの異なるボリュームへのレプリケーションを指定するサーバー内モードも使用できます。

## <a name="FAQ18"></a>データ重複除去は記憶域レプリカでサポートされていますか?

はい。データ Deduplcation、記憶域レプリカでサポートされています。 移行元サーバーのボリュームでデータ重複除去を有効にすると、レプリケーション中に、移行先サーバーはボリュームの重複除去コピーを受け取ります。

移行元サーバーと移行先サーバーの両方にデータ重複除去を*インストール*する必要があります (「[データ重複除去のインストールと有効化](../data-deduplication/install-enable.md)」を参照してください)。移行先サーバーでデータ重複除去を*有効*にしないことが重要です。 記憶域レプリカは、移行元サーバーでのみ書き込みを許可します。 データ重複除去はボリュームへの書き込みを行うため、ソースサーバー上でのみ実行する必要があります。 

## <a name="FAQ19"></a>Windows Server 2019 と Windows Server 2016 の間でレプリケートできますか。

残念ながら、Windows Server 2019 と Windows Server 2016 の*新しい*パートナーシップの作成はサポートされていません。 Windows Server 2016 を実行しているサーバーまたはクラスターを Windows Server 2019 に安全にアップグレードすることができ、*既存*のパートナーシップは引き続き機能します。

ただし、Windows Server 2019 のレプリケーションパフォーマンスを向上させるには、パートナーシップのすべてのメンバーが Windows Server 2019 を実行している必要があります。また、既存のパートナーシップと関連するレプリケーショングループを削除し、シードされたデータを使用してそれらを再作成する必要があります (Windows 管理センターでパートナーシップを作成する場合、または新しい-SRPartnership コマンドレットを使用して作成する場合。

## <a name="FAQ17"></a>記憶域レプリカまたはこのガイドに関する問題を操作方法報告しますか?  
記憶域レプリカの技術的サポートが必要な場合は、[Microsoft TechNet フォーラム](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview)から投稿できます。 記憶域レプリカに関する質問や、このドキュメントに関する問題は、srfeed@microsoft.com に電子メールを送信することもできます。 <https://windowsserver.uservoice.com> アイデアについては、サポートとフィードバックを提供する、他のお客様が許可されている、サイトが設計変更要求、優先的に使用します。



## <a name="related-topics"></a>関連トピック  
- [記憶域レプリカの概要](storage-replica-overview.md) 
- [共有記憶域を使用した拡張クラスターレプリケーション](stretch-cluster-replication-using-shared-storage.md)  
- [サーバー間の記憶域レプリケーション](server-to-server-storage-replication.md)  
- [クラスターからクラスターへの記憶域のレプリケーション](cluster-to-cluster-storage-replication.md)  
- [記憶域レプリカ:既知の問題](storage-replica-known-issues.md)  

## <a name="see-also"></a>関連項目  
- [ストレージの概要](../storage.md)  
- [記憶域スペース ダイレクト](../storage-spaces/storage-spaces-direct-overview.md)  
