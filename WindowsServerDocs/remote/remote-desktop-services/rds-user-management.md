---
title: RDS コレクションでのユーザーの管理
description: リモート デスクトップ サービスでユーザーを管理する方法について説明します。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: ff782bc4d01709f56d19ee3e9a06a95267cf7a12
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870743"
---
# <a name="manage-users-in-your-rds-collection"></a>RDS コレクションでのユーザーの管理

>適用対象:Windows Server (半期チャネル)、Windows Server 2019、Windows Server 2016

管理者であれば、特定のコレクションにアクセスできるユーザーを直接管理できます。 このようにして、インフォメーション ワーカー用の標準アプリケーションを使用して 1 つのコレクションを作成しながら、エンジニア用のグラフィックを多用するモデリング アプリケーションを使用して別のコレクションを作成できます。 リモート デスクトップ サービス (RDS) 展開でユーザー アクセスを管理するには、主に 2 つの手順があります。

1.  [Active Directory でユーザーとグループを作成する](#create-your-users-and-groups-in-active-directory)
2.  [ユーザーとグループをコレクションに割り当てる](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Active Directory でユーザーとグループを作成する

RDS 展開では、Active Directory Domain Services (AD DS) はドメイン内のすべてのユーザー、グループ、およびその他のオブジェクトのソースです。 PowerShell を使用して Active Directory を直接管理することや、使いやすさと柔軟性を高める組み込みの UI ツールを使用することができます。 次の手順では、これらのツールを (まだインストールしていない場合は) インストールし、次にそれらのツールを使用してユーザーとグループを管理します。

### <a name="install-ad-ds-tools"></a>AD DS ツールをインストールする

次の手順では、既に AD DS を実行しているサーバーに AD DS ツールをインストールする方法について詳しく説明します。 インストールが完了したら、ユーザーを作成することやグループを作成することができます。

1. Active Directory Domain Services を実行するサーバーに接続する Azure 展開の場合:
   1. Azure portal で、 **[参照] > [リソース グループ]** をクリックし、展開用のリソース グループをクリックします。
   2. AD 仮想マシンを選択します。
   3. **[接続] > [開く]** をクリックしてリモート デスクトップ クライアントを開きます。 **[接続]** が淡色表示されている場合は、仮想マシンがパブリック IP アドレスを持っていない可能性があります。 それを付与するには、次の手順を実行してから、この手順をもう一度試してください。
      1. **[設定] > [ネットワーク インターフェイス]** の順にクリックして、対応するネットワーク インターフェイスをクリックします。
      2. **[設定] > [IP アドレス]** の順にクリックします。
      3. **[パブリック IP アドレス]** では、 **[有効]** を選択し、 **[IP アドレス]** をクリックします。
      4. 使用したい既存のパブリック IP アドレスがある場合は、一覧からそれを選択します。 それ以外の場合は、 **[新規作成]** をクリックし、名前を入力したら、 **[OK]** 、 **[保存]** の順にクリックします。
   4. クライアントで、 **[接続]** をクリックし、 **[別のアカウントを使用する]** をクリックします。 ドメイン管理者アカウントのユーザー名とパスワードを入力します。
   5. 証明書について確認されたら **[はい]** をクリックします。
2. AD DS ツールをインストールします。
   1. サーバー マネージャーで、 **[管理] > [役割と機能の追加]** をクリックします。
   2. **[役割ベースまたは機能ベースのインストール]** をクリックし、現在の AD サーバーをクリックします。 手順に従って **[機能]** タブまで進みます。
   3. **[リモート サーバー管理ツール] > [役割管理ツール] > [AD DS および AD LDS ツール]** の順に展開し、 **[AD DS ツール]** を選択します。
   4. **[必要に応じて対象サーバーを自動的に再起動する]** を選択し、 **[インストール]** をクリックします。

### <a name="create-a-group"></a>グループの作成

AD DS グループを使用して、同じリモート リソースを使用する必要がある一連のユーザーにアクセスを許可できます。

1. AD DS を実行しているサーバー上のサーバー マネージャーで、 **[ツール] > [Active Directory ユーザーとコンピューター]** の順にクリックします。
2. 左側のウィンドウでドメインを展開し、そのサブフォルダーを表示します。
3. グループを作成するフォルダーを右クリックし、 **[新規] > [グループ]** の順にクリックします。
4. 適切なグループ名を入力し、 **[グローバル]** と **[セキュリティ]** を選択します。

### <a name="create-a-user-and-add-to-a-group"></a>ユーザーを作成してグループに追加する
1. AD DS を実行しているサーバー上のサーバー マネージャーで、 **[ツール] > [Active Directory ユーザーとコンピューター]** の順にクリックします。
2. 左側のウィンドウでドメインを展開し、そのサブフォルダーを表示します。
3. **[ユーザー]** を右クリックし、 **[新規] > [ユーザー]** をクリックします。
4. 少なくとも、名前とユーザー ログオン名を入力します。
5. ユーザーのパスワードを入力し、確認用にもう一度入力します。 **[ユーザーは次回ログオン時にパスワード変更が必要]** のような適切なユーザー オプションを設定します。
6. 新しいユーザーをグループに追加します。
   1. **ユーザー** フォルダーで、新しいユーザーを右クリックします。
   2. **[グループに追加]** をクリックします。
   3. ユーザーを追加するグループの名前を入力します。

## <a name="assign-users-and-groups-to-collections"></a>ユーザーとグループをコレクションに割り当てる
Active Directory でユーザーとグループを作成したので、次は展開内のリモート デスクトップ コレクションにアクセスできるユーザーについて細かく設定します。

1. 前述の手順に従って、リモート デスクトップ接続ブローカー (RD 接続ブローカー) の役割を実行しているサーバーに接続します。
2. 他のリモート デスクトップ サーバーを RD 接続ブローカーのマネージド サーバーのプールに追加します。
   1. サーバー マネージャーで、 **[管理] > [サーバーの追加]** の順にクリックします。
   2. **[Find Now]** をクリックします。
   3. 展開内のリモート デスクトップ サービスの役割を実行している各サーバーをクリックし、 **[OK]** をクリックします。
3. 特定のユーザーまたはグループにアクセスを割り当てるコレクションを編集します。
   1. サーバー マネージャーで **[リモート デスクトップ サービス] > [概要]** の順にクリックし、特定のコレクションをクリックします。
   2. **[プロパティ]** で、 **[タスク] > [プロパティの編集]** の順にクリックします。
   3. **[ユーザー グループ]** をクリックします。
   4. **[追加]** をクリックし、コレクションへのアクセス権を付与するユーザーまたはグループを入力します。 削除するユーザーまたはグループを選択し、 **[削除]** をクリックして、このウィンドウからユーザーおよびグループを削除することもできます。 
   
   >[!NOTE] 
   > [ユーザー グループ] ウィンドウを空にすることはできません。 コレクションにアクセスできるユーザーの範囲を絞り込むには、まず特定のユーザーまたはグループを追加してから、より広いグループを削除する必要があります。