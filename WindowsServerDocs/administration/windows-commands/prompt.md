---
title: prompt
description: コマンド プロンプトをカスタマイズする方法について説明します。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5ef487ce9799c1f09660cdfcd6fba71336fc4d9a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442138"
---
# <a name="prompt"></a>prompt



Cmd.exe コマンド プロンプトを変更します。 パラメーターを指定せずに使用する場合 **プロンプト** コマンド プロンプトを現在のドライブ文字と大なり記号に続くディレクトリは、既定の設定にリセット ( **>** )。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
prompt [<Text>]
```

## <a name="parameters"></a>パラメーター

|パラメーター|説明|
|---------|-----------|
|\<テキスト >|テキストと、コマンド プロンプトに追加する情報を指定します。|
|/?|コマンド プロンプトにヘルプを表示します。|

## <a name="remarks"></a>注釈

現在のディレクトリ、時刻、日付、および Microsoft Windows のバージョン番号の名前としては、このような情報を含む、必要なテキストを表示するコマンド プロンプトをカスタマイズできます。

次の表の代わりに、またはネイティブ コードに加えて内で 1 つまたは複数の文字列を含めることができる文字の組み合わせ、 *テキスト* パラメーター。 一覧には、文字列またはコマンド プロンプトに各文字の組み合わせを追加する情報の簡単な説明が含まれています。  

| 文字 |                                 説明                                 |
|-----------|-----------------------------------------------------------------------------|
|    $q     |                               = (等号)                                |
|    $$     |                               $ (ドル記号)                               |
|    $t     |                                現在の時刻                                 |
|    $d     |                                現在の日付                                 |
|    $p     |                           現在のドライブおよびパス                            |
|    $v     |                           Windows のバージョン番号                            |
|    $n     |                                現在のドライブ                                |
|    $g     |                            > (不等号)                            |
|    $l     |                             < (小なり記号)                              |
|    $b     |                                                                             |
|    $_     |                               改行を入力してください。                                |
|    $e     |                         ANSI エスケープ コード (コード 27)                          |
|    $h     | バック スペース (がコマンドラインに書き込まれた文字を削除) する |
|    $、     |                                & (アンパサンド)                                |
|    $c     |                            ((左かっこ)                             |
|    $f     |                            ) (かっこ)                            |
|    $s     |                                    領域                                    |

コマンド拡張機能が有効になっている (つまり、既定値)、 **プロンプト** コマンドは、次の書式指定文字をサポートしています。  

|文字|説明|
|---------|-----------|
|$+|0 個以上の正符号 ( **+** ) 文字の深さによって、 **pushd** ディレクトリ スタック (プッシュ レベルごとに 1 つの文字)。|
|$m|現在のドライブがネットワーク ドライブではない場合は、現在のドライブ文字または空の文字列に関連付けられているリモートの名前。|

含める場合、 **$p** 文字 (現在のドライブとパスを決定) するには、各コマンドを入力した後に、ディスクを読み取るテキスト パラメーターにします。 これにより、特にフロッピー ディスク ドライブの場合の余分な時間がかかります。

## <a name="BKMK_examples"></a>例

を最初の行と、大なり記号、次の行に現在の時刻と日付で 2 行のコマンド プロンプトを設定するには、次のように入力します。
```
prompt $d$s$s$t$_$g 
```
プロンプトに、日付と時刻が現在、次のように変更します。
```
Fri 06/01/2007  13:53:28.91
>
```
矢印として表示するコマンド プロンプトを設定する (`-->`)、型。
```
prompt --$g
```
既定の設定 (現在のドライブとパスの後に不等号) をコマンド プロンプトを手動で変更するには、次のように入力します。
```
prompt $p$g
```

#### <a name="additional-references"></a>その他の参照情報

[コマンド ライン構文の記号](command-line-syntax-key.md)