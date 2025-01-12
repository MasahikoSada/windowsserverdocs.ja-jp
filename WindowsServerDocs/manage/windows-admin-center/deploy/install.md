---
title: Windows Admin Center のインストール
description: Windows 管理センターを Windows PC またはサーバーにインストールして、複数のユーザーが web ブラウザーを使用して Windows 管理センターにアクセスできるようにする方法。
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e67102d1fa8b35d90e97df64cb8bd2991b205ad5
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658878"
---
# <a name="install-windows-admin-center"></a>Windows Admin Center のインストール

> 適用対象:Windows Admin Center、Windows Admin Center Preview

このトピックでは、windows 管理センターを Windows PC またはサーバーにインストールして、複数のユーザーが web ブラウザーを使用して Windows 管理センターにアクセスできるようにする方法について説明します。

> [!Tip]
> Windows Admin Center を初めて使用する場合
> [Windows Admin Center についての詳細を確認する](../understand/windows-admin-center.md)か、[今すぐダウンロード](https://aka.ms/windowsadmincenter)してください。

## <a name="determine-your-installation-type"></a>インストールの種類を決定する

サポートされている[オペレーティングシステム](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems)を含む[インストールオプション](../plan/installation-options.md)を確認します。 Azure の VM に Windows 管理センターをインストールする方法については、「 [azure での Windows 管理センターのデプロイ](../azure/deploy-wac-in-azure.md)」を参照してください。

## <a name="install-on-windows-10"></a>Windows 10 へのインストール

Windows 10 に Windows Admin Center をインストールする場合、既定でポート 6515 が使用されますが、別のポートを指定することもできます。 また、デスクトップ ショートカットを作成したり、Windows Admin Center を使用して TrustedHosts を管理したりすることもできます。

> [!NOTE]
> ワークグループ環境の場合、またはドメイン内でローカル管理者の資格情報を使用する場合は、TrustedHosts を変更する必要があります。 この設定を行わなかった場合は、[TrustedHosts を手動で構成する](../support/troubleshooting.md#configure-trustedhosts)必要があります。

**[スタート]** メニューから Windows Admin Center を起動すると、既定のブラウザーで開きます。

初めて Windows Admin Center を起動したときに、デスクトップの通知領域にアイコンが表示されます。 このアイコンを右クリックし、 **[開く]** を選択して既定のブラウザーでツールを開くか、 **[終了]** を選択してバックグラウンド プロセスを終了します。

## <a name="install-on-windows-server-with-desktop-experience"></a>Windows Server (デスクトップ エクスペリエンスあり) へのインストール

Windows Server では、Windows Admin Center がネットワーク サービスとしてインストールされます。 サービスがリッスンするポートを指定する必要があり、それには HTTPS 用の証明書が必要です。 インストーラーは、テスト用の自己署名証明書を作成するか、コンピューターに既にインストールされている証明書のサムプリントを提供することができます。 生成された証明書を使用する場合は、証明書がサーバーの DNS 名と一致する必要があります。 独自の証明書を使用する場合は、証明書に指定されている名前がコンピューター名と一致していることを確認します (ワイルドカード証明書はサポートされていません)。また、Windows 管理センターが TrustedHosts を管理できるようにするための選択肢もあります。

> [!NOTE]
> ワークグループ環境の場合、またはドメイン内でローカル管理者の資格情報を使用する場合は、TrustedHosts を変更する必要があります。 この設定を行わなかった場合は、[TrustedHosts を手動で構成する](../support/troubleshooting.md#configure-trustedhosts)必要があります。

インストールが完了したら、リモートコンピューターからブラウザーを開き、インストーラーの最後の手順で表示されている URL に移動します。

> [!WARNING]
> 自動的に生成された証明書は、インストール後 60 日で期限が切れます。

## <a name="install-on-server-core"></a>Server Core のインストール

Windows Server の Server Core インストールがある場合は、(管理者として実行されている) コマンド プロンプトから Windows Admin Center をインストールできます。 ポートと SSL 証明書を、それぞれ `SME_PORT` および `SSL_CERTIFICATE_OPTION` 引数を使用して指定します。 既存の証明書を使用する場合は、`SME_THUMBPRINT` を使用してその拇印を指定します。

> [!WARNING]
> Windows 管理センターをインストールすると、WinRM サービスが再起動され、すべてのリモート PowerShells セッションがサーバーによって処理されます。 ローカルの Cmd または PowerShell からをインストールすることをお勧めします。 Winrm サービスの再起動によって破損するオートメーションソリューションを使用してをインストールする場合は、パラメーター ```RESTART_WINRM=0```を install 引数に追加できますが、Windows 管理センターを機能させるには winrm を再起動する必要があります。

次のコマンドを実行して Windows Admin Center をインストールし、自己署名証明書を自動生成します。

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

次のコマンドを実行して既存の証明書を使用して Windows Admin Center をインストールします。

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> PowerShell からドットとスラッシュによる相対パス表記 (`.\<WindowsAdminCenterInstallerName>.msi` など) を使用して `msiexec` を起動しないでください。 その表記はサポートされていないため、インストールが失敗します。 `.\`プレフィックスを削除するか、MSI への完全パスを指定してください。

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>新しいバージョンの Windows 管理センターへのアップグレード

Windows Admin Center の非プレビュー バージョンは、Microsoft Update を使用して更新することも、手動インストールにより更新することもできます。

新しいバージョンの Windows 管理センターにアップグレードすると、設定は維持されます。 Windows 管理センターの Insider Preview バージョンのアップグレードは正式にはサポートされていません。クリーンインストールを行う方がよいと思いますが、ブロックすることはありません。

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Windows 管理センターで使用される証明書の更新

Windows 管理センターがサービスとして展開されている場合は、HTTPS 用の証明書を指定する必要があります。 後でこの証明書を更新するには、インストーラーを再実行し```change```、を選択します。
