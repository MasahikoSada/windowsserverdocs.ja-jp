---
title: デスクトップ ホスティングの Azure サービスと考慮事項
description: リモート デスクトップ ホスティング ソリューションに関する Azure に固有の考慮事項について説明します。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: dougkim
ms.openlocfilehash: c07a86c8168d323cf6e2af373ad51dc6a6b640b5
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "63749243"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>デスクトップ ホスティングの Azure サービスと考慮事項

>適用対象:Windows Server (半期チャネル)、Windows Server 2019、Windows Server 2016

次のセクションでは、Azure インフラストラクチャ サービスについて説明します。
  
## <a name="azure-portal"></a>Azure portal

プロバイダーが Azure サブスクリプションを作成した後、Azure portal を使用して、各テナントの環境を手動で作成できます。 PowerShell スクリプトを使用してこのプロセスを自動化することもできます。  

詳細については、[Microsoft Azure](https://www.azure.microsoft.com) の Web サイトをご覧ください。
  
## <a name="azure-load-balancer"></a>Azure Load Balancer

テナントのコンポーネントは、分離されたネットワーク上で相互通信する仮想マシン上で実行されます。 展開プロセス中は、リモート デスクトップ プロトコル エンドポイントまたはリモート PowerShell エンドポイントを使用して Azure Load Balancer を介してこれらの仮想マシンに外部的にアクセスできます。 通常、展開が完了すると、これらのエンドポイントは攻撃の危険性を縮小するために削除されます。 エンドポイントは、RD Web と RD ゲートウェイ コンポーネントを実行する仮想マシンのために作成された HTTPS および UDP エンドポイントだけになります。 これにより、インターネット上のクライアントはテナントのデスクトップ ホスティング サービスで実行されているセッションに接続できます。 Web ブラウザーなどのインターネットに接続するアプリケーションをユーザーが開いた場合、接続は Azure Load Balancer 経由で渡されます。  
  
詳しくは、「[Azure Load Balancer の概要](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)」を参照してください。
  
## <a name="security-considerations"></a>セキュリティに関する考慮事項

この Azure デスクトップ ホスティングの参照アーキテクチャ ガイドは、各テナントに対して高い安全性と分離された環境を提供するように設計されています。 システム セキュリティは、ホステッド サービスの展開と操作中にプロバイダーが講ずる安全対策にも依存します。 次の一覧で、この参照アーキテクチャに基づいてデスクトップ ホスティング ソリューションを安全に保つために、プロバイダーが考慮すべきいくつかの事項について説明します。

- すべての管理パスワードは強力なものとし、理想的にはランダムに生成され、頻繁に変更され、限られた数人のプロバイダー管理者のみがアクセスできるセキュリティが保護された中央の場所に保存される必要があります。  
- テナント環境を新しいテナント用に複製する場合、同一または脆弱な管理者パスワードを使用しないでください。
- RD Web アクセスのサイト URL、名前、および証明書は、スプーフィング攻撃を防止するために、一意であり各テナントから認識できるものにする必要があります。  
- デスクトップ ホスティング サービスの通常の操作中は、ユーザーをテナントのデスクトップ ホスティング クラウド サービスに安全に接続できるようにする RD Web および RD ゲートウェイ仮想マシンを除く、すべての仮想マシン用のすべてのパブリック IP アドレスを削除してください。 管理タスクに必要な場合にパブリック IP アドレスを一時的に追加することがありますが、後で必ず削除してください。  
  
詳しくは、次の記事をご覧ください。

- [セキュリティと保護](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [IIS 8 について推奨するセキュリティ運用方法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Windows Server 2012 R2 のセキュリティ](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>設計時の考慮事項

マルチ テナントのデスクトップ ホスティング サービスを設計するときに、Microsoft Azure インフラストラクチャ サービスの制約について考慮することが重要になります。 次の一覧で、この参照アーキテクチャに基づいて機能的かつコスト効果の高いデスクトップ ホスティング ソリューションを実現するために、プロバイダーが考慮すべき事項について説明します。  
  
- Azure サブスクリプションには、使用可能な仮想ネットワーク、VM コア、およびクラウド サービスの最大数が定められています。 これより多くのリソースを必要とする場合、プロバイダーは複数のサブスクリプションを作成しなければならない場合があります。
- Azure クラウド サービスには、使用可能な仮想マシンの最大数が定められています。 最大数を超える大規模なテナントの場合、プロバイダーは複数のクラウド サービスを作成しなければならない場合があります。  
- Azure の展開コストは、仮想マシンのサイズと数に部分的に基づいています。 プロバイダーは、機能的で安全性の高いデスクトップ ホスティング環境を低コストで提供するために、各テナントの仮想マシンの数とサイズを最適化する必要があります。  
- Azure データ センター内の物理コンピューター リソースは、Hyper-V を使用して仮想化されます。 Hyper-V ホストはホスト クラスター内で構成されていないため、仮想マシンの可用性は、Azure インフラストラクチャ内で使用される個々のサーバーの可用性に依存します。 可用性を高めるために、各役割サービスの仮想マシンの複数インスタンスを 1 つの可用性セット内に作成でき、その後、仮想マシン内にゲスト クラスタリングを実装できます。  
- 一般的なストレージ構成では、サービス プロバイダーは、複数のコンテナー (たとえばテナントごとに 1 つ) を持つ 1 つのストレージ アカウントを持ち、各コンテナーには複数のディスクがあります。 ただし、1 つのストレージ アカウントで実現できる記憶域とパフォーマンスの合計には制限があります。 多数のテナントをサポートするサービス プロバイダーや、ストレージ容量またはパフォーマンスについて高い要件を持つテナントの場合、サービス プロバイダーは複数のストレージ アカウントを作成する必要があります。  
  
詳しくは、次の記事をご覧ください。

- [Cloud Services のサイズ](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Microsoft Azure 仮想マシンの料金の詳細](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Hyper-V の概要](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Azure Storage のスケーラビリティとパフォーマンスのターゲット](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory アプリケーション プロキシ

Azure Active Directory (AD) アプリケーション プロキシは、ユーザーが Azure 独自のリバース プロキシ サービス経由で内部アプリケーションに接続できるようにする、Azure AD の有料 SKU で提供されるサービスです。 これにより、RD Web と RD ゲートウェイ エンドポイントを仮想ネットワーク内に隠し、パブリック IP アドレスによってインターネットに公開する必要性をなくすことができます。 ホスト側は Azure AD アプリケーション プロキシを使用して、完全な展開を維持しながら、テナントの環境内の仮想マシンの数を減らすことができます。 また、Azure AD アプリケーション プロキシでは、条件付きアクセスや多要素認証など、Azure AD が提供する多くの利点を得ることもできます。

詳細については、[アプリケーション プロキシの概要とコネクタのインストール](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable)に関するページを参照してください。