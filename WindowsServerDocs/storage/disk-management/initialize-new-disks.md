---
title: 新しいディスクの初期化
description: ディスクの管理を使用して新しいディスクを初期化し、使用できるようにする方法。 問題のトラブルシューティングへのリンクも記載しています。
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7a275c372e1486b26821f797a7663eecbc3e8784
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812428"
---
# <a name="initialize-new-disks"></a>新しいディスクの初期化

> **適用対象:** Windows 10、Windows 8.1、Windows 7、Windows Server (半期チャネル)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

お使いの PC に新しいディスクを追加したが、エクスプローラーに表示されない場合、[ドライブ文字を追加する](change-a-drive-letter.md)か、使用する前にディスクを初期化することが必要な場合があります。 フォーマットされていないドライブのみ初期化できます。 ディスクの初期化では、ディスク上のすべてのデータを消去して Windows で使用できるように準備し、その後ディスクをフォーマットしてディスクにファイルを保存できます。

> [!WARNING]
> 必要なファイルがディスクに保存されている場合は、初期化しないでください。すべてのファイルが失われます。 代わりに、ディスクのトラブルシューティングを行って、ファイルを読み取れるかどうかを確認することをお勧めします。「[ディスクの状態が "初期化されていません" であるか、ディスクが不足している](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing)」を参照してください。

## <a name="to-initialize-new-disks"></a>新しいディスクを初期化するには

ディスクの管理を使用して新しいディスクを初期化する方法を次に示します。 PowerShell を使用する場合、[initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) コマンドレットを代わりに使用します。

1. 管理者のアクセス許可でディスクの管理を開きます。 
 
    これを行うには、タスクバーの検索ボックスに「**ディスクの管理**」と入力し、 **[ディスクの管理]** を長押しし (または右クリックし)、 **[管理者として実行]**  >  **[はい]** を選択します。 管理者として開くことができない場合は、「**コンピューターの管理**」と入力し、 **[記憶域]**  >  **[ディスクの管理]** を選択します。
1. [ディスクの管理] で、初期化するディスクを右クリックし、次に **[ディスクの初期化]** をクリックします (図を参照)。 ディスクが *[オフライン]* として一覧表示されている場合、最初にそれを右クリックし、 **[オンライン]** を選択します。

     一部の USB ドライブについては初期化するオプションはなく、フォーマットして[ドライブ文字](change-a-drive-letter.md)の指定のみが行われます。

    ![未フォーマットのディスクと、[ディスクの初期化] のショートカット メニューを表示する [ディスクの管理]](media/uninitialized-disk.PNG)
2. **[ディスクの初期化]** ダイアログ ボックスで (図を参照)、正しいディスクが選択されていることを確認し、 **[OK]** をクリックして既定のパーティション スタイルを受け入れます。 パーティション スタイルを変更する必要がある場合 (GPT または MBR)、「[パーティション スタイルについて - GPT と MBR](#about-partition-styles---gpt-and-mbr)」を参照してください。

     ディスクの状態は、一時的に **[初期化中]** になった後、 **[オンライン]** 状態になります。 初期化が何らかの理由で失敗する場合、「[ディスクの状態が "初期化されていません" であるか、ディスクが不足している](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing)」を参照してください。

    ![GPT パーティション スタイルが選択されている [ディスクの初期化] ダイアログ ボックス](media/initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>パーティション スタイルについて - GPT と MBR

ディスクは、パーティションと呼ばれる複数のチャンクに分割できます。 各パーティションは (たとえパーティションが 1 つしかない場合でも)、GPT または MBR のいずれかのパーティション スタイルを持つ必要があります。 Windows ではパーティション スタイルを使用して、ディスク上のデータにアクセスする方法が認識されます。

結論から言えば、最近ではほとんどの場合、パーティション スタイルについて気にする必要はなく、Windows が自動的に適切なディスクの種類を使用します。

ほとんどの PC では、ハード ドライブと SSD に対して GUID パーティション テーブル (GPT) のディスクの種類が使用されます。 GPT は堅牢性が高く、2 TB を超えるボリュームに対応できます。 以前のマスター ブート レコード (MBR) のディスクの種類は、32 ビット PC、古い PC、およびメモリ カードなどのリムーバブル ドライブで使用されます。

MBR から GPT に、あるいはその逆にディスクを変換するには、最初にディスクからすべてのボリュームを削除して、ディスク上のすべてのデータを消去する必要があります。 詳細については、「[MBR ディスクを GPT ディスクに変換する](change-an-mbr-disk-into-a-gpt-disk.md)」、または「[GPT ディスクを MBR ディスクに変換する](change-a-gpt-disk-into-an-mbr-disk.md)」を参照してください。