---
title: セット (set)
description: 'Windows コマンド」のトピック * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21661b30c5779907e8cac417439a0935a2126ad8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441272"
---
# <a name="set"></a>セット (set)



表示、セット、または削除します。EXE の環境変数。 パラメーターを指定せずに使用する場合 **設定** 現在の環境変数の設定が表示されます。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
set [<Variable>=[<String>]]
set [/p] <Variable>=[<PromptString>]
set /a <Variable>=<Expression>
```

## <a name="parameters"></a>パラメーター

|パラメーター|説明|
|---------|-----------|
|\<変数 >|設定または変更するには、環境変数を指定します。|
|\<文字列 >|指定された環境変数に関連付ける文字列を指定します。|
|/p|値を設定 *変数* の入力をユーザーが入力行にします。|
|\<PromptString >|(省略可能)。 入力をユーザーに入力を求めるメッセージを指定します。 このパラメーターを併用、 **/p** コマンド ライン オプションです。|
|/a|セット *文字列* に評価される数値式です。|
|\<式 >|数値式を指定します。 使用できる有効な演算子の「解説」を参照してください *式*します。|
|/?|コマンド プロンプトにヘルプを表示します。|

## <a name="remarks"></a>注釈

- 使用して **設定** コマンド拡張機能を有効になっています。

  コマンド拡張機能が有効にすると (既定値) を実行して **設定** 表示すべてがその値で始まる変数の値を持つ。
- 特殊文字を使用します。

  文字 **<** , 、 **>** , 、 **|** , 、 **&** , 、 **^** 特別なコマンド シェルの文字し、する前にエスケープ文字が必要 ( **^** ) で使用する場合に、引用符で囲まれた、または *文字列* (たとえば、 **"折り返し & シンボル"** )。 特殊文字のいずれかを表す文字列を囲む引用符を使用する場合、引用符は、環境変数の値の一部として設定されます。
- 環境変数の使用

  環境変数を使用して、バッチ ファイルやプログラムの動作を制御し、Windows と、MS-DOS を制御するサブシステムの外観し機能します。 **設定** コマンドでよく使用されて、項目ファイル環境変数を設定します。
- 現在の環境設定を表示します。

  入力すると、 **設定** コマンド単独での現在の環境設定が表示されます。 通常、これらの設定には、ディスク上のプログラムを検索するために使用 COMSPEC および PATH 環境変数が含まれます。 Windows で使用されるその他の 2 つの環境変数には、プロンプトおよび DIRCMD です。
- パラメーターを使用します。

  値を指定すると *変数* と *文字列*, 、指定した *変数* 値は、環境に追加し、 *文字列* がその変数に関連付けられています。 環境内の変数が既に存在する場合、新しい文字列値には、元の文字列値が置き換えられます。

  変数にのみ、等号 (=) を指定するかどうか (せず *文字列*) の **設定** コマンド、 *文字列* 変数に関連付けられている値をクリアすると、(ように、変数がない場合があります)。
- 使用して **/a**

  次の表のサポートされている演算子 **/a** 優先順位の順に並べられています。  

  |        演算子         | 実行される操作  |
  |-------------------------|----------------------|
  |           ( )           |       グループ化       |
  |          ! ~ -          |        単項         |
  |         \* / %          |      算術演算子      |
  |           + -           |      算術演算子      |
  |          << >>          |    論理シフト     |
  |            &            |     ビットごとの AND      |
  |            ^            | ビットごとの排他的 OR |
  |                         |                      |
  | = \*= /= %= += -= &= ^= |      = <<= >>=       |
  |            、            | 式の区切り記号 |

  論理を使用する場合 ( **&&** または **||** ) または剰余 ( **%** ) 演算子、式の文字列を引用符で囲みます。 式で任意の数値ではない文字列には、環境変数の名前と見なされ、それらの値は、処理される前に、数値に変換します。 現在の環境で定義されていない環境変数名を指定する値は 0 が割り当てられている、% を使用して値を取得することがなく環境変数の値の算術演算を実行できます。

  実行すると **set/a** コマンド スクリプトの外部でコマンドラインから、式の最終的な値が表示されます。

  数値は、0 × 16 進数または 8 進数 0 で始まっていない限り、10 進の番号です。 そのため、0 ~ 12 は、18 の場合と同じある 022 と同じです。
- 遅延の環境変数の拡張をサポートします。

  既定では、遅延環境変数の拡張サポートが無効になりますが、有効にするかを使用して無効にするに **cmd/v**します。
- コマンド拡張機能の使用

  コマンド拡張機能が有効にすると (既定値) を実行して **設定** 単独で、現在のすべての環境変数が表示されます。 実行すると **設定** 値と、その値に一致する変数が表示されます。
- 使用して **設定** バッチ ファイルで

  バッチ ファイルを作成するときに行うこともできます **設定** 変数を作成し、それらを番号付きの変数を使用することと同じ方法で使用する **%0** を通じて **%9**します。 変数を使用することもできます。 **%0** を通じて **%9** の入力として **設定**します。
- 呼び出す、 **設定** バッチ ファイルの変数

  バッチ ファイルから変数の値を呼び出すときに、パーセント記号と値を囲む ( **%** )。 たとえば、バッチ ファイルは、ボーという環境変数を作成する場合は、」と入力して置き換え可能パラメーター ボーに関連付けられている文字列を使用できます **ボー %** コマンド プロンプト。
- 使用して **設定** 回復コンソールで

  **設定** コマンドで他のパラメーターは、回復コンソールから利用できます。

## <a name="BKMK_examples"></a>例

TEST という名前の環境変数を設定する ^1 の場合、型。
```
set testVar=test^^1
```

> [!NOTE]
> **設定** コマンドは、変数の値を等号 (=) に依存しているすべてのデータを割り当てます。 入力した場合。
> ```
> set testVar="test^1"
> ```
> 次の結果が得られます。
> ```
> testVar="test^1"
> ```
> 環境変数を設定するには、という名前のテストと 1 の種類。
> ```
> set testVar=test^&1
> ```
> 文字列 C:\Inc (ドライブ C 上の \Inc ディレクトリ) は、関連付けられているため、include 環境変数を設定するには、次のように入力します。
> ```
> set include=c:\inc
> ```
> 囲むをパーセント記号の名前を含めることによって文字列 C:\Inc をバッチ ファイルでを使用することができます ( **%** )。 たとえば、INCLUDE 環境変数に関連付けられているディレクトリの内容を表示できるように、バッチ ファイルで、次のコマンドを含めることが。
> ```
> dir %include%
> ```
> このコマンドが処理されるときに、文字列 C:\Inc を置き換えます **% 含める %** します。

使用することも **設定** 新しいディレクトリを PATH 環境変数に追加するバッチ ファイルでします。 例:
```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```
すべての文字 P で始まる環境変数の一覧を表示するには、次のように入力します。
```
set p 
```

> [!NOTE]
> このコマンドでは、既定で有効になっているコマンド拡張機能が必要です。

#### <a name="additional-references"></a>その他の参照情報

[コマンド ライン構文の記号](command-line-syntax-key.md)