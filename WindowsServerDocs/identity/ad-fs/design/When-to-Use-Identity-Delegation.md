---
ms.assetid: 6e711a96-9055-4508-b6d4-318d6aa95fd1
title: ID 委任が必要になるシナリオ
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2544001b871a1eda2c03005c384a99d5209e7282
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190546"
---
# <a name="when-to-use-identity-delegation"></a>ID 委任が必要になるシナリオ
  
## <a name="what-is-identity-delegation"></a>ID 委任はどのような場合に必要になるか  
Id 委任は、Active Directory フェデレーション サービスの機能\(AD FS\)管理者できる\-ユーザーの権限を借用するアカウントを指定します。 ユーザーの権限を借用するアカウントを、 *代理人と呼びます*。 この委任機能は、元の要求の承認チェーン内にある各アプリケーション、データベース、またはサービスに対して、一連のアクセス制御チェックを順次行う必要がある、多くの分散アプリケーションにおいて非常に重要です。 多くの現実\-世界のシナリオの存在を"フロント エンドの Web アプリケーションがより安全な「バックエンド」、Microsoft SQL Server データベースに接続されている Web サービスなどからデータを取得する必要があります。  
  
既存のパーツではたとえば、\-順序組織独自の購入履歴やアカウントの状態を表示するパートナーを許可するように Web サイトをプログラムで拡張できます。 専用の構造化照会言語上のセキュリティで保護されたデータベースにセキュリティ上の理由から、すべてのパートナーの財務データが格納されている\(SQL\)サーバー。 この状況では、前のコードで\-エンド アプリケーションをパートナー組織の財務データについて何も知っています。 ホストしているネットワーク上の別のコンピューターからそのデータをそのため、取得する必要があります\(ここで\)部品データベース用の Web サービス\(バック エンド\)します。  
  
このデータに対して\-取得プロセスを実行する一連の認証"手\-振る"の次の図に示すようには、Web アプリケーションと部品データベース用の Web サービス間で行う必要があります。  
  
![id 委任](media/adfs2_identitydelegationconcept.gif)  
  
元の要求は Web サーバー自体に対して送られますが、この種の Web サーバーは通常、アクセスしようとしているユーザーの組織とはまったく別の組織に存在するため、要求と共に送信されるセキュリティ トークンでは、Web サーバーの承認条件しか満たすことができず、それ以外のコンピューターにアクセスできません。 そのため、発信元のユーザーの要求に応えるには、リソース パートナー組織に中間フェデレーション サーバーを配置して、適切なアクセス特権を持ったセキュリティ トークンを再発行できるようにする必要があります。  
  
## <a name="how-does-identity-delegation-work"></a>ID 委任のしくみ  
多層アプリケーション アーキテクチャ内の Web アプリケーションは多くの場合、共通のデータや機能にアクセスするために、Web サービスを呼び出します。 ここで重要になるのは、これらの Web サービスが発信元ユーザーの ID を把握できるようにして、承認の判断や監査をそのサービスから行えるようにすることです。 この場合は、前面\-エンド Web アプリケーションは、代理人として、Web サービスへのユーザーを表します。 AD FS は、ユーザーが別の証明書利用者のパーティとして機能する Active Directory アカウントを許可することで、このシナリオを容易にします。 次の図は、ID 委任のシナリオを説明したものです。  
  
![id 委任](media/adfs2_identitydelegationsteps.gif)  
  
1.  Frank が、パーツにアクセスしようとしています。\-別の組織で Web アプリケーションからの履歴の順序付けします。 彼のクライアント コンピュータを要求して前面の AD FS からトークンを受信\-終了一部\-Web アプリケーションを順序付けします。  
  
2.  クライアント コンピューターは、手順 1 で取得したトークンと共に、Web アプリケーションへ要求を送信し、クライアントの ID を証明します。  
  
3.  Web アプリケーションは、クライアントのトランザクションを完了するために、Web サービスと通信する必要があります。 Web アプリケーションは、Web サービスと対話する委任トークンを取得する AD FS にアクセスします。 委任トークンとは、ユーザーとして動作する代理人に発行されるセキュリティ トークンです。 AD FS では、Web サービスの対象となる、クライアントの信頼性情報と共に、委任トークンを返します。  
  
4.  Web アプリケーションでは、手順 3. でクライアントとして動作している Web サービスへのアクセスの AD FS から取得されたトークンを使用します。 委任トークンを調べることで、Web サービスは Web アプリケーションがクライアントとして動作していることを確認できます。 Web サービスは承認ポリシーを実行し、要求をログに記録して、もともと Frank が要求した部品履歴データを、Web アプリケーション経由で Frank に提供します。  
  
特定のデリゲートでは、AD FS は、Web アプリケーションが、委任トークンを要求が Web サービスを制限できます。 クライアント コンピューターは、Active Directory アカウントを持っていなくてもこの操作を完了できます。 前述のように、Web サービスはユーザーとして動作している代理人の ID を簡単に確認できます。 そのため Web サービスは、クライアント コンピューターと直接やりとりしているのか、それとも代理人を通じてやりとりしているのかによって、異なる動作を実行することができます。  
  
## <a name="configuring-ad-fs-for-identity-delegation"></a>ID 委任を使用するための AD FS の構成  
AD FS 管理スナップインを使用する\-でデータの取得プロセスを容易にする必要があるたびに、id 委任用に AD FS を構成します。 AD FS が承認コンテキストに含まれる新しいセキュリティ トークンを生成を構成した後を背面\-エンド サービスは前に、保護されたデータへのアクセスを提供する必要があります。  
  
AD FS では、ユーザーの権限を借用できるは制限されません。 Id 委任用に AD FS を構成した後は、次は。  
  
-   ユーザー権限を借用するためのトークンを要求する権限を、どのサーバーに委任できるかを決定する。  
  
-   委任されるクライアント アカウントの ID コンテキストと、代理人として動作するサーバーの両方を確立し、個別に保持する。  
  
Id 委任を構成するには、AD FS 管理スナップインで信頼する証明書利用者のパーティに委任承認規則を追加することで\-でします。 これを行う方法の詳細については、次を参照してください。[チェックリスト。A Relying Party Trust の要求規則を作成する](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)します。  
  
## <a name="configuring-the-front-end-web-application-for-identity-delegation"></a>前の構成\-id 委任用の Web アプリケーションの終了  
開発者が Web フロントを適切にプログラムを使用できるいくつかのオプションがある\-AD FS コンピューターに委任要求をリダイレクトするには、アプリケーションまたはサービスを終了します。 Web アプリケーションをカスタマイズして ID 委任と連携させる方法の詳細については、「 [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)」を参照してください。  
  
## <a name="see-also"></a>関連項目
[Windows Server 2012 での AD FS 設計ガイド](AD-FS-Design-Guide-in-Windows-Server-2012.md)