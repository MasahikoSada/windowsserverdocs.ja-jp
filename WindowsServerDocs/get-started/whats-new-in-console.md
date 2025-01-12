---
title: Windows Server 2016 での Windows コンソールの新機能
description: Windows Server 2016 コンソールの重要な新機能の一覧を示します。
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: server-general
ms.reviewer: na
ms.suite: na
ms.date: 10/04/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9fc582-033b-4973-84e7-0c6024ecfcbc
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 2f05bcffa7c8c4f9e74f3699b9838b8a627af1b1
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "63688673"
---
# <a name="whats-new-in-the-windows-console-in-windows-server-2016"></a>Windows Server 2016 での Windows コンソールの新機能
>適用先:Windows Server 2016

コンソール ホスト (Windows コマンド プロンプト、Windows PowerShell プロンプトなどを含むすべての文字モード アプリケーションをサポートする基本的なコード) にさまざまな更新が行われ、各種新機能が追加されました。  

## <a name="controlling-the-new-features"></a>新しい機能の制御  
新しい機能は既定で有効になっていますが、各新機能のオンとオフを切り替えたり、[プロパティ] インターフェイス (ほとんどの場合 **[オプション]** タブにあります) を介して、または次のレジストリ キー (すべてのキーは **HKEY_CURRENT_USER\Console** にある DWORD 値です) を使用して以前のコンソール ホストに戻したりできます。  

|レジストリ キー|説明|  
|----------------|---------------|  
|ForceV2|1 では、すべての新しいコンソール機能が有効になります。0 では、すべての新機能が無効になります。 注: この値はショートカットには格納されず、このレジストリ キーにのみ格納されます。|  
|LineSelection|1 では、行の選択が有効になります。0 では、ブロック モードのみが使用されます。|  
|FilterOnPaste|1 では、新しい貼り付け動作が有効になります。|  
|LineWrap|1 では、コンソール ウィンドウのサイズ変更時にテキストが折り返されます。|  
|CtrlKeyShortcutsDisabled|0 では新しいキーのショートカットが有効になります。1 ではこれらのショートカットが無効になります。|  
|ExtendedEdit Keys|1 では、キーボードの選択キーの完全なセットが有効になります。0 では、これらは無効になります。|  
|TrimLeadingZeros|1 では、ダブルクリックでの選択時に先頭の 0 が削除されます。0 では、先頭の 0 は保持されます。|  
|WindowsAlpha|30% - 100% で不透明度の値を設定します。 値は 0x4C - 0 xff または 76 - 255 を使用して指定します。|  
|WordDelimiters|Ctrl + Shift + 方向キーでテキストや単語全体を一度に選択したときに、区切りとして使用される文字を定義します (既定では空白文字)。 この REG_SZ 値は、区切り記号として扱うすべての文字を指定して設定します。 注: この値はショートカットには格納されず、このレジストリ キーにのみ格納されます。|  

これらの設定は、HKCU\Console の下のレジストリ内の各ウィンドウ タイトルごとに格納されます。 コンソール ウィンドウをショートカットで開いた場合、これらの設定はショートカットに格納されます。このショートカットを別のコンピューターにコピーすると、ショートカットに格納されている設定も新しいコンピューターに移動します。 ショートカットに含まれる設定によって、グローバル設定や既定の設定を含む、他のすべての設定がオーバーライドされます。 ただし、 **[オプション]** タブの **[従来のコンソールを使う]** を使用して元のコンソールに戻す場合、この設定はグローバルになり、それ以降コンピューターを再起動してもすべてのウィンドウで保持されます。  

無人セットアップ ファイルでレジストリを適切に構成するか、Windows PowerShell を使用することで、これらの設定を事前に構成するか設定のスクリプトを作成できます。  

16 ビット NTVDM アプリケーションは、常に古いコンソール ホストに戻ります。  

> [!NOTE]  
> 新しいコンソールの設定で問題が発生し、ここで示した特定のオプションで解決できない場合は、ForceV2 を 0 に設定するか、 **[オプション]** の **[従来のコンソールを使う]** を使用して、いつでも元のコンソールに戻ることができます。  

## <a name="console-behavior"></a>コンソールの動作  
マウスでコンソール ウィンドウの端をつかんでドラッグして、コンソール ウィンドウのサイズを自由に変更できるようになりました。 スクロール バーは、ウィンドウのディメンションを ( **[プロパティ]** の **[レイアウト]** タブを使用して) 手動で設定した場合、またはバッファー内のテキストの最も長い行が現在のウィンドウのサイズよりも長い場合にのみ表示されます。  

この新しいコンソール ウィンドウでは、ワード ラップがサポートされるようになりました。 ただし、コンソール API を使用してバッファー内のテキストを変更した場合、コンソールのテキストは挿入されたときのままになります。  

コンソール ウィンドウを半透明 (最小の不透明度である 30%) にできるようになりました。 不透明度は [プロパティ] メニューから、または以下のキーボード コマンドで調整できます。  

|これには、次の手順を実行します。|次のキーの組み合わせを使用します。|  
|---------------|-----------------------------|  
|不透明度を上げる|Ctrl + Shift + プラス記号 (+) または Ctrl + Shift + マウス ホイールを上にスクロール|  
|不透明度を下げる|Ctrl + Shift + マイナス記号 (-) または Ctrl + Shift + マウス ホイールを下にスクロール|  
|全画面表示モードに切り替える|Alt + Enter|  

## <a name="selection"></a>選択  
テキストと行の選択、テキストのハイライト、およびバッファー履歴の使用について多数の新しいオプションが存在します。 コンソールは、同じキーを使っているアプリケーションとの競合を回避しようとします。  

**開発者向け:** 競合が発生した場合、通常はアプリケーションでの行単位入力、処理入力、およびエコー入力の各モードの使用動作を SetConsoleMode() API を使用して制御できます。 処理入力モードで実行する場合は以下のショートカット キーが適用されますが、他のモードでは、アプリケーションによってこれらが処理される必要があります。 ここに記載されていないキーの組み合わせはすべて、以前のバージョンのコンソールと同様に機能します。 **[オプション]** タブ上のさまざまな設定を使用して、競合の解決を試みることもできます。それでも解決できない場合は、元のコンソールにいつでも戻ることができます。  

簡易編集モード以外で "クリックアンドドラッグ" 選択を使用できるようになりました。この選択では、長方形のブロックではなく、行全体のテキストをメモ帳のように選択できます。 コピー操作で改行を削除する必要はなくなりました。 "クリックアンドドラッグ" 選択に加えて、次のキーの組み合わせが利用可能です。  

**テキストの選択**  

|これには、次の手順を実行します。|次のキーの組み合わせを使用します。|  
|---------------|-----------------------------|  
|カーソルを左に 1 文字分移動して選択範囲を拡張する|Shift + 左方向キー|  
|カーソルを右に 1 文字分移動して選択範囲を拡張する|Shift + 右方向キー|  
|テキストをカーソル位置から行単位で上方向に選択する|Shift + 上方向キー|  
|テキストをカーソル位置から行単位で下方向に選択する|Shift + 下方向キー|  
|現在編集中の行にカーソルがある場合に選択範囲を入力行の最後の文字まで拡張するには、このコマンドを 1 回使用します。 コマンドをもう 1 回使用すると、選択範囲は右の余白まで拡張されます。|Shift + End|  
|現在編集中の行にカーソルが**ない**場合にカーソル位置から右の余白までのすべてのテキストを選択するには、このコマンドを使用します。|Shift + End|  
|現在編集中の行にカーソルがある場合に選択範囲をコマンド プロンプトの直後の文字まで拡張するには、このコマンドを 1 回使用します。 コマンドをもう 1 回使用すると、選択範囲は右の余白まで拡張されます。|Shift + Home|  
|現在編集中の行にカーソルが**ない**場合に選択範囲を左の余白まで拡張するには、このコマンドを使用します。|Shift + Home|  
|選択範囲を下に 1 画面分拡張する|Shift + PageDown|  
|選択範囲を上に 1 画面分拡張する|Shift + PageUp|  
|選択範囲を右側に単語 1 つ分拡張する ("単語" の区切り記号は WordDelimiters レジストリ キーを使用して定義できます)。|Ctrl + Shift + 右方向キー|  
|選択範囲を左側に単語 1 つ分拡張する|Ctrl + Shift + Home|  
|選択範囲を画面バッファーの先頭まで拡張する|Ctrl + Shift + End|  
|現在の行にカーソルがあり行が空でない場合、プロンプトの後ろにあるすべてのテキストを選択する|Ctrl + A|  
|カーソルが現在の行に**ない**場合に、バッファー全体を選択する|Ctrl + A|  

**テキストの編集**  

キーボード コマンドを使用して、テキストをコピーしてコンソールに貼り付けることができます。 Ctrl + C の機能は 2 通りになりました。 Ctrl + C を使用するときにテキストが選択されていない場合は、通常どおり BREAK コマンドが送信されます。 テキストが選択されている場合は、最初に Ctrl + C を使うとテキストがコピーされて選択が解除されます。2 回目に使うと BREAK が送信されます。 その他の編集コマンドを次に示します。  

|これには、次の手順を実行します。|次のキーの組み合わせを使用します。|  
|---------------|-----------------------------|  
|コマンド ラインにテキストを貼り付ける|Ctrl + V|  
|選択したテキストをコピーしてクリップボードに貼り付ける|Ctrl + Ins|  
|選択したテキストをコピーしクリップボードに貼り付けて、BREAK を送信する|CTRL キーを押しながら C|  
|コマンド ラインにテキストを貼り付ける|Shift + Ins|  

**マーク モード**  

マーク モードにするには、コンソールのタイトル バーを右クリックし、 **[編集]** にマウスを合わせて表示されたメニューから **[マーク]** を選択します。 Ctrl + M を使用することもできます。 マーク モードでは、Alt キーを使用して、行の折り返し選択の開始位置を指定できます ( **[行の折り返し選択を有効にする]** が無効な場合、マーク モードではブロック内のテキストが選択されます)。マーク モードでは、Ctrl + Shift + 方向キーでは、通常モードの単語単位での選択ではなく、文字単位での選択が行われます。 マーク モードでは、「**テキストの編集**」セクションでの選択キーに加えて次のキーの組み合わせも利用可能です。  

|これには、次の手順を実行します。|次のキーの組み合わせを使用します。|  
|---------------|-----------------------------|  
|マーク モードにしてウィンドウでカーソルを動かす|Ctrl + M|  
|マーク モードでの行の折り返し選択を開始する (他のキーと併用)|Alt|  
|指定した方向にカーソルを移動する|方向キー|  
|指定した方向にページ単位でカーソルを移動する|Page キー|  
|バッファーの先頭にカーソルを移動する|Ctrl + Home|  
|バッファーの末尾にカーソルを移動する|Ctrl + End|  

**履歴内の移動**  

|これには、次の手順を実行します。|次のキーの組み合わせを使用します。|  
|---------------|-----------------------------|  
|出力履歴内で 1 行上に移動する|Ctrl + 上方向キー|  
|出力履歴内で 1 行下に移動する|Ctrl + 下方向キー|  
|(コマンド ラインが空の場合) バッファーの上部にビューポートを移動する。(コマンド ラインが空でない場合) カーソルの左側の文字をすべて削除する。|Ctrl + Home|  
|(コマンド ラインが空の場合) コマンド ラインにビューポートを移動する。(コマンドラインが空でない場合) カーソルの右側の文字をすべて削除する。|Ctrl + End|  

**その他のキーボード コマンド**  

|これには、次の手順を実行します。|次のキーの組み合わせを使用します。|  
|---------------|-----------------------------|  
|[検索] ダイアログを開く|CTRL + F|  
|コンソール ウィンドウを閉じる|Alt + F4|  
