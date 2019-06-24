---
title: 複数言語のクライアント復元メディアの作成
description: Windows Server Essentials を使用する方法について説明します
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ad934d297c3092050bd6adbb6bb0f50d1ec6f36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879873"
---
# <a name="build-multi-language-client-restore-media"></a>複数言語のクライアント復元メディアの作成

>適用先:Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

> [!NOTE]
>  多言語 Windows イメージを作成する必要があります最初、[チュートリアル。多言語 Windows イメージの作成](https://technet.microsoft.com/library/jj126995)Windows Server Essentials 言語パックを install.wim に追加する前にします。  
  
 複数言語のサーバー インストール DVD を作成すると、サーバーの install.wim に言語パックがインストールされます。 言語パックの一部として、復元ウィザードのローカライズされたリソースがインストールされます。  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>複数言語のクライアント復元メディアを作成するには  
  
1.  c:\mount に install.wim をマウントします (以下では、c:\mount\Program Files\Windows Server\bin\ClientRestore フォルダーを、クライアント復元メディアのルートとして、[RestoreMediaRoot] と記述します)。  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  [RestoreMediaRoot]\Sources\Boot.wim にクライアント復元 WIM ファイルをマウントします (boot_x86.wim でも同じ手順を実行する必要があります)。 コマンド ラインは次のとおりです。  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  次の手順を実行して、復元メディアに WinPE-Setup.cab パッケージを追加します。  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  メモ帳を使用して c:\mountRestore\windows\system32\winpeshl.ini を編集し、次の内容を指定します。  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  復元メディアに言語パックを追加します。 次のコマンドを実行して言語パックを追加します。  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     次の言語パックを追加する必要があります。  
  
    1.  WinPE 言語パック (lp.cab)  
  
    2.  WinPE-Setup 言語パック (WinPE-Setup_[lang].cab。たとえば、WinPE-Setup_en-us.cab など)  
  
    3.  zh-cn、zh-tw、zh-hk、ko-kr、ja-jp などのアジア系言語用フォントの場合、追加のフォント パックをインストールする必要があります (winpe-fontsupport-zh-cn.cab などの winpe-fontsupport-[lang].cab)  
  
6.  次の手順を実行して、新しい Lang.ini ファイルを生成します。  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  次の手順を実行して、イメージのコミットとマウント解除を行います。  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  [RestoreMediaroot]\Sources\Boot_x86.wim に対して手順 2 ～ 7 を繰り返します。  
  
9. 次の手順を実行して、イメージのコミットとマウント解除を行います。  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>関連項目  

 [作成して、イメージをカスタマイズします。](Creating-and-Customizing-the-Image.md)   
 [追加のカスタマイズ](Additional-Customizations.md)   
 [イメージの展開の準備](Preparing-the-Image-for-Deployment.md)   
 [カスタマー エクスペリエンスのテスト](Testing-the-Customer-Experience.md)

 [作成して、イメージをカスタマイズします。](../install/Creating-and-Customizing-the-Image.md)   
 [追加のカスタマイズ](../install/Additional-Customizations.md)   
 [イメージの展開の準備](../install/Preparing-the-Image-for-Deployment.md)   
 [カスタマー エクスペリエンスのテスト](../install/Testing-the-Customer-Experience.md)
