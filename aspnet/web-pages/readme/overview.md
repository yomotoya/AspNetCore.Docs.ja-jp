---
uid: web-pages/readme/overview
title: WebMatrix Readme |Microsoft ドキュメント
author: rick-anderson
description: WebMatrix と ASP.NET Web Pages (Razor) 1.0 リリースの Readme
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="webmatrix-readme"></a>WebMatrix の Readme
====================
2011 年 1 月 13日

## <a name="contents"></a>目次

> [!NOTE]
> この readme は WebMatrix の 1.0 リリースに適用されます。


- [概要](#Overview)
- [インストール](#Installation_Notes)
- [アプリケーションを公開する方法](#InstructionsForPublishingApplications)
- [変更との問題](#ChangesAndIssues)

    - [WebMatrix 1.0 のインストール](#Known_Issues_Installation)
    - [ASP.NET Web ページ](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [アプリケーションをインストールします。](#Known_Issues_Installing_Applications)
    - [アプリケーションの発行](#Known_Issues_Publishing_Applications)
- [詳細については](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>概要

> Microsoft WebMatrix 1.0 は、数分でインストールする無料の web 開発スタックです。 データベース プログラミングのフレームワークを 1 つの統合環境を作成すると、web サーバーが統合されています。 効率よくコーディング、テスト、および独自 ASP.NET または PHP web サイトを発行、WebMatrix を使用することや、DotNetNuke、Umbraco、WordPress、または Joomla などの人気のオープン ソース アプリケーションを使用して新しい web サイトの起動に WebMatrix を使用することができます。 WebMatrix は、同じ強力な web サーバー、データベース エンジン、およびインターネット上に円滑かつシームレスな開発から運用環境への移行のため、web サイトを実行しているフレームワークの環境を使用します。


<a id="Installation_Notes"></a>

## <a name="installation"></a>インストール

> WebMatrix 1.0 をインストールするには、まず、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。 Web Platform Installer をインストールした後は、WebMatrix のインストールに使用できます。
> 
> インストール中に問題がある場合を参照してください[に関する問題のトラブルシューティング Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212)です。


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>アプリケーションを公開する方法

> 参照してください[アプリケーションを公開するための手順](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>変更との問題

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 のインストールに関する問題

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>問題点: WebMatrix 1.0 は Microsoft .NET Framework 4 をサポートするプラットフォームでのみ使用できます。

> .NET Framework version 4 が WebMatrix 必要です。 場合によっては、WebMatrix 1.0 インストーラー、サポートされている構成セットの一部ではない、プラットフォームにインストールしようとします。 具体的には、Windows Vista SP1 更新プログラムなしは、WebMatrix のインストールを開始するが、.NET Framework 4 コンポーネントは失敗し、インストールをブロックします。
> 
> **回避策**  
> 含む、サポートされているプラットフォームにインストールします。
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 以降
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Microsoft Visual Studio 2008 sp1 を適用せずに Microsoft Visual Studio 2008 がインストールされている場合、問題点: は WebMatrix 1.0 をインストールできません。

> **回避策**  
> インストール[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft ダウンロード センターからです。


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>問題点: SQL Server Compact 4.0 の一部のアセンブリは GAC にインストールされません。

> 64 ビット コンピューターで SQL Server Compact 4.0 をインストールして、コンピューターにのみ、.NET Framework 3.5 SP1 Client Profile をインストール時に SQL Server Compact 4.0 のマネージ アセンブリでは、グローバル アセンブリ キャッシュ (GAC) に配置されていません。 マネージ アセンブリを GAC にインストールされていない次のとおりです。
> 
> - *System.Data.SqlServerCe.dll* (ADO.NET プロバイダー)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **回避策**  
> SQL Server をアンインストール Compact 4.0 です。 ダウンロードして、次の場所から、フル バージョンの .NET Framework 3.5 SP1 をインストールします。  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> SQL Server Compact 4.0 を再インストールします。


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>問題点: SQL Server Compact のコマンドラインを使用してをアンインストールできません。

> SQL Server Compact のコマンド ライン オプションを使用してのアンインストールは、このリリースでは機能しません。
> 
> **回避策**  
> 使用して*プログラムと機能*Windows コントロール パネルで Microsoft SQL Server Compact 4.0 をアンインストールします。


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web ページ

ドキュメントのこのセクションでは、新機能、変更、および Razor 構文を使用して ASP.NET Web Pages の 1.0 リリースの既知の問題について説明します。

- [新機能](#NewFeatures)
- [変更](#Changes)
- [問題](#Issues)

#### <a id="NewFeatures"></a>  新機能

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>パッケージ マネージャーを無効にする構成設定が追加された新機能。

> 新しい`asp:AdminManagerEnabled`キーが利用可能、`<appSettings>`内の要素、 *web.config*ファイルで、パッケージ マネージャーを完全に無効にすることができます。 この要素の既定値は true に含まれていない場合、つまり、 *web.config*ファイル、パッケージ マネージャーは有効にします。 パッケージ マネージャーを無効にするには、次の要素を追加、 *web.config* web サイトのルートにあるファイル。
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  変更

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>"Asp:adminfoldervirtualpath"に変更の変更:"webPages:AdminFolderVirtualPath"キー

> `webPages:AdminFolderVirtualPath`キーに追加できる、 *web.config*を使用して、パッケージ マネージャーの場所を指定するファイルの名前は、`asp:`名前空間の代わりに、`webPages`名前空間。 この要素を使用している場合は、構成ファイルで変更する必要があります。


#### <a id="Issues"></a>  既知の問題

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>問題: メンバーシップ、ユーザーのパスワード認識されなくなった

> 安全なを作成して、メンバーシップ (ログイン) パスワードを格納するためのアルゴリズムが変更されました。 その結果、ASP.NET Razor のベータ版で作成されたメンバー (ユーザー) に格納されているパスワードは認識されません。 
> 
> **回避策**サイトが運用環境にまだ移行されていない場合は、メンバーシップ データベースからユーザー レコードを削除します。 データベースがライブの場合は、プログラムによって、メンバーシップ データベース内の既存のパスワードを再生成します。


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>メンバーシップのカスタム ユーザー テーブルを使用する場合の問題点: 予期しない動作

> ASP.NET Razor の web サイトのメンバーシップ プロバイダーを初期化するために呼び出す、`WebSecurity.InitializeDatabaseConnection`メソッドです。 (WebMatrix で、スターター サイト テンプレートにはこのメソッドの呼び出しが含まれています、  *\_AppStart.cshtml*ファイルです)。場合、`autoCreateTables`このメソッドのパラメーターが設定を true に (に既定では、設定は、スターター サイト テンプレートの場合は true)、認識できないテーブル名がメソッド (2 番目のパラメーター) に渡される場合、メソッドはエラーをスローしません。 代わりに、テーブルを自動的に作成します。
> 
> これは、カスタム ユーザー テーブルを使用してメンバーシップに間違ったテーブル名を渡す場合問題になることができます、`WebSecurity.InitializeDatabaseConnection`メソッドです。 メソッドは既定ではエラーを生成しない指定したテーブルが存在しない場合、代わりに新しいテーブルを作成するため、アプリケーションが動作して表示できます。 ただし、カスタム ユーザー テーブル (および内のフィールド) に依存するアプリケーション コード最終的に、予期しないエラーが失敗します。
> 
> **回避策**  
> 名前を渡すことを確認してください、`InitializeDatabaseConnection`メソッドと一致するユーザー プロファイル、メンバーシップ データベース内のテーブルまたはことを確認して、`autoCreateTables`パラメーターが false に設定します。


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>問題点: エラー メッセージ"Admin モジュールには、~/App へのアクセスが必要な\_データ"

> 状況によっては、ユーザーを作成またはそれ以外の場合、ASP.NET メンバーシップ システムを使用しようとしている可能性があります、エラーを表示するページ*Admin モジュールでは、~/App へのアクセスを必要と\_データ*です。 これで、IIS または IIS Express が実行されているアカウントが作成およびへの書き込みアクセス許可を持たない場合に発生、*アプリ\_データ*web サイトのルートの下のフォルダーです。 
> 
> **回避策**を手動で作成、*アプリ\_データ*web サイトのフォルダーです。 (通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントに、アプリケーションのルート フォルダーとサブフォルダー アプリなどの読み取り/書き込みアクセス許可があるかどうかを確認し、\_データ。 詳細については、サポート技術情報の記事で使用できる[問題と、SQL Server Express ユーザー インスタンスの ASP.net Web アプリケーション プロジェクト](https://support.microsoft.com/kb/2002980)です。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>問題:"を SQL Server のユーザー インスタンスを生成できませんでした。

> WebMatrix Web アプリケーションが SQL Server Express を使用して、Windows 7 または Windows Server 2008 R2 に IIS 7.5 を実行している場合は、SQL Server が実行時にユーザーのローカル アプリケーション パスを取得できないことを示すエラーを参照してください可能性があります。
> 
> **回避策**(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントがアプリケーションのルート フォルダーとサブフォルダーの読み取り/書き込みアクセス許可をなどがあるかどうかを確認*アプリ\_データ*. 詳細については、サポート技術情報の記事で使用できる[問題と、SQL Server Express ユーザー インスタンスの ASP.net Web アプリケーション プロジェクト](https://support.microsoft.com/kb/2002980)です。


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>問題点: パッケージ マネージャーのリソースまたはパッケージ マネージャーのパスワードを含むファイルは IIS 6.0 で含んだおよびそれ以前

> RC2 リリースを使用してビルドされた ASP.NET Web Pages (Razor) アプリケーションを展開する場合と、アプリケーションが含まれている場合、 *password.txt*または*packagesources.txt*下にあるファイル*/App\_データ管理/*、IIS 6.0 が要求されると、パッケージ マネージャー インスタンスのパスワードを公開する可能性がある場合、ファイルにサービスを提供します。 
> 
> **回避策**の名前を変更、 *password.txt*または*packagesources.txt*ファイルの名前を*password.config*または*packagesources.configに変更*.既定では、IIS 6.0 ファイルを処理しませんが、 *.config*拡張機能です。 (IIS 7 でのファイルはありません、*アプリ\_データ*フォルダーが提供されるは、ので、ファイルの名前を変更する必要はありません)。


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>問題: Beta 3 リリースを使用してインストールされているパッケージをアンインストールする完全に削除されませんパッケージのコンポーネント

> ベータ 3 リリースでは、パッケージ マネージャーを使用してパッケージをインストールしてから、現在のリリースを使用してアンインストールを再試行してください。 パッケージは完全にアンインストールされません。 パッケージ マネージャーの**アンインストール**ボタンの一部のコンポーネントを削除しますが、パッケージのライブラリ コードなりは更新されません、 *package.config*ファイル。
> 
> **回避策**   
> 次の手順を実行します。  
> 1. 削除、*アプリ\_Data\packages*フォルダーです。 これには、すべてのパッケージを削除します。   
> 2. 削除、 *packages.config* web サイトのルートにあるファイルです。


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>問題: Visual Studio で、web ベースのパッケージ マネージャーを呼び出すアプリケーションがオフライン、

> Visual Studio (WebMatrix ではなく) で作業しているし、使用するかどうか、  *\_admin*はアプリケーションをオフラインし、パッケージ マネージャー Visual Studio を起動する機能、*アプリ\_offline.htm*を web サイトのルートに、パッケージ マネージャーを使用する機能を中断されます。
> 
> [!NOTE]
> 最も多く、web ベースのパッケージ マネージャー インターフェイスを使用するときに、この動作を表示、動作は同じ場合、追加、削除、または内のファイルを変更、*アプリ\_データ*フォルダーです。
> 
> **回避策**   
> Visual Studio でパッケージを使用するには、web ベースのパッケージ マネージャーではなく、NuGet 拡張機能を使用します。 詳細については、次を参照してください。、 [NuGet のドキュメント](https://docs.microsoft.com/nuget/)です。 その他のファイルで作業している場合、*アプリ\_データ*フォルダーで、この問題を回避する別の場所でファイルのままにしてください。 実用的でない場合は、削除、*アプリ\_offline.htm*ファイルを手動でまたは自動的に (既定では、30 秒後)、サイトがオンラインに戻るまで待機します。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>問題: Visual Studio IntelliSense およびプロジェクトのテンプレート ASP.NET MVC version 3 でのみ使用できます。

> ASP.NET Web Pages をインストールするもインストールしません tools for Visual Studio の ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートなど。
> 
> **回避策**Visual Studio での ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートを使用するインストール ASP.NET MVC 3 RC、Web Platform Installer を使用するか、または[スタンドアロン インストーラー](https://go.microsoft.com/fwlink/?LinkID=191797)です。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>問題: フィードやプロキシ サーバーを経由して他の外部データの読み取り

> サイトを実行しているサーバーがプロキシ サーバーの背後にある場合でプロキシ情報を構成する必要があります、 *web.config*サイトの外部から来る情報を読み取ることができるためにファイル。 たとえば、使用する場合、`ReCaptcha`ヘルパーに渡し、ヘルパーは、reCAPTCHA サービスと通信が、プロキシ サーバーによってブロックされる可能性があります。 同様に、パッケージ マネージャーによって使用されているフィードなど ASP.NET Web Pages で使用されるフィードでは、プロキシの構成が必要です。
> 
> 外部サービスを扱うやパッケージ フィードの操作で問題が発生する場合は、アプリケーションのルートに、次の要素を格納*web.config*ファイル。
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> プロキシ サーバーの構成の詳細については、次を参照してください。 [&lt;プロキシ&gt;要素 (ネットワーク設定)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web サイトです。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>問題点:、Razor 構文を使用して ASP.NET Web Pages が無効になります、.NET Framework version 4 をアンインストールします。

> .NET Framework version 4 をアンインストールし、再インストールして、Razor 構文を使用して ASP.NET Web Pages は無効になります。 ページ、 *.cshtml*拡張機能が正しく動作しないことです。 ASP.NET Web ページで、マシン ルート アセンブリが登録*web.config*ファイル、および .NET Framework の削除は、そのファイルを削除します。 .NET Framework を再インストール、構成ファイルの新しいバージョンがインストールされますが、ASP.NET Web Pages アセンブリの参照を追加できません。
> 
> **回避策**.NET Framework を再インストール後に Razor 構文を使用する ASP.NET Web Pages を再インストールします。 次の要素を追加、 *web.config*マシン ルートでは、次の場所には、通常のファイル。  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>問題点: 拡張子のない Url が見つからない IIS 7 や IIS 7.5 で.cshtml/.vbhtml ファイル

> IIS 7 や IIS 7.5 では、次のように URL を使用して要求ができませんがページを検索する、 *.cshtml*または*.vbhtml*拡張機能。  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> URL 書き換えが有効でないため既定の IIS 7 や IIS 7.5、問題が発生します。 考えられるシナリオは IIS Express を使用してローカルでテストするときに問題は表示されないホスティング web サイトに web サイトを展開するときに発生します。
> 
> **回避策**
> 
> - サーバー コンピューターを制御する場合は、サーバー コンピューターの更新プログラムをインストールに記載されている[、更新プログラムを処理する IIS 7.0 または IIS 7.5 のハンドラーの要求の Url を特定の有効期間で終わっていないことがある](https://support.microsoft.com/kb/980368)です。
> - サーバー コンピューターの制御がないかどうか (たとえばを展開しているホスティング web サイトに) 次の web サイトの追加*web.config*ファイル。 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>問題点: SQL Server Compact がインストールされていないコンピューターにアプリケーションを展開します。

> SQL Server Compact データベースを含むアプリケーションは、SQL Server Compact がインストールされていないコンピューターで実行できます。 Microsoft WebMatrix 1.0 は自動的にするようなバイナリをコピーし、適切な実行*web.config*ファイルの変換。
> 
> **回避策**これらのファイルをコピーしする必要がある場合、 *web.config*ファイルの変更を手動で、次の操作します。
> 
> 1. アセンブリをコピーする、データベース エンジンに、 *Bin*ターゲット コンピューター上のアプリケーションのフォルダー (およびサブフォルダー)。  
> 
>    - コピー *C:\Program files \microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - コピー <em>C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>に</em></strong>\Bin\x86*
>    - Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>to</strong><em>\Bin\amd64</em>
> 
> 2. Web サイトのルート フォルダーに作成または開く、 *web.config*ファイル。 (WebMatrix 1.0 では、このファイルの種類がクリックした場合に使用可能な**すべて**で、**ファイルの種類を選択** ダイアログ ボックス)。
> 3. 次の要素の子として追加、`<configuration>`要素 (内部ではなく、`<system.web>`要素)。
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>問題: Visual Basic で中程度の信頼で「データベース」と"の WebGrid"ヘルパーは機能しません

> Visual Basic を使用している場合 (作成*.vbhtml*ファイル)、`Database`と`WebGrid`中程度の信頼を使用する、アプリケーションが設定されている場合、ヘルパーは機能しません。
> 
> **回避策**  
> Visual Studio 2010 を使用する場合は、Service Pack 1 リリースをインストールすることによってこの問題を解決できます。 SP1 からのベータ版をダウンロードするには SP1 のリリースの最終バージョンを使用するまで、 [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft ダウンロード センターのページです。   
>   
> これは実用的ではないかを一時的に Visual Studio 2010 を使用しない場合は、完全な信頼を使用するアプリケーションを設定します。


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>問題:"ApplicationPart"のリソースが外部からアクセスできます。

> アセンブリから派生したオブジェクトに含まれる場合、`ApplicationPart`クラス、アセンブリのリソースがによって公開されること、`ResourceRouteHandler`クラスです。 たとえば、次のような URL があるとします。  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> この要求は、リソースの文字列のすべてをダウンロード、 *System.Web.WebPages.Administration.dll*アセンブリ。 すべての埋め込みリソース (も含めてを静的なコンテンツとして提供するものではありません) がダウンロードされます。 埋め込みリソースに機密情報が含まれている場合は、セキュリティ上のリスクを表すこのことができます。 
> 
> **回避策**   
> 作成する場合、 **ApplicationPart**オブジェクトを埋め込みリソースに関連付けられているかどうかを確認**ApplicationPart**オブジェクトのアセンブリには機密情報は含まれません。


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> WebMatrix のインストールに関する問題については、次を参照してください。 [WebMatrix のインストールに関する問題](#Known_Issues_Installation)このドキュメントで既に説明します。


ドキュメントのこのセクションでは、WebMatrix 開発環境の既知の問題について説明します。

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>問題: データベース ワークスペースで、ユーザー名または web.config ファイル内のデータベース接続文字列のパスワードの変更は反映されません。

> **回避策**  
> 
> 1. *Web.config*ファイル、接続文字列でデータベース名を変更 (たとえば、「1」を追加) します。
> 2. 保存、 *web.config*ファイル。
> 3. をクリックして**データベース**および更新します。
> 4. 内の接続文字列でデータベース名を変更、 *web.config*ファイルを再度元のデータベース名。
> 5. 保存、 *web.config*ファイル。
> 6. をクリックして**データベース**および更新します。


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>問題点: WebMatrix によって作成されたフォルダーを削除できません。

> WebMatrix が高度な権限で実行されている場合 (WebMatrix を使用してを起動するは、**管理者として実行**Windows のオプション)、Windows エクスプ ローラーを使用して WebMatrix によって作成されるフォルダーを削除することはできません。
> 
> **回避策**  
> 高度な権限を使用して Windows エクスプ ローラーを実行します。 この場合は、以下の手順に従ってください。  
> 
> 1. Windows では、をクリックして**開始**です。
> 2. "Windows Explorer"を入力しのエントリを右クリックして**Windows エクスプ ローラー**です。
> 3. をクリックして**管理者として実行**です。 フォルダーを削除することができます。


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>問題: WebMatrix 1.0 は、昇格が必要な特定のタスクを実行することが

> WebMatrix 1.0 では、次の状況で追加のコンポーネントのインストールなどの昇格が必要な特定のタスクを実行できます。
> 
> - Windows Vista または Windows 7 を管理者特権を持たないアカウントでログオンして、ユーザー アカウント制御 (UAC) が無効になっています。
> - Microsoft Windows XP または Microsoft Windows Server 2003 を使用しています。
> 
> **回避策**  
> WebMatrix 1.0 のほとんどのタスクでは、管理アクセス許可は必要ありません。 ような状況に、管理者は、操作を実行したり、これらの手順に従います。
> 
> - Windows Vista または Windows 7 では、UAC を有効にします。
> - Windows xp では、Administrators セキュリティ グループにユーザーを追加します。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>問題:「サイトから Web ギャラリー」が無効になっています

> **Web ギャラリーからサイト**オプションには、Web Platform Installer 3.0 がインストールされていない場合は無効になります。
> 
> **回避策**  
> インストール、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>問題点: Google Chrome は、実行オプションとして使用できません。

> Google Chrome が下にあるブラウザーの一覧に表示されない**実行**上、**ホーム**タブです。
> 
> **回避策**  
> Google Chrome の一部のバージョンに登録しないで自体正しく、Windows の既定のプログラム機能します。 この問題を回避するには、Google Chrome を開始 をクリックして、*カスタマイズおよび制御 Google Chrome*  メニューのをクリックして*オプション*、順にクリック*Make Google Chrome の既定ブラウザー*です。


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>問題:「外部キー」 ダイアログ ボックス許可しない、プライマリ キーを入力

> **Foreign Key**  ダイアログ ボックス主キー テーブルからの主キーの名前を入力はできません。
> 
> **回避策**  
> これは意図的なものであり、 主キー テーブルの主キーの名前を入力する必要はありません。


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>問題点: IntelliSense では使用できません WebMatrix Razor 構文、C# の場合、または Visual Basic です。

> WebMatrix で、HTML および CSS 用の IntelliSense がサポートされています。 ただし、他の言語の使用はできません。 
> 
> **回避策**   
> なし。


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>問題点: HTML および CSS 用の IntelliSense の提案は適切なコンテキストではない要素

> WebMatrix 内のマークアップの IntelliSense サポートを使用して HTML、 [XHTML 1.0 Transitional スキーマ](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional)CSS を使用して、 [CSS 2.1 スキーマ](http://www.w3.org/TR/CSS2/)です。 IntelliSense は、これらの特定のスキーマに基づいているため特定のタグ、属性、またはプロパティ可能性がありますが提案するには不適切な現在のページまたはスタイルの定義。 HTML、可能性がありますと見なされます (たとえば、タグが閉じられていません) の形式が正しくない XHTML コンテンツでは予期しない提案する可能性がもできます。 この問題は、カーソルが不完全なタグ; 内にある場合に顕著する可能性があります。その場合は、IntelliSense は、新しいタグを開始を提案したり、他の不適切なヒントを表示することがします。 
> 
> **回避策**   
> HTML、適切な形式で完全な XHTML ページ内で作業していることを確認します。 CSS、回避策はありません。


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>問題点: 入力したときに IntelliSense は呼び出されません

> ときに、IntelliSense 可能性がありますは呼び出されません HTML または CSS がエディターで入力されているとします。 具体的には、この操作は、カーソルが別の要素の横にあるか、ファイルの最後にします。 
> 
> **回避策**   
> カーソルを前後に空白があることと、カーソルがファイルの末尾がないことを確認してください。 Ctrl キーを押しながら Space キーを押して手動で IntelliSense を呼び出すことができます。


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>問題点: UI にも IntelliSense を無効にします。

> WebMatrix 1.0 は UI またはジェスチャを IntelliSense を無効にします。 
> 
> **回避策**   
> IntelliSense を無効にするスイッチが含まれています、次のコマンドを使用して WebMatrix を開始します。  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express には、次の URL で使用できるは、独自の readme ファイルがあります。

[https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact では、次の URL で使用できるは、独自の readme ファイルがあります。

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

WebMatrix の一部として SQL Server Compact のインストールが関係する問題については、次を参照してください。 [WebMatrix のインストールに関する問題](#Known_Issues_Installation)このドキュメントで既に説明します。

### <a id="Known_Issues_Installing_Applications"></a>  アプリケーションをインストールします。

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>問題: アプリケーションをインストールすることができます、長い時間がかかる場合は、ユーザーのマイ ドキュメント フォルダーは、ネットワーク共有にリダイレクト

> **回避策**  
> なし。 アプリケーションをインストールするには時間かかる場合がありますが、正常にインストールされます。


### <a id="Known_Issues_Publishing_Applications"></a>  アプリケーションの発行

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>SQL Compact データベースを発行するときの問題:「必要なアクセス許可を取得できません」エラー

> WebMatrix は完全にサポートされません中程度の信頼の構成で .NET Framework version 3.5 を実行しているサーバーへの SQL Server Compact のサポートのバイナリの展開。
> 
> **回避策**  
> 推奨される回避策は、サーバーに .NET Framework 4 をインストールすることです。 また、次の操作を行います。
> 
> 1. 次の要素を追加して、 `SecurityClasses` 」の「 *Web\_MediumTrust.config*ファイル。
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. 新しいアクセス許可セットを作成、 *Web\_MediumTrust.config*次の必要なアクセス許可を持つファイル。
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. 次の要素を配置することにより、SQL Server Compact に設定するアクセス許可の適用、 *Web\_MediumTrust.config*ファイル。
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>発行後に問題点: ギャラリー、PhpBB の web アプリケーションの表示「サービスは利用できません」エラー

> 状況によっては、アプリケーションを公開すると、「サービスは利用できません」エラーが発生します。
> 
> **回避策**  
> WebMatrix で、円記号を追加 (\)でサーバー名の末尾に、**発行設定**ウィンドウからもう一度アプリケーションを発行するとします。


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>問題点: Moodle web サイトのレイアウトとリンクが破損している発行した後

> Moodle アプリケーションを発行した後、アプリケーションが正しく機能しません。
> 
> **回避策**  
> WebMatrix での末尾にスラッシュ (/) を追加、**サイト名**フィールドで、**発行設定**ウィンドウからもう一度アプリケーションを発行します。


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>問題点: nopCommerce を発行が失敗し、データベース エラー

> 発行 nopCommerce は失敗しのようなデータベース エラーの報告"、nop 挿入\_ログ table が失敗しました"。
> 
> **回避策**  
> 
> 1. クリックして WebMatrix で**実行**nopCommerce をローカルでを起動します。
> 2. [管理] ページにログインします。
> 3. クリックして、**システム**メニュー。
> 4. クリックして、**ログ**オプション。
> 5. クリックして、**ログの消去**ボタンをクリックします。
> 6. NopCommerce をもう一度公開します。


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>問題点: Silverstripe CMS エラーが表示される、"HTTP 500 PHP FCGI"発行されたサイトをダウンロードするときに

> **回避策**  
> クリックした後**ダウンロード サイトに発行**、スキップ`silverstripe-cache/manifest_main`で**公開プレビュー**です。 このファイルは、キャッシュの目的で使用され、各コンピューターに固有です。


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>問題点: サブ文字列が表示されます「サーバー エラーは '/' アプリケーションの」発行されたサイトをダウンロードするとき

> **回避策**  
> サイトの開く*web.config*ファイルを開き、SQL Server の管理者資格情報 ("sa"の資格情報) でユーザー ID と、データベース接続文字列にパスワードを置き換えます。
> 
> また、次の手順をログインで使用するユーザー アカウントを指定するために`db_owner`アクセス許可。
> 
> 1. Web Platform Installer を使用して SQL Server Management Studio をインストールします。
> 2. ローカルの SQL Server Express インスタンスに接続 (既定では、 `.\SQLEXPRESS`)。
> 3. をクリックして**データベース** &gt; *[localSubtextDatabase]* &gt; **セキュリティ** &gt; **ユーザー**&gt; *[localSubtextUser*] (既定値は`subtextuser`] を右クリックし、クリック**プロパティ**です。
> 4. 選択**db\_所有者**ロール メンバーシップ セクションでします。


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>問題:"リンク先の URL"フィールドは、プレフィックスが付いていない http:// または https:// 場合の発行後にサイトが機能しません。

> **公開設定**ダイアログ ボックスで、リンク先の URL で始まらない場合`http://`または`https://`展開後に、サイトが動かないことがあります。
> 
> **回避策**  
> 確認して、サイトにある送信先 URL を発行する前に、**発行設定** ダイアログ ボックスが始まる`http://`または`https://`です。


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>問題点: MySQL データベースのパブリッシュ エラーで失敗する、"データベースを発行できませんでした。 これは、場合に発生、リモート データベースは、スクリプトを実行できません。"

> さまざまな理由から、エラーが発生することができます。 このエラーを発生する理由の 1 つは、データベース スクリプトには、単一引用符文字 (') が含まれているとなり、対象の MySQL データベースの既定の文字セットは utf-8 です。
> 
> **回避策**  
> 既定の文字を utf-8 に、リモートの MySQL データベースのセットを設定します。


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>問題点: 一部のリンクが表示されない DotNetNuke の発行や、サイトのダウンロード

> パブリッシュまたは DotNetNuke サイトをダウンロードする場合は、サイトに表示する新しいリンクを取得するキャッシュをクリアする必要があります。
> 
> **回避策**
> 
> 1. "Host"としてログインします。
> 2. [ホスト] メニューに移動し、選択**ホスト設定**です。
> 3. スクロール ダウンし、**詳細設定**、展開**パフォーマンス設定**です。
> 4. クリックして、**キャッシュのクリア**ページへのリンク。
> 5. ページの下部に移動し、アプリケーションを再起動します。


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>問題: AtomSite で一部のリンクが壊れている発行されたサイトをダウンロードした後

> **回避策**  
> *Service.config*ファイル、 *users.config*ファイル、およびすべて*.xml*ファイル、URL 文字列を置換する (たとえば、 `http://myhost.com/atomsite`) ローカル サイトの (たとえば、 `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>発行し、データベース エラーを報告する問題点: WordPress と同様に、MySQL ベースのアプリケーションが失敗します。

> 既定では、WebMatrix は、utf-8 文字セットと MySQL をインストールします。 MySQL をインストールするには、独自の文字セットが utf-8 ではない場合 (たとえば、これがラテン語-1 の場合)、データベースの発行プロセスが失敗する可能性があります。
> 
> **回避策**
> 
> 1. MySQL を utf-8 の文字セットを変更します。 (詳細については、「[サーバー文字セットと照合順序](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html)MySQL web サイトです)。
> 2. アプリケーションを再インストールします。
> 3. アプリケーションを再発行します。


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>問題:「ダウンロード サイトに発行」セットアップのブラウザー ベースのアプリケーションが失敗します。

> 一部のアプリケーション (たとえば、Kentico CMS) では、データベースを作成するなど、インストール後セットアップを実行するために、ブラウザーで起動する必要があります。 ブラウザー ベースのセットアップを完了しなくても次のようにアプリケーションを発行すると、リモート サーバーから、同じサイトをダウンロードしようとしましたが失敗します。
> 
> **回避策**  
> サイトを発行する前に、ブラウザー ベースのセットアップを完了します。


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>問題:「ダウンロード サイトに発行」DotNetNuke および Kooboo CMS がデータベース エラーで失敗します。

> サーバーからアプリケーションをダウンロードしようとして、内のデータベース接続文字列に管理者の資格情報があるかどうか、**発行設定**ダイアログ ボックスで、発行は、ログに次のエラーを参照してください可能性があります。
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **回避策**  
> 可能であれば、サイトを再パブリッシュ (または公開する)、データベースに対する非管理者の資格情報を使用します。


<a id="More_Info"></a>

## <a name="for-more-information"></a>参照項目

WebMatrix 1.0 の詳細については、次の web サイトを参照してください。

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. All Rights Reserved. [利用規約](https://msdn.microsoft.cos/cc300389.aspx)です。
