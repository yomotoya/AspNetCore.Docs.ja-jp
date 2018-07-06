---
uid: web-pages/readme/beta3
title: Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme |Microsoft Docs
author: rick-anderson
description: Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme
ms.author: aspnetcontent
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 16b324e555b5450fc1e05c63e7e19985a2d02b89
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831833"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="23d85-103">Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme</span><span class="sxs-lookup"><span data-stu-id="23d85-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="23d85-104">Web Matrix と ASP.NET Web Pages (Razor) Beta 3 リリース Readme</span><span class="sxs-lookup"><span data-stu-id="23d85-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="23d85-105">2010 年 11 月 9 日</span><span class="sxs-lookup"><span data-stu-id="23d85-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="23d85-106">目次</span><span class="sxs-lookup"><span data-stu-id="23d85-106">Contents</span></span>

- [<span data-ttu-id="23d85-107">概要</span><span class="sxs-lookup"><span data-stu-id="23d85-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="23d85-108">インストール</span><span class="sxs-lookup"><span data-stu-id="23d85-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="23d85-109">新しい機能、変更、およびベータ 3 リリースの既知の問題</span><span class="sxs-lookup"><span data-stu-id="23d85-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="23d85-110">WebMatrix のインストールに関する問題</span><span class="sxs-lookup"><span data-stu-id="23d85-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="23d85-111">ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="23d85-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="23d85-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="23d85-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="23d85-113">アプリケーションのインストール</span><span class="sxs-lookup"><span data-stu-id="23d85-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="23d85-114">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="23d85-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="23d85-115">その他の問題</span><span class="sxs-lookup"><span data-stu-id="23d85-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="23d85-116">詳細については</span><span class="sxs-lookup"><span data-stu-id="23d85-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="23d85-117">概要</span><span class="sxs-lookup"><span data-stu-id="23d85-117">Overview</span></span>

> <span data-ttu-id="23d85-118">Microsoft WebMatrix のベータ版は、数分でインストールする無料の web 開発スタックです。</span><span class="sxs-lookup"><span data-stu-id="23d85-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="23d85-119">データベース プログラミング フレームワークが 1 つ、統合されたエクスペリエンスを作成すると、web サーバーが統合されています。</span><span class="sxs-lookup"><span data-stu-id="23d85-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="23d85-120">WebMatrix のベータ版を使用して、コーディング、テスト、および独自の ASP.NET または PHP web のサイトを公開する方法を効率化、または WebMatrix のベータ版を使用して、DotNetNuke、Umbraco、WordPress、Joomla などの一般的なオープン ソース アプリを使用して新しい web サイトを開始することができます。</span><span class="sxs-lookup"><span data-stu-id="23d85-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="23d85-121">WebMatrix のベータ版では、同じ強力な web サーバー、データベース エンジン、および円滑かつシームレスに開発から運用環境への移行を使用すると、インターネット上の web サイトを実行するフレームワークの環境を使用します。</span><span class="sxs-lookup"><span data-stu-id="23d85-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="23d85-122">インストール</span><span class="sxs-lookup"><span data-stu-id="23d85-122">Installation</span></span>

> <span data-ttu-id="23d85-123">使用して WebMatrix Beta 3 をインストールする[Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="23d85-124">Web Platform Installer をインストールした後は、Beta 3 の WebMatrix のインストールに使用できます。</span><span class="sxs-lookup"><span data-stu-id="23d85-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="23d85-125">インストール中に問題がある場合を参照してください[Microsoft Web Platform Installer に関する問題をトラブルシューティング](https://go.microsoft.com/fwlink/?LinkId=196212)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="23d85-126">アプリケーションを発行する手順</span><span class="sxs-lookup"><span data-stu-id="23d85-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="23d85-127">参照してください[でアプリケーションを公開する手順](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="23d85-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="23d85-128">新しい機能、変更、andKnown 問題</span><span class="sxs-lookup"><span data-stu-id="23d85-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="23d85-129">WebMatrix 3 をベータ版のインストール</span><span class="sxs-lookup"><span data-stu-id="23d85-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="23d85-130">問題: Beta 3 の WebMatrix は、Microsoft .NET Framework 4 をサポートするプラットフォームで利用可能なのみ</span><span class="sxs-lookup"><span data-stu-id="23d85-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="23d85-131">.NET Framework version 4 が WebMatrix のベータ版に必要です。</span><span class="sxs-lookup"><span data-stu-id="23d85-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="23d85-132">場合によっては、WebMatrix のベータ版のインストーラーができます、サポートされている構成セットの一部ではないプラットフォームをインストールしようとしています。</span><span class="sxs-lookup"><span data-stu-id="23d85-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="23d85-133">具体的には、Windows Vista SP1 更新プログラムがないと、WebMatrix のベータ版のインストールを開始するが、.NET Framework 4 のコンポーネントは失敗し、インストールをブロックします。</span><span class="sxs-lookup"><span data-stu-id="23d85-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="23d85-134">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-134">**Workaround**</span></span>  
> <span data-ttu-id="23d85-135">含む、サポートされているプラットフォームにインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="23d85-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="23d85-136">Windows 7</span></span>
> - <span data-ttu-id="23d85-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="23d85-137">Windows Server 2008</span></span>
> - <span data-ttu-id="23d85-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="23d85-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="23d85-139">Windows Vista SP1 以降</span><span class="sxs-lookup"><span data-stu-id="23d85-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="23d85-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="23d85-140">Windows XP SP3</span></span>
> - <span data-ttu-id="23d85-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="23d85-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="23d85-142">Microsoft Visual Studio 2008 SP1 がない場合に Microsoft Visual Studio 2008 がインストールされている場合、問題点: は WebMatrix Beta 3 をインストールことはできません。</span><span class="sxs-lookup"><span data-stu-id="23d85-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="23d85-143">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-143">**Workaround**</span></span>  
> <span data-ttu-id="23d85-144">インストール[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft ダウンロード センターから。</span><span class="sxs-lookup"><span data-stu-id="23d85-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="23d85-145">問題: SQL Server Compact 4.0 の一部のアセンブリが GAC にインストールされません。</span><span class="sxs-lookup"><span data-stu-id="23d85-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="23d85-146">64 ビット コンピューターで SQL Server Compact 4.0 をインストールして、コンピューターにのみ、.NET Framework 3.5 SP1 Client Profile インストールされているときに、SQL Server Compact 4.0 のマネージ アセンブリがグローバル アセンブリ キャッシュ (GAC) に配置されていません。</span><span class="sxs-lookup"><span data-stu-id="23d85-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="23d85-147">マネージ アセンブリを GAC にインストールされていない次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="23d85-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="23d85-148">*System.Data.SqlServerCe.dll* (ADO.NET プロバイダー)</span><span class="sxs-lookup"><span data-stu-id="23d85-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="23d85-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="23d85-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="23d85-150">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-150">**Workaround**</span></span>  
> <span data-ttu-id="23d85-151">アンインストールの SQL Server Compact 4.0。</span><span class="sxs-lookup"><span data-stu-id="23d85-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="23d85-152">ダウンロードして、次の場所から、フル バージョンの .NET Framework 3.5 SP1 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="23d85-153">Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)</span><span class="sxs-lookup"><span data-stu-id="23d85-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="23d85-154">SQL Server Compact 4.0 を再インストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="23d85-155">問題: SQL Server Compact のコマンドラインを使用してをアンインストールできません。</span><span class="sxs-lookup"><span data-stu-id="23d85-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="23d85-156">SQL Server Compact のコマンド ライン オプションを使用してのアンインストールは、このリリースでは機能しません。</span><span class="sxs-lookup"><span data-stu-id="23d85-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="23d85-157">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-157">**Workaround**</span></span>  
> <span data-ttu-id="23d85-158">使用*プログラムと機能*Windows コントロール パネルの Microsoft SQL Server Compact 4.0 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="23d85-159">ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="23d85-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="23d85-160">ドキュメントのこのセクションでは、新機能、変更、および Razor 構文を使用して ASP.NET Web Pages のベータ 3 リリースの既知の問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="23d85-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="23d85-161">新機能</span><span class="sxs-lookup"><span data-stu-id="23d85-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="23d85-162">変更</span><span class="sxs-lookup"><span data-stu-id="23d85-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="23d85-163">問題</span><span class="sxs-lookup"><span data-stu-id="23d85-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="23d85-164">Razor 構文を使用して ASP.NET Web Pages の Beta 3 の新機能</span><span class="sxs-lookup"><span data-stu-id="23d85-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="23d85-165">"Html.Raw"メソッドがエンコードされていないマークアップを表示する新機能。</span><span class="sxs-lookup"><span data-stu-id="23d85-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="23d85-166">新しい`Html.Raw`メソッドを使用して、エンコード済み出力をレンダリングするのではなくマークアップとして HTML マークアップを表示できます。</span><span class="sxs-lookup"><span data-stu-id="23d85-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="23d85-167">(既定では、ASP.NET Razor エンコード文字列に表示する前にします。)構文は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="23d85-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="23d85-168">`Html.Raw` を使用する方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="23d85-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="23d85-169">Razor 構文を使用して ASP.NET Web Pages の Beta 3 の変更</span><span class="sxs-lookup"><span data-stu-id="23d85-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="23d85-170">変更:"HrefAttribute"メソッドの削除</span><span class="sxs-lookup"><span data-stu-id="23d85-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="23d85-171">`HrefAttribute`のメソッド、`WebPage`クラスは削除されました。</span><span class="sxs-lookup"><span data-stu-id="23d85-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="23d85-172">このヘルパーは Url の安全でない文字のエンコードに使用されました。</span><span class="sxs-lookup"><span data-stu-id="23d85-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="23d85-173">ASP.NET Razor 文字列を自動的にエンコードするためは必要なくなりました。</span><span class="sxs-lookup"><span data-stu-id="23d85-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="23d85-174">(新しい使用`Html.Raw`エンコードされていない文字列をレンダリングするメソッド)。</span><span class="sxs-lookup"><span data-stu-id="23d85-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="23d85-175">変更: 構文の宣言型"@helper"ヘルパーの変更</span><span class="sxs-lookup"><span data-stu-id="23d85-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="23d85-176">使用して作成されたヘルパーを解析する方法が ASP.NET Beta 3 リリースでは、変更、`@helper`構文。</span><span class="sxs-lookup"><span data-stu-id="23d85-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="23d85-177">基本的には、`@helper`構文がコードを含めることができるマークアップのブロックとしての代わりに、コード ブロックとして解析されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="23d85-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="23d85-178">そのため、ヘルパー内のコードで囲む必要はない`@{ }`ブロックします。</span><span class="sxs-lookup"><span data-stu-id="23d85-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="23d85-179">ヘルパー内のマークアップがまたは ASP.NET Razor の HTML 要素で明示的に追加するのには逆に、`<text></text>`タグ。</span><span class="sxs-lookup"><span data-stu-id="23d85-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="23d85-180">たとえば、次`@helper`Beta 3 のリリースで構文の動作します。</span><span class="sxs-lookup"><span data-stu-id="23d85-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="23d85-181">Beta 3 リリースでは、このヘルパーは、次の例のように変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="23d85-182">注意、`@{ }`ヘルパーの最初のコードの周りの文字は使用されなくなりました。</span><span class="sxs-lookup"><span data-stu-id="23d85-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="23d85-183">これは、ヘルパーの内容が既定では、コード ブロックとして扱われるためにです。</span><span class="sxs-lookup"><span data-stu-id="23d85-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="23d85-184">ヘルパーは、オープンで始まる、マークアップを出力します。`<a>`タグ。</span><span class="sxs-lookup"><span data-stu-id="23d85-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="23d85-185">場合は、ヘルパーは、プレーン テキストまたは終了タグを含まないタグをレンダリングする必要があります (たとえば、`<meta>`タグ)、表示される内容がである必要があります`<text></text>`タグ。</span><span class="sxs-lookup"><span data-stu-id="23d85-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="23d85-186">変更:"WebPageContext.HttpContext"の削除</span><span class="sxs-lookup"><span data-stu-id="23d85-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="23d85-187">`WebPageContext.HttpContext`プロパティが削除されました。</span><span class="sxs-lookup"><span data-stu-id="23d85-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="23d85-188">代わりに、`HttpContext.Current` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="23d85-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="23d85-189">(、`WebPageContext.HttpContext`プロパティが単にこれをラップします)。</span><span class="sxs-lookup"><span data-stu-id="23d85-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="23d85-190">新しいパッケージに変更:"Facebook"ヘルパーの移動</span><span class="sxs-lookup"><span data-stu-id="23d85-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="23d85-191">`Facebook`ヘルパー ページに移動しました、 *Facebook.Helper*ライブラリが含まれています、`Facebook`ヘルパーと機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="23d85-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="23d85-192">必要がありますとしてインストールするこのライブラリを別のパッケージ「ヘルパーでマネージャーのパッケージをインストールする」」の説明に従って、チュートリアルでは[ASP.NET ページの概要](https://go.microsoft.com/fwlink/?LinkId=202889)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="23d85-193">新しいアセンブリへの変更: メンバーシップ、ロール、およびセキュリティの種類の移動</span><span class="sxs-lookup"><span data-stu-id="23d85-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="23d85-194">次の種類に移動された、`WebMatrix.WebData`アセンブリ。</span><span class="sxs-lookup"><span data-stu-id="23d85-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="23d85-195">System.Web.WebPages.dll アセンブリへの変更:"TagBuilder"クラスの移動</span><span class="sxs-lookup"><span data-stu-id="23d85-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="23d85-196">`TagBuilde` System.Web.WebPages.dll アセンブリに移動しました r クラス。</span><span class="sxs-lookup"><span data-stu-id="23d85-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="23d85-197">以前は、これは、ASP.NET MVC の一部であるアセンブリででした。</span><span class="sxs-lookup"><span data-stu-id="23d85-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="23d85-198">この変更により、ASP.NET MVC を使用するためにインストールする必要はありません、`TagBuilder`クラス。</span><span class="sxs-lookup"><span data-stu-id="23d85-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="23d85-199">ただし、クラスは、引き続き、`System.Web.Mvc`名前空間。</span><span class="sxs-lookup"><span data-stu-id="23d85-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="23d85-200">使用するには、`TagBuilder`クラス (たとえば、カスタム ASP.NET Razor ヘルパー)、名前空間を参照する必要があります (たとえば、追加することで`@using System.Web.Mvc`をコードに)。</span><span class="sxs-lookup"><span data-stu-id="23d85-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="23d85-201">変更: 要求の検証の構文が変更されてください。「検証」クラスの削除</span><span class="sxs-lookup"><span data-stu-id="23d85-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="23d85-202">ベータ 3 リリースでは、個々 のフィールドまたはフィールドのセットの検証を無効に呼び出すことができます、`Validation.Exclude`メソッド、検証から除外するフィールドの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="23d85-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="23d85-203">新しい構文が検証をバイパス Beta 3 のリリースで使用します。</span><span class="sxs-lookup"><span data-stu-id="23d85-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="23d85-204">`Validation` Beta 3 で使用されるメソッドが削除されました。</span><span class="sxs-lookup"><span data-stu-id="23d85-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="23d85-205">Web サイトでのようなエラーが報告される場合、ユーザーが、HTML マークアップを (たとえば、リッチ テキスト エディターを使用して、ページ上) をアップロードしようとしています場合、要求の検証を無効にしないでください、 *クライアントから可能性のある危険なRequest.Form値が検出されました*。ユーザー入力は許可されません。</span><span class="sxs-lookup"><span data-stu-id="23d85-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="23d85-206">要求の検証を無効にした場合、危険性のあるマークアップが含まれますしたりしないようなものを使用してスクリプトを確認するユーザー入力を手動で確認する必要があります、 [Microsoft Anti-cross Site Scripting ライブラリ V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="23d85-207">自動の要求の検証を無効にするを呼び出す、`Request.Unvalidated`メソッド、フィールドまたはの要求の検証をバイパスするその他の投稿オブジェクトの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="23d85-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="23d85-208">このメソッドを使用してすべての項目の検証をバイパスすることができます、 `Form`、 `QueryString`、 `Cookies`、および`ServerVariables`コレクション。</span><span class="sxs-lookup"><span data-stu-id="23d85-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="23d85-209">次の例を使用する方法を示して、`Unvalidated`メソッド。</span><span class="sxs-lookup"><span data-stu-id="23d85-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="23d85-210">Razor 構文を使用する ASP.NET Web Pages の既知の問題</span><span class="sxs-lookup"><span data-stu-id="23d85-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="23d85-211">問題: 予期しない動作のメンバーシップをカスタム ユーザー テーブルを使用する場合</span><span class="sxs-lookup"><span data-stu-id="23d85-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="23d85-212">ASP.NET Razor の web サイトのメンバーシップ プロバイダーを初期化するために呼び出す、`WebSecurity.InitializeDatabaseConnection`メソッド。</span><span class="sxs-lookup"><span data-stu-id="23d85-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="23d85-213">(WebMatrix で、スターター サイト テンプレートには、このメソッドの呼び出しが含まれています、  *\_AppStart.cshtml*ファイルです)。場合、`autoCreateTables`このメソッドのパラメーターの設定を true に (に既定では、設定は、スターター サイト テンプレートの場合は true)、メソッドがエラーをスローしない場合は、認識できないテーブル名は、(2 番目のパラメーター) のメソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="23d85-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="23d85-214">代わりに、テーブルを自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="23d85-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="23d85-215">これをカスタム ユーザー テーブルを使用して、メンバーシップが間違ったテーブル名を渡すする場合、問題になることができます、`WebSecurity.InitializeDatabaseConnection`メソッド。</span><span class="sxs-lookup"><span data-stu-id="23d85-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="23d85-216">メソッドは既定では、エラーは発生を指定するテーブルが存在しない場合、代わりに新しいテーブルを作成するため、アプリケーションが動作して表示できます。</span><span class="sxs-lookup"><span data-stu-id="23d85-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="23d85-217">ただし、カスタム ユーザー テーブル (および内のフィールド) に依存しているアプリケーション コード最終的に予期しないエラーで失敗します。</span><span class="sxs-lookup"><span data-stu-id="23d85-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="23d85-218">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-218">**Workaround**</span></span>  
> <span data-ttu-id="23d85-219">渡された名前を確認、`InitializeDatabaseConnection`メソッドと一致するユーザー プロファイル、メンバーシップ データベースにテーブルまたはことを確認、`autoCreateTables`パラメーターが false に設定します。</span><span class="sxs-lookup"><span data-stu-id="23d85-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="23d85-220">問題:"を SQL Server のユーザー インスタンスを生成できませんでした。</span><span class="sxs-lookup"><span data-stu-id="23d85-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="23d85-221">WebMatrix Web アプリケーションが SQL Server Express を使用して、Windows 7 または Windows Server 2008 R2 に IIS 7.5 を実行している場合は、SQL Server が実行時にユーザーのローカル アプリケーション パスを取得できませんかを示すエラーが表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="23d85-222">**回避策**(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントが、アプリケーションのルート フォルダーとサブフォルダーの読み取り/書き込みアクセス許可をなどがあるかどうかを確認*アプリ\_データ*.</span><span class="sxs-lookup"><span data-stu-id="23d85-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="23d85-223">詳細については、サポート技術情報の記事で使用可能な[SQL Server Express ユーザー インスタンスと ASP.net Web アプリケーション プロジェクトに関する問題を](https://support.microsoft.com/kb/2002980)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="23d85-224">問題: Visual Studio で、カスタム アセンブリ (Dll) の名前空間が自動的にインポートされません。</span><span class="sxs-lookup"><span data-stu-id="23d85-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="23d85-225">Visual Studio のプロジェクトでカスタム アセンブリを使用する場合これらのアセンブリで宣言された名前空間はデザイン時に自動的にインポートされません。</span><span class="sxs-lookup"><span data-stu-id="23d85-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="23d85-226">その結果、カスタム型への参照は、デザイン時に認識されない可能性があり、(「波線」を使用) では認識されないの Visual Studio にマークされます。</span><span class="sxs-lookup"><span data-stu-id="23d85-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="23d85-227">この問題は Visual Studio でデザイン時にのみ発生します。アプリケーション自体が正常に実行されます。</span><span class="sxs-lookup"><span data-stu-id="23d85-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="23d85-228">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-228">**Workaround**</span></span>  
> <span data-ttu-id="23d85-229">含まれて、`using`ステートメント (`imports` Visual Basic で) を参照するエンティティをデザイン時に認識されません。</span><span class="sxs-lookup"><span data-stu-id="23d85-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="23d85-230">問題: Visual Studio IntelliSense およびプロジェクトで使用できるテンプレートのみ ASP.NET MVC version 3</span><span class="sxs-lookup"><span data-stu-id="23d85-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="23d85-231">ASP.NET Web Pages をインストールすることもはインストールされません tools for Visual Studio ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートなど。</span><span class="sxs-lookup"><span data-stu-id="23d85-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="23d85-232">**回避策**を Visual Studio での ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクト テンプレートを使用するインストール ASP.NET MVC 3 RC、Web Platform Installer を使用するか、または[スタンドアロン インストーラー](https://go.microsoft.com/fwlink/?LinkID=191797)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="23d85-233">問題:"&lt;ヘルパー&gt;クラスが見つかりません"エラー</span><span class="sxs-lookup"><span data-stu-id="23d85-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="23d85-234">Beta 3 にアップグレードした後は、エラーが表示可能性があるヘルパー クラス (たとえば、`Facebook`クラス) は見つかりません。</span><span class="sxs-lookup"><span data-stu-id="23d85-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="23d85-235">Beta 2 を起動して、Beta 3 で手順を続行、ヘルパーを明示的にインストールする必要のあるパッケージに移動されました。</span><span class="sxs-lookup"><span data-stu-id="23d85-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="23d85-236">これらのパッケージを含めるには、既存のサイトはアップグレードされていません。内のサイトが含まれます、 *\My Documents\IISExpress*または*\My Documents\My Websites*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="23d85-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="23d85-237">既定のサイトを使用する場合にこのエラーが表示されます具体的には、*個人用サイト*(WebSite1) への参照を含む、`Twitter`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="23d85-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="23d85-238">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-238">**Workaround**</span></span>  
> <span data-ttu-id="23d85-239">コメント アウトの実行、サイトのすべてのヘルパーを呼び出し、 *\_管理者*ページ、およびパッケージまたはを使用するヘルパーを含むパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="23d85-240">パッケージをインストールしたら後のヘルパーを参照している行のコメントを解除できます。</span><span class="sxs-lookup"><span data-stu-id="23d85-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="23d85-241">問題: Bin フォルダーに 3 ASP.NET Razor のベータ版のアセンブリの展開では使用できませんのサイトのホスト</span><span class="sxs-lookup"><span data-stu-id="23d85-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="23d85-242">サイトの ASP.NET Razor のベータ 3 のアセンブリを展開する場合、ASP.NET Web Pages の web サイトをホストしているサイトに配置する場合と*Bin*フォルダーでは、次を含むエラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="23d85-243">これは、ホスティング プロバイダーが ASP.NET Web ページの Beta 1 のアセンブリをサーバーのグローバル アプリケーション キャッシュ (GAC) にインストールされている場合に発生することができます。</span><span class="sxs-lookup"><span data-stu-id="23d85-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="23d85-244">GAC のアセンブリがローカルでインストールされているアセンブリを優先順位を取得、 *Bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="23d85-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="23d85-245">**回避策**が表示されるエラー プロバイダーのバージョン間の競合によりことを確認するホスティング プロバイダーに問い合わせて、アセンブリと自分の。</span><span class="sxs-lookup"><span data-stu-id="23d85-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="23d85-246">そうである場合、ホスティング プロバイダーが、サーバーの GAC にアセンブリを更新することを要求します。</span><span class="sxs-lookup"><span data-stu-id="23d85-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="23d85-247">問題: フィードまたはプロキシ サーバー経由で他の外部データの読み取り</span><span class="sxs-lookup"><span data-stu-id="23d85-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="23d85-248">プロキシ情報を構成する必要がありますが、サイトを実行しているサーバーがプロキシ サーバーの背後にある場合は、 *Web.config*ファイルをサイトの外部から送信される情報を読み取ることができるようにします。</span><span class="sxs-lookup"><span data-stu-id="23d85-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="23d85-249">たとえば、使用する場合、`ReCaptcha`ヘルパー、ヘルパーは、reCAPTCHA サービスと通信がプロキシ サーバーによってブロックされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="23d85-250">同様に、フィード、パッケージ マネージャーによって使用されているフィードなどの ASP.NET Web Pages を使用するには、プロキシ構成を必要があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="23d85-251">外部サービスの操作またはパッケージ フィードの操作で問題が発生する場合は、アプリケーションのルートに、次の要素を配置*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="23d85-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="23d85-252">プロキシ サーバーを構成する方法の詳細については、次を参照してください。 [&lt;プロキシ&gt;要素 (ネットワーク設定)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web サイト。</span><span class="sxs-lookup"><span data-stu-id="23d85-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="23d85-253">問題:「Microsoft.Web.Infrastructure.dll を読み込むことができません」エラー</span><span class="sxs-lookup"><span data-stu-id="23d85-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="23d85-254">すべての適切なアセンブリを除く GAC にインストールは Razor 構文を使用して、Beta 1 バージョンの ASP.NET Web ページを以前にインストールし、Beta 3 をインストールした場合、 *Microsoft.Web.Infrastructure.dll*します。</span><span class="sxs-lookup"><span data-stu-id="23d85-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="23d85-255">結果として、ASP.NET Razor ページを実行するときにエラーが表示されることを示します*Microsoft.Web.Infrastructure.dll*読み込めませんでした。</span><span class="sxs-lookup"><span data-stu-id="23d85-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="23d85-256">クリーンなコンピューターにベータ 3 リリースが読み込まれている場合、この問題は発生しません。</span><span class="sxs-lookup"><span data-stu-id="23d85-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="23d85-257">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-257">**Workaround**</span></span>  
> <span data-ttu-id="23d85-258">コントロール パネルで、ASP.NET Web Pages をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="23d85-259">ベータ 3 リリースの再インストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="23d85-260">問題: .NET Framework version 4 をアンインストールする Razor 構文を使用して ASP.NET Web ページを無効にします</span><span class="sxs-lookup"><span data-stu-id="23d85-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="23d85-261">.NET Framework version 4 をアンインストールし、再インストールして、Razor 構文を使用して ASP.NET Web Pages は無効になります。</span><span class="sxs-lookup"><span data-stu-id="23d85-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="23d85-262">ページで、 *.cshtml*拡張機能が正常に動作しません。</span><span class="sxs-lookup"><span data-stu-id="23d85-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="23d85-263">ASP.NET Web ページには、マシン ルートのアセンブリが登録されます*Web.config*ファイル、および .NET Framework の削除は、そのファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="23d85-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="23d85-264">.NET Framework を再インストール、構成ファイルの新しいバージョンがインストールされますが、ASP.NET Web Pages アセンブリの参照を追加できません。</span><span class="sxs-lookup"><span data-stu-id="23d85-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="23d85-265">**回避策**.NET Framework を再インストール後に Razor 構文を使用する ASP.NET Web ページを再インストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="23d85-266">次の要素が追加されます、 *Web.config*マシンのルートには、通常、次の場所にファイル。</span><span class="sxs-lookup"><span data-stu-id="23d85-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="23d85-267">問題: 以前に展開された Bin フォルダーにアセンブリを ASP.NET アプリケーションにエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="23d85-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="23d85-268">デプロイでは、ASP.NET Web Pages アセンブリのコピー中に (たとえば、 *Microsoft.WebPages.dll*) に、 *Bin*サーバー上の web サイトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="23d85-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="23d85-269">(デプロイ時に自動的にこの発生した可能性または開発者が明示的にアセンブリをコピーするためです)。ただし、ベータ 3 リリースがインストールされているときにエラーが発生した特定の種類が見つからないエラーなどです。</span><span class="sxs-lookup"><span data-stu-id="23d85-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="23d85-270">これは、さまざまな ASP.NET Web ページの種類は、ベータ 3 リリースの異なる名前空間に移動されたために発生します。</span><span class="sxs-lookup"><span data-stu-id="23d85-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="23d85-271">**回避策** </span><span class="sxs-lookup"><span data-stu-id="23d85-271">**Workaround** </span></span>  
> <span data-ttu-id="23d85-272">クリア、 *Bin*デプロイしたアプリケーションのフォルダー、フォルダーに新しいアセンブリをコピー (またはアプリケーションを再配置) し、アプリケーションを再起動します。</span><span class="sxs-lookup"><span data-stu-id="23d85-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="23d85-273">問題: 拡張子のない Url が見つからない IIS 7 または IIS 7.5.cshtml/.vbhtml ファイル</span><span class="sxs-lookup"><span data-stu-id="23d85-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="23d85-274">IIS 7 または IIS 7.5 では、次のような URL を使用して要求をできないがページを検索、 *.cshtml*または *.vbhtml*拡張機能。</span><span class="sxs-lookup"><span data-stu-id="23d85-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="23d85-275">この問題は、URL リライトが有効でない既定で IIS 7 または IIS 7.5 のために発生します。</span><span class="sxs-lookup"><span data-stu-id="23d85-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="23d85-276">考えられるシナリオでは、IIS Express を使用してローカルでテストするときに問題は表示されないが、web サイトをホスティング web サイトにデプロイするときに発生することです。</span><span class="sxs-lookup"><span data-stu-id="23d85-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="23d85-277">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="23d85-278">インストールで説明されている更新プログラムをサーバー コンピューターの場合は、サーバー コンピューター上のコントロールがある場合は、 [、更新プログラムを処理するために IIS 7.0 または IIS 7.5 のハンドラーは要求 Url を特定できますが、期間で終わっていない利用可能な](https://support.microsoft.com/kb/980368)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="23d85-279">サーバー コンピューター上のコントロールがいないかどうか (たとえばを展開しているホスティング web サイトに) 次に、web サイトの追加*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="23d85-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="23d85-280">問題: 同じアプリケーションでの Web アプリケーション プロジェクトまたは ASP.NET MVC と ASP.NET Web ページの使用</span><span class="sxs-lookup"><span data-stu-id="23d85-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="23d85-281">Web アプリケーション プロジェクトまたは ASP.NET MVC アプリケーションでは、ASP.NET Web Pages を使用していた、エラーは表示がありますを*WebPageHttpApplication*が見つかりません。</span><span class="sxs-lookup"><span data-stu-id="23d85-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="23d85-282">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-282">**Workaround**</span></span>  
> <span data-ttu-id="23d85-283">このエラーが発生する場合は、アプリケーションの派生元である基本クラスを変更します。</span><span class="sxs-lookup"><span data-stu-id="23d85-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="23d85-284">*Global.asax*ファイルで、次の行を変更します。</span><span class="sxs-lookup"><span data-stu-id="23d85-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="23d85-285">次のように</span><span class="sxs-lookup"><span data-stu-id="23d85-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="23d85-286">これが、有効で導入された変更を反転、Beta 1 リリースの ASP.NET Web ページ Razor 構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="23d85-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="23d85-287">問題: SQL Server Compact がインストールされていないコンピューターにアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="23d85-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="23d85-288">SQL Server Compact データベースを含むアプリケーションは、SQL Server Compact がインストールされていないコンピューターで実行できます。</span><span class="sxs-lookup"><span data-stu-id="23d85-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="23d85-289">Microsoft WebMatrix の Beta 3 が自動的がこれらのバイナリをコピーし、適切な実行*Web.config*ファイルの変換。</span><span class="sxs-lookup"><span data-stu-id="23d85-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="23d85-290">**回避策**これらのファイルをコピーする必要がある場合、 *Web.config*ファイルの変更を手動で、次の操作します。</span><span class="sxs-lookup"><span data-stu-id="23d85-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="23d85-291">データベース エンジン アセンブリをコピー、 *Bin*ターゲット コンピューター上のアプリケーションのフォルダー (とサブフォルダー)。</span><span class="sxs-lookup"><span data-stu-id="23d85-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="23d85-292">コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **に** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="23d85-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="23d85-293">コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **に** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="23d85-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="23d85-294">コピー *C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **に** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="23d85-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="23d85-295">Web サイトのルート フォルダーに作成または開き、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="23d85-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="23d85-296">(WebMatrix Beta 3 では、このファイルの種類がクリックした場合に使用可能な**すべて**で、**ファイルの種類を選択** ダイアログ ボックス)。</span><span class="sxs-lookup"><span data-stu-id="23d85-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="23d85-297">子として次の要素を追加、 **&lt;構成&gt;** 要素 (内部ではなく、 **&lt;system.web&gt;** 要素)。</span><span class="sxs-lookup"><span data-stu-id="23d85-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="23d85-298">問題: Visual Basic で中程度の信頼でデータベースとの WebGrid ヘルパーは機能しません</span><span class="sxs-lookup"><span data-stu-id="23d85-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="23d85-299">Visual Basic を使用している場合 (作成 *.vbhtml*ファイル)、`Database`と`WebGrid`中程度の信頼を使用するアプリケーションが設定されている場合、ヘルパーは機能しません。</span><span class="sxs-lookup"><span data-stu-id="23d85-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="23d85-300">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-300">**Workaround**</span></span>  
> <span data-ttu-id="23d85-301">完全な信頼を使用するアプリケーションを一時的に設定します。</span><span class="sxs-lookup"><span data-stu-id="23d85-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="23d85-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="23d85-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="23d85-303">問題:"Encrypt"プロパティを認識できません。</span><span class="sxs-lookup"><span data-stu-id="23d85-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="23d85-304">SQL Server Compact 4.0 が認識されない、`Encrypt`のプロパティ、`SqlCeConnection`クラス。</span><span class="sxs-lookup"><span data-stu-id="23d85-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="23d85-305">データベース ファイルの暗号化には、このプロパティを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="23d85-306">`Encrypt`プロパティは、SQL Server Compact 3.5 のリリースで非推奨とされましたし、旧バージョンと互換性のためだけに保持されました。</span><span class="sxs-lookup"><span data-stu-id="23d85-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="23d85-307">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-307">**Workaround**</span></span>  
> <span data-ttu-id="23d85-308">使用して、`Encryption Mode`のプロパティ、 `SqlCeConnection` SQL Server Compact 4.0 データベース ファイルを暗号化するクラス。</span><span class="sxs-lookup"><span data-stu-id="23d85-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="23d85-309">次の例では、暗号化された SQL Server Compact 4.0 データベースを作成する方法を示しています、`Encryption Mode`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="23d85-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="23d85-310">既存の SQL Server Compact 4.0 データベースの暗号化モードを変更するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="23d85-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="23d85-311">暗号化されていない SQL Server Compact 4.0 データベースを暗号化するには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="23d85-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="23d85-312">問題: Microsoft Visual C 2008 のランタイム ライブラリが必要です。</span><span class="sxs-lookup"><span data-stu-id="23d85-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="23d85-313">ネイティブ Dll の SQL Server Compact 4.0 には、Microsoft Visual C 2008 ランタイム ライブラリ (x86、IA64、および x64)、Service Pack 1 が必要があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="23d85-314">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-314">**Workaround**</span></span>  
> <span data-ttu-id="23d85-315">.NET Framework 3.5 SP1 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="23d85-316">これには、Visual C 2008 ランタイム ライブラリ SP1 もインストールされます。</span><span class="sxs-lookup"><span data-stu-id="23d85-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="23d85-317">ライブラリは、次の場所からダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="23d85-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="23d85-318">Microsoft Visual C 2008 Service Pack 1 再頒布可能パッケージ ATL セキュリティ更新プログラム</span><span class="sxs-lookup"><span data-stu-id="23d85-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="23d85-319">.NET Framework 2.0、3.0 をインストールすることに注意してください。 または、4 は*いない*Visual c 2008 ランタイム ライブラリ SP1 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="23d85-320">問題: SQL Server Compact がインストールされている場合、コンピューターに .NET Framework をインストールする前に、そのプロバイダーの不変名に登録されていない .NET Framework の machine.config ファイル</span><span class="sxs-lookup"><span data-stu-id="23d85-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="23d85-321">SQL Server Compact は .NET Framework SQL Server Compact は .NET framework を必要があるためにインストールされていないコンピューターにインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="23d85-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="23d85-322">SQL Server Compact をインストールする前に .NET Framework version 3.5 も 4 をインストールする場合、SQL Server Compact のセットアップがでそのプロバイダー固定名を登録できません、 *machine.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="23d85-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="23d85-323">SQL Server Compact のエントリに依存している任意のアプリケーション、 *machine.config*ファイルは失敗します。</span><span class="sxs-lookup"><span data-stu-id="23d85-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="23d85-324">不変名登録エントリ*machine.config*次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="23d85-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="23d85-325">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-325">**Workaround**</span></span>  
> <span data-ttu-id="23d85-326">SQL Server Compact 4.0 CTP1 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="23d85-327">ダウンロードして、次の場所から .NET Framework の完全なバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="23d85-328">Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)</span><span class="sxs-lookup"><span data-stu-id="23d85-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="23d85-329">Microsoft .NET Framework 4.0 のリリース (フル パッケージ)</span><span class="sxs-lookup"><span data-stu-id="23d85-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="23d85-330">再インストールし、 [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="23d85-331">アプリケーションのインストール</span><span class="sxs-lookup"><span data-stu-id="23d85-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="23d85-332">問題: アプリケーションをインストールすることができます、長い時間がかかる場合は、ユーザーのマイ ドキュメント フォルダーはネットワーク共有にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="23d85-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="23d85-333">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-333">**Workaround**</span></span>  
> <span data-ttu-id="23d85-334">なし。</span><span class="sxs-lookup"><span data-stu-id="23d85-334">None.</span></span> <span data-ttu-id="23d85-335">アプリケーションをインストールするにはしばらくかかる場合がありますが、正常にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="23d85-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="23d85-336">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="23d85-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="23d85-337">問題:"送信先 URL"フィールドが http:// または https:// でプレフィックスが付いていない場合に発行した後サイトが機能しません。</span><span class="sxs-lookup"><span data-stu-id="23d85-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="23d85-338">**公開設定**ダイアログ ボックスで、リンク先の URL で始まらない場合`http://`または`https://`デプロイ後に、サイトが機能しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="23d85-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="23d85-339">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-339">**Workaround**</span></span>  
> <span data-ttu-id="23d85-340">確認して、サイトでリンク先の URL を発行する前に、**発行設定** ダイアログ ボックスが始まる`http://`または`https://`します。</span><span class="sxs-lookup"><span data-stu-id="23d85-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="23d85-341">"データベースを発行できませんでしたエラーで失敗 MySQL データベースのパブリッシュ問題点:。</span><span class="sxs-lookup"><span data-stu-id="23d85-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="23d85-342">これは、場合に発生、リモート データベースは、スクリプトを実行できません。"</span><span class="sxs-lookup"><span data-stu-id="23d85-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="23d85-343">さまざまな理由からこのエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="23d85-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="23d85-344">このエラーが表示できる理由の 1 つは、データベース スクリプトには、単一引用符文字 (') が含まれているし、対象の MySQL データベースの既定の文字セットを utf-8 ではないです。</span><span class="sxs-lookup"><span data-stu-id="23d85-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="23d85-345">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-345">**Workaround**</span></span>  
> <span data-ttu-id="23d85-346">既定の文字が utf-8 に、リモートの MySQL データベースの設定を設定します。</span><span class="sxs-lookup"><span data-stu-id="23d85-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="23d85-347">その他の問題</span><span class="sxs-lookup"><span data-stu-id="23d85-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="23d85-348">問題点:/検索フィルターでは機能しませんレポート グループ化: 問題の種類</span><span class="sxs-lookup"><span data-stu-id="23d85-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="23d85-349">内のテキストを入力した場合に、サイトのレポートを実行すると、 *URL フィルター*ボックスし、をクリックして*検索*、何も起こりません。</span><span class="sxs-lookup"><span data-stu-id="23d85-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="23d85-350">これは、このコントロールが中に機能していないため、 *Group By*レポートの状態に設定されて*問題の種類*、既定値します。</span><span class="sxs-lookup"><span data-stu-id="23d85-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="23d85-351">**回避策**で、 *Group By*  タブのリボンで、次のようにクリックします。 *URL*ソース URL を、別のエントリをグループ化します。</span><span class="sxs-lookup"><span data-stu-id="23d85-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="23d85-352">テキスト ボックスと、エントリをフィルター処理するボタンはこの状態中に機能します。</span><span class="sxs-lookup"><span data-stu-id="23d85-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="23d85-353">問題: WCF アプリケーションの IIS Express の実行に失敗します。</span><span class="sxs-lookup"><span data-stu-id="23d85-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="23d85-354">次のようなエラーが発生、WCF アプリケーションを参照します。</span><span class="sxs-lookup"><span data-stu-id="23d85-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="23d85-355">*ファイルまたはアセンブリを読み込むことができません ' Microsoft.Web.Administration、バージョン 7.0.0.0、カルチャを = = neutral, PublicKeyToken = 31bf3856ad364e35' またはその依存関係の 1 つ。指定されたファイルが見つかりません。*</span><span class="sxs-lookup"><span data-stu-id="23d85-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="23d85-356">これは、IIS Express の Beta リリースは、既定で WCF をサポートしないために発生します。</span><span class="sxs-lookup"><span data-stu-id="23d85-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="23d85-357">**回避策**以下の回避策のいずれかを使用して、(回避策 2 には、Microsoft Windows Vista が必要がありますまたはそれ以降)。</span><span class="sxs-lookup"><span data-stu-id="23d85-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="23d85-358">コピー、 *Microsoft.Web.dll*と*Microsoft.Web.Administration.dll*に WebMatrix のインストール場所からアセンブリ、 *bin* WCF のディレクトリアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="23d85-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="23d85-359">既定では、WebMatrix がインストールされている、 *Microsoft WebMatrix*システムの下のサブフォルダー *Program Files*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="23d85-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="23d85-360">Microsoft Windows vista またはそれ以降、内のアセンブリへのシンボリック リンクを作成、 *bin*次のコマンドを使用してディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="23d85-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="23d85-361">(このアプローチによって、アセンブリのコピーは作成できませんという利点があります)。</span><span class="sxs-lookup"><span data-stu-id="23d85-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="23d85-362">2 つのアセンブリを GAC にインストールします。</span><span class="sxs-lookup"><span data-stu-id="23d85-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="23d85-363">管理者特権のプロンプトから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="23d85-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="23d85-364">問題: Beta 3 の WebMatrix は昇格が必要な特定のタスクを実行できません。</span><span class="sxs-lookup"><span data-stu-id="23d85-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="23d85-365">Beta 3 の WebMatrix では、次の状況で追加コンポーネントのインストールなどの昇格が必要な特定のタスクを実行できません。</span><span class="sxs-lookup"><span data-stu-id="23d85-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="23d85-366">Windows Vista または Windows 7 では、管理者特権がないアカウントを使用してログオンして、ユーザー アカウント制御 (UAC) が無効になっています。</span><span class="sxs-lookup"><span data-stu-id="23d85-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="23d85-367">Microsoft Windows XP または Microsoft Windows Server 2003 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="23d85-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="23d85-368">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-368">**Workaround**</span></span>  
> <span data-ttu-id="23d85-369">WebMatrix の Beta 3 のほとんどのタスクでは、管理アクセス許可は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="23d85-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="23d85-370">該当するには、管理者は、操作を実行するか、これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="23d85-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="23d85-371">Windows Vista または Windows 7 で UAC を有効にします。</span><span class="sxs-lookup"><span data-stu-id="23d85-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="23d85-372">Windows xp では、Administrators セキュリティ グループにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="23d85-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="23d85-373">問題:「サイトから Web ギャラリー」が無効になっています</span><span class="sxs-lookup"><span data-stu-id="23d85-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="23d85-374">**Web ギャラリーからのサイト**Web Platform Installer 3.0 がインストールされていない場合に、オプションが無効になっています。</span><span class="sxs-lookup"><span data-stu-id="23d85-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="23d85-375">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-375">**Workaround**</span></span>  
> <span data-ttu-id="23d85-376">インストール、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="23d85-377">問題: Windows Server 2003 で IIS Express が起動しない管理者以外のユーザーの</span><span class="sxs-lookup"><span data-stu-id="23d85-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="23d85-378">Windows Server 2003 では、ページを起動または IIS Express を起動するときに IIS Express は起動しません。</span><span class="sxs-lookup"><span data-stu-id="23d85-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="23d85-379">Web ページの管理者以外のユーザーによって、アプリケーションが開始されたことを示すエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="23d85-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="23d85-380">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-380">**Workaround**</span></span>  
> <span data-ttu-id="23d85-381">管理ユーザーとしては、Beta 3 の WebMatrix を起動します。</span><span class="sxs-lookup"><span data-stu-id="23d85-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="23d85-382">詳細については、次のサポート技術情報の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="23d85-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="23d85-383">管理者以外のユーザーによって開始されたアプリケーションは、アプリケーションが Windows Vista、Windows Server 2003、または Windows XP で実行されているコンピューターの HTTP トラフィックをリッスンできません。</span><span class="sxs-lookup"><span data-stu-id="23d85-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="23d85-384">問題: Google Chrome は、[実行] オプションとして使用できません。</span><span class="sxs-lookup"><span data-stu-id="23d85-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="23d85-385">Google Chrome が ブラウザーの一覧に表示されない**実行**上、**ホーム**タブ。</span><span class="sxs-lookup"><span data-stu-id="23d85-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="23d85-386">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-386">**Workaround**</span></span>  
> <span data-ttu-id="23d85-387">Google Chrome の一部のバージョンを登録しないで自体正しく Windows の既定のプログラム機能。</span><span class="sxs-lookup"><span data-stu-id="23d85-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="23d85-388">この問題を回避するには、Google Chrome を開始 をクリックして、*カスタマイズと制御 Google Chrome*  メニューのをクリックして*オプション*、順にクリックします*Make Google Chrome、既定のブラウザー*します。</span><span class="sxs-lookup"><span data-stu-id="23d85-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="23d85-389">問題:「外部キー」ダイアログ ボックスのしない許可に主キーを入力します。</span><span class="sxs-lookup"><span data-stu-id="23d85-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="23d85-390">**Foreign Key**  ダイアログ ボックスは、主キー テーブルから主キーの名前を入力することを許可しません。</span><span class="sxs-lookup"><span data-stu-id="23d85-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="23d85-391">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-391">**Workaround**</span></span>  
> <span data-ttu-id="23d85-392">これは意図的なものであり、</span><span class="sxs-lookup"><span data-stu-id="23d85-392">This is intentional.</span></span> <span data-ttu-id="23d85-393">主キー テーブルの主キーの名前を入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="23d85-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="23d85-394">問題:「リレーションシップ」ボタンが無効になっています</span><span class="sxs-lookup"><span data-stu-id="23d85-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="23d85-395">**リレーションシップ**下ボタン、**テーブル** タブで、**データベース**SQL Server Compact データベースのワークスペースが無効になっています。</span><span class="sxs-lookup"><span data-stu-id="23d85-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="23d85-396">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-396">**Workaround**</span></span>  
> <span data-ttu-id="23d85-397">なし。</span><span class="sxs-lookup"><span data-stu-id="23d85-397">None.</span></span> <span data-ttu-id="23d85-398">SQL Server Compact は、テーブル間のリレーションシップはサポートしません。</span><span class="sxs-lookup"><span data-stu-id="23d85-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="23d85-399">パラメーター化された SQL クエリが例外をスローする問題点。</span><span class="sxs-lookup"><span data-stu-id="23d85-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="23d85-400">SQL Server Compact 4.0 では、データ型をなど、指定しない場合に`SqlDbType`または`DbType`クエリの実行時にパラメーター化クエリでパラメーターの場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="23d85-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="23d85-401">**回避策**</span><span class="sxs-lookup"><span data-stu-id="23d85-401">**Workaround**</span></span>  
> <span data-ttu-id="23d85-402">などのパラメーターのデータ型を明示的に設定`SqlDbType`または`DbType`します。</span><span class="sxs-lookup"><span data-stu-id="23d85-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="23d85-403">これは、BLOB データ型の場合に重要 (`image`と`ntext`)。</span><span class="sxs-lookup"><span data-stu-id="23d85-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="23d85-404">次のようなコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="23d85-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="23d85-405">参照項目</span><span class="sxs-lookup"><span data-stu-id="23d85-405">For More Information</span></span>

<span data-ttu-id="23d85-406">Beta 3 の WebMatrix の詳細については、次の web サイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="23d85-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="23d85-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="23d85-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="23d85-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="23d85-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="23d85-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="23d85-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="23d85-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="23d85-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="23d85-411">All Rights Reserved.</span><span class="sxs-lookup"><span data-stu-id="23d85-411">All Rights Reserved.</span></span> <span data-ttu-id="23d85-412">[利用規約](https://msdn.microsoft.cos/cc300389.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="23d85-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
