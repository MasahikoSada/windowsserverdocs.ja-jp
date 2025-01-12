---
title: ネットワークポリシーサーバーのユーザーデータコレクション
description: Windows Server 2016 のネットワークポリシーサーバーによってユーザーを認証するためにどのような情報が使用されます。
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: cd145402ed70aa52da7188dee9dd64ce17fea155
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871882"
---
# <a name="network-policy-server-user-data-collection"></a>ネットワークポリシーサーバーのユーザーデータコレクション

このドキュメントでは、ネットワークポリシーサーバー (NPS) によって収集されたユーザー情報を削除したい場合に、その情報を検索する方法について説明します。

>[!Note]
>個人データの表示や削除に関心がある場合は、「 [GDPR サイトの Windows データ主体の要求](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows)」に記載されているマイクロソフトのガイダンスを参照してください。 GDPR に関する一般情報をお探しの場合は、 [Service Trust portal の GDPR セクション](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)を参照してください。

## <a name="information-collected-by-nps"></a>NPS によって収集された情報

- Timestamp
- イベントのタイムスタンプ
- Username
- 完全修飾ユーザー名
- クライアント IP アドレス
- クライアントベンダー
- クライアントのフレンドリ名
- [認証の種類]
- RADIUS プロトコルに関する他の多くのフィールド

## <a name="gather-data-from-nps"></a>NPS からデータを収集する

アカウンティングデータが有効で構成されている場合は、構成に応じて、SQL Server またはログファイルからユーザーの NPS 認証試行のレコードを取得できます。 

アカウンティングデータが SQL Server 用に構成されている場合は、User_Name `'<username>'`= であるすべてのレコードに対してクエリを実行します。

アカウンティングデータがログファイルに対して構成されている場合は、ログ`<username>`ファイルでを検索して、すべてのログエントリを検索します。

ネットワークポリシーとアクセスサービスのイベントログエントリは、アカウンティングデータにマルチ商法と見なされ、収集する必要はありません。

アカウンティングデータが有効になっていない場合は、 `<username>`を検索することにより、ネットワークポリシーとアクセスサービスのイベントログからユーザーの NPS 認証試行のレコードを取得できます。
