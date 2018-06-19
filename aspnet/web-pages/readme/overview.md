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
ms.locfileid: "30898971"
---
<a name="webmatrix-readme"></a><span data-ttu-id="e8981-103">WebMatrix の Readme</span><span class="sxs-lookup"><span data-stu-id="e8981-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="e8981-104">2011 年 1 月 13日</span><span class="sxs-lookup"><span data-stu-id="e8981-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="e8981-105">目次</span><span class="sxs-lookup"><span data-stu-id="e8981-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="e8981-106">この readme は WebMatrix の 1.0 リリースに適用されます。</span><span class="sxs-lookup"><span data-stu-id="e8981-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="e8981-107">概要</span><span class="sxs-lookup"><span data-stu-id="e8981-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="e8981-108">インストール</span><span class="sxs-lookup"><span data-stu-id="e8981-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="e8981-109">アプリケーションを公開する方法</span><span class="sxs-lookup"><span data-stu-id="e8981-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="e8981-110">変更との問題</span><span class="sxs-lookup"><span data-stu-id="e8981-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="e8981-111">WebMatrix 1.0 のインストール</span><span class="sxs-lookup"><span data-stu-id="e8981-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="e8981-112">ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="e8981-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="e8981-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e8981-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="e8981-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e8981-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="e8981-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="e8981-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="e8981-116">アプリケーションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="e8981-117">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="e8981-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="e8981-118">詳細については</span><span class="sxs-lookup"><span data-stu-id="e8981-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="e8981-119">概要</span><span class="sxs-lookup"><span data-stu-id="e8981-119">Overview</span></span>

> <span data-ttu-id="e8981-120">Microsoft WebMatrix 1.0 は、数分でインストールする無料の web 開発スタックです。</span><span class="sxs-lookup"><span data-stu-id="e8981-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="e8981-121">データベース プログラミングのフレームワークを 1 つの統合環境を作成すると、web サーバーが統合されています。</span><span class="sxs-lookup"><span data-stu-id="e8981-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="e8981-122">効率よくコーディング、テスト、および独自 ASP.NET または PHP web サイトを発行、WebMatrix を使用することや、DotNetNuke、Umbraco、WordPress、または Joomla などの人気のオープン ソース アプリケーションを使用して新しい web サイトの起動に WebMatrix を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e8981-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="e8981-123">WebMatrix は、同じ強力な web サーバー、データベース エンジン、およびインターネット上に円滑かつシームレスな開発から運用環境への移行のため、web サイトを実行しているフレームワークの環境を使用します。</span><span class="sxs-lookup"><span data-stu-id="e8981-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="e8981-124">インストール</span><span class="sxs-lookup"><span data-stu-id="e8981-124">Installation</span></span>

> <span data-ttu-id="e8981-125">WebMatrix 1.0 をインストールするには、まず、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="e8981-126">Web Platform Installer をインストールした後は、WebMatrix のインストールに使用できます。</span><span class="sxs-lookup"><span data-stu-id="e8981-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="e8981-127">インストール中に問題がある場合を参照してください[に関する問題のトラブルシューティング Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="e8981-128">アプリケーションを公開する方法</span><span class="sxs-lookup"><span data-stu-id="e8981-128">How to Publish Applications</span></span>

> <span data-ttu-id="e8981-129">参照してください[アプリケーションを公開するための手順](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="e8981-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="e8981-130">変更との問題</span><span class="sxs-lookup"><span data-stu-id="e8981-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="e8981-131">WebMatrix 1.0 のインストールに関する問題</span><span class="sxs-lookup"><span data-stu-id="e8981-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="e8981-132">問題点: WebMatrix 1.0 は Microsoft .NET Framework 4 をサポートするプラットフォームでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="e8981-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="e8981-133">.NET Framework version 4 が WebMatrix 必要です。</span><span class="sxs-lookup"><span data-stu-id="e8981-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="e8981-134">場合によっては、WebMatrix 1.0 インストーラー、サポートされている構成セットの一部ではない、プラットフォームにインストールしようとします。</span><span class="sxs-lookup"><span data-stu-id="e8981-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="e8981-135">具体的には、Windows Vista SP1 更新プログラムなしは、WebMatrix のインストールを開始するが、.NET Framework 4 コンポーネントは失敗し、インストールをブロックします。</span><span class="sxs-lookup"><span data-stu-id="e8981-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="e8981-136">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-136">**Workaround**</span></span>  
> <span data-ttu-id="e8981-137">含む、サポートされているプラットフォームにインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="e8981-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="e8981-138">Windows 7</span></span>
> - <span data-ttu-id="e8981-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="e8981-139">Windows Server 2008</span></span>
> - <span data-ttu-id="e8981-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="e8981-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="e8981-141">Windows Vista SP1 以降</span><span class="sxs-lookup"><span data-stu-id="e8981-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="e8981-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="e8981-142">Windows XP SP3</span></span>
> - <span data-ttu-id="e8981-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="e8981-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="e8981-144">Microsoft Visual Studio 2008 sp1 を適用せずに Microsoft Visual Studio 2008 がインストールされている場合、問題点: は WebMatrix 1.0 をインストールできません。</span><span class="sxs-lookup"><span data-stu-id="e8981-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="e8981-145">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-145">**Workaround**</span></span>  
> <span data-ttu-id="e8981-146">インストール[Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) Microsoft ダウンロード センターからです。</span><span class="sxs-lookup"><span data-stu-id="e8981-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="e8981-147">問題点: SQL Server Compact 4.0 の一部のアセンブリは GAC にインストールされません。</span><span class="sxs-lookup"><span data-stu-id="e8981-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="e8981-148">64 ビット コンピューターで SQL Server Compact 4.0 をインストールして、コンピューターにのみ、.NET Framework 3.5 SP1 Client Profile をインストール時に SQL Server Compact 4.0 のマネージ アセンブリでは、グローバル アセンブリ キャッシュ (GAC) に配置されていません。</span><span class="sxs-lookup"><span data-stu-id="e8981-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="e8981-149">マネージ アセンブリを GAC にインストールされていない次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e8981-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="e8981-150">*System.Data.SqlServerCe.dll* (ADO.NET プロバイダー)</span><span class="sxs-lookup"><span data-stu-id="e8981-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="e8981-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="e8981-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="e8981-152">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-152">**Workaround**</span></span>  
> <span data-ttu-id="e8981-153">SQL Server をアンインストール Compact 4.0 です。</span><span class="sxs-lookup"><span data-stu-id="e8981-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="e8981-154">ダウンロードして、次の場所から、フル バージョンの .NET Framework 3.5 SP1 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="e8981-155">Microsoft .NET Framework 3.5 Service pack 1 (フル パッケージ)</span><span class="sxs-lookup"><span data-stu-id="e8981-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="e8981-156">SQL Server Compact 4.0 を再インストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="e8981-157">問題点: SQL Server Compact のコマンドラインを使用してをアンインストールできません。</span><span class="sxs-lookup"><span data-stu-id="e8981-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="e8981-158">SQL Server Compact のコマンド ライン オプションを使用してのアンインストールは、このリリースでは機能しません。</span><span class="sxs-lookup"><span data-stu-id="e8981-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="e8981-159">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-159">**Workaround**</span></span>  
> <span data-ttu-id="e8981-160">使用して*プログラムと機能*Windows コントロール パネルで Microsoft SQL Server Compact 4.0 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="e8981-161">ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="e8981-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="e8981-162">ドキュメントのこのセクションでは、新機能、変更、および Razor 構文を使用して ASP.NET Web Pages の 1.0 リリースの既知の問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="e8981-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="e8981-163">新機能</span><span class="sxs-lookup"><span data-stu-id="e8981-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="e8981-164">変更</span><span class="sxs-lookup"><span data-stu-id="e8981-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="e8981-165">問題</span><span class="sxs-lookup"><span data-stu-id="e8981-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="e8981-166">新機能</span><span class="sxs-lookup"><span data-stu-id="e8981-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="e8981-167">パッケージ マネージャーを無効にする構成設定が追加された新機能。</span><span class="sxs-lookup"><span data-stu-id="e8981-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="e8981-168">新しい`asp:AdminManagerEnabled`キーが利用可能、`<appSettings>`内の要素、 *web.config*ファイルで、パッケージ マネージャーを完全に無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="e8981-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="e8981-169">この要素の既定値は true に含まれていない場合、つまり、 *web.config*ファイル、パッケージ マネージャーは有効にします。</span><span class="sxs-lookup"><span data-stu-id="e8981-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="e8981-170">パッケージ マネージャーを無効にするには、次の要素を追加、 *web.config* web サイトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="e8981-171">変更</span><span class="sxs-lookup"><span data-stu-id="e8981-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="e8981-172">"Asp:adminfoldervirtualpath"に変更の変更:"webPages:AdminFolderVirtualPath"キー</span><span class="sxs-lookup"><span data-stu-id="e8981-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="e8981-173">`webPages:AdminFolderVirtualPath`キーに追加できる、 *web.config*を使用して、パッケージ マネージャーの場所を指定するファイルの名前は、`asp:`名前空間の代わりに、`webPages`名前空間。</span><span class="sxs-lookup"><span data-stu-id="e8981-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="e8981-174">この要素を使用している場合は、構成ファイルで変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8981-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="e8981-175">既知の問題</span><span class="sxs-lookup"><span data-stu-id="e8981-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="e8981-176">問題: メンバーシップ、ユーザーのパスワード認識されなくなった</span><span class="sxs-lookup"><span data-stu-id="e8981-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="e8981-177">安全なを作成して、メンバーシップ (ログイン) パスワードを格納するためのアルゴリズムが変更されました。</span><span class="sxs-lookup"><span data-stu-id="e8981-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="e8981-178">その結果、ASP.NET Razor のベータ版で作成されたメンバー (ユーザー) に格納されているパスワードは認識されません。</span><span class="sxs-lookup"><span data-stu-id="e8981-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="e8981-179">**回避策**サイトが運用環境にまだ移行されていない場合は、メンバーシップ データベースからユーザー レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="e8981-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="e8981-180">データベースがライブの場合は、プログラムによって、メンバーシップ データベース内の既存のパスワードを再生成します。</span><span class="sxs-lookup"><span data-stu-id="e8981-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="e8981-181">メンバーシップのカスタム ユーザー テーブルを使用する場合の問題点: 予期しない動作</span><span class="sxs-lookup"><span data-stu-id="e8981-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="e8981-182">ASP.NET Razor の web サイトのメンバーシップ プロバイダーを初期化するために呼び出す、`WebSecurity.InitializeDatabaseConnection`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e8981-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="e8981-183">(WebMatrix で、スターター サイト テンプレートにはこのメソッドの呼び出しが含まれています、  *\_AppStart.cshtml*ファイルです)。場合、`autoCreateTables`このメソッドのパラメーターが設定を true に (に既定では、設定は、スターター サイト テンプレートの場合は true)、認識できないテーブル名がメソッド (2 番目のパラメーター) に渡される場合、メソッドはエラーをスローしません。</span><span class="sxs-lookup"><span data-stu-id="e8981-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="e8981-184">代わりに、テーブルを自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="e8981-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="e8981-185">これは、カスタム ユーザー テーブルを使用してメンバーシップに間違ったテーブル名を渡す場合問題になることができます、`WebSecurity.InitializeDatabaseConnection`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e8981-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="e8981-186">メソッドは既定ではエラーを生成しない指定したテーブルが存在しない場合、代わりに新しいテーブルを作成するため、アプリケーションが動作して表示できます。</span><span class="sxs-lookup"><span data-stu-id="e8981-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="e8981-187">ただし、カスタム ユーザー テーブル (および内のフィールド) に依存するアプリケーション コード最終的に、予期しないエラーが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e8981-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="e8981-188">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-188">**Workaround**</span></span>  
> <span data-ttu-id="e8981-189">名前を渡すことを確認してください、`InitializeDatabaseConnection`メソッドと一致するユーザー プロファイル、メンバーシップ データベース内のテーブルまたはことを確認して、`autoCreateTables`パラメーターが false に設定します。</span><span class="sxs-lookup"><span data-stu-id="e8981-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="e8981-190">問題点: エラー メッセージ"Admin モジュールには、~/App へのアクセスが必要な\_データ"</span><span class="sxs-lookup"><span data-stu-id="e8981-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="e8981-191">状況によっては、ユーザーを作成またはそれ以外の場合、ASP.NET メンバーシップ システムを使用しようとしている可能性があります、エラーを表示するページ*Admin モジュールでは、~/App へのアクセスを必要と\_データ*です。</span><span class="sxs-lookup"><span data-stu-id="e8981-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="e8981-192">これで、IIS または IIS Express が実行されているアカウントが作成およびへの書き込みアクセス許可を持たない場合に発生、*アプリ\_データ*web サイトのルートの下のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e8981-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="e8981-193">**回避策**を手動で作成、*アプリ\_データ*web サイトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e8981-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="e8981-194">(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントに、アプリケーションのルート フォルダーとサブフォルダー アプリなどの読み取り/書き込みアクセス許可があるかどうかを確認し、\_データ。</span><span class="sxs-lookup"><span data-stu-id="e8981-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="e8981-195">詳細については、サポート技術情報の記事で使用できる[問題と、SQL Server Express ユーザー インスタンスの ASP.net Web アプリケーション プロジェクト](https://support.microsoft.com/kb/2002980)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="e8981-196">問題:"を SQL Server のユーザー インスタンスを生成できませんでした。</span><span class="sxs-lookup"><span data-stu-id="e8981-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="e8981-197">WebMatrix Web アプリケーションが SQL Server Express を使用して、Windows 7 または Windows Server 2008 R2 に IIS 7.5 を実行している場合は、SQL Server が実行時にユーザーのローカル アプリケーション パスを取得できないことを示すエラーを参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e8981-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="e8981-198">**回避策**(通常は NETWORK SERVICE) でアプリケーションを実行する Windows アカウントがアプリケーションのルート フォルダーとサブフォルダーの読み取り/書き込みアクセス許可をなどがあるかどうかを確認*アプリ\_データ*.</span><span class="sxs-lookup"><span data-stu-id="e8981-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="e8981-199">詳細については、サポート技術情報の記事で使用できる[問題と、SQL Server Express ユーザー インスタンスの ASP.net Web アプリケーション プロジェクト](https://support.microsoft.com/kb/2002980)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="e8981-200">問題点: パッケージ マネージャーのリソースまたはパッケージ マネージャーのパスワードを含むファイルは IIS 6.0 で含んだおよびそれ以前</span><span class="sxs-lookup"><span data-stu-id="e8981-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="e8981-201">RC2 リリースを使用してビルドされた ASP.NET Web Pages (Razor) アプリケーションを展開する場合と、アプリケーションが含まれている場合、 *password.txt*または*packagesources.txt*下にあるファイル */App\_データ管理/*、IIS 6.0 が要求されると、パッケージ マネージャー インスタンスのパスワードを公開する可能性がある場合、ファイルにサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="e8981-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="e8981-202">**回避策**の名前を変更、 *password.txt*または*packagesources.txt*ファイルの名前を*password.config*または*packagesources.configに変更*.既定では、IIS 6.0 ファイルを処理しませんが、 *.config*拡張機能です。</span><span class="sxs-lookup"><span data-stu-id="e8981-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="e8981-203">(IIS 7 でのファイルはありません、*アプリ\_データ*フォルダーが提供されるは、ので、ファイルの名前を変更する必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="e8981-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="e8981-204">問題: Beta 3 リリースを使用してインストールされているパッケージをアンインストールする完全に削除されませんパッケージのコンポーネント</span><span class="sxs-lookup"><span data-stu-id="e8981-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="e8981-205">ベータ 3 リリースでは、パッケージ マネージャーを使用してパッケージをインストールしてから、現在のリリースを使用してアンインストールを再試行してください。 パッケージは完全にアンインストールされません。</span><span class="sxs-lookup"><span data-stu-id="e8981-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="e8981-206">パッケージ マネージャーの**アンインストール**ボタンの一部のコンポーネントを削除しますが、パッケージのライブラリ コードなりは更新されません、 *package.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="e8981-207">**回避策** </span><span class="sxs-lookup"><span data-stu-id="e8981-207">**Workaround** </span></span>  
> <span data-ttu-id="e8981-208">次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="e8981-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="e8981-209">削除、*アプリ\_Data\packages*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e8981-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="e8981-210">これには、すべてのパッケージを削除します。</span><span class="sxs-lookup"><span data-stu-id="e8981-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="e8981-211">削除、 *packages.config* web サイトのルートにあるファイルです。</span><span class="sxs-lookup"><span data-stu-id="e8981-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="e8981-212">問題: Visual Studio で、web ベースのパッケージ マネージャーを呼び出すアプリケーションがオフライン、</span><span class="sxs-lookup"><span data-stu-id="e8981-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="e8981-213">Visual Studio (WebMatrix ではなく) で作業しているし、使用するかどうか、  *\_admin*はアプリケーションをオフラインし、パッケージ マネージャー Visual Studio を起動する機能、*アプリ\_offline.htm*を web サイトのルートに、パッケージ マネージャーを使用する機能を中断されます。</span><span class="sxs-lookup"><span data-stu-id="e8981-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="e8981-214">最も多く、web ベースのパッケージ マネージャー インターフェイスを使用するときに、この動作を表示、動作は同じ場合、追加、削除、または内のファイルを変更、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e8981-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="e8981-215">**回避策** </span><span class="sxs-lookup"><span data-stu-id="e8981-215">**Workaround** </span></span>  
> <span data-ttu-id="e8981-216">Visual Studio でパッケージを使用するには、web ベースのパッケージ マネージャーではなく、NuGet 拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="e8981-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="e8981-217">詳細については、次を参照してください。、 [NuGet のドキュメント](https://docs.microsoft.com/nuget/)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="e8981-218">その他のファイルで作業している場合、*アプリ\_データ*フォルダーで、この問題を回避する別の場所でファイルのままにしてください。</span><span class="sxs-lookup"><span data-stu-id="e8981-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="e8981-219">実用的でない場合は、削除、*アプリ\_offline.htm*ファイルを手動でまたは自動的に (既定では、30 秒後)、サイトがオンラインに戻るまで待機します。</span><span class="sxs-lookup"><span data-stu-id="e8981-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="e8981-220">問題: Visual Studio IntelliSense およびプロジェクトのテンプレート ASP.NET MVC version 3 でのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="e8981-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="e8981-221">ASP.NET Web Pages をインストールするもインストールしません tools for Visual Studio の ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートなど。</span><span class="sxs-lookup"><span data-stu-id="e8981-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="e8981-222">**回避策**Visual Studio での ASP.NET Web Pages アプリケーション用の IntelliSense およびプロジェクトのテンプレートを使用するインストール ASP.NET MVC 3 RC、Web Platform Installer を使用するか、または[スタンドアロン インストーラー](https://go.microsoft.com/fwlink/?LinkID=191797)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="e8981-223">問題: フィードやプロキシ サーバーを経由して他の外部データの読み取り</span><span class="sxs-lookup"><span data-stu-id="e8981-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="e8981-224">サイトを実行しているサーバーがプロキシ サーバーの背後にある場合でプロキシ情報を構成する必要があります、 *web.config*サイトの外部から来る情報を読み取ることができるためにファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="e8981-225">たとえば、使用する場合、`ReCaptcha`ヘルパーに渡し、ヘルパーは、reCAPTCHA サービスと通信が、プロキシ サーバーによってブロックされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e8981-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="e8981-226">同様に、パッケージ マネージャーによって使用されているフィードなど ASP.NET Web Pages で使用されるフィードでは、プロキシの構成が必要です。</span><span class="sxs-lookup"><span data-stu-id="e8981-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="e8981-227">外部サービスを扱うやパッケージ フィードの操作で問題が発生する場合は、アプリケーションのルートに、次の要素を格納*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="e8981-228">プロキシ サーバーの構成の詳細については、次を参照してください。 [&lt;プロキシ&gt;要素 (ネットワーク設定)](https://msdn.microsoft.com/library/sa91de1e.aspx) MSDN Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="e8981-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="e8981-229">問題点:、Razor 構文を使用して ASP.NET Web Pages が無効になります、.NET Framework version 4 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="e8981-230">.NET Framework version 4 をアンインストールし、再インストールして、Razor 構文を使用して ASP.NET Web Pages は無効になります。</span><span class="sxs-lookup"><span data-stu-id="e8981-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="e8981-231">ページ、 *.cshtml*拡張機能が正しく動作しないことです。</span><span class="sxs-lookup"><span data-stu-id="e8981-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="e8981-232">ASP.NET Web ページで、マシン ルート アセンブリが登録*web.config*ファイル、および .NET Framework の削除は、そのファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="e8981-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="e8981-233">.NET Framework を再インストール、構成ファイルの新しいバージョンがインストールされますが、ASP.NET Web Pages アセンブリの参照を追加できません。</span><span class="sxs-lookup"><span data-stu-id="e8981-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="e8981-234">**回避策**.NET Framework を再インストール後に Razor 構文を使用する ASP.NET Web Pages を再インストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="e8981-235">次の要素を追加、 *web.config*マシン ルートでは、次の場所には、通常のファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="e8981-236">問題点: 拡張子のない Url が見つからない IIS 7 や IIS 7.5 で.cshtml/.vbhtml ファイル</span><span class="sxs-lookup"><span data-stu-id="e8981-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="e8981-237">IIS 7 や IIS 7.5 では、次のように URL を使用して要求ができませんがページを検索する、 *.cshtml*または *.vbhtml*拡張機能。</span><span class="sxs-lookup"><span data-stu-id="e8981-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="e8981-238">URL 書き換えが有効でないため既定の IIS 7 や IIS 7.5、問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="e8981-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="e8981-239">考えられるシナリオは IIS Express を使用してローカルでテストするときに問題は表示されないホスティング web サイトに web サイトを展開するときに発生します。</span><span class="sxs-lookup"><span data-stu-id="e8981-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="e8981-240">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="e8981-241">サーバー コンピューターを制御する場合は、サーバー コンピューターの更新プログラムをインストールに記載されている[、更新プログラムを処理する IIS 7.0 または IIS 7.5 のハンドラーの要求の Url を特定の有効期間で終わっていないことがある](https://support.microsoft.com/kb/980368)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="e8981-242">サーバー コンピューターの制御がないかどうか (たとえばを展開しているホスティング web サイトに) 次の web サイトの追加*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="e8981-243">問題点: SQL Server Compact がインストールされていないコンピューターにアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="e8981-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="e8981-244">SQL Server Compact データベースを含むアプリケーションは、SQL Server Compact がインストールされていないコンピューターで実行できます。</span><span class="sxs-lookup"><span data-stu-id="e8981-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="e8981-245">Microsoft WebMatrix 1.0 は自動的にするようなバイナリをコピーし、適切な実行*web.config*ファイルの変換。</span><span class="sxs-lookup"><span data-stu-id="e8981-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="e8981-246">**回避策**これらのファイルをコピーしする必要がある場合、 *web.config*ファイルの変更を手動で、次の操作します。</span><span class="sxs-lookup"><span data-stu-id="e8981-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="e8981-247">アセンブリをコピーする、データベース エンジンに、 *Bin*ターゲット コンピューター上のアプリケーションのフォルダー (およびサブフォルダー)。</span><span class="sxs-lookup"><span data-stu-id="e8981-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="e8981-248">コピー *C:\Program files \microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="e8981-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="e8981-249">**to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="e8981-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="e8981-250">コピー <em>C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>に</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="e8981-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="e8981-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="e8981-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="e8981-252">Web サイトのルート フォルダーに作成または開く、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="e8981-253">(WebMatrix 1.0 では、このファイルの種類がクリックした場合に使用可能な**すべて**で、**ファイルの種類を選択** ダイアログ ボックス)。</span><span class="sxs-lookup"><span data-stu-id="e8981-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="e8981-254">次の要素の子として追加、`<configuration>`要素 (内部ではなく、`<system.web>`要素)。</span><span class="sxs-lookup"><span data-stu-id="e8981-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="e8981-255">問題: Visual Basic で中程度の信頼で「データベース」と"の WebGrid"ヘルパーは機能しません</span><span class="sxs-lookup"><span data-stu-id="e8981-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="e8981-256">Visual Basic を使用している場合 (作成 *.vbhtml*ファイル)、`Database`と`WebGrid`中程度の信頼を使用する、アプリケーションが設定されている場合、ヘルパーは機能しません。</span><span class="sxs-lookup"><span data-stu-id="e8981-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="e8981-257">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-257">**Workaround**</span></span>  
> <span data-ttu-id="e8981-258">Visual Studio 2010 を使用する場合は、Service Pack 1 リリースをインストールすることによってこの問題を解決できます。</span><span class="sxs-lookup"><span data-stu-id="e8981-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="e8981-259">SP1 からのベータ版をダウンロードするには SP1 のリリースの最終バージョンを使用するまで、 [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) Microsoft ダウンロード センターのページです。</span><span class="sxs-lookup"><span data-stu-id="e8981-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="e8981-260">これは実用的ではないかを一時的に Visual Studio 2010 を使用しない場合は、完全な信頼を使用するアプリケーションを設定します。</span><span class="sxs-lookup"><span data-stu-id="e8981-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="e8981-261">問題:"ApplicationPart"のリソースが外部からアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="e8981-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="e8981-262">アセンブリから派生したオブジェクトに含まれる場合、`ApplicationPart`クラス、アセンブリのリソースがによって公開されること、`ResourceRouteHandler`クラスです。</span><span class="sxs-lookup"><span data-stu-id="e8981-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="e8981-263">たとえば、次のような URL があるとします。</span><span class="sxs-lookup"><span data-stu-id="e8981-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="e8981-264">この要求は、リソースの文字列のすべてをダウンロード、 *System.Web.WebPages.Administration.dll*アセンブリ。</span><span class="sxs-lookup"><span data-stu-id="e8981-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="e8981-265">すべての埋め込みリソース (も含めてを静的なコンテンツとして提供するものではありません) がダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="e8981-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="e8981-266">埋め込みリソースに機密情報が含まれている場合は、セキュリティ上のリスクを表すこのことができます。</span><span class="sxs-lookup"><span data-stu-id="e8981-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="e8981-267">**回避策** </span><span class="sxs-lookup"><span data-stu-id="e8981-267">**Workaround** </span></span>  
> <span data-ttu-id="e8981-268">作成する場合、 **ApplicationPart**オブジェクトを埋め込みリソースに関連付けられているかどうかを確認**ApplicationPart**オブジェクトのアセンブリには機密情報は含まれません。</span><span class="sxs-lookup"><span data-stu-id="e8981-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="e8981-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e8981-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="e8981-270">WebMatrix のインストールに関する問題については、次を参照してください。 [WebMatrix のインストールに関する問題](#Known_Issues_Installation)このドキュメントで既に説明します。</span><span class="sxs-lookup"><span data-stu-id="e8981-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="e8981-271">ドキュメントのこのセクションでは、WebMatrix 開発環境の既知の問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="e8981-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="e8981-272">問題: データベース ワークスペースで、ユーザー名または web.config ファイル内のデータベース接続文字列のパスワードの変更は反映されません。</span><span class="sxs-lookup"><span data-stu-id="e8981-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="e8981-273">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="e8981-274">*Web.config*ファイル、接続文字列でデータベース名を変更 (たとえば、「1」を追加) します。</span><span class="sxs-lookup"><span data-stu-id="e8981-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="e8981-275">保存、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="e8981-276">をクリックして**データベース**および更新します。</span><span class="sxs-lookup"><span data-stu-id="e8981-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="e8981-277">内の接続文字列でデータベース名を変更、 *web.config*ファイルを再度元のデータベース名。</span><span class="sxs-lookup"><span data-stu-id="e8981-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="e8981-278">保存、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="e8981-279">をクリックして**データベース**および更新します。</span><span class="sxs-lookup"><span data-stu-id="e8981-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="e8981-280">問題点: WebMatrix によって作成されたフォルダーを削除できません。</span><span class="sxs-lookup"><span data-stu-id="e8981-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="e8981-281">WebMatrix が高度な権限で実行されている場合 (WebMatrix を使用してを起動するは、**管理者として実行**Windows のオプション)、Windows エクスプ ローラーを使用して WebMatrix によって作成されるフォルダーを削除することはできません。</span><span class="sxs-lookup"><span data-stu-id="e8981-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="e8981-282">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-282">**Workaround**</span></span>  
> <span data-ttu-id="e8981-283">高度な権限を使用して Windows エクスプ ローラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="e8981-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="e8981-284">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e8981-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="e8981-285">Windows では、をクリックして**開始**です。</span><span class="sxs-lookup"><span data-stu-id="e8981-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="e8981-286">"Windows Explorer"を入力しのエントリを右クリックして**Windows エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="e8981-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="e8981-287">をクリックして**管理者として実行**です。</span><span class="sxs-lookup"><span data-stu-id="e8981-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="e8981-288">フォルダーを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="e8981-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="e8981-289">問題: WebMatrix 1.0 は、昇格が必要な特定のタスクを実行することが</span><span class="sxs-lookup"><span data-stu-id="e8981-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="e8981-290">WebMatrix 1.0 では、次の状況で追加のコンポーネントのインストールなどの昇格が必要な特定のタスクを実行できます。</span><span class="sxs-lookup"><span data-stu-id="e8981-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="e8981-291">Windows Vista または Windows 7 を管理者特権を持たないアカウントでログオンして、ユーザー アカウント制御 (UAC) が無効になっています。</span><span class="sxs-lookup"><span data-stu-id="e8981-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="e8981-292">Microsoft Windows XP または Microsoft Windows Server 2003 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="e8981-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="e8981-293">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-293">**Workaround**</span></span>  
> <span data-ttu-id="e8981-294">WebMatrix 1.0 のほとんどのタスクでは、管理アクセス許可は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="e8981-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="e8981-295">ような状況に、管理者は、操作を実行したり、これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e8981-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="e8981-296">Windows Vista または Windows 7 では、UAC を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e8981-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="e8981-297">Windows xp では、Administrators セキュリティ グループにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="e8981-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="e8981-298">問題:「サイトから Web ギャラリー」が無効になっています</span><span class="sxs-lookup"><span data-stu-id="e8981-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="e8981-299">**Web ギャラリーからサイト**オプションには、Web Platform Installer 3.0 がインストールされていない場合は無効になります。</span><span class="sxs-lookup"><span data-stu-id="e8981-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="e8981-300">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-300">**Workaround**</span></span>  
> <span data-ttu-id="e8981-301">インストール、 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="e8981-302">問題点: Google Chrome は、実行オプションとして使用できません。</span><span class="sxs-lookup"><span data-stu-id="e8981-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="e8981-303">Google Chrome が下にあるブラウザーの一覧に表示されない**実行**上、**ホーム**タブです。</span><span class="sxs-lookup"><span data-stu-id="e8981-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="e8981-304">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-304">**Workaround**</span></span>  
> <span data-ttu-id="e8981-305">Google Chrome の一部のバージョンに登録しないで自体正しく、Windows の既定のプログラム機能します。</span><span class="sxs-lookup"><span data-stu-id="e8981-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="e8981-306">この問題を回避するには、Google Chrome を開始 をクリックして、*カスタマイズおよび制御 Google Chrome*  メニューのをクリックして*オプション*、順にクリック*Make Google Chrome の既定ブラウザー*です。</span><span class="sxs-lookup"><span data-stu-id="e8981-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="e8981-307">問題:「外部キー」 ダイアログ ボックス許可しない、プライマリ キーを入力</span><span class="sxs-lookup"><span data-stu-id="e8981-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="e8981-308">**Foreign Key**  ダイアログ ボックス主キー テーブルからの主キーの名前を入力はできません。</span><span class="sxs-lookup"><span data-stu-id="e8981-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="e8981-309">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-309">**Workaround**</span></span>  
> <span data-ttu-id="e8981-310">これは意図的なものであり、</span><span class="sxs-lookup"><span data-stu-id="e8981-310">This is intentional.</span></span> <span data-ttu-id="e8981-311">主キー テーブルの主キーの名前を入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e8981-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="e8981-312">問題点: IntelliSense では使用できません WebMatrix Razor 構文、C# の場合、または Visual Basic です。</span><span class="sxs-lookup"><span data-stu-id="e8981-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="e8981-313">WebMatrix で、HTML および CSS 用の IntelliSense がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e8981-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="e8981-314">ただし、他の言語の使用はできません。</span><span class="sxs-lookup"><span data-stu-id="e8981-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="e8981-315">**回避策** </span><span class="sxs-lookup"><span data-stu-id="e8981-315">**Workaround** </span></span>  
> <span data-ttu-id="e8981-316">なし。</span><span class="sxs-lookup"><span data-stu-id="e8981-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="e8981-317">問題点: HTML および CSS 用の IntelliSense の提案は適切なコンテキストではない要素</span><span class="sxs-lookup"><span data-stu-id="e8981-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="e8981-318">WebMatrix 内のマークアップの IntelliSense サポートを使用して HTML、 [XHTML 1.0 Transitional スキーマ](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional)CSS を使用して、 [CSS 2.1 スキーマ](http://www.w3.org/TR/CSS2/)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="e8981-319">IntelliSense は、これらの特定のスキーマに基づいているため特定のタグ、属性、またはプロパティ可能性がありますが提案するには不適切な現在のページまたはスタイルの定義。</span><span class="sxs-lookup"><span data-stu-id="e8981-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="e8981-320">HTML、可能性がありますと見なされます (たとえば、タグが閉じられていません) の形式が正しくない XHTML コンテンツでは予期しない提案する可能性がもできます。</span><span class="sxs-lookup"><span data-stu-id="e8981-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="e8981-321">この問題は、カーソルが不完全なタグ; 内にある場合に顕著する可能性があります。その場合は、IntelliSense は、新しいタグを開始を提案したり、他の不適切なヒントを表示することがします。</span><span class="sxs-lookup"><span data-stu-id="e8981-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="e8981-322">**回避策** </span><span class="sxs-lookup"><span data-stu-id="e8981-322">**Workaround** </span></span>  
> <span data-ttu-id="e8981-323">HTML、適切な形式で完全な XHTML ページ内で作業していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e8981-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="e8981-324">CSS、回避策はありません。</span><span class="sxs-lookup"><span data-stu-id="e8981-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="e8981-325">問題点: 入力したときに IntelliSense は呼び出されません</span><span class="sxs-lookup"><span data-stu-id="e8981-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="e8981-326">ときに、IntelliSense 可能性がありますは呼び出されません HTML または CSS がエディターで入力されているとします。</span><span class="sxs-lookup"><span data-stu-id="e8981-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="e8981-327">具体的には、この操作は、カーソルが別の要素の横にあるか、ファイルの最後にします。</span><span class="sxs-lookup"><span data-stu-id="e8981-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="e8981-328">**回避策** </span><span class="sxs-lookup"><span data-stu-id="e8981-328">**Workaround** </span></span>  
> <span data-ttu-id="e8981-329">カーソルを前後に空白があることと、カーソルがファイルの末尾がないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e8981-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="e8981-330">Ctrl キーを押しながら Space キーを押して手動で IntelliSense を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="e8981-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="e8981-331">問題点: UI にも IntelliSense を無効にします。</span><span class="sxs-lookup"><span data-stu-id="e8981-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="e8981-332">WebMatrix 1.0 は UI またはジェスチャを IntelliSense を無効にします。</span><span class="sxs-lookup"><span data-stu-id="e8981-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="e8981-333">**回避策** </span><span class="sxs-lookup"><span data-stu-id="e8981-333">**Workaround** </span></span>  
> <span data-ttu-id="e8981-334">IntelliSense を無効にするスイッチが含まれています、次のコマンドを使用して WebMatrix を開始します。</span><span class="sxs-lookup"><span data-stu-id="e8981-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="e8981-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e8981-335">IIS Express</span></span>

<span data-ttu-id="e8981-336">IIS Express には、次の URL で使用できるは、独自の readme ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="e8981-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="e8981-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="e8981-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="e8981-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="e8981-338">SQL Server Compact</span></span>

<span data-ttu-id="e8981-339">SQL Server Compact では、次の URL で使用できるは、独自の readme ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="e8981-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="e8981-340">WebMatrix の一部として SQL Server Compact のインストールが関係する問題については、次を参照してください。 [WebMatrix のインストールに関する問題](#Known_Issues_Installation)このドキュメントで既に説明します。</span><span class="sxs-lookup"><span data-stu-id="e8981-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="e8981-341">アプリケーションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="e8981-342">問題: アプリケーションをインストールすることができます、長い時間がかかる場合は、ユーザーのマイ ドキュメント フォルダーは、ネットワーク共有にリダイレクト</span><span class="sxs-lookup"><span data-stu-id="e8981-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="e8981-343">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-343">**Workaround**</span></span>  
> <span data-ttu-id="e8981-344">なし。</span><span class="sxs-lookup"><span data-stu-id="e8981-344">None.</span></span> <span data-ttu-id="e8981-345">アプリケーションをインストールするには時間かかる場合がありますが、正常にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e8981-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="e8981-346">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="e8981-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="e8981-347">SQL Compact データベースを発行するときの問題:「必要なアクセス許可を取得できません」エラー</span><span class="sxs-lookup"><span data-stu-id="e8981-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="e8981-348">WebMatrix は完全にサポートされません中程度の信頼の構成で .NET Framework version 3.5 を実行しているサーバーへの SQL Server Compact のサポートのバイナリの展開。</span><span class="sxs-lookup"><span data-stu-id="e8981-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="e8981-349">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-349">**Workaround**</span></span>  
> <span data-ttu-id="e8981-350">推奨される回避策は、サーバーに .NET Framework 4 をインストールすることです。</span><span class="sxs-lookup"><span data-stu-id="e8981-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="e8981-351">また、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="e8981-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="e8981-352">次の要素を追加して、 `SecurityClasses` 」の「 *Web\_MediumTrust.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="e8981-353">新しいアクセス許可セットを作成、 *Web\_MediumTrust.config*次の必要なアクセス許可を持つファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="e8981-354">次の要素を配置することにより、SQL Server Compact に設定するアクセス許可の適用、 *Web\_MediumTrust.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e8981-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="e8981-355">発行後に問題点: ギャラリー、PhpBB の web アプリケーションの表示「サービスは利用できません」エラー</span><span class="sxs-lookup"><span data-stu-id="e8981-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="e8981-356">状況によっては、アプリケーションを公開すると、「サービスは利用できません」エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="e8981-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="e8981-357">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-357">**Workaround**</span></span>  
> <span data-ttu-id="e8981-358">WebMatrix で、円記号を追加 (\)でサーバー名の末尾に、**発行設定**ウィンドウからもう一度アプリケーションを発行するとします。</span><span class="sxs-lookup"><span data-stu-id="e8981-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="e8981-359">問題点: Moodle web サイトのレイアウトとリンクが破損している発行した後</span><span class="sxs-lookup"><span data-stu-id="e8981-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="e8981-360">Moodle アプリケーションを発行した後、アプリケーションが正しく機能しません。</span><span class="sxs-lookup"><span data-stu-id="e8981-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="e8981-361">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-361">**Workaround**</span></span>  
> <span data-ttu-id="e8981-362">WebMatrix での末尾にスラッシュ (/) を追加、**サイト名**フィールドで、**発行設定**ウィンドウからもう一度アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="e8981-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="e8981-363">問題点: nopCommerce を発行が失敗し、データベース エラー</span><span class="sxs-lookup"><span data-stu-id="e8981-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="e8981-364">発行 nopCommerce は失敗しのようなデータベース エラーの報告"、nop 挿入\_ログ table が失敗しました"。</span><span class="sxs-lookup"><span data-stu-id="e8981-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="e8981-365">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="e8981-366">クリックして WebMatrix で**実行**nopCommerce をローカルでを起動します。</span><span class="sxs-lookup"><span data-stu-id="e8981-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="e8981-367">[管理] ページにログインします。</span><span class="sxs-lookup"><span data-stu-id="e8981-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="e8981-368">クリックして、**システム**メニュー。</span><span class="sxs-lookup"><span data-stu-id="e8981-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="e8981-369">クリックして、**ログ**オプション。</span><span class="sxs-lookup"><span data-stu-id="e8981-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="e8981-370">クリックして、**ログの消去**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e8981-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="e8981-371">NopCommerce をもう一度公開します。</span><span class="sxs-lookup"><span data-stu-id="e8981-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="e8981-372">問題点: Silverstripe CMS エラーが表示される、"HTTP 500 PHP FCGI"発行されたサイトをダウンロードするときに</span><span class="sxs-lookup"><span data-stu-id="e8981-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="e8981-373">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-373">**Workaround**</span></span>  
> <span data-ttu-id="e8981-374">クリックした後**ダウンロード サイトに発行**、スキップ`silverstripe-cache/manifest_main`で**公開プレビュー**です。</span><span class="sxs-lookup"><span data-stu-id="e8981-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="e8981-375">このファイルは、キャッシュの目的で使用され、各コンピューターに固有です。</span><span class="sxs-lookup"><span data-stu-id="e8981-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="e8981-376">問題点: サブ文字列が表示されます「サーバー エラーは '/' アプリケーションの」発行されたサイトをダウンロードするとき</span><span class="sxs-lookup"><span data-stu-id="e8981-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="e8981-377">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-377">**Workaround**</span></span>  
> <span data-ttu-id="e8981-378">サイトの開く*web.config*ファイルを開き、SQL Server の管理者資格情報 ("sa"の資格情報) でユーザー ID と、データベース接続文字列にパスワードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e8981-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="e8981-379">また、次の手順をログインで使用するユーザー アカウントを指定するために`db_owner`アクセス許可。</span><span class="sxs-lookup"><span data-stu-id="e8981-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="e8981-380">Web Platform Installer を使用して SQL Server Management Studio をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="e8981-381">ローカルの SQL Server Express インスタンスに接続 (既定では、 `.\SQLEXPRESS`)。</span><span class="sxs-lookup"><span data-stu-id="e8981-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="e8981-382">をクリックして**データベース** &gt; *[localSubtextDatabase]* &gt; **セキュリティ** &gt; **ユーザー**&gt; *[localSubtextUser*] (既定値は`subtextuser`] を右クリックし、クリック**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="e8981-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="e8981-383">選択**db\_所有者**ロール メンバーシップ セクションでします。</span><span class="sxs-lookup"><span data-stu-id="e8981-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="e8981-384">問題:"リンク先の URL"フィールドは、プレフィックスが付いていない http:// または https:// 場合の発行後にサイトが機能しません。</span><span class="sxs-lookup"><span data-stu-id="e8981-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="e8981-385">**公開設定**ダイアログ ボックスで、リンク先の URL で始まらない場合`http://`または`https://`展開後に、サイトが動かないことがあります。</span><span class="sxs-lookup"><span data-stu-id="e8981-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="e8981-386">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-386">**Workaround**</span></span>  
> <span data-ttu-id="e8981-387">確認して、サイトにある送信先 URL を発行する前に、**発行設定** ダイアログ ボックスが始まる`http://`または`https://`です。</span><span class="sxs-lookup"><span data-stu-id="e8981-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="e8981-388">問題点: MySQL データベースのパブリッシュ エラーで失敗する、"データベースを発行できませんでした。</span><span class="sxs-lookup"><span data-stu-id="e8981-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="e8981-389">これは、場合に発生、リモート データベースは、スクリプトを実行できません。"</span><span class="sxs-lookup"><span data-stu-id="e8981-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="e8981-390">さまざまな理由から、エラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="e8981-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="e8981-391">このエラーを発生する理由の 1 つは、データベース スクリプトには、単一引用符文字 (') が含まれているとなり、対象の MySQL データベースの既定の文字セットは utf-8 です。</span><span class="sxs-lookup"><span data-stu-id="e8981-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="e8981-392">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-392">**Workaround**</span></span>  
> <span data-ttu-id="e8981-393">既定の文字を utf-8 に、リモートの MySQL データベースのセットを設定します。</span><span class="sxs-lookup"><span data-stu-id="e8981-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="e8981-394">問題点: 一部のリンクが表示されない DotNetNuke の発行や、サイトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="e8981-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="e8981-395">パブリッシュまたは DotNetNuke サイトをダウンロードする場合は、サイトに表示する新しいリンクを取得するキャッシュをクリアする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8981-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="e8981-396">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="e8981-397">"Host"としてログインします。</span><span class="sxs-lookup"><span data-stu-id="e8981-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="e8981-398">[ホスト] メニューに移動し、選択**ホスト設定**です。</span><span class="sxs-lookup"><span data-stu-id="e8981-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="e8981-399">スクロール ダウンし、**詳細設定**、展開**パフォーマンス設定**です。</span><span class="sxs-lookup"><span data-stu-id="e8981-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="e8981-400">クリックして、**キャッシュのクリア**ページへのリンク。</span><span class="sxs-lookup"><span data-stu-id="e8981-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="e8981-401">ページの下部に移動し、アプリケーションを再起動します。</span><span class="sxs-lookup"><span data-stu-id="e8981-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="e8981-402">問題: AtomSite で一部のリンクが壊れている発行されたサイトをダウンロードした後</span><span class="sxs-lookup"><span data-stu-id="e8981-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="e8981-403">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-403">**Workaround**</span></span>  
> <span data-ttu-id="e8981-404">*Service.config*ファイル、 *users.config*ファイル、およびすべて *.xml*ファイル、URL 文字列を置換する (たとえば、 `http://myhost.com/atomsite`) ローカル サイトの (たとえば、 `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="e8981-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="e8981-405">発行し、データベース エラーを報告する問題点: WordPress と同様に、MySQL ベースのアプリケーションが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e8981-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="e8981-406">既定では、WebMatrix は、utf-8 文字セットと MySQL をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="e8981-407">MySQL をインストールするには、独自の文字セットが utf-8 ではない場合 (たとえば、これがラテン語-1 の場合)、データベースの発行プロセスが失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e8981-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="e8981-408">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="e8981-409">MySQL を utf-8 の文字セットを変更します。</span><span class="sxs-lookup"><span data-stu-id="e8981-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="e8981-410">(詳細については、「[サーバー文字セットと照合順序](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html)MySQL web サイトです)。</span><span class="sxs-lookup"><span data-stu-id="e8981-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="e8981-411">アプリケーションを再インストールします。</span><span class="sxs-lookup"><span data-stu-id="e8981-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="e8981-412">アプリケーションを再発行します。</span><span class="sxs-lookup"><span data-stu-id="e8981-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="e8981-413">問題:「ダウンロード サイトに発行」セットアップのブラウザー ベースのアプリケーションが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e8981-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="e8981-414">一部のアプリケーション (たとえば、Kentico CMS) では、データベースを作成するなど、インストール後セットアップを実行するために、ブラウザーで起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8981-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="e8981-415">ブラウザー ベースのセットアップを完了しなくても次のようにアプリケーションを発行すると、リモート サーバーから、同じサイトをダウンロードしようとしましたが失敗します。</span><span class="sxs-lookup"><span data-stu-id="e8981-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="e8981-416">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-416">**Workaround**</span></span>  
> <span data-ttu-id="e8981-417">サイトを発行する前に、ブラウザー ベースのセットアップを完了します。</span><span class="sxs-lookup"><span data-stu-id="e8981-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="e8981-418">問題:「ダウンロード サイトに発行」DotNetNuke および Kooboo CMS がデータベース エラーで失敗します。</span><span class="sxs-lookup"><span data-stu-id="e8981-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="e8981-419">サーバーからアプリケーションをダウンロードしようとして、内のデータベース接続文字列に管理者の資格情報があるかどうか、**発行設定**ダイアログ ボックスで、発行は、ログに次のエラーを参照してください可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e8981-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="e8981-420">**回避策**</span><span class="sxs-lookup"><span data-stu-id="e8981-420">**Workaround**</span></span>  
> <span data-ttu-id="e8981-421">可能であれば、サイトを再パブリッシュ (または公開する)、データベースに対する非管理者の資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="e8981-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="e8981-422">参照項目</span><span class="sxs-lookup"><span data-stu-id="e8981-422">For More Information</span></span>

<span data-ttu-id="e8981-423">WebMatrix 1.0 の詳細については、次の web サイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e8981-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="e8981-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="e8981-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="e8981-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e8981-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="e8981-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="e8981-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="e8981-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="e8981-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="e8981-428">All Rights Reserved.</span><span class="sxs-lookup"><span data-stu-id="e8981-428">All Rights Reserved.</span></span> <span data-ttu-id="e8981-429">[利用規約](https://msdn.microsoft.cos/cc300389.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="e8981-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
