---
uid: web-pages/readme/beta3
title: Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme |Microsoft Docs
author: rick-anderson
description: Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5aa609bf31499485dc7a1298fa689f3a7cee4774
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363534"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme
====================
> Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme

2010 年 11 月 9 日

## <a name="contents"></a>目次

- [概要](#Overview)
- [インストール](#Installation_Notes)
- [新しい機能、変更、およびベータ 3 リリースの既知の問題](#Known_Issues)

    - [WebMatrix のインストールに関する問題](#Known_Issues_Installation)
    - [ASP.NET Web ページ](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [アプリケーションのインストール](#Known_Issues_Installing_Applications)
    - [アプリケーションの発行](#Known_Issues_Publishing_Applications)
    - [その他の問題](#Known_Issues_Other_Issues)
- [詳細については](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>概要

> Microsoft WebMatrix のベータ版は、数分でインストールする無料の web 開発スタックです。 データベース プログラミング フレームワークが 1 つ、統合されたエクスペリエンスを作成すると、web サーバーが統合されています。 WebMatrix のベータ版を使用して、コーディング、テスト、および独自の ASP.NET または PHP web のサイトを公開する方法を効率化、または WebMatrix のベータ版を使用して、DotNetNuke、Umbraco、WordPress、Joomla などの一般的なオープン ソース アプリを使用して新しい web サイトを開始することができます。 WebMatrix のベータ版では、同じ強力な web サーバー、データベース エンジン、および円滑かつシームレスに開発から運用環境への移行を使用すると、インターネット上の web サイトを実行するフレームワークの環境を使用します。


<a id="Installation_Notes"></a>

## <a name="installation"></a>インストール

> 使用して WebMatrix Beta 3 をインストールする[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)します。 Web Platform Installer をインストールした後は、Beta 3 の WebMatrix のインストールに使用できます。
> 
> インストール中に問題がある場合を参照してください[Microsoft Web Platform Installer に関する問題をトラブルシューティング](https://go.microsoft.com/fwlink/?LinkId=196212)します。


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>アプリケーションを発行する手順

> 参照してください[でアプリケーションを公開する手順](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>新しい機能、変更、andKnown 問題

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix 3 をベータ版のインストール

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>問題: Beta 3 の WebMatrix は、Microsoft .NET Framework 4 をサポートするプラットフォームで利用可能なのみ

> .NET Framework version 4 が WebMatrix のベータ版に必要です。 場合によっては、WebMatrix のベータ版のインストーラーができます、サポートされている構成セットの一部ではないプラットフォームをインストールしようとしています。 具体的には、Windows Vista SP1 更新プログラムがないと、WebMatrix のベータ版のインストールを開始するが、.NET Framework 4 のコンポーネントは失敗し、インストールをブロックします。
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Microsoft Visual Studio 2008 SP1 がない場合に Microsoft Visual Studio 2008 がインストールされている場合、問題点: は WebMatrix Beta 3 をインストールことはできません。

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

ドキュメントのこのセクションでは、新機能、変更、および Razor 構文を使用して ASP.NET Web Pages のベータ 3 リリースの既知の問題について説明します。

- [新機能](#NewFeatures)
- [変更](#Changes)
- [問題](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Razor 構文を使用して ASP.NET Web Pages の Beta 3 の新機能

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>"Html.Raw"メソッドがエンコードされていないマークアップを表示する新機能。

> 新しい`Html.Raw`メソッドを使用して、エンコード済み出力をレンダリングするのではなくマークアップとして HTML マークアップを表示できます。 (既定では、ASP.NET Razor エンコード文字列に表示する前にします。)構文は次のとおりです。
> 
> `Html.Raw(value)`
> 
> `Html.Raw` を使用する方法を次の例に示します。
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Razor 構文を使用して ASP.NET Web Pages の Beta 3 の変更

#### <a name="change-hrefattribute-method-removed"></a>変更:"HrefAttribute"メソッドの削除

> `HrefAttribute`のメソッド、`WebPage`クラスは削除されました。 このヘルパーは Url の安全でない文字のエンコードに使用されました。 ASP.NET Razor 文字列を自動的にエンコードするためは必要なくなりました。 (新しい使用`Html.Raw`エンコードされていない文字列をレンダリングするメソッド)。


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>変更: 構文の宣言型"@helper"ヘルパーの変更

> 使用して作成されたヘルパーを解析する方法が ASP.NET Beta 3 リリースでは、変更、`@helper`構文。 基本的には、`@helper`構文がコードを含めることができるマークアップのブロックとしての代わりに、コード ブロックとして解析されるようになりました。 そのため、ヘルパー内のコードで囲む必要はない`@{ }`ブロックします。 ヘルパー内のマークアップがまたは ASP.NET Razor の HTML 要素で明示的に追加するのには逆に、`<text></text>`タグ。
> 
> たとえば、次`@helper`Beta 3 のリリースで構文の動作します。
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Beta 3 リリースでは、このヘルパーは、次の例のように変更する必要があります。
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> 注意、`@{ }`ヘルパーの最初のコードの周りの文字は使用されなくなりました。 これは、ヘルパーの内容が既定では、コード ブロックとして扱われるためにです。 ヘルパーは、オープンで始まる、マークアップを出力します。`<a>`タグ。 場合は、ヘルパーは、プレーン テキストまたは終了タグを含まないタグをレンダリングする必要があります (たとえば、`<meta>`タグ)、表示される内容がである必要があります`<text></text>`タグ。


#### <a name="change-webpagecontexthttpcontext-removed"></a>変更:"WebPageContext.HttpContext"の削除

> `WebPageContext.HttpContext`プロパティが削除されました。 代わりに、`HttpContext.Current` を使用してください。 (、`WebPageContext.HttpContext`プロパティが単にこれをラップします)。


#### <a name="change-facebook-helper-moved-to-new-package"></a>新しいパッケージに変更:"Facebook"ヘルパーの移動

> `Facebook`ヘルパー ページに移動しました、 *Facebook.Helper*ライブラリが含まれています、`Facebook`ヘルパーと機能を追加します。 必要がありますとしてインストールするこのライブラリを別のパッケージ「ヘルパーでマネージャーのパッケージをインストールする」」の説明に従って、チュートリアルでは[ASP.NET ページの概要](https://go.microsoft.com/fwlink/?LinkId=202889)します。


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>新しいアセンブリへの変更: メンバーシップ、ロール、およびセキュリティの種類の移動

> 次の種類に移動された、`WebMatrix.WebData`アセンブリ。
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>System.Web.WebPages.dll アセンブリへの変更:"TagBuilder"クラスの移動

> `TagBuilde` System.Web.WebPages.dll アセンブリに移動しました r クラス。 以前は、これは、ASP.NET MVC の一部であるアセンブリででした。 この変更により、ASP.NET MVC を使用するためにインストールする必要はありません、`TagBuilder`クラス。
> 
> ただし、クラスは、引き続き、`System.Web.Mvc`名前空間。 使用するには、`TagBuilder`クラス (たとえば、カスタム ASP.NET Razor ヘルパー)、名前空間を参照する必要があります (たとえば、追加することで`@using System.Web.Mvc`をコードに)。


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>変更: 要求の検証の構文が変更されてください。「検証」クラスの削除

> ベータ 3 リリースでは、個々 のフィールドまたはフィールドのセットの検証を無効に呼び出すことができます、`Validation.Exclude`メソッド、検証から除外するフィールドの名前を渡します。 新しい構文が検証をバイパス Beta 3 のリリースで使用します。 `Validation` Beta 3 で使用されるメソッドが削除されました。
> 
> > [!NOTE]
> > Web サイトでのようなエラーが報告される場合、ユーザーが、HTML マークアップを (たとえば、リッチ テキスト エディターを使用して、ページ上) をアップロードしようとしています場合、要求の検証を無効にしないでください、 *クライアントから可能性のある危険なRequest.Form値が検出されました*。ユーザー入力は許可されません。 要求の検証を無効にした場合、危険性のあるマークアップが含まれますしたりしないようなものを使用してスクリプトを確認するユーザー入力を手動で確認する必要があります、 [Microsoft Anti-cross Site Scripting ライブラリ V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)します。
> 
> 
> 自動の要求の検証を無効にするを呼び出す、`Request.Unvalidated`メソッド、フィールドまたはの要求の検証をバイパスするその他の投稿オブジェクトの名前を渡します。 このメソッドを使用してすべての項目の検証をバイパスすることができます、 `Form`、 `QueryString`、 `Cookies`、および`ServerVariables`コレクション。 次の例を使用する方法を示して、`Unvalidated`メソッド。
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Razor 構文を使用する ASP.NET Web Pages の既知の問題

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>問題: 予期しない動作のメンバーシップをカスタム ユーザー テーブルを使用する場合

> ASP.NET Razor の web サイトのメンバーシップ プロバイダーを初期化するために呼び出す、`WebSecurity.InitializeDatabaseConnection`メソッド。 (WebMatrix で、スターター サイト テンプレートには、このメソッドの呼び出しが含まれています、  *\_AppStart.cshtml*ファイルです)。場合、`autoCreateTables`このメソッドのパラメーターの設定を true に (に既定では、設定は、スターター サイト テンプレートの場合は true)、メソッドがエラーをスローしない場合は、認識できないテーブル名は、(2 番目のパラメーター) のメソッドに渡されます。 代わりに、テーブルを自動的に作成します。
> 
> これをカスタム ユーザー テーブルを使用して、メンバーシップが間違ったテーブル名を渡すする場合、問題になることができます、`WebSecurity.InitializeDatabaseConnection`メソッド。 メソッドは既定では、エラーは発生を指定するテーブルが存在しない場合、代わりに新しいテーブルを作成するため、アプリケーションが動作して表示できます。 ただし、カスタム ユーザー テーブル (および内のフィールド) に依存しているアプリケーション コード最終的に予期しないエラーで失敗します。
> 
> **回避策**  
> 渡された名前を確認、`InitializeDatabaseConnection`メソッドと一致するユーザー プロファイル、メンバーシップ データベースにテーブルまたはことを確認、`autoCreateTables`パラメーターが false に設定します。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>問題:"を SQL Server のユーザー インスタンスを生成できませんでした。

> WebMatrix Web アプリケーションが SQL Server Express を使用して、Windows 7 または Windows Server 2008 R2 に IIS 7.5 を実行している場合は、SQL Server が実行時にユーザーのローカル アプリケーション パスを取得できませんかを示すエラーが表示可能性があります。
> 
> **回避策**(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントが、アプリケーションのルート フォルダーとサブフォルダーの読み取り/書き込みアクセス許可をなどがあるかどうかを確認*アプリ\_データ*. 詳細については、サポート技術情報の記事で使用可能な[SQL Server Express ユーザー インスタンスと ASP.net Web アプリケーション プロジェクトに関する問題を](https://support.microsoft.com/kb/2002980)します。


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>問題: Visual Studio で、カスタム アセンブリ (Dll) の名前空間が自動的にインポートされません。

> Visual Studio のプロジェクトでカスタム アセンブリを使用する場合これらのアセンブリで宣言された名前空間はデザイン時に自動的にインポートされません。 その結果、カスタム型への参照は、デザイン時に認識されない可能性があり、(「波線」を使用) では認識されないの Visual Studio にマークされます。 この問題は Visual Studio でデザイン時にのみ発生します。アプリケーション自体が正常に実行されます。
> 
> **回避策**  
> 含まれて、`using`ステートメント (`imports` Visual Basic で) を参照するエンティティをデザイン時に認識されません。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>問題: Visual Studio IntelliSense およびプロジェクトで使用できるテンプレートのみ ASP.NET MVC version 3

> ASP.NET Web Pages をインストールすることもはインストールされません tools for Visual Studio ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートなど。
> 
> **回避策**を Visual Studio での ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクト テンプレートを使用するインストール ASP.NET MVC 3 RC、Web Platform Installer を使用するか、または[スタンドアロン インストーラー](https://go.microsoft.com/fwlink/?LinkID=191797)します。


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>問題:"&lt;ヘルパー&gt;クラスが見つかりません"エラー

> Beta 3 にアップグレードした後は、エラーが表示可能性があるヘルパー クラス (たとえば、`Facebook`クラス) は見つかりません。 Beta 2 を起動して、Beta 3 で手順を続行、ヘルパーを明示的にインストールする必要のあるパッケージに移動されました。 これらのパッケージを含めるには、既存のサイトはアップグレードされていません。内のサイトが含まれます、 *\My Documents\IISExpress*または*\My Documents\My Websites*フォルダー。 既定のサイトを使用する場合にこのエラーが表示されます具体的には、*個人用サイト*(WebSite1) への参照を含む、`Twitter`ヘルパー。
> 
> **回避策**  
> コメント アウトの実行、サイトのすべてのヘルパーを呼び出し、 *\_管理者*ページ、およびパッケージまたはを使用するヘルパーを含むパッケージをインストールします。 パッケージをインストールしたら後のヘルパーを参照している行のコメントを解除できます。


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>問題: Bin フォルダーに 3 ASP.NET Razor のベータ版のアセンブリの展開では使用できませんのサイトのホスト

> サイトの ASP.NET Razor のベータ 3 のアセンブリを展開する場合、ASP.NET Web Pages の web サイトをホストしているサイトに配置する場合と*Bin*フォルダーでは、次を含むエラーが発生する可能性があります。
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> これは、ホスティング プロバイダーが ASP.NET Web ページの Beta 1 のアセンブリをサーバーのグローバル アプリケーション キャッシュ (GAC) にインストールされている場合に発生することができます。 GAC のアセンブリがローカルでインストールされているアセンブリを優先順位を取得、 *Bin*フォルダー。
> 
> **回避策**が表示されるエラー プロバイダーのバージョン間の競合によりことを確認するホスティング プロバイダーに問い合わせて、アセンブリと自分の。 そうである場合、ホスティング プロバイダーが、サーバーの GAC にアセンブリを更新することを要求します。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>問題: フィードまたはプロキシ サーバー経由で他の外部データの読み取り

> プロキシ情報を構成する必要がありますが、サイトを実行しているサーバーがプロキシ サーバーの背後にある場合は、 *Web.config*ファイルをサイトの外部から送信される情報を読み取ることができるようにします。 たとえば、使用する場合、`ReCaptcha`ヘルパー、ヘルパーは、reCAPTCHA サービスと通信がプロキシ サーバーによってブロックされる可能性があります。 同様に、フィード、パッケージ マネージャーによって使用されているフィードなどの ASP.NET Web Pages を使用するには、プロキシ構成を必要があります。
> 
> 外部サービスの操作またはパッケージ フィードの操作で問題が発生する場合は、アプリケーションのルートに、次の要素を配置*Web.config*ファイル。
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> プロキシ サーバーを構成する方法の詳細については、次を参照してください。 [&lt;プロキシ&gt;要素 (ネットワーク設定)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web サイト。


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>問題:「Microsoft.Web.Infrastructure.dll を読み込むことができません」エラー

> すべての適切なアセンブリを除く GAC にインストールは Razor 構文を使用して、Beta 1 バージョンの ASP.NET Web ページを以前にインストールし、Beta 3 をインストールした場合、 *Microsoft.Web.Infrastructure.dll*します。 結果として、ASP.NET Razor ページを実行するときにエラーが表示されることを示します*Microsoft.Web.Infrastructure.dll*読み込めませんでした。
> 
> クリーンなコンピューターにベータ 3 リリースが読み込まれている場合、この問題は発生しません。
> 
> **回避策**  
> コントロール パネルで、ASP.NET Web Pages をアンインストールします。 ベータ 3 リリースの再インストールします。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>問題: .NET Framework version 4 をアンインストールする Razor 構文を使用して ASP.NET Web ページを無効にします

> .NET Framework version 4 をアンインストールし、再インストールして、Razor 構文を使用して ASP.NET Web Pages は無効になります。 ページで、 *.cshtml*拡張機能が正常に動作しません。 ASP.NET Web ページには、マシン ルートのアセンブリが登録されます*Web.config*ファイル、および .NET Framework の削除は、そのファイルを削除します。 .NET Framework を再インストール、構成ファイルの新しいバージョンがインストールされますが、ASP.NET Web Pages アセンブリの参照を追加できません。
> 
> **回避策**.NET Framework を再インストール後に Razor 構文を使用する ASP.NET Web ページを再インストールします。 次の要素が追加されます、 *Web.config*マシンのルートには、通常、次の場所にファイル。  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>問題: 以前に展開された Bin フォルダーにアセンブリを ASP.NET アプリケーションにエラーが発生します。

> デプロイでは、ASP.NET Web Pages アセンブリのコピー中に (たとえば、 *Microsoft.WebPages.dll*) に、 *Bin*サーバー上の web サイトのフォルダー。 (デプロイ時に自動的にこの発生した可能性または開発者が明示的にアセンブリをコピーするためです)。ただし、ベータ 3 リリースがインストールされているときにエラーが発生した特定の種類が見つからないエラーなどです。 これは、さまざまな ASP.NET Web ページの種類は、ベータ 3 リリースの異なる名前空間に移動されたために発生します。
> 
> **回避策**   
> クリア、 *Bin*デプロイしたアプリケーションのフォルダー、フォルダーに新しいアセンブリをコピー (またはアプリケーションを再配置) し、アプリケーションを再起動します。


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
> - サーバー コンピューター上のコントロールがいないかどうか (たとえばを展開しているホスティング web サイトに) 次に、web サイトの追加*Web.config*ファイル。
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>問題: 同じアプリケーションでの Web アプリケーション プロジェクトまたは ASP.NET MVC と ASP.NET Web ページの使用

> Web アプリケーション プロジェクトまたは ASP.NET MVC アプリケーションでは、ASP.NET Web Pages を使用していた、エラーは表示がありますを*WebPageHttpApplication*が見つかりません。
> 
> **回避策**  
> このエラーが発生する場合は、アプリケーションの派生元である基本クラスを変更します。 *Global.asax*ファイルで、次の行を変更します。
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> 次のように
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> これが、有効で導入された変更を反転、Beta 1 リリースの ASP.NET Web ページ Razor 構文を使用します。


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>問題: SQL Server Compact がインストールされていないコンピューターにアプリケーションを展開します。

> SQL Server Compact データベースを含むアプリケーションは、SQL Server Compact がインストールされていないコンピューターで実行できます。 Microsoft WebMatrix の Beta 3 が自動的がこれらのバイナリをコピーし、適切な実行*Web.config*ファイルの変換。
> 
> **回避策**これらのファイルをコピーする必要がある場合、 *Web.config*ファイルの変更を手動で、次の操作します。
> 
> 1. データベース エンジン アセンブリをコピー、 *Bin*ターゲット コンピューター上のアプリケーションのフォルダー (とサブフォルダー)。 
> 
>     - コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **に** *\Bin*
>     - コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **に** *\Bin\x86*
>     - コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **に** *\Bin\amd64*
> 2. Web サイトのルート フォルダーに作成または開き、 *Web.config*ファイル。 (WebMatrix Beta 3 では、このファイルの種類がクリックした場合に使用可能な**すべて**で、**ファイルの種類を選択** ダイアログ ボックス)。
> 3. 子として次の要素を追加、 **&lt;構成&gt;** 要素 (内部ではなく、 **&lt;system.web&gt;** 要素)。
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>問題: Visual Basic で中程度の信頼でデータベースとの WebGrid ヘルパーは機能しません

> Visual Basic を使用している場合 (作成 *.vbhtml*ファイル)、`Database`と`WebGrid`中程度の信頼を使用するアプリケーションが設定されている場合、ヘルパーは機能しません。
> 
> **回避策**  
> 完全な信頼を使用するアプリケーションを一時的に設定します。

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>問題:"Encrypt"プロパティを認識できません。

> SQL Server Compact 4.0 が認識されない、`Encrypt`のプロパティ、`SqlCeConnection`クラス。 データベース ファイルの暗号化には、このプロパティを使用する必要があります。 `Encrypt`プロパティは、SQL Server Compact 3.5 のリリースで非推奨とされましたし、旧バージョンと互換性のためだけに保持されました。 
> 
> **回避策**  
> 使用して、`Encryption Mode`のプロパティ、 `SqlCeConnection` SQL Server Compact 4.0 データベース ファイルを暗号化するクラス。 次の例では、暗号化された SQL Server Compact 4.0 データベースを作成する方法を示しています、`Encryption Mode`プロパティ。
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> 既存の SQL Server Compact 4.0 データベースの暗号化モードを変更するには、次の操作を行います。
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> 暗号化されていない SQL Server Compact 4.0 データベースを暗号化するには、次の操作を行います。
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>問題: Microsoft Visual C 2008 のランタイム ライブラリが必要です。

> ネイティブ Dll の SQL Server Compact 4.0 には、Microsoft Visual C 2008 ランタイム ライブラリ (x86、IA64、および x64)、Service Pack 1 が必要があります。
> 
> **回避策**  
> .NET Framework 3.5 SP1 をインストールします。 これには、Visual C 2008 ランタイム ライブラリ SP1 もインストールされます。 ライブラリは、次の場所からダウンロードできます。   
>   
> [Microsoft Visual C 2008 Service Pack 1 再頒布可能パッケージ ATL セキュリティ更新プログラム](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> .NET Framework 2.0、3.0 をインストールすることに注意してください。 または、4 は*いない*Visual c 2008 ランタイム ライブラリ SP1 をインストールします。


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>問題: SQL Server Compact がインストールされている場合、コンピューターに .NET Framework をインストールする前に、そのプロバイダーの不変名に登録されていない .NET Framework の machine.config ファイル

> SQL Server Compact は .NET Framework SQL Server Compact は .NET framework を必要があるためにインストールされていないコンピューターにインストールすることができます。 SQL Server Compact をインストールする前に .NET Framework version 3.5 も 4 をインストールする場合、SQL Server Compact のセットアップがでそのプロバイダー固定名を登録できません、 *machine.config*ファイル。 SQL Server Compact のエントリに依存している任意のアプリケーション、 *machine.config*ファイルは失敗します。 不変名登録エントリ*machine.config*次の例のようになります。
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **回避策**  
> SQL Server Compact 4.0 CTP1 をアンインストールします。 ダウンロードして、次の場所から .NET Framework の完全なバージョンをインストールします。
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0 のリリース (フル パッケージ)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> 再インストールし、 [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)します。


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>アプリケーションのインストール

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>問題: アプリケーションをインストールすることができます、長い時間がかかる場合は、ユーザーのマイ ドキュメント フォルダーはネットワーク共有にリダイレクトされます。

> **回避策**  
> なし。 アプリケーションをインストールするにはしばらくかかる場合がありますが、正常にインストールされます。


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>アプリケーションの発行

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>その他の問題

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>問題点:/検索フィルターでは機能しませんレポート グループ化: 問題の種類

> 内のテキストを入力した場合に、サイトのレポートを実行すると、 *URL フィルター*ボックスし、をクリックして*検索*、何も起こりません。 これは、このコントロールが中に機能していないため、 *Group By*レポートの状態に設定されて*問題の種類*、既定値します。
> 
> **回避策**で、 *Group By*  タブのリボンで、次のようにクリックします。 *URL*ソース URL を、別のエントリをグループ化します。 テキスト ボックスと、エントリをフィルター処理するボタンはこの状態中に機能します。


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>問題: WCF アプリケーションの IIS Express の実行に失敗します。

> 次のようなエラーが発生、WCF アプリケーションを参照します。
> 
> *ファイルまたはアセンブリを読み込むことができません ' Microsoft.Web.Administration、バージョン 7.0.0.0、カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つ。指定されたファイルが見つかりません。*
> 
> これは、IIS Express の Beta リリースは、既定で WCF をサポートしないために発生します。
> 
> **回避策**以下の回避策のいずれかを使用して、(回避策 2 には、Microsoft Windows Vista が必要がありますまたはそれ以降)。
> 
> 
> 1. コピー、 *Microsoft.Web.dll*と*Microsoft.Web.Administration.dll*に WebMatrix のインストール場所からアセンブリ、 *bin* WCF のディレクトリアプリケーション。 既定では、WebMatrix がインストールされている、 *Microsoft WebMatrix*システムの下のサブフォルダー *Program Files*フォルダー。
> 2. Microsoft Windows vista またはそれ以降、内のアセンブリへのシンボリック リンクを作成、 *bin*次のコマンドを使用してディレクトリ。 (このアプローチによって、アセンブリのコピーは作成できませんという利点があります)。
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. 2 つのアセンブリを GAC にインストールします。 管理者特権のプロンプトから次のコマンドを実行します。
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>問題: Beta 3 の WebMatrix は昇格が必要な特定のタスクを実行できません。

> Beta 3 の WebMatrix では、次の状況で追加コンポーネントのインストールなどの昇格が必要な特定のタスクを実行できません。
> 
> - Windows Vista または Windows 7 では、管理者特権がないアカウントを使用してログオンして、ユーザー アカウント制御 (UAC) が無効になっています。
> - Microsoft Windows XP または Microsoft Windows Server 2003 を使用しています。
> 
> **回避策**  
> WebMatrix の Beta 3 のほとんどのタスクでは、管理アクセス許可は必要ありません。 該当するには、管理者は、操作を実行するか、これらの手順に従います。
> 
> - Windows Vista または Windows 7 で UAC を有効にします。
> - Windows xp では、Administrators セキュリティ グループにユーザーを追加します。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>問題:「サイトから Web ギャラリー」が無効になっています

> **Web ギャラリーからのサイト**Web Platform Installer 3.0 がインストールされていない場合に、オプションが無効になっています。
> 
> **回避策**  
> インストール、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)します。


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>問題: Windows Server 2003 で IIS Express が起動しない管理者以外のユーザーの

> Windows Server 2003 では、ページを起動または IIS Express を起動するときに IIS Express は起動しません。 Web ページの管理者以外のユーザーによって、アプリケーションが開始されたことを示すエラーが表示されます。
> 
> **回避策**  
> 管理ユーザーとしては、Beta 3 の WebMatrix を起動します。 詳細については、次のサポート技術情報の記事を参照してください。  
>   
> [管理者以外のユーザーによって開始されたアプリケーションは、アプリケーションが Windows Vista、Windows Server 2003、または Windows XP で実行されているコンピューターの HTTP トラフィックをリッスンできません。](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>問題:「リレーションシップ」ボタンが無効になっています

> **リレーションシップ**下ボタン、**テーブル** タブで、**データベース**SQL Server Compact データベースのワークスペースが無効になっています。
> 
> **回避策**  
> なし。 SQL Server Compact は、テーブル間のリレーションシップはサポートしません。


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>パラメーター化された SQL クエリが例外をスローする問題点。

> SQL Server Compact 4.0 では、データ型をなど、指定しない場合に`SqlDbType`または`DbType`クエリの実行時にパラメーター化クエリでパラメーターの場合、例外がスローされます。
> 
> **回避策**  
> などのパラメーターのデータ型を明示的に設定`SqlDbType`または`DbType`します。 これは、BLOB データ型の場合に重要 (`image`と`ntext`)。 次のようなコードを使用します。
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>参照項目

Beta 3 の WebMatrix の詳細については、次の web サイトを参照してください。

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. All Rights Reserved. [利用規約](https://msdn.microsoft.cos/cc300389.aspx)します。
