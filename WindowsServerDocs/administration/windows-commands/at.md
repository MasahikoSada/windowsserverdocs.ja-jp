---
title: at
description: Windows コマンド」のトピック**で**- コマンドをスケジュールし、指定の日時に、コンピューターで実行するプログラムします。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc9d9f3d008db1bb85bfb6afa0308834c929b5f0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435279"
---
# <a name="at"></a>at

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

指定した日時に、コンピューターで実行するには、コマンドとプログラムをスケジュールします。 使用することができます**で**スケジュール サービスを実行するときにのみです。 パラメーターを指定せずに使用される**で**スケジュールされたコマンドが表示されます。
## <a name="syntax"></a>構文
```
at [\\computername] [[id] [/delete] | /delete [/yes]]
at [\\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```
## <a name="parameters"></a>パラメーター

|      パラメーター       |                                                                                                                                                                                                               説明                                                                                                                                                                                                                |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \\\\\<コンピューター名\> |                                                                                                                                                        リモート コンピューターを指定します。 このパラメーターを省略した場合**で**コマンドや、ローカル コンピューター上のプログラムをスケジュールします。                                                                                                                                                        |
|        \<id\>        |                                                                                                                                                                                   スケジュールされたコマンドに割り当てられる id 番号を指定します。                                                                                                                                                                                   |
|       /delete        |                                                                                                                                                                スケジュールされたコマンドを取り消します。 省略した場合*ID*、すべてのコンピューターでスケジュールされたコマンドが取り消されます。                                                                                                                                                                |
|         /yes         |                                                                                                                                                                               スケジュールされたイベントを削除すると、システムからのすべてのクエリを [はい] の回答です。                                                                                                                                                                               |
|       \<time\>       |                                                                                                                                          コマンドを実行する時刻を指定します。 時間は 24 時間表記 (つまり、00:00 [午前 0 時] 23:59 まで) では、時間: 分として表されます。                                                                                                                                          |
|     対話型/     |                                                                                                                                                                  により、*コマンド*時にログオンしているユーザーのデスクトップと対話する*コマンド*を実行します。                                                                                                                                                                  |
|       /すべて。        |                                                                                                                                                    実行*コマンド*すべて指定した日または曜日や月 (たとえば、毎週木曜日、または毎月の日目) の日にします。                                                                                                                                                    |
|       \<date\>       |                                                  コマンドを実行する日付を指定します。 1 つまたは複数の曜日を指定することができます (つまり、型**M**、**T**、**W**、**Th**、**F**、**S**、**Su**) または、月の 1 つまたは複数の日 (つまり、型は 1 ~ 31)。 複数の日付エントリをコンマで区切ります。 省略した場合*日付*、**で**月の現在の日付を使用します。                                                  |
|        、次へ。        |                                                                                                                                                                              実行*コマンド*(たとえば、次の火曜日) 1 日の次の出現箇所にします。                                                                                                                                                                              |
|     \<command\>      | Windows のコマンドを指定します (つまり、.exe や .com ファイル)、またはを実行するバッチ プログラム (つまり、.bat または .cmd ファイル)。 コマンドでは、パスを引数として必要とする場合は、絶対パス (ドライブ文字で始まる全体パス) を使用します。 コマンドがリモート コンピューター上にある場合は、サーバーの汎用名前付け規則 (UNC) 表記を指定し、リモート ドライブ文字ではなく、名前、共有します。 |
|          /?          |                                                                                                                                                                                                   コマンド プロンプトにヘルプを表示します。                                                                                                                                                                                                   |

## <a name="remarks"></a>注釈
- **schtasks**の作成し、スケジュールされたタスクの管理に使用できるもう 1 つのコマンド ライン スケジューリング ツールです。 詳細については**schtasks**、関連トピックを参照してください。
- 使用して**で**  
  使用する**で**、ローカルの Administrators グループのメンバーがあります。
- Cmd.exe の読み込み  
  は自動的に読み込むことができません、Cmd.exe、コマンド インタープリターのコマンドを実行する前にします。 実行可能 (.exe) ファイルを実行していない場合は、明示的に読み込む必要があります Cmd.exe コマンドの先頭に次のように: **cmd/c dir > c:\test.out**
- スケジュールの表示コマンド  
  使用すると**で**コマンド ライン オプションをスケジュールされたタスク表示テーブル形式で、次のようにします。
  ```
  Status  ID   Day        time        Command Line
  OK      1    Each F     4:30 PM     net send group leads status due
  OK      2    Each M     12:00 AM    chkstor > check.file
  OK      3    Each F     11:59 PM    backup2.bat
  ```
- 識別番号を含む (*ID*)  
  識別番号を含める場合 (*ID*) で**で**次のような形式でコマンド プロンプトで 1 つのエントリの情報が表示されます。  
  ```
  Task ID:      1
  Status:       OK
  Schedule:     Each  F
  time of Day:  4:30 PM
  Command:      net send group leads status due
  ```
  コマンドでスケジュールを設定したら**で**、コマンド」と入力して、コマンドの構文が正しいことを確認、コマンド ライン オプションを特に**で**コマンド ライン オプションなし。 コマンド ライン列の情報が正しくない場合は、コマンドを削除して再入力します。 まだ位置が正しくない場合は、以下のコマンド ライン オプションを指定してコマンドを再入力します。
- 結果の表示  
  スケジュール設定コマンド**で**バック グラウンド プロセスとして実行します。 出力は、コンピューターの画面には表示されません。 出力をファイルにリダイレクトするには、リダイレクトの記号 (>) を使用します。 使用しているかどうか、リダイレクト記号の前にエスケープ記号 (^) を使用する必要をファイルに出力をリダイレクトする場合**で**コマンドラインまたはバッチ ファイル。 たとえば、Output.text に出力をリダイレクトする次のように入力します。  
  `at 14:45 c:\test.bat ^>c:\output.txt`  
  実行中のコマンドの現在のディレクトリは、システム ルート フォルダーです。
- システム時刻を変更します。  
  コマンドを実行するスケジュールを設定したら、コンピューターのシステム時刻を変更する場合**で**、同期、**で**」と入力して変更後のシステム時刻とスケジューラ**で**なしコマンド ライン オプション。
- コマンドを格納します。  
  スケジュールされたコマンドは、レジストリに格納されます。 その結果、Schedule サービスを再起動する場合、スケジュールされたタスクは失われません。
- ドライブをネットワークに接続します。  
  ネットワークにアクセスするスケジュールされたジョブは、リダイレクトされたドライブを使用しないでください。 スケジュール サービスで、リダイレクトされたドライブにアクセスできないか、リダイレクトされたドライブは、スケジュールされたタスクの実行時に、別のユーザーがログオンしている場合に存在できない可能性があります。 代わりに、スケジュールされたジョブの UNC パスを使用します。 例:  
  `at 1:00pm my_backup \\\server\share`  
  次の構文を使用しない場所**x:** ユーザーによる接続します。  
  `at 1:00pm my_backup x:`  
  スケジュールする場合、**で**、共有ディレクトリへの接続にドライブ文字を使用してコマンドを含める、**で**ドライブの使用が終了するときに、ドライブを切断するコマンド。 ドライブがない切断されている場合は、割り当てられたドライブ文字は、コマンド プロンプトで使用できません。
- 72 時間後に停止するタスク  
  既定を使用してスケジュールされたタスクによって、**で**72 時間後にコマンド停止します。 この既定値を変更するレジストリを変更することができます。
  1.  レジストリ エディター (regedit.exe) を起動します。
  2.  見つけて次のレジストリ キーをクリックします。**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule**
  3.  [編集] メニューで、次のようにクリックします。 値を追加し、次のレジストリ値を追加します。値の名前: atTaskMaxHours データ型: reg_DWOrd の基数。10 進値のデータ:0。 値のデータ フィールドに 0 の値は、制限がないことを示しますは停止しません。 1 ~ 99 の値では、時間数を示します。
  **注意**
- レジストリを正しく編集しないと、システムが正常に動作しなくなる場合があります。 レジストリを変更する前に、コンピューター上の重要なデータのバックアップを作成する必要があります。
- タスク スケジューラと**で**コマンド  
  [タスク] フォルダーを使用するには、設定を使用して作成されたタスクの表示または変更する、**で**コマンド。 使用してタスクをスケジュールするとき、**で**コマンドをタスクが、次などの名前で、タスク フォルダーに一覧表示:**at3478**します。 ただし、変更する場合、スケジュールされたタスク フォルダーから、タスクのスケジュールされたタスクは通常にアップグレードされます。 タスクに表示されなく、**で**コマンドをアカウントの設定に適用されなくなったことです。 タスクのユーザー アカウントとパスワードを明示的に入力する必要があります。
  ## <a name="examples"></a>例
  Marketing サーバーでスケジュールされているコマンドの一覧を表示するには、次のように入力します。

`at \\marketing`

Corp サーバーの識別番号 3 のコマンドに関する詳細については、次のように入力します。

`at \\corp 3`

午前 8 時に、Corp サーバーで実行する net share コマンドをスケジュールするには 一覧をレポートの共有ディレクトリ、および Corp.txt ファイルの種類で、メンテナンス サーバーにリダイレクトします。

`at \\corp 08:00 cmd /c "net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt"`

5 日おきの午前 0 時にテープ ドライブを Marketing サーバーのハード ドライブをバックアップするバックアップ コマンドを含む archive.cmd バッチ プログラムを作成し、バッチ ファイルが実行をスケジュールし、次のように入力します。

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

現在のサーバーでスケジュールされたすべてのコマンドを取り消す場合に、オフ、**で**情報を次のようにスケジュールします。

`at /delete`

実行可能ファイル (つまり、.exe) ファイルでないコマンドを実行すると、コマンドの前に**cmd/c**次のように Cmd.exe を読み込めません。

`cmd /c dir > c:\test.out`
