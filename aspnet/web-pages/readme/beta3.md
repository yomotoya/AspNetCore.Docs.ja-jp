---
uid: web-pages/readme/beta3
title: "マトリックスと ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme を web |Microsoft ドキュメント"
author: rick-anderson
description: "Webmatrix と ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: def2f4b3e54c8de539e10c1b526a1dababeca8fb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Webmatrix と ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme
====================
> Webmatrix と ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme

2010 年 11 月 9日

## <a name="contents"></a>目次

- [概要](#Overview)
- [インストール](#Installation_Notes)
- [新しい機能、変更、および、ベータ 3 リリースの既知の問題](#Known_Issues)

    - [WebMatrix のインストールに関する問題](#Known_Issues_Installation)
    - [ASP.NET Web ページ](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [アプリケーションをインストールします。](#Known_Issues_Installing_Applications)
    - [アプリケーションの発行](#Known_Issues_Publishing_Applications)
    - [その他の問題](#Known_Issues_Other_Issues)
- [詳細については](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>概要

> Microsoft WebMatrix のベータ版は、数分でインストールする無料の web 開発スタックです。 データベース プログラミングのフレームワークを 1 つの統合環境を作成すると、web サーバーが統合されています。 効率よくコーディング、テスト、および独自 ASP.NET または PHP web サイトを発行、WebMatrix のベータ版を使用するか、DotNetNuke、Umbraco、WordPress、または Joomla などの人気のオープン ソース アプリケーションを使用して新しい web サイトの起動に WebMatrix のベータ版を使用することができます。 WebMatrix のベータ版では、同じ強力な web サーバー、データベース エンジン、およびインターネット上に円滑かつシームレスな開発から運用環境への移行のため、web サイトを実行しているフレームワークの環境を使用します。


<a id="Installation_Notes"></a>

## <a name="installation"></a>インストール

> 使用するベータ 3 の WebMatrix をインストールするには、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。 Web Platform Installer をインストールした後は、ベータ 3 の WebMatrix をインストールして使用できます。
> 
> インストール中に問題がある場合を参照してください[に関する問題のトラブルシューティング Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212)です。


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>アプリケーションを公開するための手順

> 参照してください[アプリケーションを公開するための手順](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>新しい機能、変更、andKnown 問題

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix ベータ 3 のインストール

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>問題点: WebMatrix ベータ 3 は Microsoft .NET Framework 4 をサポートするプラットフォームで利用可能

> .NET Framework version 4 が WebMatrix のベータ版必要です。 場合によっては、WebMatrix のベータ版のインストーラー、サポートされている構成セットの一部ではない、プラットフォームにインストールするとします。 具体的には、Windows Vista SP1 更新プログラムなしで、WebMatrix のベータ版のインストールを開始しますが、.NET Framework 4 コンポーネントは失敗し、インストールはブロックします。
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Microsoft Visual Studio 2008 sp1 を適用せずに Microsoft Visual Studio 2008 がインストールされている場合、問題点: は WebMatrix Beta 3 をインストールことはできません。

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

ドキュメントのこのセクションでは、新機能、変更、および Razor 構文を使用して ASP.NET Web Pages の Beta 3 リリースの既知の問題について説明します。

- [新機能](#NewFeatures)
- [変更](#Changes)
- [問題](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Razor 構文を使用する ASP.NET Web Pages のベータ 3 の新機能

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>"Html.Raw"メソッドがエンコードされていないマークアップを表示する新機能。

> 新しい`Html.Raw`メソッドにより、エンコードされた出力をレンダリングするのではなくマークアップとして HTML マークアップを表示できます。 (既定では、ASP.NET Razor エンコード文字列それらを表示する前にします。)構文は次のとおりです。
> 
> `Html.Raw(value)`
> 
> `Html.Raw` を使用する方法を次の例に示します。
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Razor 構文を使用する ASP.NET Web Pages のベータ 3 の変更

#### <a name="change-hrefattribute-method-removed"></a>削除の変更:"HrefAttribute"メソッド

> `HrefAttribute`のメソッド、`WebPage`クラスは削除されました。 このヘルパーは、Url に安全でない文字をエンコードに使用されました。 ASP.NET Razor 文字列を自動的にエンコードするためは必要なくなりました。 (新しい使用`Html.Raw`エンコードされていない文字列をレンダリングするメソッドです)。


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>変更: 構文宣言"@helper"ヘルパーの変更

> 使用して作成するヘルパーを解析する方法が ASP.NET Beta 3 リリースで変更、`@helper`構文です。 基本的には、`@helper`構文が、マークアップ コードを含めることができるブロックの代わりに、コード ブロックとして解析されるようになりました。 したがって、ヘルパー内のコードがで囲む必要ありません`@{ }`ブロックします。 ヘルパーの内側のマークアップがや ASP.NET Razor の HTML 要素で明示的に追加するのには逆に、`<text></text>`タグ。
> 
> たとえば、次`@helper`構文は、ベータ 3 リリースで機能します。
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> ベータ 3 リリースでは、次の例のようになりますにこのヘルパーを変更する必要があります。
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> 注意して、`@{ }`ヘルパーの最初のコードの周りの文字は使用されなくなりました。 これは、ヘルパーの内容が既定では、コード ブロックとして扱われるためです。 ヘルパー マークアップ、オープンで始まる`<a>`タグ。 ヘルパーは、プレーン テキストや終了タグを含まないタグをレンダリングする必要があります場合、(たとえば、`<meta>`タグ) である必要があります、表示される内容を`<text></text>`タグ。


#### <a name="change-webpagecontexthttpcontext-removed"></a>変更:"WebPageContext.HttpContext"の削除

> `WebPageContext.HttpContext`プロパティが削除されました。 代わりに、`HttpContext.Current` を使用してください。 (、`WebPageContext.HttpContext`プロパティが単にこれをラップします)。


#### <a name="change-facebook-helper-moved-to-new-package"></a>新しいパッケージを変更:"Facebook"ヘルパーの移動

> `Facebook`ヘルパーに移動されました、 *Facebook.Helper*ライブラリが含まれている`Facebook`ヘルパーと機能を追加します。 必要がありますとしてインストールするこのライブラリ別のパッケージ「ヘルパーとマネージャーのパッケージをインストールする」」の説明に従って、チュートリアルでは[ASP.NET ページの使用を開始する](https://go.microsoft.com/fwlink/?LinkId=202889)です。


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>新しいアセンブリに変更します。 メンバーシップ、ロール、およびセキュリティの種類の移動

> 次の種類に移動された、`WebMatrix.WebData`アセンブリ。
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>System.Web.WebPages.dll アセンブリへの変更:"TagBuilder"クラスの移動

> `TagBuilde` R クラスは System.Web.WebPages.dll アセンブリに移動されました。 以前は、これは、ASP.NET MVC の一部であるアセンブリででした。 この変更により、ASP.NET MVC を使用するためにインストールする必要はありません、`TagBuilder`クラスです。
> 
> ただし、クラスは、まだ、`System.Web.Mvc`名前空間。 使用するために、`TagBuilder`クラス (たとえば、カスタム ASP.NET Razor ヘルパー)、名前空間を参照する必要があります (追加することで、たとえば、`@using System.Web.Mvc`をコードに)。


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>変更: 要求の検証の構文が変更されてください。「検証」クラスの削除

> ベータ 3 リリースでは、個々 のフィールドまたはフィールドのセットの検証を無効にすることができますを呼び出す、`Validation.Exclude`メソッド、または検証から除外するフィールドの名前を渡します。 新しい構文は、検証をバイパスするように、ベータ 3 リリースで提供します。 `Validation`ベータ 3 で使用されるメソッドが削除されました。
> 
> > [!NOTE]
> > Web サイトでのようなエラーが報告される場合、ユーザーが、(たとえば、リッチ テキスト エディターを使用して、ページ上) によって HTML マークアップをアップロードしようとしています要求の検証を無効にしないでください、場合*クライアントから危険性のあるRequest.Form値が検出されました*。、ユーザー入力は許可されません。 要求の検証を無効にした場合、危険性のあるマークアップが含まれてしたりしないようなものを使用してスクリプトを確認するユーザー入力を手動でチェックする必要があります、 [Microsoft 対策クロス サイト スクリプト ライブラリ V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)です。
> 
> 
> 自動の要求の検証を無効にするを呼び出して、`Request.Unvalidated`メソッド、フィールドまたはの要求の検証をバイパスするその他の post オブジェクトの名前を渡します。 このメソッドを使用するにはすべての項目の検証をバイパスする、 `Form`、 `QueryString`、 `Cookies`、および`ServerVariables`コレクション。 次の例を使用する方法を示して、`Unvalidated`メソッド。
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Razor 構文を使用する ASP.NET Web Pages の既知の問題

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>メンバーシップのカスタム ユーザー テーブルを使用する場合の問題点: 予期しない動作

> ASP.NET Razor の web サイトのメンバーシップ プロバイダーを初期化するために呼び出す、`WebSecurity.InitializeDatabaseConnection`メソッドです。 (WebMatrix で、スターター サイト テンプレートにはこのメソッドの呼び出しが含まれています、  *\_AppStart.cshtml*ファイルです)。場合、`autoCreateTables`このメソッドのパラメーターが設定を true に (に既定では、設定は、スターター サイト テンプレートの場合は true)、認識できないテーブル名がメソッド (2 番目のパラメーター) に渡される場合、メソッドはエラーをスローしません。 代わりに、テーブルを自動的に作成します。
> 
> これは、カスタム ユーザー テーブルを使用してメンバーシップに間違ったテーブル名を渡す場合問題になることができます、`WebSecurity.InitializeDatabaseConnection`メソッドです。 メソッドは既定ではエラーを生成しない指定したテーブルが存在しない場合、代わりに新しいテーブルを作成するため、アプリケーションが動作して表示できます。 ただし、カスタム ユーザー テーブル (および内のフィールド) に依存するアプリケーション コード最終的に、予期しないエラーが失敗します。
> 
> **回避策**  
> 名前を渡すことを確認してください、`InitializeDatabaseConnection`メソッドと一致するユーザー プロファイル、メンバーシップ データベース内のテーブルまたはことを確認して、`autoCreateTables`パラメーターが false に設定します。


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>問題:"を SQL Server のユーザー インスタンスを生成できませんでした。

> WebMatrix Web アプリケーションが SQL Server Express を使用して、Windows 7 または Windows Server 2008 R2 に IIS 7.5 を実行している場合は、SQL Server が実行時にユーザーのローカル アプリケーション パスを取得できないことを示すエラーを参照してください可能性があります。
> 
> **回避策**(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントがアプリケーションのルート フォルダーとサブフォルダーの読み取り/書き込みアクセス許可をなどがあるかどうかを確認*アプリ\_データ*. 詳細については、サポート技術情報の記事で使用できる[問題と、SQL Server Express ユーザー インスタンスの ASP.net Web アプリケーション プロジェクト](https://support.microsoft.com/kb/2002980)です。


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>問題: Visual Studio で、カスタム アセンブリ (Dll) の名前空間が自動的にインポートされません。

> Visual Studio でのプロジェクトでカスタム アセンブリを使用する場合は、それらのアセンブリで宣言された名前空間はデザイン時に自動的にインポートされません。 その結果、カスタム型への参照は、デザイン時に認識されない可能性があり、Visual Studio のでは認識されません (「波線」を使用) としてマークされています。 デザイン時のみです。 Visual Studio でこの問題が発生します。アプリケーション自体が正常に実行されます。
> 
> **回避策**  
> 含める、`using`ステートメント (`imports` Visual Basic で) はデザイン時に認識されないエンティティを参照します。


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>問題: Visual Studio IntelliSense およびプロジェクトのテンプレート ASP.NET MVC version 3 でのみ使用できます。

> ASP.NET Web Pages をインストールするもインストールしません tools for Visual Studio の ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートなど。
> 
> **回避策**Visual Studio での ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートを使用するインストール ASP.NET MVC 3 RC、Web Platform Installer を使用するか、または[スタンドアロン インストーラー](https://go.microsoft.com/fwlink/?LinkID=191797)です。


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>問題:"&lt;ヘルパー&gt;クラスが見つかりません"エラー

> ベータ 3 にアップグレードした後、エラーを参照してください可能性があるヘルパー クラス (たとえば、`Facebook`クラス) は見つかりません。 Beta 2 を起動して続行ベータ 3 では、ヘルパーを明示的にインストールする必要のあるパッケージに移動されました。 これらのパッケージを含めるには、既存のサイトはアップグレードされていません。内のサイトが含まれます、 *\My Documents\IISExpress*または*\My Documents\My Websites*フォルダーです。 既定のサイトを使用する場合にこのエラーが表示されます具体的には、*個人用サイト*(WebSite1) への参照を含む、`Twitter`ヘルパー。
> 
> **回避策**  
> サイトでは、実行、ヘルパーへの呼び出しをコメント、  *\_Admin*ページ、およびパッケージまたはを使用する場合、ヘルパーを含むパッケージをインストールします。 パッケージをインストールした後は、ヘルパーを参照する行をコメント解除できます。


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>問題点: Bin フォルダーに 3 ASP.NET Razor のベータ版のアセンブリの展開では使用できませんのサイトをホストしています。

> サイトの ASP.NET Razor のベータ 3 のアセンブリを展開する場合、ホストするサイトに ASP.NET Web Pages の web サイトを展開する場合と*Bin*フォルダーで、次を含むエラーが発生する可能性があります。
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> これは、ホスティング プロバイダーが、ASP.NET Web Pages の Beta 1 のアセンブリをサーバーのグローバルなアプリケーション キャッシュ (GAC) にインストールされている場合に発生することができます。 GAC のアセンブリがローカルにインストールされているアセンブリでの優先順位を取得、 *Bin*フォルダーです。
> 
> **回避策**プロバイダーのバージョン間の競合により、エラーが表示されていることを確認するホスティング プロバイダーに問い合わせて、アセンブリと自分のです。 場合は、ホスティング プロバイダーが、サーバーの GAC にアセンブリを更新することを要求します。


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>問題: フィードやプロキシ サーバーを経由して他の外部データの読み取り

> サイトを実行しているサーバーがプロキシ サーバーの背後にある場合でプロキシ情報を構成する必要があります、 *Web.config*サイトの外部から来る情報を読み取ることができるためにファイル。 たとえば、使用する場合、`ReCaptcha`ヘルパーに渡し、ヘルパーは、reCAPTCHA サービスと通信が、プロキシ サーバーによってブロックされる可能性があります。 同様に、パッケージ マネージャーによって使用されているフィードなど ASP.NET Web Pages で使用されるフィードでは、プロキシの構成が必要です。
> 
> 外部サービスを扱うやパッケージ フィードの操作で問題が発生する場合は、アプリケーションのルートに、次の要素を格納*Web.config*ファイル。
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> プロキシ サーバーの構成の詳細については、次を参照してください。 [&lt;プロキシ&gt;要素 (ネットワーク設定)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web サイトです。


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>問題:「Microsoft.Web.Infrastructure.dll を読み込むことができません」エラー

> すべての適切なアセンブリが以外の GAC にインストールは、Razor 構文を使用して、Beta 1 バージョンの ASP.NET Web Pages を以前にインストールし、し、Beta 3 バージョンをインストールする場合*Microsoft.Web.Infrastructure.dll*です。 結果として、ASP.NET Razor ページを実行するときにエラーが表示されることを示します*Microsoft.Web.Infrastructure.dll*ロードできませんでした。
> 
> クリーンなコンピューターにベータ 3 リリースをロードした場合は、この問題は発生しません。
> 
> **回避策**  
> コントロール パネルで、ASP.NET Web Pages をアンインストールします。 ベータ 3 リリースを再インストールします。


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>問題点:、Razor 構文を使用して ASP.NET Web Pages が無効になります、.NET Framework version 4 をアンインストールします。

> .NET Framework version 4 をアンインストールし、再インストールして、Razor 構文を使用して ASP.NET Web Pages は無効になります。 ページ、 *.cshtml*拡張機能が正しく動作しないことです。 ASP.NET Web ページで、マシン ルート アセンブリが登録*Web.config*ファイル、および .NET Framework の削除は、そのファイルを削除します。 .NET Framework を再インストール、構成ファイルの新しいバージョンがインストールされますが、ASP.NET Web Pages アセンブリの参照を追加できません。
> 
> **回避策**.NET Framework を再インストール後に Razor 構文を使用する ASP.NET Web Pages を再インストールします。 次の要素を追加、 *Web.config*マシン ルートでは、次の場所には、通常のファイル。  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>問題点: Bin フォルダーにアセンブリを ASP.NET で以前に配置されたアプリケーションにエラーが発生します。

> 展開時に、ASP.NET Web Pages アセンブリのコピー (たとえば、 *Microsoft.WebPages.dll*) に、 *Bin*サーバー上の web サイトのフォルダーです。 (これが発生して自動的に展開中に、開発者は、アセンブリを明示的にコピーされるため、またはです)。ただし、ベータ 3 リリースのインストール時にエラーが発生した特定の型が見つからないエラーなどです。 これは、ベータ 3 リリースの ASP.NET Web ページの種類の数が異なる名前空間に移動されたために発生します。
> 
> **回避策**   
> クリア、 *Bin*展開されたアプリケーションのフォルダーは、新しいアセンブリをフォルダーにコピー (または、アプリケーションを再配置)、アプリケーションを再起動します。


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
> - サーバー コンピューターの制御がないかどうか (たとえばを展開しているホスティング web サイトに) 次の web サイトの追加*Web.config*ファイル。
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>問題点: は、同じアプリケーション内に Web アプリケーション プロジェクトまたは ASP.NET MVC と ASP.NET Web ページを使用します。

> 場合は、Web アプリケーション プロジェクトや ASP.NET MVC アプリケーションでは、ASP.NET Web Pages を使用していた、エラーが表示されるを*WebPageHttpApplication*が見つかりません。
> 
> **回避策**  
> このエラーが発生する場合は、アプリケーションの派生元の基底クラスを変更します。 *Global.asax*ファイル、次の行を変更します。
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> 次のように
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> 導入された変更を有効に反転このベータ 1 リリース ASP.NET Web ページの Razor 構文を使用します。


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>問題点: SQL Server Compact がインストールされていないコンピューターにアプリケーションを展開します。

> SQL Server Compact データベースを含むアプリケーションは、SQL Server Compact がインストールされていないコンピューターで実行できます。 Microsoft WebMatrix ベータ 3 が自動的にするようなバイナリをコピーし、適切な実行*Web.config*ファイルの変換。
> 
> **回避策**これらのファイルをコピーしする必要がある場合、 *Web.config*ファイルの変更を手動で、次の操作します。
> 
> 1. アセンブリをコピーする、データベース エンジンに、 *Bin*ターゲット コンピューター上のアプリケーションのフォルダー (およびサブフォルダー)。 
> 
>     - コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **に** *\Bin*
>     - コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **に** *\Bin\x86*
>     - コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **に** *\Bin\amd64*
> 2. Web サイトのルート フォルダーに作成または開く、 *Web.config*ファイル。 (WebMatrix ベータ 3 の場合、このファイルの種類は をクリックすると利用可能な**すべて**で、**ファイルの種類を選択** ダイアログ ボックス)。
> 3. 次の要素の子として追加、 **&lt;構成&gt;**要素 (内部ではなく、  **&lt;system.web&gt;** 要素)。
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>問題: Visual Basic で中程度の信頼でデータベースとの WebGrid ヘルパーは機能しません

> Visual Basic を使用している場合 (作成*.vbhtml*ファイル)、`Database`と`WebGrid`中程度の信頼を使用する、アプリケーションが設定されている場合、ヘルパーは機能しません。
> 
> **回避策**  
> 完全な信頼を使用するアプリケーションを一時的に設定します。

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>問題:"Encrypt"プロパティは認識されません。

> SQL Server Compact 4.0 では認識されません、`Encrypt`のプロパティ、`SqlCeConnection`クラスです。 データベース ファイルの暗号化にこのプロパティは使用する必要があります。 `Encrypt`プロパティが SQL Server Compact 3.5 リリースでは使用されなくなり、旧バージョンと互換性のためだけに保持されました。 
> 
> **回避策**  
> 使用して、`Encryption Mode`のプロパティ、 `SqlCeConnection` SQL Server Compact 4.0 データベース ファイルを暗号化するクラス。 次の例は、暗号化された SQL Server Compact 4.0 を使用してデータベースを作成する方法を示します、`Encryption Mode`プロパティ。
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


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>問題: Microsoft Visual C 2008 ランタイム ライブラリが必要

> ネイティブ Dll の SQL Server Compact 4.0 には、Microsoft Visual C 2008 ランタイム ライブラリ (x86、IA64、および x64)、Service Pack 1 が必要があります。
> 
> **回避策**  
> .NET Framework 3.5 SP1 をインストールします。 これには、Visual C 2008 ランタイム ライブラリ SP1 もインストールされます。 ライブラリは、次の場所からダウンロードできます。   
>   
> [Microsoft Visual C 2008 のサービス パック 1 再頒布可能パッケージ ATL セキュリティ更新プログラム](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> .NET Framework 2.0、3.0 をインストールすることに注意してください。 または、4 は*いない*Visual c 2008 ランタイム ライブラリ SP1 をインストールします。


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>問題点: SQL Server Compact がインストールされている場合、コンピューターに .NET Framework をインストールする前に、そのプロバイダーの不変名に登録されていない .NET Framework の machine.config ファイル

> SQL Server Compact は SQL Server Compact は必要があるため、.NET framework がインストールされている .NET Framework がないコンピューターにインストールすることができます。 SQL Server Compact のセットアップでは、プロバイダーの不変名では登録されません SQL Server Compact をインストールする前に、どちらも .NET Framework version 3.5 と 4 がインストールされて場合、 *machine.config*ファイル。 SQL Server Compact のエントリに依存している任意のアプリケーション、 *machine.config*ファイルは失敗します。 内のインバリアント名登録エントリ*machine.config*次の例のようになります。
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **回避策**  
> SQL Server Compact 4.0 CTP1 をアンインストールします。 ダウンロードして、次の場所から完全バージョンの .NET Framework をインストールします。
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0 リリース (フル パッケージ)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> 再インストールし、 [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)です。


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>アプリケーションをインストールします。

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>問題: アプリケーションをインストールすることができます、長い時間がかかる場合は、ユーザーのマイ ドキュメント フォルダーは、ネットワーク共有にリダイレクト

> **回避策**  
> なし。 アプリケーションをインストールするには時間かかる場合がありますが、正常にインストールされます。


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>アプリケーションの発行

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


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>その他の問題

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>問題点: 検索/フィルターが機能しないレポートにグループ化します懸案事項の種類。

> 内のテキストを入力する場合に、サイトのレポートを実行すると、 *URL によってフィルター*ボックスし、をクリックして*検索*、何も起こりません。 これは、このコントロールが中に機能していないため、 *Group By*レポートの状態が に設定されている*懸案事項の種類*、既定値です。
> 
> **回避策**で、 *Group By*  タブのリボンで、をクリックして*URL*をそのソース URL でエントリをグループ化します。 テキスト ボックスと、エントリをフィルター処理ボタンはこの状態中に機能します。


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>問題点: WCF アプリケーションを IIS Express の実行が失敗します。

> WCF アプリケーションを参照する結果、次のいずれかのようなエラーになります。
> 
> *ファイルまたはアセンブリを読み込めませんでした ' Microsoft.Web.Administration, Version = 7.0.0.0、カルチャ = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つです。指定されたファイルが見つかりません。*
> 
> これは、IIS Express の Beta リリースは、既定で WCF をサポートしないために発生します。
> 
> **回避策**次の回避策のいずれかを使用して (回避策 2 は、Microsoft Windows Vista を必要またはそれ以降)。
> 
> 
> 1. コピー、 *Microsoft.Web.dll*と*Microsoft.Web.Administration.dll*に WebMatrix のインストール場所からアセンブリ、 *bin* WCF のディレクトリアプリケーション。 既定では、WebMatrix がインストールされている、 *Microsoft WebMatrix*システムの下のサブフォルダー *Program Files*フォルダーです。
> 2. Microsoft Windows Vista 以降では、または内のアセンブリへのシンボリック リンクを作成、 *bin*ディレクトリが次のコマンドを使用します。 (このアプローチによって、アセンブリのコピーは作成されません利点があります)。
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. 2 つのアセンブリを GAC にインストールします。 管理者特権のプロンプトから次のコマンドを実行します。
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>問題: Beta 3 の WebMatrix は、昇格が必要な特定のタスクを実行することが

> ベータ 3 の WebMatrix では、次の状況で追加のコンポーネントのインストールなどの昇格が必要な特定のタスクを実行できます。
> 
> - Windows Vista または Windows 7 を管理者特権を持たないアカウントでログオンして、ユーザー アカウント制御 (UAC) が無効になっています。
> - Microsoft Windows XP または Microsoft Windows Server 2003 を使用しています。
> 
> **回避策**  
> WebMatrix ベータ 3 のほとんどのタスクでは、管理アクセス許可は必要ありません。 ような状況に、管理者は、操作を実行したり、これらの手順に従います。
> 
> - Windows Vista または Windows 7 では、UAC を有効にします。
> - Windows xp では、Administrators セキュリティ グループにユーザーを追加します。


#### <a name="issue-site-from-web-gallery-is-disabled"></a>問題:「サイトから Web ギャラリー」が無効になっています

> **Web ギャラリーからサイト**オプションには、Web Platform Installer 3.0 がインストールされていない場合は無効になります。
> 
> **回避策**  
> インストール、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>問題: Windows Server 2003 で IIS Express が起動しない管理者以外のユーザーの

> Windows Server 2003 でページを起動または IIS Express を起動するときに IIS Express は開始されません。 Web ページのエラーが表示されます、アプリケーションが管理者以外のユーザーによって開始されていることを示します。
> 
> **回避策**  
> 管理ユーザーとしてベータ 3 の WebMatrix を起動します。 詳細については、次のサポート技術情報の記事を参照してください。  
>   
> [管理者以外のユーザーによって開始されたアプリケーションは、アプリケーションが Windows Vista、Windows Server 2003 または Windows XP で実行されているコンピューターの HTTP トラフィックをリッスンできません。](https://support.microsoft.com/kb/939786)


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


#### <a name="issue-the-relationships-button-is-disabled"></a>問題:「リレーションシップ」ボタンが無効になっています

> **リレーションシップ**下にあるボタンをクリックして、**テーブル** タブで、**データベース**SQL Server Compact データベースのワークスペースが無効になっています。
> 
> **回避策**  
> なし。 SQL Server Compact ありませんではテーブル間のリレーションシップ。


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>問題: パラメーター化された SQL クエリが例外をスローします。

> SQL Server Compact 4.0 を指定しない場合、データ型などで`SqlDbType`または`DbType`クエリの実行時にパラメーター化クエリでパラメーターの場合、例外がスローされます。
> 
> **回避策**  
> などのパラメーターのデータ型を明示的に設定`SqlDbType`または`DbType`です。 これは、BLOB データ型の場合に重要 (`image`と`ntext`)。 次のようにコードを使用します。
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>参照項目

WebMatrix ベータ 3 の詳細については、次の web サイトを参照してください。

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. All Rights Reserved. [利用規約](https://msdn.microsoft.cos/cc300389.aspx)です。
