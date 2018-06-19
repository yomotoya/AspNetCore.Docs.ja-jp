---
uid: web-pages/readme/beta3
title: マトリックスと ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme を web |Microsoft ドキュメント
author: rick-anderson
description: Webmatrix と ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892882"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="66a6d-103">Webmatrix と ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme</span><span class="sxs-lookup"><span data-stu-id="66a6d-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="66a6d-104">Webmatrix と ASP.NET Web Pages (Razor) ベータ 3 リリースの Readme</span><span class="sxs-lookup"><span data-stu-id="66a6d-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="66a6d-105">2010 年 11 月 9日</span><span class="sxs-lookup"><span data-stu-id="66a6d-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="66a6d-106">目次</span><span class="sxs-lookup"><span data-stu-id="66a6d-106">Contents</span></span>

- [<span data-ttu-id="66a6d-107">概要</span><span class="sxs-lookup"><span data-stu-id="66a6d-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="66a6d-108">インストール</span><span class="sxs-lookup"><span data-stu-id="66a6d-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="66a6d-109">新しい機能、変更、および、ベータ 3 リリースの既知の問題</span><span class="sxs-lookup"><span data-stu-id="66a6d-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="66a6d-110">WebMatrix のインストールに関する問題</span><span class="sxs-lookup"><span data-stu-id="66a6d-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="66a6d-111">ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="66a6d-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="66a6d-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="66a6d-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="66a6d-113">アプリケーションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="66a6d-114">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="66a6d-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="66a6d-115">その他の問題</span><span class="sxs-lookup"><span data-stu-id="66a6d-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="66a6d-116">詳細については</span><span class="sxs-lookup"><span data-stu-id="66a6d-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="66a6d-117">概要</span><span class="sxs-lookup"><span data-stu-id="66a6d-117">Overview</span></span>

> <span data-ttu-id="66a6d-118">Microsoft WebMatrix のベータ版は、数分でインストールする無料の web 開発スタックです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="66a6d-119">データベース プログラミングのフレームワークを 1 つの統合環境を作成すると、web サーバーが統合されています。</span><span class="sxs-lookup"><span data-stu-id="66a6d-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="66a6d-120">効率よくコーディング、テスト、および独自 ASP.NET または PHP web サイトを発行、WebMatrix のベータ版を使用するか、DotNetNuke、Umbraco、WordPress、または Joomla などの人気のオープン ソース アプリケーションを使用して新しい web サイトの起動に WebMatrix のベータ版を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="66a6d-121">WebMatrix のベータ版では、同じ強力な web サーバー、データベース エンジン、およびインターネット上に円滑かつシームレスな開発から運用環境への移行のため、web サイトを実行しているフレームワークの環境を使用します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="66a6d-122">インストール</span><span class="sxs-lookup"><span data-stu-id="66a6d-122">Installation</span></span>

> <span data-ttu-id="66a6d-123">使用するベータ 3 の WebMatrix をインストールするには、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="66a6d-124">Web Platform Installer をインストールした後は、ベータ 3 の WebMatrix をインストールして使用できます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="66a6d-125">インストール中に問題がある場合を参照してください[に関する問題のトラブルシューティング Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="66a6d-126">アプリケーションを公開するための手順</span><span class="sxs-lookup"><span data-stu-id="66a6d-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="66a6d-127">参照してください[アプリケーションを公開するための手順](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="66a6d-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="66a6d-128">新しい機能、変更、andKnown 問題</span><span class="sxs-lookup"><span data-stu-id="66a6d-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="66a6d-129">WebMatrix ベータ 3 のインストール</span><span class="sxs-lookup"><span data-stu-id="66a6d-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="66a6d-130">問題点: WebMatrix ベータ 3 は Microsoft .NET Framework 4 をサポートするプラットフォームで利用可能</span><span class="sxs-lookup"><span data-stu-id="66a6d-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="66a6d-131">.NET Framework version 4 が WebMatrix のベータ版必要です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="66a6d-132">場合によっては、WebMatrix のベータ版のインストーラー、サポートされている構成セットの一部ではない、プラットフォームにインストールするとします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="66a6d-133">具体的には、Windows Vista SP1 更新プログラムなしで、WebMatrix のベータ版のインストールを開始しますが、.NET Framework 4 コンポーネントは失敗し、インストールはブロックします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="66a6d-134">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-134">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-135">含む、サポートされているプラットフォームにインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="66a6d-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="66a6d-136">Windows 7</span></span>
> - <span data-ttu-id="66a6d-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="66a6d-137">Windows Server 2008</span></span>
> - <span data-ttu-id="66a6d-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="66a6d-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="66a6d-139">Windows Vista SP1 以降</span><span class="sxs-lookup"><span data-stu-id="66a6d-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="66a6d-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="66a6d-140">Windows XP SP3</span></span>
> - <span data-ttu-id="66a6d-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="66a6d-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="66a6d-142">Microsoft Visual Studio 2008 sp1 を適用せずに Microsoft Visual Studio 2008 がインストールされている場合、問題点: は WebMatrix Beta 3 をインストールことはできません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="66a6d-143">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-143">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-144">インストール[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft ダウンロード センターからです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="66a6d-145">問題点: SQL Server Compact 4.0 の一部のアセンブリは GAC にインストールされません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="66a6d-146">64 ビット コンピューターで SQL Server Compact 4.0 をインストールして、コンピューターにのみ、.NET Framework 3.5 SP1 Client Profile をインストール時に SQL Server Compact 4.0 のマネージ アセンブリでは、グローバル アセンブリ キャッシュ (GAC) に配置されていません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="66a6d-147">マネージ アセンブリを GAC にインストールされていない次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="66a6d-148">*System.Data.SqlServerCe.dll* (ADO.NET プロバイダー)</span><span class="sxs-lookup"><span data-stu-id="66a6d-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="66a6d-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="66a6d-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="66a6d-150">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-150">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-151">SQL Server をアンインストール Compact 4.0 です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="66a6d-152">ダウンロードして、次の場所から、フル バージョンの .NET Framework 3.5 SP1 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="66a6d-153">Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)</span><span class="sxs-lookup"><span data-stu-id="66a6d-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="66a6d-154">SQL Server Compact 4.0 を再インストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="66a6d-155">問題点: SQL Server Compact のコマンドラインを使用してをアンインストールできません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="66a6d-156">SQL Server Compact のコマンド ライン オプションを使用してのアンインストールは、このリリースでは機能しません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="66a6d-157">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-157">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-158">使用して*プログラムと機能*Windows コントロール パネルで Microsoft SQL Server Compact 4.0 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="66a6d-159">ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="66a6d-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="66a6d-160">ドキュメントのこのセクションでは、新機能、変更、および Razor 構文を使用して ASP.NET Web Pages の Beta 3 リリースの既知の問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="66a6d-161">新機能</span><span class="sxs-lookup"><span data-stu-id="66a6d-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="66a6d-162">変更</span><span class="sxs-lookup"><span data-stu-id="66a6d-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="66a6d-163">問題</span><span class="sxs-lookup"><span data-stu-id="66a6d-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="66a6d-164">Razor 構文を使用する ASP.NET Web Pages のベータ 3 の新機能</span><span class="sxs-lookup"><span data-stu-id="66a6d-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="66a6d-165">"Html.Raw"メソッドがエンコードされていないマークアップを表示する新機能。</span><span class="sxs-lookup"><span data-stu-id="66a6d-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="66a6d-166">新しい`Html.Raw`メソッドにより、エンコードされた出力をレンダリングするのではなくマークアップとして HTML マークアップを表示できます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="66a6d-167">(既定では、ASP.NET Razor エンコード文字列それらを表示する前にします。)構文は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="66a6d-168">`Html.Raw` を使用する方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="66a6d-169">Razor 構文を使用する ASP.NET Web Pages のベータ 3 の変更</span><span class="sxs-lookup"><span data-stu-id="66a6d-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="66a6d-170">削除の変更:"HrefAttribute"メソッド</span><span class="sxs-lookup"><span data-stu-id="66a6d-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="66a6d-171">`HrefAttribute`のメソッド、`WebPage`クラスは削除されました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="66a6d-172">このヘルパーは、Url に安全でない文字をエンコードに使用されました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="66a6d-173">ASP.NET Razor 文字列を自動的にエンコードするためは必要なくなりました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="66a6d-174">(新しい使用`Html.Raw`エンコードされていない文字列をレンダリングするメソッドです)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="66a6d-175">変更: 構文宣言"@helper"ヘルパーの変更</span><span class="sxs-lookup"><span data-stu-id="66a6d-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="66a6d-176">使用して作成するヘルパーを解析する方法が ASP.NET Beta 3 リリースで変更、`@helper`構文です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="66a6d-177">基本的には、`@helper`構文が、マークアップ コードを含めることができるブロックの代わりに、コード ブロックとして解析されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="66a6d-178">したがって、ヘルパー内のコードがで囲む必要ありません`@{ }`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="66a6d-179">ヘルパーの内側のマークアップがや ASP.NET Razor の HTML 要素で明示的に追加するのには逆に、`<text></text>`タグ。</span><span class="sxs-lookup"><span data-stu-id="66a6d-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="66a6d-180">たとえば、次`@helper`構文は、ベータ 3 リリースで機能します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="66a6d-181">ベータ 3 リリースでは、次の例のようになりますにこのヘルパーを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="66a6d-182">注意して、`@{ }`ヘルパーの最初のコードの周りの文字は使用されなくなりました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="66a6d-183">これは、ヘルパーの内容が既定では、コード ブロックとして扱われるためです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="66a6d-184">ヘルパー マークアップ、オープンで始まる`<a>`タグ。</span><span class="sxs-lookup"><span data-stu-id="66a6d-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="66a6d-185">ヘルパーは、プレーン テキストや終了タグを含まないタグをレンダリングする必要があります場合、(たとえば、`<meta>`タグ) である必要があります、表示される内容を`<text></text>`タグ。</span><span class="sxs-lookup"><span data-stu-id="66a6d-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="66a6d-186">変更:"WebPageContext.HttpContext"の削除</span><span class="sxs-lookup"><span data-stu-id="66a6d-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="66a6d-187">`WebPageContext.HttpContext`プロパティが削除されました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="66a6d-188">代わりに、`HttpContext.Current` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="66a6d-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="66a6d-189">(、`WebPageContext.HttpContext`プロパティが単にこれをラップします)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="66a6d-190">新しいパッケージを変更:"Facebook"ヘルパーの移動</span><span class="sxs-lookup"><span data-stu-id="66a6d-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="66a6d-191">`Facebook`ヘルパーに移動されました、 *Facebook.Helper*ライブラリが含まれている`Facebook`ヘルパーと機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="66a6d-192">必要がありますとしてインストールするこのライブラリ別のパッケージ「ヘルパーとマネージャーのパッケージをインストールする」」の説明に従って、チュートリアルでは[ASP.NET ページの使用を開始する](https://go.microsoft.com/fwlink/?LinkId=202889)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="66a6d-193">新しいアセンブリに変更します。 メンバーシップ、ロール、およびセキュリティの種類の移動</span><span class="sxs-lookup"><span data-stu-id="66a6d-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="66a6d-194">次の種類に移動された、`WebMatrix.WebData`アセンブリ。</span><span class="sxs-lookup"><span data-stu-id="66a6d-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="66a6d-195">System.Web.WebPages.dll アセンブリへの変更:"TagBuilder"クラスの移動</span><span class="sxs-lookup"><span data-stu-id="66a6d-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="66a6d-196">`TagBuilde` R クラスは System.Web.WebPages.dll アセンブリに移動されました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="66a6d-197">以前は、これは、ASP.NET MVC の一部であるアセンブリででした。</span><span class="sxs-lookup"><span data-stu-id="66a6d-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="66a6d-198">この変更により、ASP.NET MVC を使用するためにインストールする必要はありません、`TagBuilder`クラスです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="66a6d-199">ただし、クラスは、まだ、`System.Web.Mvc`名前空間。</span><span class="sxs-lookup"><span data-stu-id="66a6d-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="66a6d-200">使用するために、`TagBuilder`クラス (たとえば、カスタム ASP.NET Razor ヘルパー)、名前空間を参照する必要があります (追加することで、たとえば、`@using System.Web.Mvc`をコードに)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="66a6d-201">変更: 要求の検証の構文が変更されてください。「検証」クラスの削除</span><span class="sxs-lookup"><span data-stu-id="66a6d-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="66a6d-202">ベータ 3 リリースでは、個々 のフィールドまたはフィールドのセットの検証を無効にすることができますを呼び出す、`Validation.Exclude`メソッド、または検証から除外するフィールドの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="66a6d-203">新しい構文は、検証をバイパスするように、ベータ 3 リリースで提供します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="66a6d-204">`Validation`ベータ 3 で使用されるメソッドが削除されました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="66a6d-205">Web サイトでのようなエラーが報告される場合、ユーザーが、(たとえば、リッチ テキスト エディターを使用して、ページ上) によって HTML マークアップをアップロードしようとしています要求の検証を無効にしないでください、場合*クライアントから危険性のあるRequest.Form値が検出されました*。、ユーザー入力は許可されません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="66a6d-206">要求の検証を無効にした場合、危険性のあるマークアップが含まれてしたりしないようなものを使用してスクリプトを確認するユーザー入力を手動でチェックする必要があります、 [Microsoft 対策クロス サイト スクリプト ライブラリ V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="66a6d-207">自動の要求の検証を無効にするを呼び出して、`Request.Unvalidated`メソッド、フィールドまたはの要求の検証をバイパスするその他の post オブジェクトの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="66a6d-208">このメソッドを使用するにはすべての項目の検証をバイパスする、 `Form`、 `QueryString`、 `Cookies`、および`ServerVariables`コレクション。</span><span class="sxs-lookup"><span data-stu-id="66a6d-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="66a6d-209">次の例を使用する方法を示して、`Unvalidated`メソッド。</span><span class="sxs-lookup"><span data-stu-id="66a6d-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="66a6d-210">Razor 構文を使用する ASP.NET Web Pages の既知の問題</span><span class="sxs-lookup"><span data-stu-id="66a6d-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="66a6d-211">メンバーシップのカスタム ユーザー テーブルを使用する場合の問題点: 予期しない動作</span><span class="sxs-lookup"><span data-stu-id="66a6d-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="66a6d-212">ASP.NET Razor の web サイトのメンバーシップ プロバイダーを初期化するために呼び出す、`WebSecurity.InitializeDatabaseConnection`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="66a6d-213">(WebMatrix で、スターター サイト テンプレートにはこのメソッドの呼び出しが含まれています、  *\_AppStart.cshtml*ファイルです)。場合、`autoCreateTables`このメソッドのパラメーターが設定を true に (に既定では、設定は、スターター サイト テンプレートの場合は true)、認識できないテーブル名がメソッド (2 番目のパラメーター) に渡される場合、メソッドはエラーをスローしません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="66a6d-214">代わりに、テーブルを自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="66a6d-215">これは、カスタム ユーザー テーブルを使用してメンバーシップに間違ったテーブル名を渡す場合問題になることができます、`WebSecurity.InitializeDatabaseConnection`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="66a6d-216">メソッドは既定ではエラーを生成しない指定したテーブルが存在しない場合、代わりに新しいテーブルを作成するため、アプリケーションが動作して表示できます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="66a6d-217">ただし、カスタム ユーザー テーブル (および内のフィールド) に依存するアプリケーション コード最終的に、予期しないエラーが失敗します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="66a6d-218">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-218">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-219">名前を渡すことを確認してください、`InitializeDatabaseConnection`メソッドと一致するユーザー プロファイル、メンバーシップ データベース内のテーブルまたはことを確認して、`autoCreateTables`パラメーターが false に設定します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="66a6d-220">問題:"を SQL Server のユーザー インスタンスを生成できませんでした。</span><span class="sxs-lookup"><span data-stu-id="66a6d-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="66a6d-221">WebMatrix Web アプリケーションが SQL Server Express を使用して、Windows 7 または Windows Server 2008 R2 に IIS 7.5 を実行している場合は、SQL Server が実行時にユーザーのローカル アプリケーション パスを取得できないことを示すエラーを参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="66a6d-222">**回避策**(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントがアプリケーションのルート フォルダーとサブフォルダーの読み取り/書き込みアクセス許可をなどがあるかどうかを確認*アプリ\_データ*.</span><span class="sxs-lookup"><span data-stu-id="66a6d-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="66a6d-223">詳細については、サポート技術情報の記事で使用できる[問題と、SQL Server Express ユーザー インスタンスの ASP.net Web アプリケーション プロジェクト](https://support.microsoft.com/kb/2002980)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="66a6d-224">問題: Visual Studio で、カスタム アセンブリ (Dll) の名前空間が自動的にインポートされません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="66a6d-225">Visual Studio でのプロジェクトでカスタム アセンブリを使用する場合は、それらのアセンブリで宣言された名前空間はデザイン時に自動的にインポートされません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="66a6d-226">その結果、カスタム型への参照は、デザイン時に認識されない可能性があり、Visual Studio のでは認識されません (「波線」を使用) としてマークされています。</span><span class="sxs-lookup"><span data-stu-id="66a6d-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="66a6d-227">デザイン時のみです。 Visual Studio でこの問題が発生します。アプリケーション自体が正常に実行されます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="66a6d-228">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-228">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-229">含める、`using`ステートメント (`imports` Visual Basic で) はデザイン時に認識されないエンティティを参照します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="66a6d-230">問題: Visual Studio IntelliSense およびプロジェクトのテンプレート ASP.NET MVC version 3 でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="66a6d-231">ASP.NET Web Pages をインストールするもインストールしません tools for Visual Studio の ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートなど。</span><span class="sxs-lookup"><span data-stu-id="66a6d-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="66a6d-232">**回避策**Visual Studio での ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートを使用するインストール ASP.NET MVC 3 RC、Web Platform Installer を使用するか、または[スタンドアロン インストーラー](https://go.microsoft.com/fwlink/?LinkID=191797)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="66a6d-233">問題:"&lt;ヘルパー&gt;クラスが見つかりません"エラー</span><span class="sxs-lookup"><span data-stu-id="66a6d-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="66a6d-234">ベータ 3 にアップグレードした後、エラーを参照してください可能性があるヘルパー クラス (たとえば、`Facebook`クラス) は見つかりません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="66a6d-235">Beta 2 を起動して続行ベータ 3 では、ヘルパーを明示的にインストールする必要のあるパッケージに移動されました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="66a6d-236">これらのパッケージを含めるには、既存のサイトはアップグレードされていません。内のサイトが含まれます、 *\My Documents\IISExpress*または*\My Documents\My Websites*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="66a6d-237">既定のサイトを使用する場合にこのエラーが表示されます具体的には、*個人用サイト*(WebSite1) への参照を含む、`Twitter`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="66a6d-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="66a6d-238">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-238">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-239">サイトでは、実行、ヘルパーへの呼び出しをコメント、  *\_Admin*ページ、およびパッケージまたはを使用する場合、ヘルパーを含むパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="66a6d-240">パッケージをインストールした後は、ヘルパーを参照する行をコメント解除できます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="66a6d-241">問題点: Bin フォルダーに 3 ASP.NET Razor のベータ版のアセンブリの展開では使用できませんのサイトをホストしています。</span><span class="sxs-lookup"><span data-stu-id="66a6d-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="66a6d-242">サイトの ASP.NET Razor のベータ 3 のアセンブリを展開する場合、ホストするサイトに ASP.NET Web Pages の web サイトを展開する場合と*Bin*フォルダーで、次を含むエラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="66a6d-243">これは、ホスティング プロバイダーが、ASP.NET Web Pages の Beta 1 のアセンブリをサーバーのグローバルなアプリケーション キャッシュ (GAC) にインストールされている場合に発生することができます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="66a6d-244">GAC のアセンブリがローカルにインストールされているアセンブリでの優先順位を取得、 *Bin*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="66a6d-245">**回避策**プロバイダーのバージョン間の競合により、エラーが表示されていることを確認するホスティング プロバイダーに問い合わせて、アセンブリと自分のです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="66a6d-246">場合は、ホスティング プロバイダーが、サーバーの GAC にアセンブリを更新することを要求します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="66a6d-247">問題: フィードやプロキシ サーバーを経由して他の外部データの読み取り</span><span class="sxs-lookup"><span data-stu-id="66a6d-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="66a6d-248">サイトを実行しているサーバーがプロキシ サーバーの背後にある場合でプロキシ情報を構成する必要があります、 *Web.config*サイトの外部から来る情報を読み取ることができるためにファイル。</span><span class="sxs-lookup"><span data-stu-id="66a6d-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="66a6d-249">たとえば、使用する場合、`ReCaptcha`ヘルパーに渡し、ヘルパーは、reCAPTCHA サービスと通信が、プロキシ サーバーによってブロックされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="66a6d-250">同様に、パッケージ マネージャーによって使用されているフィードなど ASP.NET Web Pages で使用されるフィードでは、プロキシの構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="66a6d-251">外部サービスを扱うやパッケージ フィードの操作で問題が発生する場合は、アプリケーションのルートに、次の要素を格納*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="66a6d-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="66a6d-252">プロキシ サーバーの構成の詳細については、次を参照してください。 [&lt;プロキシ&gt;要素 (ネットワーク設定)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="66a6d-253">問題:「Microsoft.Web.Infrastructure.dll を読み込むことができません」エラー</span><span class="sxs-lookup"><span data-stu-id="66a6d-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="66a6d-254">すべての適切なアセンブリが以外の GAC にインストールは、Razor 構文を使用して、Beta 1 バージョンの ASP.NET Web Pages を以前にインストールし、し、Beta 3 バージョンをインストールする場合*Microsoft.Web.Infrastructure.dll*です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="66a6d-255">結果として、ASP.NET Razor ページを実行するときにエラーが表示されることを示します*Microsoft.Web.Infrastructure.dll*ロードできませんでした。</span><span class="sxs-lookup"><span data-stu-id="66a6d-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="66a6d-256">クリーンなコンピューターにベータ 3 リリースをロードした場合は、この問題は発生しません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="66a6d-257">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-257">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-258">コントロール パネルで、ASP.NET Web Pages をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="66a6d-259">ベータ 3 リリースを再インストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="66a6d-260">問題点:、Razor 構文を使用して ASP.NET Web Pages が無効になります、.NET Framework version 4 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="66a6d-261">.NET Framework version 4 をアンインストールし、再インストールして、Razor 構文を使用して ASP.NET Web Pages は無効になります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="66a6d-262">ページ、 *.cshtml*拡張機能が正しく動作しないことです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="66a6d-263">ASP.NET Web ページで、マシン ルート アセンブリが登録*Web.config*ファイル、および .NET Framework の削除は、そのファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="66a6d-264">.NET Framework を再インストール、構成ファイルの新しいバージョンがインストールされますが、ASP.NET Web Pages アセンブリの参照を追加できません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="66a6d-265">**回避策**.NET Framework を再インストール後に Razor 構文を使用する ASP.NET Web Pages を再インストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="66a6d-266">次の要素を追加、 *Web.config*マシン ルートでは、次の場所には、通常のファイル。</span><span class="sxs-lookup"><span data-stu-id="66a6d-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="66a6d-267">問題点: Bin フォルダーにアセンブリを ASP.NET で以前に配置されたアプリケーションにエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="66a6d-268">展開時に、ASP.NET Web Pages アセンブリのコピー (たとえば、 *Microsoft.WebPages.dll*) に、 *Bin*サーバー上の web サイトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="66a6d-269">(これが発生して自動的に展開中に、開発者は、アセンブリを明示的にコピーされるため、またはです)。ただし、ベータ 3 リリースのインストール時にエラーが発生した特定の型が見つからないエラーなどです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="66a6d-270">これは、ベータ 3 リリースの ASP.NET Web ページの種類の数が異なる名前空間に移動されたために発生します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="66a6d-271">**回避策** </span><span class="sxs-lookup"><span data-stu-id="66a6d-271">**Workaround** </span></span>  
> <span data-ttu-id="66a6d-272">クリア、 *Bin*展開されたアプリケーションのフォルダーは、新しいアセンブリをフォルダーにコピー (または、アプリケーションを再配置)、アプリケーションを再起動します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="66a6d-273">問題点: 拡張子のない Url が見つからない IIS 7 や IIS 7.5 で.cshtml/.vbhtml ファイル</span><span class="sxs-lookup"><span data-stu-id="66a6d-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="66a6d-274">IIS 7 や IIS 7.5 では、次のように URL を使用して要求ができませんがページを検索する、 *.cshtml*または *.vbhtml*拡張機能。</span><span class="sxs-lookup"><span data-stu-id="66a6d-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="66a6d-275">URL 書き換えが有効でないため既定の IIS 7 や IIS 7.5、問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="66a6d-276">考えられるシナリオは IIS Express を使用してローカルでテストするときに問題は表示されないホスティング web サイトに web サイトを展開するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="66a6d-277">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="66a6d-278">サーバー コンピューターを制御する場合は、サーバー コンピューターの更新プログラムをインストールに記載されている[、更新プログラムを処理する IIS 7.0 または IIS 7.5 のハンドラーの要求の Url を特定の有効期間で終わっていないことがある](https://support.microsoft.com/kb/980368)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="66a6d-279">サーバー コンピューターの制御がないかどうか (たとえばを展開しているホスティング web サイトに) 次の web サイトの追加*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="66a6d-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="66a6d-280">問題点: は、同じアプリケーション内に Web アプリケーション プロジェクトまたは ASP.NET MVC と ASP.NET Web ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="66a6d-281">場合は、Web アプリケーション プロジェクトや ASP.NET MVC アプリケーションでは、ASP.NET Web Pages を使用していた、エラーが表示されるを*WebPageHttpApplication*が見つかりません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="66a6d-282">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-282">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-283">このエラーが発生する場合は、アプリケーションの派生元の基底クラスを変更します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="66a6d-284">*Global.asax*ファイル、次の行を変更します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="66a6d-285">次のように</span><span class="sxs-lookup"><span data-stu-id="66a6d-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="66a6d-286">導入された変更を有効に反転このベータ 1 リリース ASP.NET Web ページの Razor 構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="66a6d-287">問題点: SQL Server Compact がインストールされていないコンピューターにアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="66a6d-288">SQL Server Compact データベースを含むアプリケーションは、SQL Server Compact がインストールされていないコンピューターで実行できます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="66a6d-289">Microsoft WebMatrix ベータ 3 が自動的にするようなバイナリをコピーし、適切な実行*Web.config*ファイルの変換。</span><span class="sxs-lookup"><span data-stu-id="66a6d-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="66a6d-290">**回避策**これらのファイルをコピーしする必要がある場合、 *Web.config*ファイルの変更を手動で、次の操作します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="66a6d-291">アセンブリをコピーする、データベース エンジンに、 *Bin*ターゲット コンピューター上のアプリケーションのフォルダー (およびサブフォルダー)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="66a6d-292">コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **に** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="66a6d-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="66a6d-293">コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **に** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="66a6d-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="66a6d-294">コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **に** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="66a6d-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="66a6d-295">Web サイトのルート フォルダーに作成または開く、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="66a6d-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="66a6d-296">(WebMatrix ベータ 3 の場合、このファイルの種類は をクリックすると利用可能な**すべて**で、**ファイルの種類を選択** ダイアログ ボックス)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="66a6d-297">次の要素の子として追加、 **&lt;構成&gt;** 要素 (内部ではなく、 **&lt;system.web&gt;** 要素)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="66a6d-298">問題: Visual Basic で中程度の信頼でデータベースとの WebGrid ヘルパーは機能しません</span><span class="sxs-lookup"><span data-stu-id="66a6d-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="66a6d-299">Visual Basic を使用している場合 (作成 *.vbhtml*ファイル)、`Database`と`WebGrid`中程度の信頼を使用する、アプリケーションが設定されている場合、ヘルパーは機能しません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="66a6d-300">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-300">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-301">完全な信頼を使用するアプリケーションを一時的に設定します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="66a6d-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="66a6d-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="66a6d-303">問題:"Encrypt"プロパティは認識されません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="66a6d-304">SQL Server Compact 4.0 では認識されません、`Encrypt`のプロパティ、`SqlCeConnection`クラスです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="66a6d-305">データベース ファイルの暗号化にこのプロパティは使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="66a6d-306">`Encrypt`プロパティが SQL Server Compact 3.5 リリースでは使用されなくなり、旧バージョンと互換性のためだけに保持されました。</span><span class="sxs-lookup"><span data-stu-id="66a6d-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="66a6d-307">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-307">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-308">使用して、`Encryption Mode`のプロパティ、 `SqlCeConnection` SQL Server Compact 4.0 データベース ファイルを暗号化するクラス。</span><span class="sxs-lookup"><span data-stu-id="66a6d-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="66a6d-309">次の例は、暗号化された SQL Server Compact 4.0 を使用してデータベースを作成する方法を示します、`Encryption Mode`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="66a6d-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="66a6d-310">既存の SQL Server Compact 4.0 データベースの暗号化モードを変更するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="66a6d-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="66a6d-311">暗号化されていない SQL Server Compact 4.0 データベースを暗号化するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="66a6d-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="66a6d-312">問題: Microsoft Visual C 2008 ランタイム ライブラリが必要</span><span class="sxs-lookup"><span data-stu-id="66a6d-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="66a6d-313">ネイティブ Dll の SQL Server Compact 4.0 には、Microsoft Visual C 2008 ランタイム ライブラリ (x86、IA64、および x64)、Service Pack 1 が必要があります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="66a6d-314">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-314">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-315">.NET Framework 3.5 SP1 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="66a6d-316">これには、Visual C 2008 ランタイム ライブラリ SP1 もインストールされます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="66a6d-317">ライブラリは、次の場所からダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="66a6d-318">Microsoft Visual C 2008 のサービス パック 1 再頒布可能パッケージ ATL セキュリティ更新プログラム</span><span class="sxs-lookup"><span data-stu-id="66a6d-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="66a6d-319">.NET Framework 2.0、3.0 をインストールすることに注意してください。 または、4 は*いない*Visual c 2008 ランタイム ライブラリ SP1 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="66a6d-320">問題点: SQL Server Compact がインストールされている場合、コンピューターに .NET Framework をインストールする前に、そのプロバイダーの不変名に登録されていない .NET Framework の machine.config ファイル</span><span class="sxs-lookup"><span data-stu-id="66a6d-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="66a6d-321">SQL Server Compact は SQL Server Compact は必要があるため、.NET framework がインストールされている .NET Framework がないコンピューターにインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="66a6d-322">SQL Server Compact のセットアップでは、プロバイダーの不変名では登録されません SQL Server Compact をインストールする前に、どちらも .NET Framework version 3.5 と 4 がインストールされて場合、 *machine.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="66a6d-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="66a6d-323">SQL Server Compact のエントリに依存している任意のアプリケーション、 *machine.config*ファイルは失敗します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="66a6d-324">内のインバリアント名登録エントリ*machine.config*次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="66a6d-325">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-325">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-326">SQL Server Compact 4.0 CTP1 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="66a6d-327">ダウンロードして、次の場所から完全バージョンの .NET Framework をインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="66a6d-328">Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)</span><span class="sxs-lookup"><span data-stu-id="66a6d-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="66a6d-329">Microsoft .NET Framework 4.0 リリース (フル パッケージ)</span><span class="sxs-lookup"><span data-stu-id="66a6d-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="66a6d-330">再インストールし、 [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="66a6d-331">アプリケーションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="66a6d-332">問題: アプリケーションをインストールすることができます、長い時間がかかる場合は、ユーザーのマイ ドキュメント フォルダーは、ネットワーク共有にリダイレクト</span><span class="sxs-lookup"><span data-stu-id="66a6d-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="66a6d-333">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-333">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-334">なし。</span><span class="sxs-lookup"><span data-stu-id="66a6d-334">None.</span></span> <span data-ttu-id="66a6d-335">アプリケーションをインストールするには時間かかる場合がありますが、正常にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="66a6d-336">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="66a6d-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="66a6d-337">問題:"リンク先の URL"フィールドは、プレフィックスが付いていない http:// または https:// 場合の発行後にサイトが機能しません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="66a6d-338">**公開設定**ダイアログ ボックスで、リンク先の URL で始まらない場合`http://`または`https://`展開後に、サイトが動かないことがあります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="66a6d-339">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-339">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-340">確認して、サイトにある送信先 URL を発行する前に、**発行設定** ダイアログ ボックスが始まる`http://`または`https://`です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="66a6d-341">問題点: MySQL データベースのパブリッシュ エラーで失敗する、"データベースを発行できませんでした。</span><span class="sxs-lookup"><span data-stu-id="66a6d-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="66a6d-342">これは、場合に発生、リモート データベースは、スクリプトを実行できません。"</span><span class="sxs-lookup"><span data-stu-id="66a6d-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="66a6d-343">さまざまな理由から、エラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="66a6d-344">このエラーを発生する理由の 1 つは、データベース スクリプトには、単一引用符文字 (') が含まれているとなり、対象の MySQL データベースの既定の文字セットは utf-8 です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="66a6d-345">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-345">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-346">既定の文字を utf-8 に、リモートの MySQL データベースのセットを設定します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="66a6d-347">その他の問題</span><span class="sxs-lookup"><span data-stu-id="66a6d-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="66a6d-348">問題点: 検索/フィルターが機能しないレポートにグループ化します懸案事項の種類。</span><span class="sxs-lookup"><span data-stu-id="66a6d-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="66a6d-349">内のテキストを入力する場合に、サイトのレポートを実行すると、 *URL によってフィルター*ボックスし、をクリックして*検索*、何も起こりません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="66a6d-350">これは、このコントロールが中に機能していないため、 *Group By*レポートの状態が に設定されている*懸案事項の種類*、既定値です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="66a6d-351">**回避策**で、 *Group By*  タブのリボンで、をクリックして*URL*をそのソース URL でエントリをグループ化します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="66a6d-352">テキスト ボックスと、エントリをフィルター処理ボタンはこの状態中に機能します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="66a6d-353">問題点: WCF アプリケーションを IIS Express の実行が失敗します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="66a6d-354">WCF アプリケーションを参照する結果、次のいずれかのようなエラーになります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="66a6d-355">*ファイルまたはアセンブリを読み込めませんでした ' Microsoft.Web.Administration, Version = 7.0.0.0、カルチャ = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つです。指定されたファイルが見つかりません。*</span><span class="sxs-lookup"><span data-stu-id="66a6d-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="66a6d-356">これは、IIS Express の Beta リリースは、既定で WCF をサポートしないために発生します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="66a6d-357">**回避策**次の回避策のいずれかを使用して (回避策 2 は、Microsoft Windows Vista を必要またはそれ以降)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="66a6d-358">コピー、 *Microsoft.Web.dll*と*Microsoft.Web.Administration.dll*に WebMatrix のインストール場所からアセンブリ、 *bin* WCF のディレクトリアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="66a6d-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="66a6d-359">既定では、WebMatrix がインストールされている、 *Microsoft WebMatrix*システムの下のサブフォルダー *Program Files*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="66a6d-360">Microsoft Windows Vista 以降では、または内のアセンブリへのシンボリック リンクを作成、 *bin*ディレクトリが次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="66a6d-361">(このアプローチによって、アセンブリのコピーは作成されません利点があります)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="66a6d-362">2 つのアセンブリを GAC にインストールします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="66a6d-363">管理者特権のプロンプトから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="66a6d-364">問題: Beta 3 の WebMatrix は、昇格が必要な特定のタスクを実行することが</span><span class="sxs-lookup"><span data-stu-id="66a6d-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="66a6d-365">ベータ 3 の WebMatrix では、次の状況で追加のコンポーネントのインストールなどの昇格が必要な特定のタスクを実行できます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="66a6d-366">Windows Vista または Windows 7 を管理者特権を持たないアカウントでログオンして、ユーザー アカウント制御 (UAC) が無効になっています。</span><span class="sxs-lookup"><span data-stu-id="66a6d-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="66a6d-367">Microsoft Windows XP または Microsoft Windows Server 2003 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="66a6d-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="66a6d-368">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-368">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-369">WebMatrix ベータ 3 のほとんどのタスクでは、管理アクセス許可は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="66a6d-370">ような状況に、管理者は、操作を実行したり、これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="66a6d-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="66a6d-371">Windows Vista または Windows 7 では、UAC を有効にします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="66a6d-372">Windows xp では、Administrators セキュリティ グループにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="66a6d-373">問題:「サイトから Web ギャラリー」が無効になっています</span><span class="sxs-lookup"><span data-stu-id="66a6d-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="66a6d-374">**Web ギャラリーからサイト**オプションには、Web Platform Installer 3.0 がインストールされていない場合は無効になります。</span><span class="sxs-lookup"><span data-stu-id="66a6d-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="66a6d-375">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-375">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-376">インストール、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="66a6d-377">問題: Windows Server 2003 で IIS Express が起動しない管理者以外のユーザーの</span><span class="sxs-lookup"><span data-stu-id="66a6d-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="66a6d-378">Windows Server 2003 でページを起動または IIS Express を起動するときに IIS Express は開始されません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="66a6d-379">Web ページのエラーが表示されます、アプリケーションが管理者以外のユーザーによって開始されていることを示します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="66a6d-380">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-380">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-381">管理ユーザーとしてベータ 3 の WebMatrix を起動します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="66a6d-382">詳細については、次のサポート技術情報の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="66a6d-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="66a6d-383">管理者以外のユーザーによって開始されたアプリケーションは、アプリケーションが Windows Vista、Windows Server 2003 または Windows XP で実行されているコンピューターの HTTP トラフィックをリッスンできません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="66a6d-384">問題点: Google Chrome は、実行オプションとして使用できません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="66a6d-385">Google Chrome が下にあるブラウザーの一覧に表示されない**実行**上、**ホーム**タブです。</span><span class="sxs-lookup"><span data-stu-id="66a6d-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="66a6d-386">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-386">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-387">Google Chrome の一部のバージョンに登録しないで自体正しく、Windows の既定のプログラム機能します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="66a6d-388">この問題を回避するには、Google Chrome を開始 をクリックして、*カスタマイズおよび制御 Google Chrome*  メニューのをクリックして*オプション*、順にクリック*Make Google Chrome の既定ブラウザー*です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="66a6d-389">問題:「外部キー」 ダイアログ ボックス許可しない、プライマリ キーを入力</span><span class="sxs-lookup"><span data-stu-id="66a6d-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="66a6d-390">**Foreign Key**  ダイアログ ボックス主キー テーブルからの主キーの名前を入力はできません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="66a6d-391">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-391">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-392">これは意図的なものであり、</span><span class="sxs-lookup"><span data-stu-id="66a6d-392">This is intentional.</span></span> <span data-ttu-id="66a6d-393">主キー テーブルの主キーの名前を入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="66a6d-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="66a6d-394">問題:「リレーションシップ」ボタンが無効になっています</span><span class="sxs-lookup"><span data-stu-id="66a6d-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="66a6d-395">**リレーションシップ**下にあるボタンをクリックして、**テーブル** タブで、**データベース**SQL Server Compact データベースのワークスペースが無効になっています。</span><span class="sxs-lookup"><span data-stu-id="66a6d-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="66a6d-396">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-396">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-397">なし。</span><span class="sxs-lookup"><span data-stu-id="66a6d-397">None.</span></span> <span data-ttu-id="66a6d-398">SQL Server Compact ありませんではテーブル間のリレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="66a6d-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="66a6d-399">問題: パラメーター化された SQL クエリが例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="66a6d-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="66a6d-400">SQL Server Compact 4.0 を指定しない場合、データ型などで`SqlDbType`または`DbType`クエリの実行時にパラメーター化クエリでパラメーターの場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="66a6d-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="66a6d-401">**回避策**</span><span class="sxs-lookup"><span data-stu-id="66a6d-401">**Workaround**</span></span>  
> <span data-ttu-id="66a6d-402">などのパラメーターのデータ型を明示的に設定`SqlDbType`または`DbType`です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="66a6d-403">これは、BLOB データ型の場合に重要 (`image`と`ntext`)。</span><span class="sxs-lookup"><span data-stu-id="66a6d-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="66a6d-404">次のようにコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="66a6d-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="66a6d-405">参照項目</span><span class="sxs-lookup"><span data-stu-id="66a6d-405">For More Information</span></span>

<span data-ttu-id="66a6d-406">WebMatrix ベータ 3 の詳細については、次の web サイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="66a6d-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="66a6d-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="66a6d-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="66a6d-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="66a6d-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="66a6d-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="66a6d-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="66a6d-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="66a6d-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="66a6d-411">All Rights Reserved.</span><span class="sxs-lookup"><span data-stu-id="66a6d-411">All Rights Reserved.</span></span> <span data-ttu-id="66a6d-412">[利用規約](https://msdn.microsoft.cos/cc300389.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="66a6d-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
