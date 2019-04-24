---
title: fc
description: 'Windows コマンド」のトピック * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98fd96c73b962a13c0e715420ebbe6f3cd19a42b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888683"
---
# <a name="fc"></a>fc



2 つのファイルを比較またはファイルの設定し、それらの相違点が表示されます。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>パラメーター

|パラメーター|説明|
|---------|-----------|
|/a|ASCII 比較の出力で省略しています。 すべての行が異なる場合に表示する代わりに**fc**の相違点の各セットの最初と最後の行のみが表示されます。|
|/b|1 バイトずつバイナリ モードでは、2 つのファイルを比較し、不一致を検索した後、ファイルを再同期を試行しません。 これは、次のファイル拡張子を持つファイルを比較するための既定のモード: .exe、.com、.sys、.obj、.lib、または .bin します。|
|/c|大文字と小文字を無視します。|
|/l|ASCII モード、1 行ずつ、不一致を検索した後、ファイルを再同期の試行でファイルを比較します。 これは、次のファイル拡張子を持つファイルを除き、ファイルの比較の既定のモード: .exe、.com、.sys、.obj、.lib、または .bin します。|
|/lb\<N>|行の数を設定する内部の行のバッファーの*N*します。行のバッファーの既定の長さは、100 行です。 比較するファイルが 100 以上の連続する異なる行がある場合**fc**比較を取り消します。|
|/n|ASCII 比較中には、行番号を表示します。|
|/[オフライン]|オフライン属性が設定されているファイルをスキップしません。|
|/t|防止**fc**からタブをスペースに変換します。 既定の動作では、各 8 番目の文字位置からのスペースとしてのタブを処理します。|
|/u|Unicode テキスト ファイルとしてファイルを比較します。|
|/w|比較時に空白文字 (つまり、タブやスペースなど) を圧縮します。 多くの連続したスペースまたはタブで、行が含まれている場合 **/w**これらの文字を単一の空白として扱います。 使用すると **/w**、 **fc**行の末尾、先頭にある空白を無視します。|
|/\<NNNN>|前に次の一致しない場合と一致する必要がありますできる連続行の数を指定します**fc**が再同期ファイルが考慮されます。 かどうか、ファイルの一致する行の数はより小さい*NNNN*、 **fc**の相違点として、一致する行を表示します。 既定値は 2 です。|
|[\<Drive1>:][<Path1>]<FileName1>|比較するファイルの場所と、最初のファイルの名前を指定します。 *FileName1*が必要です。|
|[\<ドライブ 2 >:] [<Path2>]<FileName2>|比較するファイルの場所と 2 番目のファイルの名前を指定します。 *FileName2*が必要です。|
|/?|コマンド プロンプトにヘルプを表示します。|

## <a name="remarks"></a>注釈

-   このコマンドは、c:\WINDOWS\fc.exe によって implemeted です。 Powershell で、このコマンドを使用できますが、'fc' は Format-custom のエイリアスであるため、完全な実行可能ファイル (fc.exe) を綴ることを確認します。

-   ASCII 比較用のファイルの相違点の報告

    使用すると**fc** ASCII 比較では、 **fc**次の順序で 2 つのファイル間の相違点が表示されます。  
    -   最初のファイルの名前
    -   行*FileName1*ファイル間で異なる
    -   両方のファイルと一致する最初の行
    -   2 番目のファイルの名前
    -   行*FileName2*が異なる
    -   一致する最初の行
-   使用して **/b**バイナリの比較

    **/b**内にあるバイナリの比較時に、次の構文の不一致が表示されます。

    `\<XXXXXXXX: YY ZZ>`

    値*XXXXXXXX* 16 進数の相対アドレスをファイルの先頭から計測したバイトのペアを指定します。 アドレスは、00000000 から始まります。 16 進数の値を*YY*と*ZZ*から一致しないバイトを表す*FileName1*と*FileName2*、それぞれします。
-   ワイルドカード文字を使用します。

    ワイルドカード文字を使用することができます (**&#42;** と **?**) で*FileName1*と*FileName2*します。 ワイルドカードを使用する場合*FileName1*、 **fc**ファイルまたはで指定されたファイルのセットに指定されたすべてのファイルを比較します*FileName2*します。 ワイルドカードを使用する場合*FileName2*、 **fc**から対応する値を使用して*FileName1*します。
-   メモリの使用

    ASCII ファイルを比較するときに**fc** (100 行を保持できる大きさ) の内部バッファーを使用して記憶域として。 ファイルが、バッファーより大きい場合**fc**バッファーに読み込むことができます、比較します。 場合**fc**一致を見つけられなかったはファイルのアンロードの部分で停止し、次のメッセージが表示されます。

    `Resynch failed. Files are too different.`

    使用可能なメモリよりも大きいバイナリ ファイルを比較するときに**fc**両方のファイルが完全には、比較、ディスクから、[次へ] の部分でメモリ内の特定部分をオーバーレイします。 出力は、メモリに完全に入るファイルのと同じです。

## <a name="BKMK_examples"></a>例

ASCII Monthly.rpt と Sales.rpt、2 つのテキスト ファイルの比較を行い、省略された形式で結果を表示、次のように入力します。
```
fc /a monthly.rpt sales.rpt 
```
Profits.bat と Earnings.bat、2 つのバッチ ファイルをバイナリ比較を次のように入力します。
```
fc /b profits.bat earnings.bat
```
結果は次のように似ています。
```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
...
...
...
000005E8: 00 6E
FC: Earnings.bat longer than Profits.bat
```
場合は、Profits.bat および Earnings.bat ファイルは同じですが、 **fc**次のメッセージが表示されます。
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
New.bat とファイルを使用して、現在のディレクトリのすべての .bat ファイルを比較するには、次のように入力します。
```
fc *.bat new.bat
```
New.bat と D ドライブ上のファイルと C ドライブにファイル new.bat とを比較するには、次のように入力します。
```
fc c:new.bat d:*.bat
```
各バッチ ファイルを比較するルート ディレクトリにファイルを C ドライブに D ドライブのルート ディレクトリに同じ名前で、次のように入力します。
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>その他の参照情報

[コマンドライン構文キー](command-line-syntax-key.md)