---
title: リモート デスクトップ仮想化ホストの Windows Server 2016 へのアップグレード
description: この記事では、既存のリモート デスクトップ サービスの展開を Windows Server 2016 にアップグレードする方法について説明します。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: a58231d908ff1ac32eca7d4ba3f1d5a6a18dd7fe
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870568"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>リモート デスクトップ仮想化ホストの Windows Server 2016 へのアップグレード

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>RDS のロールがインストールされたサポート対象の OS のアップグレード
Windows Server 2016 へのアップグレードがサポートされるのは、Windows Server 2012 R2 および Windows Server 2016 TP5 からのみです。

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>VM がローカルに格納されている展開内の RD 仮想化ホスト サーバー
これらのサーバーは、一度にすべてアップグレードする必要があります。 次の手順に従ってアップグレードします。

1. すべてのユーザーをログオフします。
1. 各ホストですべての仮想マシンをオフにするか保存します。 
1. サーバーを Windows Server 2016 にアップグレードします。 
1. アップグレード完了後、すべてのコレクションが使用可能で機能している必要があります。      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>VM がクラスター共有ボリューム (CSV) に格納されている展開内の RD 仮想化ホスト サーバー 

1. RDVH サーバーの一部をアップグレードし、他の一部は Windows Server 2012 R2 上で引き続き VM をホストするアップグレード戦略を決定します。  
2. 元の 2012 R2 クラスターの一部であり続ける "まだアップグレードしない" 他の RDVH サーバーにすべての VM を移行することによって、最初の一区切りのアップグレードの対象となる RDVH サーバーを 1 つ以上分離します。
    1. フェールオーバー クラスター マネージャーを開きます。 
    1. **[役割]** をクリックします。 
    1. 1 つ以上の VM を選択します。 右クリックしてコンテキスト メニューを開きます。 
    1. **[移動]** をクリックし、 **[Live] (ライブ)** または **[クイック マイグレーション]** のいずれかを選択して、最初のアップグレードの一部ではない 1 つ以上の RD 仮想化ホスト サーバーに VM を移動します。 ハードウェアの互換性やオンラインの要件などの要因に応じて、**ライブ**または**クイック**の移行を使用します。 
3. アップグレードに向けて準備した RDVH サーバーを元のクラスターから削除します。 
4. 分離した RDVH サーバーをアップグレードします。 
5. 対象の RDVH サーバーが正常にアップグレードされたら、新しいクラスターと CSV を作成します。これはまったく異なる SAN ボリュームに配置する必要があります。
6. アップグレードしたすべての RDVH サーバーを新しいクラスターに参加させます。 
7. 新しい CSV 内に、既存 CSV 内の既存フォルダー構造を模倣したフォルダー構造を作成します。 ここに、コレクション フォルダーと、各 VM の最上位レベルにあるサブフォルダーを含めます。 
8. 元の CSV 上のさまざまな VM コレクション フォルダーから、/IMGS フォルダーとその内容を、新しい CSV 上の同じ場所にある新しいコレクション フォルダーにコピーします。 
9. ソース RDVH コンピューターでクラスター マネージャーを使用して、高可用性用の VM の構成を削除します。
    1. HPC クラスター マネージャーを起動します。 
    1. **[役割]** をクリックします。 
    1. VM オブジェクトを右クリックし、 **[削除]** をクリックします。 
10. アップグレードしていない RDVH サーバーのいずれかでHYPER-V マネージャーを使用して、アップグレードした RDVH サーバーのいずれかと新しいクラスター CSV にすべての VM を移動します。
    1. Hyper-V マネージャーを開きます。 
    2. アップグレードしていない RDVH サーバーのいずれかを選択します。 
    3. 移動する VM のいずれかを右クリックし、 **[移動]** をクリックします。 
    4. **[仮想マシンを移動する]** を選択し、 **[次へ]** をクリックします。 
    5. **[宛先コンピューターの指定]** ページで、対象となるアップグレードした RDVH サーバーの名前を指定し、 **[次へ]** をクリックします。 
    6. **[仮想マシンのデータを 1 つの場所に移動する]** をクリックし、 **[次へ]** をクリックします。 
    7. 移動先の場所を参照します。 
       > [!IMPORTANT]
       > これが、特定の VM 用の空のフォルダーへのパスであることを確認してください。 

       > [!NOTE]
       > 前述したように、この手順の前に、新しい移動先のサブ フォルダーを作成済みである必要があります。 この手順では、[フォルダーの選択] ダイアログでサブ フォルダーを作成できません。 
    
       **[次へ]** をクリックし、 **[完了]** をクリックします。 
11. VM が再配置されたら、それらをクラスターの**高可用性**オブジェクトとして追加します。
     1. アップグレードされた RD 仮想化ホスト サーバーでフェールオーバー クラスター マネージャーを開きます。 
     1. **[役割]** ノードを右クリックし、 **[役割の構成]** をクリックします。 高可用性ウィザードの **[開始]** ページで **[次へ]** をクリックします。 
     1. 利用できる役割の一覧から **[仮想マシン]** を選択し、 **[次へ]** をクリックします。 構成されていない VM の一覧が表示されます。 
     1. すべての VM を選択します。 **[次へ]** をクリックし、確認ページでもう一度 **[次へ]** をクリックして構成タスクを開始します。  
12. すべての VM を再配置したら、残っている RDVH サーバーをアップグレードします。 上記の手順を使用して、VM の場所のバランスを取ります。

> [!NOTE]  
> クラスター内の異種 HYPER-V サーバーはサポートされていません。 
