---
title: Windows Server の PowerShell を使用して RDS タイトル "Work Resources" をカスタマイズする
description: Windows Server で、ワークスペース名を既定値から変更する方法を説明します。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743337"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Windows Server の PowerShell を使用して RDS タイトル "Work Resources" をカスタマイズする

Windows Server を使用し、RD Web アクセスまたは新しいリモート デスクトップ アプリを介して RemoteApp またはデスクトップにアクセスするときに、ワークスペースが既定で "Work Resources" というタイトルになっていることに気付くことがあります。  このタイトルは、PowerShell コマンドレットを使用して簡単に変更できます。

タイトルを変更するには、接続ブローカー サーバーで新しい PowerShell ウィンドウを開き、次のコマンドで RemoteDesktop モジュールをインポートします。

```powershell
    Import-Module RemoteDesktop
```

次に、Set-RDWorkspace コマンドを使用してワークスペース名を変更します。

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

たとえば、次のコマンドを使用して、ワークスペース名を "Contoso RemoteApps" に変更できます。

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

高可用性モードで複数の接続ブローカーを実行している場合は、アクティブなブローカーに対してこれを実行する必要があります。 次のコマンドを使用できます。

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Set-RDWorkspace コマンドレットの詳細については、[Set-RDWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps) のリファレンスを参照してください。