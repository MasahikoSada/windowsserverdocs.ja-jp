---
title: リモート デスクトップ サービス - Multi-factor Authentication
description: RDS で MFA を使用するための計画に関する情報。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 5ca2a29b0287dbd940afeb4404a85f1d978447f9
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805113"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>リモート デスクトップ サービス - Multi-factor Authentication

>適用対象:Windows Server (半期チャネル)、Windows Server 2019、Windows Server 2016

Multi-factor Authentication で Active Directory を活用し、ビジネス リソースの高度なセキュリティ保護を適用します。

デスクトップやアプリケーションに接続するエンドユーザーの場合、そのエクスペリエンスは、目的のリソースに接続するために 2 つ目の認証方法を実行するときに既に取り組んでいるものに似ています。
- RDP ファイルから、またはリモート デスクトップ クライアント アプリケーションを介して、デスクトップまたは RemoteApp を起動します
- セキュリティで保護されたリモート アクセスのために RD ゲートウェイに接続すると、SMS またはモバイル アプリケーションによる MFA チャレンジを受けます
- 正しく認証して、そのリソースに接続します。

構成プロセスの詳細については、[ネットワーク ポリシー サーバー (NPS) 拡張機能と Azure AD を使用してリモート デスクトップ ゲートウェイ インフラストラクチャを統合する](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)ことに関するページを参照してください。
