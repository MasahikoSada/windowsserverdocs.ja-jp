---
title: パフォーマンス カウンターを使用して、リモート デスクトップ セッション ホストでのアプリケーションの応答性の問題を診断する
description: RDS でのアプリの実行速度が遅いですか。 RDS でのアプリのパフォーマンス問題を診断するために使用できるパフォーマンス カウンターについて説明する
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/11/2019
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 3eb1e4b6da971d788383b8facbf8bbcbe00a5953
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870907"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>パフォーマンス カウンターを使用して、リモート デスクトップ セッション ホストでのアプリのパフォーマンス問題を診断する

> 適用対象:Windows Server 2019、Windows 10

アプリケーションの実行速度が遅いのか、応答していないのか、診断が最も難しい問題の 1 つがアプリケーション パフォーマンスの低下です。 従来、診断は、CPU、メモリ、ディスク入出力、その他のメトリックの収集から開始し、続いて Windows Performance Analyzer などのツールを使用して問題の原因を見つけようとします。 残念ながら、ほとんどの状況で、リソース消費量カウンターには頻繁かつ大幅な変動があるため、このデータは根本原因の特定には役立ちません。 このため、データを読み取って、報告された問題に関連付けることが困難になっています。 アプリのパフォーマンス問題を迅速に解決できるように、ユーザー入力フローを測定する新しいパフォーマンス カウンターをいくつか追加しました ([Windows Insider Program](https://insider.windows.com) から[ダウンロード](#download-windows-server-insider-software)で入手可)。

>[!NOTE]
>ユーザー入力遅延カウンターが対応しているのは、以下のみです。
> - Windows Server 2019 以降
> - Windows 10 バージョン 1809 以降

ユーザー入力遅延カウンターは、エンド ユーザーの RDP の不適切なエクスペリエンスの根本原因をすばやく特定するのに役立ちます。 このカウンターは、ユーザー入力 (マウスやキーボードの使用など) がプロセスによって取得されるまでキューに留まっている時間を測定し、ローカルとリモートの両方のセッションで機能します。

次の図は、クライアントからアプリケーションへのユーザー入力フローを大まかに示したものです。

![リモート デスクトップ - ユーザーのリモート デスクトップ クライアントからアプリケーションへのユーザー入力フロー](./media/rds-user-input.png)

ユーザー入力遅延カウンターは、次のフロー図に示すように、入力がキューに入れられたときから、[従来のメッセージ ループ](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop)内でアプリによって取得されたときとの最大デルタ (時間間隔内) を測定します。

![リモート デスクトップ - ユーザー入力遅延のパフォーマンス カウンター フロー](./media/rds-user-input-delay.png)

このカウンターの 1 つの重要な詳細な内容は、構成可能な間隔内での最大のユーザー入力遅延を報告しているという点です。 これは、入力がアプリケーションに到達するまでにかかる最長の時間であり、タイピングなどの重要で目に見えるアクションの速度に影響する場合があります。

たとえば、次の表では、ユーザー入力遅延はこの間隔内で 1,000 ミリ秒として報告されます。 ユーザーの "低い" という感覚は、すべての入力合計の平均速度ではなく、ユーザーが経験する最も遅い入力時間 (最長) によって決まるため、このカウンターでは最も遅いユーザー入力遅延が報告されます。

|数値| 0 | 1 | 2 |
|------|---|---|---|
|遅延 |16 ミリ秒| 20 ミリ秒| 1,000 ミリ秒|

## <a name="enable-and-use-the-new-performance-counters"></a>新しいパフォーマンス カウンターを有効にして使用する

これらの新しいパフォーマンス カウンターを使用するには、最初に次のコマンドを実行してレジストリ キーを有効にする必要があります。

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Windows 10 バージョン 1809 以降または Windows Server 2019 以降を使用している場合は、レジストリ キーを有効にする必要はありません。

次にサーバーを再起動します。 続いて、パフォーマンス モニターを開き、次のスクリーン ショットに示すように、プラス記号 (+) を選択します。

![リモート デスクトップ - ユーザー入力遅延パフォーマンス カウンターを追加する方法を示したスクリーン ショット](./media/rds-add-user-input-counter-screen.png)

その後、[カウンターの追加] ダイアログが表示され、ここでは **[User Input Delay per Process]** (プロセスあたりのユーザー入力遅延) か **[User Input Delay per Session]** (セッションあたりのユーザー入力遅延) を選択できます。

![リモート デスクトップ - セッションあたりのユーザー入力遅延を追加する方法を示したスクリーン ショット](./media/rds-user-delay-per-session.png)

![リモート デスクトップ - プロセスあたりのユーザー入力遅延を追加する方法を示したスクリーン ショット](./media/rds-user-delay-per-process.png)

**[User Input Delay per Process]** (プロセスあたりのユーザー入力遅延) を選択した場合、**選択したオブジェクトのインスタンス** (つまりプロセス) が ```SessionID:ProcessID <Process Image>``` の形式で表示されます。

たとえば、電卓アプリが[セッション ID 1](https://msdn.microsoft.com/library/ms524326.aspx) で実行している場合、```1:4232 <Calculator.exe>``` と表示されます。

> [!NOTE]
> すべてのプロセスが含まれるわけではありません。 SYSTEM として実行しているプロセスは表示されません。

カウンターは、ユーザー入力遅延を追加するとすぐに報告し始めます。 既定では、最大の目盛りが 100 (ミリ秒) に設定されていることに注意してください。 

![リモート デスクトップ - パフォーマンス モニターにおけるプロセスあたりのユーザー入力遅延のアクティビティの例](./media/rds-sample-user-input-delay-perfmon.png)

次に、**セッションあたりのユーザー入力遅延**を見てみましょう。 セッション ID ごとにインスタンスがあり、そのカウンターは、指定されたセッション内のすべてのプロセスのユーザー入力遅延を示します。 さらに、"Max" (すべてのセッションにわたる最大ユーザー入力遅延) と "Average" (すべてのセッションの平均) という 2 つのインスタンスがあります。

次の表に、これらのインスタンスの表示例を示します。 (レポート グラフの種類に切り替えることにより、同じ情報を取得できます)。

|カウンターの種類|［インスタンス名］|報告される遅延 (ミリ秒)|
|---------------|-------------|-------------------|
|プロセスあたりのユーザー入力遅延|1:4232 <Calculator.exe>|  200|
|プロセスあたりのユーザー入力遅延|2:1000 <Calculator.exe>|  16|
|プロセスあたりのユーザー入力遅延|1:2000 <Calculator.exe>|  32|
|セッションあたりのユーザー入力遅延|1|    200|
|セッションあたりのユーザー入力遅延|2|    16|
|セッションあたりのユーザー入力遅延|平均|  108|
|セッションあたりのユーザー入力遅延|最大|  200|

## <a name="counters-used-in-an-overloaded-system"></a>過負荷状態のシステムで使用されるカウンター

次に、アプリのパフォーマンスが低下した場合にレポートで何が表示されるかを見てみましょう。 次のグラフは、Microsoft Word でリモートで作業しているユーザーの測定値を示しています。 この場合、ユーザーのログインが増えると、RDSH サーバー パフォーマンスが時間と共に低下しています。

![リモート デスクトップ - Microsoft Word を実行している RDSH サーバーのパフォーマンス グラフの例](./media/rds-user-input-perf-graph.png)

グラフの各線の読み取り方を次に示します。

- ピンクの線は、サーバーにサインインしたセッション数を示します。
- 赤の線は、CPU 使用率です。
- 緑の線は、すべてのセッションで最大のユーザー入力遅延です。
- 青の線 (このグラフでは黒として表示) は、すべてのセッションのユーザー入力遅延の平均を表します。

CPU 使用量の急増とユーザー入力遅延の間に相関関係があることがわかります。CPU の使用量が増えると、ユーザー入力遅延が増加しています。 また、より多くのユーザーがシステムに追加されると、CPU 使用率が 100% に近づき、ユーザー入力遅延が頻繁に急増することになります。 このカウンターは、サーバーがリソース不足になった場合に非常に役立ちますが、特定のアプリケーションに関連付けられたユーザー入力遅延の追跡にも使用できます。

## <a name="configuration-options"></a>構成オプション

このパフォーマンス カウンターを使用するときに注意すべき重要な点は、既定では 1,000 ミリ秒の間隔でユーザー入力遅延をレポートするということです。 パフォーマンス カウンターのサンプリング間隔プロパティ (次のスクリーンショットに表示) を別の値に設定した場合、報告された値は正しくありません。

![リモート デスクトップの - パフォーマンス モニターのプロパティ](./media/rds-user-input-perfmon-properties.png)

これを修正するには、使用する間隔 (ミリ秒単位) に一致するように次のレジストリ キーを設定できます。 たとえば、サンプリング間隔 x 秒を 5 秒に変更した場合、このキーを 5000 ミリ秒に設定する必要があります。

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Windows 10 バージョン 1809 以降または Windows Server 2019 以降を使用している場合は、パフォーマンス カウンターを修正するために LagCounterInterval を設定する必要はありません。

同じレジストリ キーの下に、役立つと思われるキーもいくつか追加しました。

**LagCounterImageNameFirst** — このキーを `DWORD 1` に設定します (既定値は 0、つまりキーが存在しません)。 これはカウンター名を "イメージ名 <セッション ID:プロセス ID>" に変更します。 たとえば、"explorer <1:7964>" とします。 これは、イメージ名で並べ替えを行う場合に役立ちます。

**LagCounterShowUnknown** — このキーを `DWORD 1` に設定します (既定値は 0、つまりキーが存在しません)。 これは、サービスまたは SYSTEM システムとして実行しているすべてのプロセスを表示します。 一部のプロセスは、セッションが "?" と設定されて表示されます。

これは、両方のキーを有効にした場合の表示です。

![リモート デスクトップ - 両方のキーを有効にしたパフォーマンス モニター](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Microsoft 以外のツールでの新しいカウンターの使用

監視ツールは、[Perfmon API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx) を使用することで、このカウンターを使用できます。

## <a name="download-windows-server-insider-software"></a>Windows Server Insider ソフトウェアをダウンロードする

登録されている Insider は、[Windows Server Insider プレビュー ダウンロード ページ](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver)に直接移動して、最新版の Insider ソフトウェアをダウンロードできます。  Insider として登録する方法については、[サーバーの概要](https://insider.windows.com/en-us/for-business-getting-started-server/)に関するページを参照してください。

## <a name="share-your-feedback"></a>フィードバックをお待ちしております

フィードバック Hub から、この機能に関するフィードバックを送信できます。 **[アプリ] > [その他すべてのアプリ]** を選択し、投稿のタイトルに "RDS performance counters—performance monitor" を含めます。

一般的な機能の概念については、[RDS UserVoice ページ](https://aka.ms/uservoice-rds)を参照してください。
