---
uid: web-pages/readme/overview
title: WebMatrix のリリース ノート |Microsoft Docs
author: rick-anderson
description: WebMatrix と ASP.NET Web Pages (Razor) 1.0 リリースの Readme
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: aa852e7bbd93622154d59e0d0a13ffa680812df2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836831"
---
<a name="webmatrix-readme"></a>WebMatrix の Readme
====================
2011 年 1 月 13日

## <a name="contents"></a>目次

> [!NOTE]
> この readme は WebMatrix の 1.0 リリースに適用されます。


- [概要](#Overview)
- [インストール](#Installation_Notes)
- [アプリケーションを発行する方法](#InstructionsForPublishingApplications)
- [変更点と問題](#ChangesAndIssues)

    - [WebMatrix 1.0 のインストール](#Known_Issues_Installation)
    - [ASP.NET Web ページ](#Known_Issues_ASPNET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [アプリケーションのインストール](#Known_Issues_Installing_Applications)
    - [アプリケーションの発行](#Known_Issues_Publishing_Applications)
- [詳細については](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>概要

> Microsoft WebMatrix 1.0 は、数分でインストールする無料の web 開発スタックです。 データベース プログラミング フレームワークが 1 つ、統合されたエクスペリエンスを作成すると、web サーバーが統合されています。 WebMatrix を使用して、コーディング、テスト、および独自の ASP.NET または PHP web のサイトを公開する方法を効率化、または WebMatrix を使用して、DotNetNuke、Umbraco、WordPress、Joomla などの一般的なオープン ソース アプリを使用して新しい web サイトを開始することができます。 WebMatrix は、同じ強力な web サーバー、データベース エンジン、および円滑かつシームレスに開発から運用環境への移行を使用すると、インターネット上の web サイトを実行するフレームワークの環境を使用します。


<a id="Installation_Notes"></a>

## <a name="installation"></a>インストール

> WebMatrix 1.0 をインストールする必要があります最初にインストールする、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)します。 Web Platform Installer をインストールした後は、WebMatrix のインストールに使用できます。
> 
> インストール中に問題がある場合を参照してください[Microsoft Web Platform Installer に関する問題をトラブルシューティング](https://go.microsoft.com/fwlink/?LinkId=196212)します。


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>アプリケーションを発行する方法

> 参照してください[でアプリケーションを公開する手順](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>変更点と問題

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>WebMatrix 1.0 のインストールに関する問題

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>問題: WebMatrix 1.0 は Microsoft .NET Framework 4 をサポートするプラットフォームでのみ使用できます。

> .NET Framework version 4 が WebMatrix に必要です。 場合によっては、WebMatrix 1.0 インストーラーができます、サポートされている構成セットの一部ではないプラットフォームをインストールしようとしています。 具体的には、Windows Vista SP1 更新プログラムがないと、WebMatrix のインストールを開始するが、.NET Framework 4 のコンポーネントは失敗し、インストールをブロックします。
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


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Microsoft Visual Studio 2008 SP1 がない場合に Microsoft Visual Studio 2008 がインストールされている場合、問題点: は WebMatrix 1.0 をインストールことはできません。

> **回避策**  
> インストール[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft ダウンロード センターから。


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>問題: SQL Server Compact 4.0 の一部のアセンブリが GAC にインストールされません。

> 64 ビット コンピューターで SQL Server Compact 4.0 をインストールして、コンピューターにのみ、.NET Framework 3.5 SP1 Client Profile インストールされているときに、SQL Server Compact 4.0 のマネージ アセンブリがグローバル アセンブリ キャッシュ (GAC) に配置されていません。 マネージ アセンブリを GAC にインストールされていない次のとおりです。
> 
> - *System.Data.SqlServerCe.dll* (ADO.NET プロバイダー)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **回避策**  
> アンインストールの SQL Server Compact 4.0。 ダウンロードして、次の場所から、フル バージョンの .NET Framework 3.5 SP1 をインストールします。  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> SQL Server Compact 4.0 を再インストールします。


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>問題: SQL Server Compact のコマンドラインを使用してをアンインストールできません。

> SQL Server Compact のコマンド ライン オプションを使用してのアンインストールは、このリリースでは機能しません。
> 
> **回避策**  
> 使用*プログラムと機能*Windows コントロール パネルの Microsoft SQL Server Compact 4.0 をアンインストールします。


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web ページ

ドキュメントのこのセクションでは、新機能、変更、および Razor 構文を使用して ASP.NET Web Pages の 1.0 リリースの既知の問題について説明します。

- [新機能](#NewFeatures)
- [変更](#Changes)
- [問題](#Issues)

#### <a id="NewFeatures"></a>  新機能

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>パッケージ マネージャーを無効にする構成設定が追加された新機能。

> 新しい`asp:AdminManagerEnabled`キーが利用可能、`<appSettings>`内の要素、 *web.config*ファイルで、パッケージ マネージャーを完全に無効にすることができます。 この要素の既定値は true、存在してしない場合、つまり、 *web.config*ファイル、パッケージ マネージャーを有効にします。 パッケージ マネージャーを無効にするには、次の要素を追加、 *web.config* web サイトのルートにあるファイル。
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  変更

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>変更:"webPages:AdminFolderVirtualPath"キーを"Asp:adminfoldervirtualpath"に変更されました

> `webPages:AdminFolderVirtualPath`キーに追加することができますが、 *web.config*を使用して、パッケージ マネージャーの場所を指定するファイルの名前は、`asp:`名前空間の代わりに、`webPages`名前空間。 この要素を使用している場合は、構成ファイルで変更する必要があります。


#### <a id="Issues"></a>  既知の問題

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>問題: メンバーシップ ユーザーが認識されなくパスワード

> 安全な作成とメンバーシップ (ログイン) パスワードを格納するためのアルゴリズムが変更されました。 その結果、ASP.NET Razor のベータ版で作成されたメンバー (ユーザー) に格納されているパスワードは認識されません。 
> 
> **回避策**サイトが運用環境にまだ配置されていない場合は、メンバーシップ データベースからユーザー レコードを削除します。 データベースがライブの場合は、プログラムでメンバーシップ データベース内の既存のパスワードを再生成します。


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>問題: 予期しない動作のメンバーシップをカスタム ユーザー テーブルを使用する場合

> ASP.NET Razor の web サイトのメンバーシップ プロバイダーを初期化するために呼び出す、`WebSecurity.InitializeDatabaseConnection`メソッド。 (WebMatrix で、スターター サイト テンプレートには、このメソッドの呼び出しが含まれています、  *\_AppStart.cshtml*ファイルです)。場合、`autoCreateTables`このメソッドのパラメーターの設定を true に (に既定では、設定は、スターター サイト テンプレートの場合は true)、メソッドがエラーをスローしない場合は、認識できないテーブル名は、(2 番目のパラメーター) のメソッドに渡されます。 代わりに、テーブルを自動的に作成します。
> 
> これをカスタム ユーザー テーブルを使用して、メンバーシップが間違ったテーブル名を渡すする場合、問題になることができます、`WebSecurity.InitializeDatabaseConnection`メソッド。 メソッドは既定では、エラーは発生を指定するテーブルが存在しない場合、代わりに新しいテーブルを作成するため、アプリケーションが動作して表示できます。 ただし、カスタム ユーザー テーブル (および内のフィールド) に依存しているアプリケーション コード最終的に予期しないエラーで失敗します。
> 
> **回避策**  
> 渡された名前を確認、`InitializeDatabaseConnection`メソッドと一致するユーザー プロファイル、メンバーシップ データベースにテーブルまたはことを確認、`autoCreateTables`パラメーターが false に設定します。


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a>問題: エラー メッセージ"Admin モジュールでは、~/App へのアクセスを必要と\_データ"

> 状況によっては、ユーザーを作成したり、それ以外の場合、ASP.NET メンバーシップ システムを使用しようとした可能性があります、エラーを表示するページ*Admin モジュールでは、~/App へのアクセスを必要と\_データ*します。 これは、IIS または IIS Express を実行しているアカウントが作成およびへの書き込みアクセス許可を持たない場合に発生します、*アプリ\_データ*web サイトのルートの下のフォルダー。 
> 
> **回避策**を手動で作成、*アプリ\_データ*web サイトのフォルダー。 (通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントが、アプリケーションのルート フォルダーとサブフォルダー アプリなどの読み取り/書き込みアクセス許可を持っているかどうかを確認して\_データ。 詳細については、サポート技術情報の記事で使用可能な[SQL Server Express ユーザー インスタンスと ASP.net Web アプリケーション プロジェクトに関する問題を](https://support.microsoft.com/kb/2002980)します。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>問題:"を SQL Server のユーザー インスタンスを生成できませんでした。

> WebMatrix Web アプリケーションが SQL Server Express を使用して、Windows 7 または Windows Server 2008 R2 に IIS 7.5 を実行している場合は、SQL Server が実行時にユーザーのローカル アプリケーション パスを取得できませんかを示すエラーが表示可能性があります。
> 
> **回避策**(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントが、アプリケーションのルート フォルダーとサブフォルダーの読み取り/書き込みアクセス許可をなどがあるかどうかを確認*アプリ\_データ*. 詳細については、サポート技術情報の記事で使用可能な[SQL Server Express ユーザー インスタンスと ASP.net Web アプリケーション プロジェクトに関する問題を](https://support.microsoft.com/kb/2002980)します。


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>問題: パッケージ マネージャーのリソースまたはパッケージ マネージャーのパスワードを含むファイルが IIS 6.0 で提供およびそれ以前

> RC2 リリースを使用して構築された ASP.NET Web Pages (Razor) アプリケーションをデプロイする場合と、アプリケーションが含まれている場合、 *password.txt*または*packagesources.txt*ファイル */App\_データ/管理者*、IIS 6.0 は要求されると、パッケージ マネージャー インスタンスのパスワードを公開する可能性がある場合は、ファイルにサービスを提供します。 
> 
> **回避策**の名前を変更、 *password.txt*または*packagesources.txt*ファイルを*password.config*または*packagesources.configに変更します*.既定では、IIS 6.0 ファイルを処理しませんが、 *.config*拡張機能。 (IIS 7 でのファイルはありません、*アプリ\_データ*フォルダーが提供されるは、ので、ファイルの名前を変更する必要はありません)。


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>問題: Beta 3 リリースを使用してインストール パッケージのアンインストールは完全に削除されませんパッケージのコンポーネント

> ベータ 3 リリースでは、パッケージ マネージャーを使用してパッケージをインストールしてから、現在のリリースを使用してアンインストールを再試行してください。 場合、パッケージは完全にアンインストールされません。 パッケージ マネージャーの使用**アンインストール**ボタンの一部のコンポーネントを削除しますが、パッケージのライブラリ コードし、は更新されません、 *package.config*ファイル。
> 
> **回避策**   
> これらの手順に従います。  
> 1. 削除、*アプリ\_Data\packages*フォルダー。 これには、すべてのパッケージが削除されます。   
> 2. 削除、 *packages.config* web サイトのルートにあるファイル。


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>問題: Visual Studio で、web ベースのパッケージ マネージャーを呼び出すはアプリケーションをオフライン

> Visual Studio (WebMatrix ではなく) で作業しているを使用したかどうか、 *\_管理者*パッケージ マネージャー Visual Studio を起動する機能、アプリケーションをオフラインにして、投稿、*アプリ\_offline.htm*を web サイトのルートに、パッケージ マネージャーを使用する機能を中断されます。
> 
> [!NOTE]
> 同じ動作には、追加、削除、または内のファイルを変更する場合に発生しますが、通常、web ベースのパッケージ マネージャーのインターフェイスを使用する場合は、この動作を確認した、*アプリ\_データ*フォルダー。
> 
> **回避策**   
> Visual Studio でパッケージを使用するには、web ベースのパッケージ マネージャーではなく、NuGet 拡張機能を使用します。 詳しくは、次を参照してください。、 [NuGet のドキュメント](https://docs.microsoft.com/nuget/)します。 他のファイルを使用している場合、*アプリ\_データ*フォルダー、この問題を回避するために別の場所でファイルを残すようにしてください。 ですが実用的でない場合は、削除、*アプリ\_offline.htm*ファイルを手動でまたは自動的に (既定では、30 秒後) で、サイトがオンラインに戻るまで待機します。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>問題: Visual Studio IntelliSense およびプロジェクトで使用できるテンプレートのみ ASP.NET MVC version 3

> ASP.NET Web Pages をインストールすることもはインストールされません tools for Visual Studio ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートなど。
> 
> **回避策**を Visual Studio での ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクト テンプレートを使用するインストール ASP.NET MVC 3 RC、Web Platform Installer を使用するか、または[スタンドアロン インストーラー](https://go.microsoft.com/fwlink/?LinkID=191797)します。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>問題: フィードまたはプロキシ サーバー経由で他の外部データの読み取り

> プロキシ情報を構成する必要がありますが、サイトを実行しているサーバーがプロキシ サーバーの背後にある場合は、 *web.config*ファイルをサイトの外部から送信される情報を読み取ることができるようにします。 たとえば、使用する場合、`ReCaptcha`ヘルパー、ヘルパーは、reCAPTCHA サービスと通信がプロキシ サーバーによってブロックされる可能性があります。 同様に、フィード、パッケージ マネージャーによって使用されているフィードなどの ASP.NET Web Pages を使用するには、プロキシ構成を必要があります。
> 
> 外部サービスの操作またはパッケージ フィードの操作で問題が発生する場合は、アプリケーションのルートに、次の要素を配置*web.config*ファイル。
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> プロキシ サーバーを構成する方法の詳細については、次を参照してください。 [&lt;プロキシ&gt;要素 (ネットワーク設定)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web サイト。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>問題: .NET Framework version 4 をアンインストールする Razor 構文を使用して ASP.NET Web ページを無効にします

> .NET Framework version 4 をアンインストールし、再インストールして、Razor 構文を使用して ASP.NET Web Pages は無効になります。 ページで、 *.cshtml*拡張機能が正常に動作しません。 ASP.NET Web ページには、マシン ルートのアセンブリが登録されます*web.config*ファイル、および .NET Framework の削除は、そのファイルを削除します。 .NET Framework を再インストール、構成ファイルの新しいバージョンがインストールされますが、ASP.NET Web Pages アセンブリの参照を追加できません。
> 
> **回避策**.NET Framework を再インストール後に Razor 構文を使用する ASP.NET Web ページを再インストールします。 次の要素が追加されます、 *web.config*マシンのルートには、通常、次の場所にファイル。  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>問題: 拡張子のない Url が見つからない IIS 7 または IIS 7.5.cshtml/.vbhtml ファイル

> IIS 7 または IIS 7.5 では、次のような URL を使用して要求をできないがページを検索、 *.cshtml*または *.vbhtml*拡張機能。  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> この問題は、URL リライトが有効でない既定で IIS 7 または IIS 7.5 のために発生します。 考えられるシナリオでは、IIS Express を使用してローカルでテストするときに問題は表示されないが、web サイトをホスティング web サイトにデプロイするときに発生することです。
> 
> **回避策**
> 
> - インストールで説明されている更新プログラムをサーバー コンピューターの場合は、サーバー コンピューター上のコントロールがある場合は、 [、更新プログラムを処理するために IIS 7.0 または IIS 7.5 のハンドラーは要求 Url を特定できますが、期間で終わっていない利用可能な](https://support.microsoft.com/kb/980368)します。
> - サーバー コンピューター上のコントロールがいないかどうか (たとえばを展開しているホスティング web サイトに) 次に、web サイトの追加*web.config*ファイル。 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>問題: SQL Server Compact がインストールされていないコンピューターにアプリケーションを展開します。

> SQL Server Compact データベースを含むアプリケーションは、SQL Server Compact がインストールされていないコンピューターで実行できます。 Microsoft WebMatrix 1.0 は自動的がこれらのバイナリをコピーし、適切な実行*web.config*ファイルの変換。
> 
> **回避策**これらのファイルをコピーする必要がある場合、 *web.config*ファイルの変更を手動で、次の操作します。
> 
> 1. データベース エンジン アセンブリをコピー、 *Bin*ターゲット コンピューター上のアプリケーションのフォルダー (とサブフォルダー)。  
> 
>    - コピー *C:\Program files \microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>        **to** *\Bin*
>    - コピー <em>C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>に</em></strong>\Bin\x86*
>    - コピー <em>C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>* <strong>に</strong><em>\Bin\amd64</em>
> 
> 2. Web サイトのルート フォルダーに作成または開き、 *web.config*ファイル。 (WebMatrix 1.0 では、このファイルの種類がクリックした場合に使用可能な**すべて**で、**ファイルの種類を選択** ダイアログ ボックス)。
> 3. 子として次の要素を追加、`<configuration>`要素 (内部ではなく、`<system.web>`要素)。
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>問題: Visual Basic で中程度の信頼で「データベース」と"の WebGrid"ヘルパーは機能しません

> Visual Basic を使用している場合 (作成 *.vbhtml*ファイル)、`Database`と`WebGrid`中程度の信頼を使用するアプリケーションが設定されている場合、ヘルパーは機能しません。
> 
> **回避策**  
> Visual Studio 2010 を使用する場合は、Service Pack 1 リリースをインストールすることによってこの問題を解決できます。 SP1 のベータ版をダウンロードするには、SP1 のリリースの最終バージョンが利用されるまで、 [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft ダウンロード センター ページ。   
>   
> これは実用的ではないかを一時的に Visual Studio 2010 を使用しない場合は、完全な信頼を使用するアプリケーションを設定します。


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>問題:"ApplicationPart"リソースが外部からアクセスできます。

> アセンブリから派生したオブジェクトに含まれる場合、`ApplicationPart`クラスによって、アセンブリのリソースが公開されること、`ResourceRouteHandler`クラス。 たとえば、次のような URL があるとします。  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> この要求は、リソースの文字列のすべてをダウンロード、 *System.Web.WebPages.Administration.dll*アセンブリ。 すべての埋め込みリソース (もしない静的なコンテンツとして提供することを意図したもの) がダウンロードされます。 埋め込みリソースに機密情報が含まれている場合は、セキュリティ上のリスクを表すこのことができます。 
> 
> **回避策**   
> 作成する場合、 **ApplicationPart**オブジェクト、その埋め込みリソースが関連付けられていることを確認**ApplicationPart**オブジェクトのアセンブリには機密情報が含まれていません。


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> WebMatrix のインストールの問題については、次を参照してください。 [WebMatrix のインストールに関する問題](#Known_Issues_Installation)この文書で前述しました。


ドキュメントのこのセクションでは、WebMatrix 開発環境の既知の問題について説明します。

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>問題: データベース ワークスペースで、ユーザー名または web.config ファイル内のデータベース接続文字列のパスワードの変更は反映されません。

> **回避策**  
> 
> 1. *Web.config*ファイルで、接続文字列でデータベース名を変更する (これには、「1」を追加など)。
> 2. 保存、 *web.config*ファイル。
> 3. クリックして**データベース**を更新します。
> 4. 接続文字列でデータベース名を変更、 *web.config*ファイルを再度元のデータベース名。
> 5. 保存、 *web.config*ファイル。
> 6. クリックして**データベース**を更新します。


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>問題: WebMatrix によって作成されたフォルダーを削除できません。

> WebMatrix では、昇格されたアクセス許可を使用して実行している場合 (WebMatrix を使用して開始した、**管理者として実行**Windows オプション)、Windows エクスプ ローラーを使用して WebMatrix で作成されるフォルダーを削除できません。
> 
> **回避策**  
> 昇格されたアクセス許可を使用して Windows エクスプ ローラーを実行します。 この場合は、以下の手順に従ってください。  
> 
> 1. Windows、 をクリックして**開始**します。
> 2. [Windows エクスプ ローラー] を入力し、エントリを右クリックして**Windows エクスプ ローラー**します。
> 3. クリックして**管理者として実行**します。 フォルダーを削除することができます。


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>問題: WebMatrix 1.0 は、昇格が必要な特定のタスクを実行することは

> WebMatrix 1.0 では、次の状況で追加コンポーネントのインストールなどの昇格が必要な特定のタスクを実行できません。
> 
> - Windows Vista または Windows 7 では、管理者特権がないアカウントを使用してログオンして、ユーザー アカウント制御 (UAC) が無効になっています。
> - Microsoft Windows XP または Microsoft Windows Server 2003 を使用しています。
> 
> **回避策**  
> WebMatrix 1.0 のほとんどのタスクでは、管理アクセス許可は必要ありません。 該当するには、管理者は、操作を実行するか、これらの手順に従います。
> 
> - Windows Vista または Windows 7 で UAC を有効にします。
> - Windows xp では、Administrators セキュリティ グループにユーザーを追加します。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>問題:「サイトから Web ギャラリー」が無効になっています

> **Web ギャラリーからのサイト**Web Platform Installer 3.0 がインストールされていない場合に、オプションが無効になっています。
> 
> **回避策**  
> インストール、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)します。


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>問題: Google Chrome は、[実行] オプションとして使用できません。

> Google Chrome が ブラウザーの一覧に表示されない**実行**上、**ホーム**タブ。
> 
> **回避策**  
> Google Chrome の一部のバージョンを登録しないで自体正しく Windows の既定のプログラム機能。 この問題を回避するには、Google Chrome を開始 をクリックして、*カスタマイズと制御 Google Chrome*  メニューのをクリックして*オプション*、順にクリックします*Make Google Chrome、既定のブラウザー*します。


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>問題:「外部キー」ダイアログ ボックスのしない許可に主キーを入力します。

> **Foreign Key**  ダイアログ ボックスは、主キー テーブルから主キーの名前を入力することを許可しません。
> 
> **回避策**  
> これは意図的なものであり、 主キー テーブルの主キーの名前を入力する必要はありません。


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>問題: IntelliSense は Razor 構文、c#、または Visual Basic の WebMatrix で使用できません。

> WebMatrix で、HTML および CSS の IntelliSense がサポートされています。 ただし、他の言語でご利用いただけません。 
> 
> **回避策**   
> なし。


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>問題: IntelliSense の HTML と CSS の提案がコンテキストに応じて適切でない要素

> WebMatrix のマークアップの IntelliSense サポートを使用して HTML、 [XHTML 1.0 Transitional スキーマ](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional)と CSS を使用して、 [CSS 2.1 スキーマ](http://www.w3.org/TR/CSS2/)します。 IntelliSense は、これらの特定のスキーマに基づいているため、特定のタグ、属性、またはプロパティが提案の現在のページまたはスタイルの定義に不適切な。 HTML の可能性もあります (たとえば、タグが閉じられていません) の形式が正しくない XHTML として解釈される可能性がありますコンテンツで予期しない候補にします。 この問題は、カーソルが; 不完全なタグ内にある場合に顕著な可能性があります。その場合は、IntelliSense は、新しいタグの開始をお勧めします。 または、他の不適切な提案を提供します。 
> 
> **回避策**   
> HTML、適切な形式で、完全な XHTML ページ内で作業していることを確認します。 CSS、回避策はありません。


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>問題: 入力するときに IntelliSense は呼び出されません

> HTML や CSS がエディターに入力されているように IntelliSense が呼び出されません可能性があります。 具体的には、カーソルが別の要素の横にある、またはファイルの最後に、これが発生可能性があります。 
> 
> **回避策**   
> カーソルを前後に空白があることと、カーソルがファイルの末尾でないことを確認してください。 Ctrl キーを押しながら Space キーを押して手動で IntelliSense を呼び出すことができます。


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>問題: UI がない IntelliSense を無効にするために使用できます。

> WebMatrix 1.0 は UI またはジェスチャを IntelliSense を無効にします。 
> 
> **回避策**   
> IntelliSense を無効にするスイッチを含む次のコマンドを使用して WebMatrix を起動します。  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express には、次の URL で利用可能な独自の readme ファイルがあります。

[https://go.microsoft.com/fwlink/?LinkID=207675&ampclcid = 0x409](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact では、次の URL で利用可能な独自の readme ファイルがあります。

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

WebMatrix の一部として SQL Server Compact のインストールに関連する問題については、次を参照してください。 [WebMatrix のインストールに関する問題](#Known_Issues_Installation)この文書で前述しました。

### <a id="Known_Issues_Installing_Applications"></a>  アプリケーションのインストール

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>問題: アプリケーションをインストールすることができます、長い時間がかかる場合は、ユーザーのマイ ドキュメント フォルダーはネットワーク共有にリダイレクトされます。

> **回避策**  
> なし。 アプリケーションをインストールするにはしばらくかかる場合がありますが、正常にインストールされます。


### <a id="Known_Issues_Publishing_Applications"></a>  アプリケーションの発行

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>SQL Compact データベースを発行するときに問題点:「に必要なアクセス許可を取得できません」エラー

> WebMatrix は完全にサポートしていません中程度の信頼の構成で .NET Framework version 3.5 を実行しているサーバーに SQL Server Compact のサポートのバイナリを展開します。
> 
> **回避策**  
> 推奨される回避策では、サーバーで、.NET Framework 4 をインストールします。 または、次の操作を行います。
> 
> 1. 次の要素を追加、`SecurityClasses`セクション*Web\_MediumTrust.config*ファイル。
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. 新しいアクセス許可のセットを作成、 *Web\_MediumTrust.config*次の必要なアクセス許可を持つファイル。
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. 次の要素を配置することで、SQL Server Compact に設定するアクセス許可の適用、 *Web\_MediumTrust.config*ファイル。
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>発行の後に問題点: ギャラリー、PhpBB の web アプリケーションの表示「サービスは利用できません」エラー

> 状況によっては、アプリケーションを公開すると、「サービスは利用できません」エラーが発生します。
> 
> **回避策**  
> WebMatrix で、追加の円記号 (\)でサーバー名の末尾に、**発行設定**ウィンドウし、もう一度アプリケーションを発行します。


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>問題: Moodle web サイトのレイアウトとリンクが破損している発行した後

> Moodle アプリケーションを発行した後、アプリケーションが正しく機能しません。
> 
> **回避策**  
> WebMatrix の末尾にスラッシュ (/) を追加、**サイト名**フィールドに、**発行設定**ウィンドウし、もう一度アプリケーションを発行します。


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>データベース エラーの問題: nopCommerce の発行が失敗します。

> NopCommerce の発行が失敗しのようなデータベース エラーの報告"します nop 挿入\_ログ table は失敗しました"。
> 
> **回避策**  
> 
> 1. WebMatrix で、次のようにクリックします。**実行**nopCommerce をローカルで起動します。
> 2. [管理] ページにログインします。
> 3. をクリックして、**システム**メニュー。
> 4. をクリックして、**ログ**オプション。
> 5. をクリックして、**ログの消去**ボタンをクリックします。
> 6. NopCommerce をもう一度発行します。


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>問題: Silverstripe CMS エラーが表示される"HTTP 500 PHP FCGI"公開済みのサイトをダウンロードするときに

> **回避策**  
> クリックした後**ダウンロード サイトに発行**、スキップ`silverstripe-cache/manifest_main`で**発行のプレビュー**します。 このファイルは、キャッシュのために使用あり、各コンピューターに固有です。


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>問題: Subtext は「サーバー エラーは '/' アプリケーションで」を表示します発行されたサイトをダウンロードするときに。

> **回避策**  
> サイトの開く*web.config*ファイルを開き、ユーザー ID とパスワード、データベース接続文字列で SQL Server 管理者の資格情報 ("sa"の資格情報) に置き換えます。
> 
> または、次の手順をログインに使用したユーザー アカウントを付与するには`db_owner`アクセス許可。
> 
> 1. Web Platform Installer を使用して SQL Server Management Studio をインストールします。
> 2. ローカルの SQL Server Express インスタンスに接続 (既定では、 `.\SQLEXPRESS`)。
> 3. クリックして**データベース** &gt; *[localSubtextDatabase]* &gt; **セキュリティ** &gt; **ユーザー**&gt; *[localSubtextUser*] (既定値は`subtextuser`] を右クリックし、クリック**プロパティ**します。
> 4. 選択**db\_所有者**ロール メンバーシップのセクションでします。


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>問題:"送信先 URL"フィールドが http:// または https:// でプレフィックスが付いていない場合に発行した後サイトが機能しません。

> **公開設定**ダイアログ ボックスで、リンク先の URL で始まらない場合`http://`または`https://`デプロイ後に、サイトが機能しない可能性があります。
> 
> **回避策**  
> 確認して、サイトでリンク先の URL を発行する前に、**発行設定** ダイアログ ボックスが始まる`http://`または`https://`します。


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>"データベースを発行できませんでしたエラーで失敗 MySQL データベースのパブリッシュ問題点:。 これは、場合に発生、リモート データベースは、スクリプトを実行できません。"

> さまざまな理由からこのエラーが発生します。 このエラーが表示できる理由の 1 つは、データベース スクリプトには、単一引用符文字 (') が含まれているし、対象の MySQL データベースの既定の文字セットを utf-8 ではないです。
> 
> **回避策**  
> 既定の文字が utf-8 に、リモートの MySQL データベースの設定を設定します。


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>問題: 一部のリンクが表示されない DotNetNuke の発行や、サイトのダウンロード

> パブリッシュまたは DotNetNuke サイトをダウンロードする場合は、サイトに表示される新しいリンクを取得するキャッシュをクリアする必要があります。
> 
> **回避策**
> 
> 1. "Host"としてログインします。
> 2. [ホスト] メニューに移動し、選択**ホスト設定**します。
> 3. スクロール ダウンし、**詳細設定**、展開**パフォーマンス設定**します。
> 4. をクリックして、**キャッシュのクリア**ページのリンク。
> 5. ページの下部に移動し、アプリケーションを再起動します。


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>問題: AtomSite でいくつかのリンクが破損している発行済みのサイトをダウンロードした後

> **回避策**  
> *Service.config*ファイル、 *users.config*ファイル、およびすべて *.xml*ファイル、URL 文字列に置き換えます (たとえば、 `http://myhost.com/atomsite`) ローカル サイトの (たとえば、 `http://localhost:1239`).


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>問題: 発行およびデータベースのエラーを報告する WordPress のように MySQL ベースのアプリケーションが失敗します。

> 既定では、WebMatrix は、utf-8 文字セットで MySQL をインストールします。 自分で MySQL をインストールして、文字セットが utf-8 ではない場合 (たとえば、これはラテン語-1 の場合)、データベースの発行プロセスが失敗する可能性があります。
> 
> **回避策**
> 
> 1. MySQL への utf-8 の文字セットを変更します。 (詳細については、次を参照してください[Server 文字セットと照合順序](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html)、MySQL の web サイトです。)。
> 2. アプリケーションを再インストールします。
> 3. アプリケーションを再発行します。


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>問題:「ダウンロードで発行されたサイト」を持つブラウザー ベースのセットアップ アプリケーションが失敗します。

> 一部のアプリケーション (たとえば、Kentico CMS) では、データベースを作成するなど、インストール後セットアップを実行するには、ブラウザーで起動する必要があります。 ブラウザー ベースのセットアップを完了しなくてもこのようなアプリケーションを発行すると、リモート サーバーから、同じサイトのダウンロードを試みては失敗します。
> 
> **回避策**  
> サイトを発行する前に、ブラウザー ベースのセットアップを完了します。


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>問題:「発行されたサイトをダウンロード」DotNetNuke と Kooboo CMS がデータベース エラーで失敗します。

> サーバーからアプリケーションをダウンロードしようとして、管理者の資格情報がデータベース接続文字列であるかどうか、**発行設定**ダイアログ ボックスで、発行ログには、次のエラーを表示可能性があります。
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **回避策**  
> 可能であれば、サイトを再パブリッシュ (または公開する) データベースの非管理者の資格情報を使用します。


<a id="More_Info"></a>

## <a name="for-more-information"></a>参照項目

WebMatrix 1.0 の詳細については、次の web サイトを参照してください。

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. All Rights Reserved. [利用規約](https://msdn.microsoft.cos/cc300389.aspx)します。
