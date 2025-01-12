---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Winlogon 自動再起動サインオン (ARSO)
description: Windows 自動再起動サインオンを使用すると、ユーザーの生産性を向上させることができます。
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56f485491340b3974d8bf5ba697c6cf01f3e56ac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868207"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自動再起動サインオン (ARSO)

Windows Update 中に、更新を完了するために必要なユーザー固有のプロセスがあります。 これらのプロセスでは、ユーザーがデバイスにログインする必要があります。 更新が開始された後の最初のログインでは、ユーザーは、デバイスの使用を開始する前に、これらのユーザー固有のプロセスが完了するまで待つ必要があります。

## <a name="how-does-it-work"></a>その場合、どのように処理されますか?

Windows Update が自動再起動を開始すると、現在ログインしているユーザーの派生した資格情報を抽出してディスクに保存し、ユーザーの自動ログオンを構成します。 TCB 特権を持つシステムとして実行されている Windows Update でこれを行う RPC 呼び出しを開始します。

最後の Windows Update 再起動後、ユーザーは自動ログオンメカニズムを使用して自動的にログインされ、ユーザーのセッションは保存されたシークレットで復元されます。 さらに、デバイスはユーザーのセッションを保護するためにロックされています。 ロックは Winlogon 経由で開始されますが、資格情報の管理はローカルセキュリティ機関 (LSA) によって行われます。 ARSO の構成とログインに成功すると、保存された資格情報がディスクから直ちに削除されます。

コンソールで自動的にログインしてユーザーをロックすることで、ユーザーがデバイスに戻る前に、ユーザー固有のプロセスを完了 Windows Update ことができます。 このようにして、ユーザーはデバイスの使用をすぐに開始できます。

ARSO は、管理されていないデバイスと管理対象のデバイスを別々に扱います。 管理されていないデバイスの場合は、デバイスの暗号化が使用されますが、ユーザーがそれを取得するためには必要ありません。 管理対象デバイスについては、ARSO 構成には TPM 2.0、セキュアブート、BitLocker が必要です。 IT 管理者は、グループポリシーを使用してこの要件をオーバーライドできます。 管理対象デバイスの場合は、現在 Azure Active Directory に参加しているデバイスのみを使用できます。

|   | Windows Update| shutdown-g-t 0  | ユーザーが開始した再起動 | SHUTDOWN_ARSO/EWX_ARSO フラグを持つ Api |
| --- | :---: | :---: | :---: | :---: |
| 管理されたデバイス | :heavy_check_mark:  | :heavy_check_mark: |   | :heavy_check_mark: |
| 管理されていないデバイス | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!NOTE]
> Windows Update が再起動されると、最後の対話ユーザーが自動的にログインし、セッションがロックされます。 これにより、Windows Update の再起動にかかわらず、ユーザーのロック画面アプリを引き続き実行できます。

![［設定］ページ](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>ポリシー #1

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>再起動後に、最後の対話型ユーザーを自動的にサインインしてロックする

Windows 10 では、サーバーの Sku に対して ARSO が無効になっており、クライアントの Sku に対してオプトアウトされています。

**グループポリシーの場所:** コンピューターの構成 > 管理用テンプレート > Windows コンポーネント > Windows ログオンオプション

**Intune ポリシー:**

- プラットフォーム:Windows 10 以降
- プロファイルの種類:管理用テンプレート
- パス: \Windows \Windows ログオンオプション

**サポート対象:** Windows 10 バージョン1903以降

**説明 :**

このポリシー設定は、システムの再起動後、またはシャットダウンとコールドブート後に、デバイスが自動的にサインインして、最後の対話ユーザーをロックするかどうかを制御します。

これは、最後の対話ユーザーが再起動またはシャットダウンの前にサインアウトしなかった場合にのみ発生します。

デバイスが Active Directory または Azure Active Directory に参加している場合、このポリシーは Windows Update の再起動にのみ適用されます。 それ以外の場合は、Windows Update の再起動と、ユーザーが開始した再起動とシャットダウンの両方に適用されます。

このポリシー設定を構成しなかった場合は、既定で有効になります。 ポリシーが有効になっている場合、ユーザーは自動的にサインインされ、デバイスの起動後に、そのユーザー用に構成されたすべてのロック画面アプリでセッションが自動的にロックされます。

このポリシーを有効にした後、ConfigAutomaticRestartSignOn ポリシーを使用して設定を構成できます。このポリシーでは、再起動またはコールドブート後に、最後の対話型ユーザーを自動的にサインインしてロックするモードを構成します。

このポリシー設定を無効にした場合、デバイスは自動サインインを構成しません。 システムの再起動後、ユーザーのロック画面アプリは再起動されません。

**レジストリエディター:**

| 値名 | 種類 | data |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0 (ARSO を有効にする) |
|   |   | 1 (ARSO を無効にする) |

**ポリシーのレジストリの場所:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**種類:** DWORD

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>ポリシー #2

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>再起動またはコールドブート後に、最後の対話ユーザーに自動的にサインインしてロックするモードを構成する

**グループポリシーの場所:** コンピューターの構成 > 管理用テンプレート > Windows コンポーネント > Windows ログオンオプション

**Intune ポリシー:**

- プラットフォーム:Windows 10 以降
- プロファイルの種類:管理用テンプレート
- パス: \Windows \Windows ログオンオプション

**サポート対象:** Windows 10 バージョン1903以降

**説明 :**

このポリシー設定では、再起動またはコールドブート後に自動再起動とサインオンおよびロックが行われる構成を制御します。 "再起動後に最後の対話型ユーザーを自動的にサインインしてロックする" ポリシーで [無効] を選択した場合、自動サインオンは実行されず、このポリシーを構成する必要はありません。

このポリシー設定を有効にした場合は、次の2つのオプションのいずれかを選択できます。

1. [BitLocker が有効で、中断されていない場合に有効にする] は、BitLocker がアクティブで、再起動またはシャットダウン中に中断されていない場合にのみ、自動サインオンとロックが発生することを指定します。 更新中に BitLocker がオンになっていないか中断されている場合、この時点では個人データにデバイスのハードドライブからアクセスできます。 BitLocker を中断すると、システムコンポーネントとデータの保護が一時的に削除されますが、起動に不可欠なコンポーネントを正常に更新するには、特定の状況で必要になることがあります。
   - BitLocker は、次の場合に更新中に中断されます。
      - デバイスに TPM 2.0 と PCR7 が搭載されていません。
      - デバイスで TPM のみのプロテクターが使用されていない
2. [Always Enabled] を指定すると、再起動またはシャットダウン中に BitLocker が無効になっている場合や、一時停止した場合でも自動サインオンが行われます。 BitLocker が有効になっていない場合、ハードドライブの個人データにアクセスできます。 構成されたデバイスが安全な物理的な場所にあると確信できる場合は、自動再起動とサインオンをこの条件下でのみ実行する必要があります。

この設定を無効にした場合、または構成しなかった場合、自動サインオンは既定で [BitLocker が有効で、中断されていない場合に有効] 動作になります。

**レジストリエディター**

| 値名 | 種類 | data |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0 (セキュリティで保護されている場合は ARSO を有効にする) |
|   |   | 1 (常に ARSO を有効にする) |

**ポリシーのレジストリの場所:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**種類:** DWORD

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>トラブルシューティング

WinLogon が自動的にロックしたときに WinLogon の状態のトレースは WinLogon イベント ログに格納されます。

自動ログオンの構成処理のステータスが記録されます。

- 成功した場合
   - そのために記録します。
- 障害の場合。
   - 失敗の原因を記録します。
- BitLocker の状態が変更されたとき。
   - 資格情報の削除がログに記録します。
   - これらは、LSA 操作のログに保存されます。

### <a name="reasons-why-autologon-might-fail"></a>自動ログオンが失敗する理由

ユーザーの自動ログインを実現できないいくつかのケースもあります。  このセクションでは、既知のシナリオが発生する可能性がこれをキャプチャします。

### <a name="user-must-change-password-at-next-login"></a>ユーザーは次回ログイン時にパスワードを変更する必要があります

ユーザーのログインは、次回のログイン時のパスワードの変更が必要な場合、ブロックされた状態を入力できます。  ほとんどの場合、再起動の前に検出されたすべてではなく使用できます (パスワードの有効期限の到達、シャット ダウンし、次回のログインの間です。

### <a name="user-account-disabled"></a>ユーザーアカウントが無効です

無効になっている場合でも、既存のユーザー セッションを管理できます。  再起動を無効になっているアカウントが検出されてローカルでほとんどの場合、事前にドメイン アカウントを (いくつかのドメインは、DC 上でアカウントが無効になっている場合でもにログイン シナリオの作業をキャッシュ) に、必ずしも gp に応じて。

### <a name="logon-hours-and-parental-controls"></a>ログオン時間と保護者による制限

保護者による制限とログオン時間は、作成すると、新しいユーザー セッションを禁止できます。  再起動がこの期間中に発生した場合、ユーザーはログインを許可されません。  ロックするか、対応するアクションとしてログアウトを原因となるその他のポリシーがあります。 自動ログオンの構成試行の状態がログに記録されます。

## <a name="security-details"></a>セキュリティの詳細

### <a name="credentials-stored"></a>格納される資格情報

|   | パスワードハッシュ | 資格情報キー | チケット保証チケット | プライマリ更新トークン |
| --- | :---: | :---: | :---: | :---: |
| ローカルアカウント | :heavy_check_mark: | :heavy_check_mark: |   |   |
| MSA アカウント | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Azure AD の参加アカウント | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark: (ハイブリッドの場合) | :heavy_check_mark: |
| ドメインに参加しているアカウント | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark: (ハイブリッドの場合) |

### <a name="credential-guard-interaction"></a>Credential Guard の相互作用

デバイスで Credential Guard が有効になっている場合、ユーザーの派生シークレットは、現在のブートセッションに固有のキーを使用して暗号化されます。 そのため、Credential Guard が有効になっているデバイスでは、現在、ARSO はサポートされていません。

## <a name="additional-resources"></a>その他の技術情報

自動ログオンは、いくつかのリリースの Windows に存在した機能です。 これは、windows [http:/technet. microsoft .com/sysinternals/bb963905](https://technet.microsoft.com/sysinternals/bb963905.aspx)の自動ログオンなどのツールを含む、windows のドキュメント化された機能です。 デバイスの 1 人のユーザーの資格情報を入力しなくても自動的にサインインできます。 資格情報が構成され、暗号化された LSA シークレットとしてレジストリに格納します。 特に、メンテナンス期間がこの期間中に一般がある場合はベッドの時間とウェイク アップのアカウントのロックダウンが発生する、多くの子の例では問題にできます。
