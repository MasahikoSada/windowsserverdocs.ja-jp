---
title: Windows Server Essentials を実行しているサーバーの復元または修復
description: Windows Server Essentials を使用する方法について説明します
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5618eb95fb8afcff2057575191699da05612a542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433065"
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Windows Server Essentials を実行しているサーバーの復元または修復

>適用先:Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
 
 このトピックでは、概要とサポートされる復元または Windows Server Essentials を実行しているサーバーの修復手順を提供し、次のセクションが含まれます。  
  
-   [サーバー システム復元の概要](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [復元するか、システム ドライブの修復](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [サーバーのファイルとフォルダーを復元します。](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a> サーバー システム復元の概要  
 復元を実行するときのサーバーの状態が、使用できる復元方法と、どれだけ包括的に復元を実行できるかに影響します。  
  
 サーバーを復元する最も一般的な理由として、次のような理由があります。  
  
- サーバー上のウイルスを駆除または削除できない。  
  
- サーバーの構成設定が正しくないためにサーバーを起動できない。  
  
- システム ドライブを交換した。  
  
- サーバーの使用を中止して、新しいサーバーに復元する。  
  
  バックアップからサーバーを復元するか、サーバーを出荷時の既定の設定に戻すことができます。  
  
- [バックアップからサーバーを復元します。](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
- [サーバーを出荷時の設定にリセットします。](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a> バックアップからサーバーを復元します。  
 このセクションでは、どのタイプのバックアップを選択する必要があるかについてガイダンスを示します。  
  
 バックアップを使用できる場合は、製造元のインストール メディアを使用して外部バックアップから復元するが、サーバーを復元するため、最適です。 復元を実行すると、サーバーの設定とフォルダーが、選択したバックアップから回復されます。 バックアップ後に作成した設定とデータは、構成、復元する必要があります。  
  
 以前のバックアップからの復元によってサーバーを回復することを選択した場合、復元する特定のバックアップを選択する必要があります。また、サーバーに直接接続された外部ハード ドライブ上に有効なバックアップ ファイルが必要です。  
  
- **正常に終了したごく最近のサーバーのバックアップが存在し**、バックアップに重要なすべてのデータが含まれていることが確認できている場合、この方法を選択すると作業が簡単になります。 必要なのは、最後の正常なバックアップ以降に作成したデータを再作成し、バックアップ以降に変更した設定を再構成することだけです。  
  
- **ウイルスが原因でサーバーを復元する場合は**、ウイルスに感染するより前に実行したことが確認できているバックアップを選択してください。 感染していないバックアップを選択するために何日間かさかのぼることが必要な場合があります。  
  
- **構成設定が正しくないという理由でサーバーを復元する場合は**、サーバー上で問題の原因になっている構成設定の変更よりも前に実行したことが確認できているバックアップを選択してください。  
  
  バックアップから復元する場合、厳密なプロセスと必要なフォローアップは、サーバー上のハード ドライブの数と、システム ドライブを交換するかどうかによって異なります。  
  
- **サーバーのハード ドライブが 1 個で、そのドライブを交換しない場合は**、サーバーを復元すればドライブ パーティション情報はそのまま残っています。 システム ボリュームが復元され、残りのボリュームのデータは保持されています。  
  
- **サーバーのハード ドライブが 1 個で、そのドライブを交換する場合は**、システム ボリュームは復元されますが、フォルダーをデータ ボリュームに手動で復元する必要があります。 既定以外の共有フォルダーは、サーバー記憶域が再作成されても作成されません。そのため、作成する必要があります。  
  
- **サーバーに複数のハード ドライブが存在し、ドライブ 0 (システム ボリュームを含む) を交換しない場合は**、サーバーを復元するとドライブ パーティション情報はそのまま残っています。 システム ボリュームが復元され、残りすべてのボリューのデータは保持されています。  
  
- **サーバーに複数のハード ドライブが存在し、ドライブ 0 (システム ボリュームを含む) を交換する場合は**、システム ボリュームは復元されますが、以前にドライブ 0 に格納した共有フォルダーを手動で復元する必要があります。  
  
###  <a name="BKMK_FactoryReset"></a> サーバーを出荷時の設定にリセットします。  
 復元のソースとなるバックアップがない場合、または他の何らかの理由で、以前のサーバー構成を復元するのではなくシステム全体の復元の実行が望まれる、または要求される場合は、サーバーを出荷時の既定の設定に戻す復元を実行できます。このためには、サーバーのハードウェア製造元から提供されているインストール メディアまたは回復メディアを使用します。  
  
 出荷時の既定の設定に戻すことによってサーバーを復元する場合、サーバー上の既存の設定とインストール済みアプリケーションはすべて削除され、サーバーを再度構成する必要があります。 出荷時の設定に戻った後、サーバーは再起動します。  
  
 出荷時の設定に戻すとき、データを保持するか削除するかを選択できます。選択は次のように影響します。  
  
-   すべてのデータを保持するように選択した場合、システム ボリューム上のすべてのデータは削除されますが、その他のボリューム上のデータは保持されます。  
  
    > [!CAUTION]
    >  ディスクの設定が既定の設定と一致しない場合、ディスク上のすべてのデータは保持されます。 システム ディスクを交換する場合、新しいディスクが元のディスクのシステム ボリュームより大きくする必要があります。  
    >   
    >  システム ドライブのパーティション情報を読み取ることができない場合、またはシステム ドライブを交換した場合、システム ドライブ上のすべてのデータが削除されます (すべてのデータを保持するように選択した場合でも、削除されます)。  
  
-   サーバーの使用を停止するか用途を変更することを計画している場合は、すべてのデータを削除するよう選択してください。 システム ボリューム上のサーバー構成、他の設定、およびデータに加えて、その他すべてのデータも削除され、サーバーのすべてのハード ドライブが再フォーマットされます。  
  
> [!NOTE]
>  サーバーに記憶域スペースが構成されている場合は、出荷時の設定に戻す前に、 **[記憶域の管理]** コンソールの **[詳細設定]** セクションを使用して、すべての記憶域スペースを手動で削除する必要があります。  
  
 出荷時の設定に戻した後、次のタスクを実行する必要があります。  
  
-   **サーバーを再構成します。** サーバーで、サーバーの構成ウィザードを使用して構成設定を再入力します。 クライアント コンピューターからリモートに管理される Windows Server Essentials サーバーを構成する web ブラウザーを開き、入力 **http://***<YourServerName\>* アドレス バーにします。  
  
-   **クライアント コンピューターをサーバーに再接続します。** コンピューターがサーバーに接続されていた場合は、もう一度サーバーにコンピューターを接続する前に、コンピューターから、Windows Server Essentials コネクタ ソフトウェアをアンインストールする必要があります。 詳細については、「 [Uninstall the Connector software](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) 」と「 [Connect computers to the server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)」を参照してください。  
  
##  <a name="BKMK_Restore"></a> 復元するか、システム ドライブの修復  
 サーバー復元の最初のステップは、サーバー システム ドライブの復元または修復です。 システム ドライブを復元した後、サーバーのデータ ドライブを復元するために必要なことを行い、復元中に失われた共有があれば復元する必要があります。  
  
 復元を実行するために 3 つの方法を使用できます。  
  
-   [Restore or repair your server using installation media](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) の順にクリックします。 サーバー製造元から提供されるインストール メディアを使用して、バックアップから復元します。  
  
-   **インストール メディアを使用してサーバーを既定の出荷時の設定に戻します**。 サーバーでこれを行う方法については、サーバー製造元のドキュメントを参照してください。  
  
-   [回復 DVD を使用してクライアント コンピューターからサーバーを復元またはリセット](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)します。 Windows Server Essentials を実行しているリモート管理サーバーを復元する必要がある場合は、サーバー製造元から提供される DVD を使用してクライアント コンピューターから復元を実行する必要があります。  
  
###  <a name="BKMK_Restore_1"></a> 復元またはインストール メディアを使用して、サーバーの修復  
 次の手順では、Windows Server Essentials のインストール メディアを使用して、バックアップからサーバー システム ドライブを復元する方法について説明します。 (インストール メディアを使用して出荷時の既定の設定に戻す方法については、サーバー製造元のドキュメントを参照してください。)  
  
> [!NOTE]
>  サーバーが記憶域スペースを使用して、新しいサーバーにデータを復元する場合は、する必要があります最初に、システム ドライブの回復、Windows Server Essentials ダッシュ ボードにログオンし、同様の方法として、前のサーバー上で記憶域スペースを構成し復旧 datボリュームの場合。  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>インストール メディアを使用してバックアップからサーバー システム ドライブを復元するには  
  
1.  サーバーの DVD ドライブに Windows Server Essentials のインストール DVD を挿入する、サーバーを再起動および DVD から起動する任意のキーを押します。  
  
    > [!NOTE]
    >  復元プロセスが自動的に開始しない場合は、サーバーの BIOS 設定を調べ、DVD ドライブがブート メニューの最初に表示されていることを確認してください。  
  
     - または -  
  
     製造元によってインストール メディアがサーバーにあらかじめ読み込まれている場合は、スタートアップ時に F8 キーを押すと、インストール メディアから起動します。  
  
2.  Windows Server ファイルが読み込まれた後、言語とその他の基本設定を選択し、 **[次へ]** をクリックします。  
  
3.  ウィザードの次のページで、 **[コンピューターの修復]** をクリックします。  
  
    > [!CAUTION]
    >  **[今すぐインストール]** オプションは選択しないでください。 そのオプションを選択すると、システムの完全インストールの手順が表示されます。この手順では、システム ドライブ上のすべての構成設定とすべてのデータが削除されます。  
  
4.  **[オプションの選択]** ページで、 **[トラブルシューティング]** をクリックします。  
  
5.  **システム イメージの回復**ページで、現在のシステムを選択しますか? か**Windows Server Essentials**または**Windows Server Essentials**します。  
  
     コンピューター イメージの再適用ウィザードが開きます。  
  
6.  **[システム イメージ バックアップの選択]** ページでは、最新のバックアップを使用するように選択することも、以前のバックアップを選択することもできます。 システムは、システムの復元または修復の目的で選択したバックアップの時点の状態に復元されます。 バックアップが保存された後にデータを追加した場合または設定を変更した場合は、そのデータや設定を再作成する必要があります。  
  
     次のいずれかのオプションを選択し、 **[次へ]** をクリックします。  
  
    -   **利用可能なシステム イメージのうち最新のものを使用する (推奨)**  
  
    -   **システム イメージを選択する**  
  
    > [!NOTE]
    >  正常に終了したごく最近のサーバーのバックアップが存在し、バックアップに重要なすべてのデータが含まれていることが確認できている場合、この方法を選択すると作業が簡単になります。 必要なのは、最後の正常なバックアップ以降に作成したデータを再作成し、バックアップ以降に変更した設定を再構成することだけです。  
    >   
    >  ウイルスが原因でサーバーを復元する場合は、ウイルスに感染するより前に実行したことが確認できているバックアップを選択してください。 感染していないバックアップを選択するために何日間かさかのぼることが必要な場合があります。  
    >   
    >  構成設定が正しくないという理由でサーバーを復元する場合は、サーバー上で問題の原因になっている構成設定の変更よりも前に実行したことが確認できているバックアップを選択してください。  
  
7.  ウィザードの手順に従って、システムの復元を完了します。  
  
8.  サーバーが正常に復元された後、インストール DVD を使用した場合は DVD を取り出してから、サーバーを再起動します。  
  
> [!NOTE]
>  サーバーにフォルダーを復元して共有するには、追加の手順を実行する必要があります。 詳細については、「 [Restore files and folders on the server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)」を参照してください。  
  
###  <a name="BKMK_Restore_2"></a> 復元または回復 DVD を使用してクライアント コンピューターからサーバーをリセット  
 Windows Server Essentials ですることができます、サーバー起動可能な USB フラッシュ ドライブから、作成したとし、回復するサーバー、クライアント コンピューターから回復 DVD をサーバー製造元から受け取ったを使用しています。 クライアント コンピューターは、サーバーと同じネットワーク上に存在している必要があります。 このメソッドは、Windows Server Essentials でご利用いただけません。  
  
 次の手順は、サーバーの復元を実行する一般的なステップを示しています。 これらのステップは、バックアップから復元する場合も、出荷時の既定の設定に戻す場合も、同じように適用できます。 より具体的な手順については、サーバー製造元のドキュメントを参照してください。  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>回復 DVD を使用してクライアント コンピューターからサーバーを復元またはリセットするには  
  
1.  クライアント コンピューターで、サーバー製造元から入手した Windows Server Essentials サーバー回復メディアを挿入します。  
  
     サーバーの回復ウィザードが開きます。  
  
2.  ウィザードに表示される手順に従って、起動可能な USB フラッシュ ドライブを作成します。作成したドライブは、回復モードでサーバーを起動するために使用します。  
  
3.  サーバーの回復ウィザードで再起動可能な USB フラッシュ ドライブが準備できたら、フラッシュ ドライブをサーバーに挿入し、回復モードでサーバーを起動します。 サーバーを回復モードで起動する方法については、サーバー ハードウェアの製造元から提供されるドキュメントを参照してください。  
  
     サーバーを回復モードで起動した後、サーバーの回復ウィザードでサーバーの場所が特定され、接続が確立されます。  
  
4.  ウィザードの手順に従ってサーバーの復元を完了します。  
  
> [!NOTE]
>  このサーバー復元方法では、復元中、サーバーに接続されている外部記憶装置は無視されます。 外部規則装置のデータを消去する必要がある場合は、手動で消去する必要があります。  
  
> [!NOTE]
>  サーバーに追加の共有フォルダーを作成していた場合は、バックアップからデータを復元した後、追加の共有フォルダーがサーバーで認識されない可能性があります。 もう一度これらのフォルダーを共有する必要があります。 詳細については、「 [Restore files and folders on the server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)」を参照してください。  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a> サーバーのファイルとフォルダーを復元します。  
 サーバーを復元または修復するために使用した方法、およびサーバーで使用している記憶域の種類により、システム ドライブを復元した後にデータ ボリュームを回復することが必要になる場合があります。 場合によっては、既存のフォルダーを、サーバーで認識されるようにもう一度共有する必要があります。  
  
 例として、次のような場合にファイルおよびフォルダーの復元が必要になる可能性があります。  
  
-   [ファイルとフォルダーをサーバーのバックアップから復元する](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup)場合。 システム ディスクを交換した場合、またはシステム ディスク上のパーティション情報を読み取ることができない場合、システムは復元できますが、このディスク上の他のボリュームからデータを復元することはできません。 他のデータ ボリュームからファイルとフォルダーを復元するには、ファイルまたはフォルダーの復元ウィザードを使用する必要があります。  
  
-   [サーバー上の共有フォルダーを復元する](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders)場合。 サーバーに追加の共有フォルダーを作成していた場合は、バックアップからシステム ドライブを復元した後、共有フォルダーはまだデータ パーティションに存在しているか、データ パーティションに復元済みですが、サーバーで認識されない可能性があります。 もう一度これらのフォルダーを共有する必要があります。  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a> サーバー バックアップからファイルとフォルダーを復元します。  
 ファイルまたはフォルダーの復元ウィザードを使用すると、ハード ディスクが動作しなくなった場合またはファイルを誤って消去した場合に備えてデータを保護できます。 Windows Server Essentials のバックアップ、ハード ドライブ上のすべてのデータのコピーを作成でき、外部記憶装置にデータを格納できます。 ハード ドライブ上の元のデータを誤って消去した場合や上書きした場合、またはドライブ異常のためデータにアクセスできなくなった場合は、バックアップからデータを復元できます。 ファイルまたはフォルダーの復元ウィザードを使用すると、1 個のファイルまたはフォルダー、複数のファイルまたはフォルダー、あるいはハード ドライブ全体を既存のバックアップから復元できます。  
  
 システム復元後、ファイルまたはフォルダーの復元ウィザードを使用して、復元されなかったファイルとフォルダーを復元することが必要になる場合があります。 たとえば、システム ディスクを交換した場合、またはシステム ディスクのパーティション情報を読み取ることができない場合、システム ディスクの他のボリュームからデータを復元することはできません。  
  
> [!NOTE]
>  ファイルまたはフォルダーの復元ウィザードを使用して完全なシステム ドライブを復元することはできません。 システム全体の復元方法の詳細については、「 [Restore or repair your server using installation media](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) 」または「 [Restore or reset your server from a client computer using the recovery DVD](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2)」を参照してください。  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>ファイルとフォルダーをサーバーのバックアップから復元するには  
  
1.  Windows Server Essentials ダッシュ ボードを開き、クリックして、**デバイス**タブ。  
  
2.  サーバー名をクリックし、 **作業** ウィンドウで **[サーバーのファイルまたはフォルダーの復元]** をクリックします。  
  
     ファイルまたはフォルダーの復元ウィザードが開きます。  
  
3.  ウィザードの指示に従って、ファイルまたはフォルダーを復元します。  
  
> [!WARNING]
>  バックアップして、ファイルとフォルダーの復元の詳細については、次を参照してください。 [Manage Backup and Restore](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)します。  
  
###  <a name="BKMK_ConfigreSharedFolders"></a> サーバー上の共有フォルダーを復元します。  
 共有フォルダーがまだデータ パーティションまたはデータのパーティションに復元された場合に、サーバー システム ドライブを復元した後は、サーバー フォルダーを認識するための順序でもう一度共有フォルダーを構成する必要があります。 次の手順では、以前に共有されていた共有フォルダーを追加する方法を説明します。  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>既存のフォルダーをサーバー共有フォルダーに追加するには  
  
1.  エクスプローラーで、ハード ドライブ上の共有フォルダーを特定します。  
  
2.  共有フォルダーを右クリックし、 **[プロパティ]** をクリックします。次に **[共有]** タブをクリックし、フォルダーのアクセス許可を書き留めます。  
  
3.  Windows Server Essentials ダッシュ ボードにログオン をクリックして、**ストレージ**タブをクリックし、をクリックし、**フォルダーを追加する**で、**サーバー フォルダーのタスク**ウィンドウ。  
  
     フォルダーの追加ウィザードが開きます。  
  
4.  **[名前]** ボックスに共有の名前を入力します。  
  
5.  クリックして**参照**に移動します *< ドライブ\>\\< ServerName\>* \ServerFolders (たとえば*d:\Contoso\ServerFolders*) をクリックして、共有するフォルダーを選択します**OK**します。  
  
6.  **[次へ]** をクリックします。  
  
7.  ステップ 2 で書き留めたアクセス許可を指定し、 **[フォルダーの追加]** をクリックします。  
  
    > [!IMPORTANT]
    >  これらのアクセス許可は、Windows Server Essentials ダッシュ ボードを使用していないフォルダーに追加された既存のアクセス許可に置き換えられます。  
  
> [!IMPORTANT]
>  共有フォルダーの一覧にフォルダーを追加した後、フォルダーがサーバーのバックアップに含まれていることを確認してください。 サーバーのバックアップにフォルダーを追加する方法については、「[サーバー バックアップのセットアップまたはカスタマイズ](Set-up-or-customize-server-backup.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目  
  
-   [バックアップの管理と復元](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Windows Server Essentials の管理](Manage-Windows-Server-Essentials.md)  
  
-   [Windows Server Essentials の使用](../use/Use-Windows-Server-Essentials.md)