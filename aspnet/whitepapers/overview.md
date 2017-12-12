---
uid: whitepapers/overview
title: "ホワイト ペーパー |Microsoft ドキュメント"
author: rick-anderson
description: "このページでは、ホワイト ペーパーをインストールし、ASP.NET を構成して、セキュリティで保護された、高速、柔軟な ASP.NET アプリケーションを作成することを支援するためが表示されます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/15/2011
ms.topic: article
ms.assetid: d5e79470-01f2-4d65-8077-11c3e10a6784
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers
msc.type: content
ms.openlocfilehash: 260d9af87b7518131abb885ce2526b1aa7971bea
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whitepapers"></a>ホワイト ペーパー
====================
> このページでは、ホワイト ペーパーをインストールし、ASP.NET を構成して、セキュリティで保護された、高速、柔軟な ASP.NET アプリケーションを作成することを支援するためが表示されます。
> 
> - [ASP.NET 4](#aspnet4)
> - [ASP.NET のセキュリティに関するホワイト ペーパー](#security)
> - [インストールとセットアップのホワイト ペーパー](#setup)
> - [SQL Server ホワイト ペーパー](#sql)
> - [[全般] のホワイト ペーパー](#general)


<a id="aspnet4"></a>
## <a name="aspnet-4"></a>ASP.NET 4

ASP.NET 4 および Visual Studio 2010 に関連する情報。

[ASP.NET MVC 4 リリース ノート](mvc4-beta-release-notes.md "mvc4 リリース ノート")

このドキュメントでは、新しい機能と Visual Studio 2010、およびインストールに関する注意事項と既知の問題の ASP.NET MVC 4 Developer Preview で導入された機能強化について説明します。

[ASP.NET MVC 3 リリース ノート](mvc3-release-notes.md "mvc3 リリース ノート")

このドキュメントは、新しい機能と ASP.NET MVC 3 で導入された機能強化について説明します。 インストールに関する注意事項と既知の問題です。

[ASP.NET 4 および Visual Studio 2010 の Web 開発の概要](aspnet4/index.md "aspnet4")

ASP.NET の多数の魅力的な変更は、.NET Framework version 4 で受信されます。 このドキュメントでは、今後のリリースに含まれている新機能の多くの概要を示します。

[ASP.NET 4 Beta 2 の重大な変更](aspnet4/breaking-changes.md "における重大な変更")

このドキュメントが加えられた変更を .NET Framework のバージョンの ASP.NET 4 Beta 1 リリースを含む、以前のリリースを使用して作成されたアプリケーションに影響を与える可能性のある 4 つの Beta 2 リリース (つまり、ASP.NET 4 Beta 2 リリース) について説明します。

[ASP.NET MVC 2 の新](what-is-new-in-aspnet-mvc.md "aspnet mvc の新機能")

このドキュメントでは、新しい機能と ASP.NET MVC 2 で導入された機能強化について説明します。

[ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションをアップグレードする](aspnet-mvc2-upgrade-notes.md "aspnet mvc2 アップグレード ノート")

ASP.NET MVC 2 は、同じサーバー上の ASP.NET MVC 1.0 サイド バイ サイドでインストールできます。 これにより、アプリケーション開発者柔軟に ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションをアップグレードするタイミングを選択します。 このドキュメントについて両方 Visual でウィザードを使用して手動でアップグレードする方法.

<a id="security"></a>
## <a name="aspnet-security-whitepapers"></a>ASP.NET のセキュリティに関するホワイト ペーパー

セキュリティは、インターネット アプリケーションの重要な側面と、これらのホワイト ペーパーでは、ユーザーが設計し、セキュリティで保護された ASP.NET アプリケーションを実装する方法について説明します。

[ASP.NET 2.0 をインストルメントするセキュリティ用のアプリケーション](https://msdn.microsoft.com/en-us/library/ms998325.aspx)

この方法では、セキュリティ関連のイベントと操作を追跡するために、ASP.NET アプリケーションをインストルメント化するカスタムの状態監視イベントを使用する方法について説明します。 ASP.NET version 2.0 では、正常性監視するにはには、多くの標準のインストルメンテーションが含まれています。 を提供しています.

[ASP.NET 2.0 用セキュリティ展開の確認を実行します。](https://msdn.microsoft.com/en-us/library/ms998367.aspx)

この方法では、ASP.NET 2.0 アプリケーションの不適切な構成設定によって導入された潜在的なセキュリティの脆弱性を識別するためのセキュリティ展開レビューを実行する方法について説明します。 レビュー プロセスの大部分は、作成しています.

[ADAM を使用して ASP.NET 2.0 での役割](https://msdn.microsoft.com/en-us/library/ms998331.aspx)

この記事で説明する ASP.NET のロールを保存する Active Directory Application Mode (ADAM) を使用する ASP.NET Web サイトを開発する方法です。 ADAM と承認マネージャー (AzMan) のポリシー ストアを構成する方法を表示する新しいロールを作成する方法としています.

[ASP.NET 2.0 で承認マネージャー (AzMan) を使用します。](https://msdn.microsoft.com/en-us/library/ms998336.aspx)

ここで説明する説明、承認マネージャー (AzMan) を使用する AzMan ポリシー ストアに対して特定の操作を実行する ASP.NET ロール マネージャー ロールを管理し、ユーザー ロールのメンバーシップを確認し、ロール承認 API と共にします。 方法.

[ASP.NET 2.0 でのメンバーシップを使用します。](https://msdn.microsoft.com/en-us/library/ms998347.aspx)

ここで説明するには、バージョン 2.0 の ASP.NET アプリケーションで、このメンバーシップ機能を使用する方法を示します。 2 つの異なるメンバーシップ プロバイダーを使用する方法を示します。 ActiveDirectoryMembershipProvider と、SqlMembershipProvider です。 メンバーシップ機能しています.

[ASP.NET 2.0 のロール マネージャーを使用します。](https://msdn.microsoft.com/en-us/library/ms998314.aspx)

この方法では、ASP.NET 2.0 ロール マネージャーを使用する方法についてを説明します。 ロール マネージャーは、アプリケーションでの管理ロールおよびロール ベースの承認を実行するタスクを容易になります。 使用するためのさまざまなロール プロバイダーを構成する方法を示しますが、.

[ASP.NET 2.0 の Windows 認証を使用します。](https://msdn.microsoft.com/en-us/library/ms998358.aspx)

この方法では、構成および ASP.NET Web アプリケーションで Windows 認証を使用する方法について説明します。 Windows 認証は、ユーザー、Windows ドメインの一部であるときに推奨されるアプローチです。 このアプローチを使用する既存の id ストアを使用しています.

[マネージ コード (ベースライン アクティビティ) のセキュリティ コード レビューを実行します。](https://msdn.microsoft.com/en-us/library/ms998364.aspx)

この方法では、セキュリティ コード レビューを実行する方法についてを説明します。 このモジュールは、アクティビティ、および手法の結果を分析するために必要な手順を表示します。 この評価する方法を使用して"セキュリティの質問の一覧: マネージ コード (.NET Framework 2.0)"しています.

[ASP.NET 2.0 用セキュリティ展開の確認を実行します。](https://msdn.microsoft.com/en-us/library/ms998367.aspx)

この方法では、ASP.NET 2.0 アプリケーションの不適切な構成設定によって導入された潜在的なセキュリティの脆弱性を識別するためのセキュリティ展開レビューを実行する方法について説明します。 レビュー プロセスの大部分は、作成しています.

[Windows 2000 用に Kerberos 委任を実装します。](https://msdn.microsoft.com/en-us/library/aa302400.aspx)

Kerberos の委任では、ダウン ストリームの認証と承認をサポートするアプリケーションの複数の物理層で認証済み id をフローすることができます。 この作業を行うために必要な構成手順を表示する方法はこのです。

[ASP.NET 2.0 での権限借用と委任を使用します。](https://msdn.microsoft.com/en-us/library/ms998351.aspx)

この記事で説明する方法とタイミングは、ASP.NET 2.0 のアプリケーションで偽装を使用する必要があります。 既定では、権限借用がオフになり、ASP.NET Web アプリケーションのプロセス id を使用してリソースにアクセスすることができます。 ただし、次のように使用することができます。

[デザイン時に Web アプリケーションの脅威モデルを作成します。](https://msdn.microsoft.com/en-us/library/ms978527.aspx)

ここでは、Web アプリケーションの脅威モデルを作成するための方法について説明します。 脅威のアクティビティをモデル化に役立つ投資する前に、潜在的なセキュリティの設計上の欠陥や脆弱性を公開することができるように、セキュリティの設計をモデル化しています.

### <a name="forms-authentication"></a>フォーム認証

[ASP.NET 2.0 でフォーム認証を保護します。](https://msdn.microsoft.com/en-us/library/ms998310.aspx)

この方法では、安全に構成して、ASP.NET 2.0 アプリケーションのフォーム認証を使用する方法について説明します。 正しく認証チケットをセキュリティで保護して、ユーザー id ストアとそのストアへのアクセスをセキュリティで保護する主な考慮事項が含まれます。 ...

[ASP.NET 2.0 での Active Directory とフォーム認証を使用します。](https://msdn.microsoft.com/en-us/library/ms998360.aspx)

この方法では、Microsoft® Active Directory® ディレクトリ サービスで、ActiveDirectoryMembershipProvider を使用して、フォーム認証を使用する方法について説明します。 では、ユーザーの認証のプロバイダーを構成および作成する方法について説明しています.

[フォーム認証で ASP.NET 2.0 の複数のドメインの Active Directory を使用します。](https://msdn.microsoft.com/en-us/library/ms998345.aspx)

この方法では、Microsoft® Active Directory® ディレクトリ サービスで、ActiveDirectoryMembershipProvider を使用して、フォーム認証を使用する方法について説明します。 では、ユーザーの認証のプロバイダーを構成および作成する方法について説明しています.

[ASP.NET 2.0 で SQL Server とフォーム認証を使用します。](https://msdn.microsoft.com/en-us/library/ms998317.aspx)

この記事で説明する SQL Server のメンバーシップ プロバイダーでフォーム認証を使用することができます。 SQL Server でのフォーム認証は、アプリケーションのユーザーが、Windows ドメインの一部ではない場合に最も適したその結果、.

[フォーム認証で ASP.NET 1.1 で GenericPrincipal オブジェクトを作成します。](https://msdn.microsoft.com/en-us/library/aa302399.aspx)

この方法では、作成し、フォーム認証を使用する場合は、GenericPrincipal オブジェクトと FormsIdentity オブジェクトを処理する方法について説明します。

[ASP.NET 1.1 での Active Directory とフォーム認証を使用します。](https://msdn.microsoft.com/en-us/library/aa302397.aspx)

この方法に記事では、Active Directory の資格情報ストアに対するフォーム認証を実装する方法を示します。

[ASP.NET 1.1 での SQL Server とフォーム認証を使用します。](https://msdn.microsoft.com/en-us/library/aa302398.aspx)

この方法では、SQL Server 資格情報ストアに対するフォーム認証を実装する方法について説明します。 また、パスワード ダイジェストをデータベースに格納する方法も示します。

### <a name="user-input-data-validation"></a>ユーザー入力データの検証

[要求の検証 - スクリプト攻撃の防止](request-validation.md "要求の検証")

このホワイト ペーパーでは、ここで、既定では、アプリケーションが回避サーバーに送信されたエンコードされていない HTML コンテンツを処理から ASP.NET の要求の検証機能について説明します。 アプリケーションがされている場合は、この要求の検証機能を無効にすることができます.

[ASP.NET でのクロス サイト スクリプトを防ぐ](https://msdn.microsoft.com/en-us/library/ms998274.aspx)

ここで説明するには、適切な入力の検証方法を使用して、出力をエンコードすることによって、ASP.NET アプリケーションをクロスサイト スクリプティング攻撃から保護を支援する方法を示します。 使用できる他の保護機能の数についても説明しています.

[ASP.NET で SQL インジェクションから保護します。](https://msdn.microsoft.com/en-us/library/ms998271.aspx)

ここで説明するさまざまな ASP.NET アプリケーションが SQL インジェクション攻撃から保護する方法を示しています。 SQL インジェクションは、動的 SQL ステートメントを構築するために入力が使用するアプリケーションで、またはストアド プロシージャを使用してに接続するときに発生することが、.

[正規表現を使用して ASP.NET での入力を制限するには](https://msdn.microsoft.com/en-us/library/ms998267.aspx)

ここで説明するには、ASP.NET アプリケーション内で正規表現を使用して、信頼されていない入力を制限する方法を示します。 正規表現は、名前、アドレス、電話番号、およびその他のユーザー情報などのテキスト フィールドを検証することをお勧めします。 使用することができます。

### <a name="code-access-security"></a>コード アクセス セキュリティ

[ASP.NET 2.0 のコード アクセス セキュリティを使用します。](https://msdn.microsoft.com/en-us/library/ms998326.aspx)

この記事で説明する、アプリケーションの適切な信頼レベルを選択する方法と、必要に応じて、カスタム ASP.NET コード アクセス セキュリティ ポリシー ファイルをカスタム定義がレベルに信頼を作成する方法です。 別のコード アクセス セキュリティの信頼を使用することができます.

[暗号化のカスタム アクセス許可を作成します。](https://msdn.microsoft.com/en-us/library/aa302362.aspx)

ここでは、Win32® データ保護 API (DPAPI) を提供するアンマネージ暗号化機能へのプログラムによるアクセスを制御するには、カスタム コード アクセス セキュリティ権限を作成する方法について説明します。 このカスタム アクセス許可を使用して、管理対象の DPAPI ラッパーにしてください.

[コード アクセス セキュリティ ポリシーを使用してアセンブリを制限するには](https://msdn.microsoft.com/en-us/library/aa302361.aspx)

管理者は、.NET Framework コード (アセンブリ) の操作を制限するコード アクセス セキュリティ ポリシーを構成できます。 ここで、ファイル I/O を実行し、制限するアセンブリの機能を制限するコード アクセス セキュリティ ポリシーを構成しています.

### <a name="communications-security"></a>通信のセキュリティ

[Web サーバーで SSL を設定します。](https://msdn.microsoft.com/en-us/library/aa302411.aspx)

クライアント アプリケーションからの https 接続をサポートするために、SSL の Web サーバーを構成する必要があります。 この方法では、Web サーバーで SSL を構成する方法についてを説明します。

[クライアント証明書を設定します。](https://msdn.microsoft.com/en-us/library/aa302412.aspx)

IIS では、クライアント証明書認証をサポートします。 この方法では、クライアント証明書を要求するように Web アプリケーションを構成する方法について説明します。 クライアント コンピューターに証明書をインストールし、Web アプリケーションを呼び出すときに使用する方法も示します。

[IPSec を使用してポートと認証をフィルター処理](https://msdn.microsoft.com/en-us/library/aa302366.aspx)

インターネット プロトコル セキュリティ (IPSec) は、プロトコル、サービスではなく、暗号化、整合性、および IP ベースのネットワーク トラフィックの認証サービスを提供します。 IPSec は、サーバーの保護を提供するため、内部の脅威に対抗する IPSec を使用することができます.

[IPSec を使用して 2 つのサーバー間でセキュリティで保護された通信を提供します](https://msdn.microsoft.com/en-us/library/aa302413.aspx)

IPSec は、次の 2 つのサーバー間で暗号化されたチャネルを作成することができますを Windows 2000 で提供されるテクノロジです。 IP トラフィックをフィルタして、サーバーに対して認証するには、IPSec を使用することができます。 この記事では、セキュリティで保護された (暗号化) を提供する IPSec を構成する方法について説明しています.

[SQL Server との通信をセキュリティで保護する SSL を使用します。](https://msdn.microsoft.com/en-us/library/aa302414.aspx)

SQL Server データベース サーバーとの間に渡されたデータをセキュリティで保護することがアプリケーションにとって不可欠です。 SQL Server で SSL を使用して、暗号化されたチャネルを作成することができます。 この記事では、データベース サーバーに証明書をインストールする方法について説明しています.

[ASP.NET 1.1 からのクライアント証明書を使用して Web サービスを呼び出す](https://msdn.microsoft.com/en-us/library/aa302408.aspx)

この方法について説明することができますを渡す方法、クライアント証明書認証用 Web サービスに ASP.NET Web アプリケーションとは、Windows フォーム アプリケーションからします。 ローカル コンピューター ストアまたはユーザー ストアのいずれかでは、クライアント証明書をインストールできます。 もし。。。

[ASP.NET 1.1 からの SSL を使用して Web サービスを呼び出す](https://msdn.microsoft.com/en-us/library/aa302409.aspx)

Secure Sockets Layer (SSL) 暗号化は、整合性と Web サービスとの間で送受信されるメッセージの機密性を保証するために使用できます。 この方法では、Web サービスで SSL を使用する方法についてを説明します。

### <a name="cryptography"></a>暗号

[.NET 1.1 で DPAPI ライブラリを作成します。](https://msdn.microsoft.com/en-us/library/aa302402.aspx)

この方法では、データベース接続文字列やアカウントの資格情報をたとえば、データを暗号化するアプリケーションに DPAPI 機能を公開するマネージ クラス ライブラリを作成する方法について説明します。

[.NET 1.1 での暗号化ライブラリを作成します。](https://msdn.microsoft.com/en-us/library/aa302405.aspx)

この方法では、アプリケーションの暗号化機能を提供するマネージ クラス ライブラリを作成する方法について説明します。 これにより、アプリケーションの暗号化アルゴリズムを選択します。 サポートされているアルゴリズムには、DES、Triple DES、RC2 では、Rijndael が含まれます。

[ASP.NET 1.1 でレジストリに暗号化された接続文字列を保存します。](https://msdn.microsoft.com/en-us/library/aa302406.aspx)

アプリケーションは、Windows レジストリに接続文字列やアカウントの資格情報などの暗号化されたデータを格納することができます。 この方法では、格納およびレジストリの暗号化された文字列を取得する方法について説明します。

[ASP.NET 1.1 からの DPAPI (Machine ストア) の使用します。](https://msdn.microsoft.com/en-us/library/aa302403.aspx)

この方法では、ASP.NET Web アプリケーションまたは Web サービスからの DPAPI を使用して機密データを暗号化する方法について説明します。

[エンタープライズ サービスと ASP.NET 1.1 からの DPAPI (ユーザー ストア) の使用します。](https://msdn.microsoft.com/en-us/library/aa302404.aspx)

この方法では、ASP.NET Web アプリケーションまたはサービスからの DPAPI を使用して機密データを暗号化する方法について説明します。 ここで説明するには、エンタープライズ サービス コンポーネントをプロセスの使用を必要とするユーザー ストアで DPAPI を使用します。

<a id="setup"></a>
## <a name="installation-and-setup-whitepapers"></a>インストールとセットアップのホワイト ペーパー

これらのホワイト ペーパーでは、インストールして、サーバーで ASP.NET を構成するための手順を提供します。

[ASP.NET 2.0 のサービス アカウントを作成するアプリケーション](https://msdn.microsoft.com/en-us/library/ms998297.aspx)

この方法では、作成および ASP.NET Web アプリケーションを実行するカスタムの最小特権サービス アカウントを構成する方法について説明します。 既定では、Microsoft Windows Server 2003 および IIS 6.0 で ASP.NET アプリケーションは、組み込みネットワーク サービスを使用してを実行しています.

[ASP.NET 2.0 の複数のアプリケーションをホストする場合は、セキュリティを向上させる](https://msdn.microsoft.com/en-us/library/aa480478.aspx)

ここで説明、Web で共有のシステム リソースとの間に複数のアプリケーションを分離する方法の環境をホストしています。 ホスト環境によって、インターネット サービス プロバイダー (ISP) 複数のホストを提供している Web サーバーがあります.

[ASP.NET 2.0 で中程度の信頼を使用します。](https://msdn.microsoft.com/en-us/library/ms998341.aspx)

この方法では、中程度の信頼で実行する ASP.NET Web アプリケーションを構成する方法について説明します。 同じサーバー上の複数のアプリケーションをホストしている場合は、アプリケーション分離を提供するコード アクセス セキュリティと、中程度の信頼レベルを使用できます。 設定しています.

[ASP.NET 2.0 のリソースにアクセスするネットワーク サービス アカウントを使用します。](https://msdn.microsoft.com/en-us/library/ms998320.aspx)

この記事で説明する NT authority \network Service のコンピューター アカウントを使用して、ローカルにアクセスする方法とネットワーク リソースとします。 既定では Windows Server 2003 で、ASP.NET アプリケーションは、このアカウントの id を使用してを実行します。 最小限の特権がしています.

[ASP.NET 2.0 のプロトコル遷移および制約付き委任を使用します。](https://msdn.microsoft.com/en-us/library/ms998355.aspx)

この方法では、構成および ASP.NET アプリケーションをネットワーク リソースにアクセスを最初の呼び出し元を偽装するときに使用できるようにプロトコル遷移および制約付き委任を使用する方法について説明します。 Microsoft® Windows® 2000 オペレーティング システムしています.

[.NET Framework 1.0 および 1.1 の ASP.NET サイド バイ サイド実行](side-by-side-with-10.md "1.0 とサイド バイ サイド")

このホワイト ペーパーでは、ASP.NET Web アプリケーションをフレームワークのいずれかのバージョンで実行することにより、コンピューターに .NET 1.0 と .NET 1.1 の両方をインストールする方法について説明します。

[ASP.NET IIS ディレクトリへのアクセスの拒否](denied-access-to-iis-directories.md "iis ディレクトリへのアクセスが拒否されました")

このホワイト ペーパーでは、必要なことを ASP.NET アプリケーションに要求が、エラーを返した場合について説明します"へのアクセスを拒否*ディレクトリ名*ディレクトリ。 開始できませんでしたディレクトリの変更を監視します。"

[IIS 6.0 と ASP.NET 1.1 を実行している](aspnet-and-iis6.md "aspnet および iis6")

Windows Server 2003 には、IIS 6.0 および ASP.NET 1.1 の両方が含まれているときに、これらのコンポーネントは、既定で無効にします。 このホワイト ペーパーでは、IIS 6.0 および ASP.NET 1.1 を有効にする方法について説明し、最適なを取得するいくつかの構成設定を推奨しています.

[IE のセキュリティ更新プログラムを適用した後に 'サーバー アプリケーションが使用できない' エラーの修正プログラム](ms03-32-issue.md "ms 03-32 の問題")

このホワイト ペーパーでは、Windows XP Professional で実行されている ASP.NET 1.0 アプリケーションに影響する Internet Explorer の ms 03 32 セキュリティ更新プログラムの問題を修正する修正プログラムについて説明します。

[カスタム ASP.NET 1.1 を実行するアカウントを作成します。](https://msdn.microsoft.com/en-us/library/aa302396.aspx)

ASP.NET Web アプリケーションは通常、組み込みの ASPNET アカウントを使用してを実行します。 場合によっては、代わりにカスタム アカウントを使用することがあります。 この方法に記事では、ASP.NET Web アプリケーションの実行に最低限の特権のローカル アカウントを作成する方法を示します。 ...

### <a name="configuration"></a>構成

[ASP.NET 2.0 では、マシン キーを構成します。](https://msdn.microsoft.com/en-us/library/ms998288.aspx)

この方法について説明します、 &lt;machineKey&gt; Web.config ファイル内の要素を構成する方法を示しています、 &lt;machineKey&gt;コントロール不正使用防止機能の校正と ViewState、暗号化する要素のフォーム認証チケット、およびロール クッキー。 ViewState が署名しています.

[ASP.NET 2.0 の構成セクションを暗号化 DPAPI を使用します。](https://msdn.microsoft.com/en-us/library/ms998280.aspx)

ここで説明するには、プログラミング インターフェイス (DPAPI) の保護構成プロバイダーと Aspnet、Windows データ保護アプリケーションを使用する方法を示しています。\_regiis.exe ツール、構成ファイルのセクションを暗号化します。 Aspnet を使用する\_regiis.exe ツールをしています.

[ASP.NET 2.0 の構成セクションを暗号化 RSA を使用して](https://msdn.microsoft.com/en-us/library/ms998283.aspx)

ここで説明するには、RSA 保護構成プロバイダーと、"aspnet"の使用方法を示しています。\_regiis.exe ツール、構成ファイルのセクションを暗号化します。 Aspnet を使用する\_に保持されている接続文字列などの機密データを暗号化する regiis.exe ツール、.

[ASP.NET 2.0 での権限借用と委任を使用します。](https://msdn.microsoft.com/en-us/library/ms998351.aspx)

この記事で説明する方法とタイミングは、ASP.NET 2.0 のアプリケーションで偽装を使用する必要があります。 既定では、権限借用がオフになり、ASP.NET Web アプリケーションのプロセス id を使用してリソースにアクセスすることができます。 ただし、次のように使用することができます。

<a id="sql"></a>
## <a name="sql-server-whitepapers"></a>SQL Server ホワイト ペーパー

さまざまなデータベースと ASP.NET の連携、中にこれらのホワイト ペーパーを見て具体的には SQL Server に ASP.NET アプリケーションに接続します。

[ASP.NET 2.0 で SQL 認証を使用して SQL Server への接続します。](https://msdn.microsoft.com/en-us/library/ms998300.aspx)

この方法では、データベース アクセスの認証は、ネイティブの SQL 認証を使用する場合は、Microsoft® SQL Server™ に安全に ASP.NET アプリケーションを接続する方法について説明します。 Windows 認証があるために、SQL Server に接続することをお勧めしています.

[ASP.NET 2.0 の Windows 認証を使用して SQL Server への接続します。](https://msdn.microsoft.com/en-us/library/ms998292.aspx)

ここで説明する方法を示します SQL Server 2000 に接続する ASP.NET version 2.0 アプリケーションからの Windows サービス アカウントを使用します。 資格情報を格納しないようにするため、SQL 認証可能な限りではなく Windows 認証を使用する必要があります.

[SQL Server 2000 との通信をセキュリティで保護する SSL を使用します。](https://msdn.microsoft.com/en-us/library/aa302414.aspx)

SQL Server データベース サーバーとの間に渡されたデータをセキュリティで保護することがアプリケーションにとって不可欠です。 SQL Server で SSL を使用して、暗号化されたチャネルを作成することができます。 この記事では、データベース サーバーに証明書をインストールする方法について説明しています.

<a id="general"></a>
## <a name="general-whitepapers"></a>[全般] のホワイト ペーパー

次のケース スタディでは、Microsoft の .NET コミュニティ web サイトを従来のホスティング環境から Microsoft Azure に移行するために使用されたプロセスについて説明します。

[移行する Microsoft ASP.NET および Microsoft Azure に IIS.NET コミュニティ web サイト](https://go.microsoft.com/fwlink/?LinkId=400656)

これらのホワイト ペーパーでは、さまざまな ASP.NET に関するトピックについて説明します。

[ASP.NET 2.0 の正常性の監視を使用します。](https://msdn.microsoft.com/en-us/library/ms998306.aspx)

この方法では、状態監視のカスタム イベントのアプリケーションのインストルメント化を使用する方法について説明します。 カスタム性を監視するイベントを作成する System.Web.Management.WebBaseEvent から派生するクラスを作成、構成、 &lt;healthMonitoring&gt;しています.

[修正プログラム管理の実装](https://msdn.microsoft.com/en-us/library/aa302364.aspx)

ここで説明する 1 つを保持する方法など、複数のサーバーを最新の修正プログラムの管理について説明します。 追加のソフトウェアが Microsoft からダウンロード可能なツールを除く、必要ではありません。 操作とセキュリティ ポリシーは、修正プログラムの管理を採用する必要があります.

[Silverlight で Silverlight 3 SDK で ASP.NET サーバー コントロールします。](https://go.microsoft.com/fwlink/?LinkId=153377)

ASP.NET サーバー コントロールの Silverlight (「ASP.NET Silverlight コントロール」) は ASP.NET MediaPlayer および Silverlight のコントロールは、Silverlight SDK for Silverlight バージョン 3 から削除されています。 このドキュメントは、これらの ASP.NET に携わる開発者向けガイダンスを提供します。

[パフォーマンスの高い Web アプリケーションの構築](https://devexpress.com/act)

ASP.NET Ajax ライブラリに新しい機能を使用して、パフォーマンスに優れた Web アプリケーションをビルドする方法をについてください。
