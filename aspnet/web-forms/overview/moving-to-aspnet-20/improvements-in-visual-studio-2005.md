---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Visual Studio 2005 の強化 |Microsoft Docs
author: microsoft
description: Visual Studio 2005 では、Web アプリケーション開発者の改善と Web プロジェクトの機能強化の長いリストを提供します。
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 0a42699381fd326891898e01b4e98662e9ce22bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811371"
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="1e987-103">Visual Studio 2005 の機能強化</span><span class="sxs-lookup"><span data-stu-id="1e987-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="1e987-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1e987-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1e987-105">Visual Studio 2005 では、Web アプリケーション開発者の改善と Web プロジェクトの機能強化の長いリストを提供します。</span><span class="sxs-lookup"><span data-stu-id="1e987-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="1e987-106">Visual Studio 2005 では、Web アプリケーション開発者の改善と Web プロジェクトの機能強化の長いリストを提供します。</span><span class="sxs-lookup"><span data-stu-id="1e987-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="1e987-107">Visual Studio .NET 2002 および 2003 は、強力なありました多く Web プロジェクトが処理された方法でします。</span><span class="sxs-lookup"><span data-stu-id="1e987-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="1e987-108">Visual Studio 2005 では、これらの苦情に対処するために、多数の新しい機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1e987-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="1e987-109">Visual Studio .NET 2003 Web アプリケーションのコンパイルの処理方法を好む方を参照してください。 [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870)します。</span><span class="sxs-lookup"><span data-stu-id="1e987-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="1e987-110">このモジュールでは、Web プロジェクトの作成、管理、および開発の強化をについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1e987-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="1e987-111">後のモジュールでは、Web プロジェクトのビルドと配置の機能強化をについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1e987-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="1e987-112">FrontPage Server Extensions</span><span class="sxs-lookup"><span data-stu-id="1e987-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="1e987-113">Visual Studio .NET 2002 および 2003 の作成または Web プロジェクトをビルドするために、ボックスの FrontPage Server Extensions が必要です。</span><span class="sxs-lookup"><span data-stu-id="1e987-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="1e987-114">開発者は 2 つの異なるアクセス モード (FrontPage Server Extensions またはファイルへのアクセス モード) のどちらかがでした、両方は FrontPage Server Extensions を使用すると、IIS、その他のアプリケーション ルートの設定などのタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="1e987-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="1e987-115">Visual Studio 2005 では、ローカル プロジェクトでは、FrontPage Server Extensions への依存を削除します。</span><span class="sxs-lookup"><span data-stu-id="1e987-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="1e987-116">Visual Studio 2005 は、FrontPage Server Extensions を使用する代わりに、直接 IIS メタベースを今すぐにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="1e987-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="1e987-117">Visual Studio 2005 では、これは FrontPage Server Extensions を必要とせず、プロジェクトのリモート アクセスの許可 FTP サポートも追加します。</span><span class="sxs-lookup"><span data-stu-id="1e987-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="1e987-118">開発者にとって、プロジェクトで FrontPage Server Extensions を使用する、オプションは引き続き使用できます。</span><span class="sxs-lookup"><span data-stu-id="1e987-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="1e987-119">ただし、ASP.NET 開発者のコミュニティからの強力なフィードバックに基づいて、その必要はありませんです。</span><span class="sxs-lookup"><span data-stu-id="1e987-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-120">FrontPage Server Extensions は、リモートのプロジェクトの作成、開始なども必要です。</span><span class="sxs-lookup"><span data-stu-id="1e987-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="1e987-121">ASP.NET 開発サーバー</span><span class="sxs-lookup"><span data-stu-id="1e987-121">ASP.NET Development Server</span></span>

<span data-ttu-id="1e987-122">ASP.NET 開発サーバーと呼ばれる新しい Web サーバーと、visual Studio 2005 に同梱されています。</span><span class="sxs-lookup"><span data-stu-id="1e987-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="1e987-123">(この Web サーバーが以前 Cassini。)</span><span class="sxs-lookup"><span data-stu-id="1e987-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="1e987-124">ASP.NET 開発サーバーのいくつかの利点があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="1e987-125">開発とデバッグ、Web サーバーに対して管理者以外のことはようになりました。</span><span class="sxs-lookup"><span data-stu-id="1e987-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="1e987-126">仮想ディレクトリは、任意の場所に柔軟なプロジェクトの場所にファイル システムで、ASP.NET 開発サーバーによって動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="1e987-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="1e987-127">Windows XP Professional 上の IIS が既に使用しているユーザーはファイルまたはフォルダーの構造の既定の Web サイトに IIS には影響しませんが、新しい Web アプリケーションを作成することになります。</span><span class="sxs-lookup"><span data-stu-id="1e987-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="1e987-128">ASP.NET 開発サーバーを活用するために特別な構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="1e987-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="1e987-129">ファイル システムでホストされている Web プロジェクトがデバッグしたり、参照、ときに Visual Studio 2005 は自動的に開始されている要求の処理をランダムなポートで、ASP.NET 開発サーバーのインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="1e987-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="1e987-130">詳細については、後でこのモジュールでの ASP.NET 開発サーバーで取り上げます。</span><span class="sxs-lookup"><span data-stu-id="1e987-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="1e987-131">強化されたファイルの管理</span><span class="sxs-lookup"><span data-stu-id="1e987-131">Improved File Management</span></span>

<span data-ttu-id="1e987-132">Visual Studio 2002 および 2003 の場合では、プロジェクト ファイル (.vbproj vb.net の場合) と .csproj (C#) は、Web アプリケーションのすべてのファイルに情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="1e987-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="1e987-133">ソリューション エクスプ ローラーの表示は、プロジェクト ファイル内のファイル情報に基づいています。</span><span class="sxs-lookup"><span data-stu-id="1e987-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="1e987-134">このため、ソリューション エクスプ ローラーは不正確な情報を外部エディターが使用されたケースで多くの場合、表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="1e987-135">Visual Studio 2002 および 2003 は多くの場合、ファイルの変更を上書きまたはファイルの最新バージョンが表示されません。</span><span class="sxs-lookup"><span data-stu-id="1e987-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="1e987-136">Visual Studio 2005 は、プロジェクト ファイルを使用して離れたは。</span><span class="sxs-lookup"><span data-stu-id="1e987-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="1e987-137">代わりに、その結果、プロジェクト内のファイルの正確性の表示、ディスクから直接ファイルとフォルダーの情報を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="1e987-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="1e987-138">Visual Studio 2002 および 2003 の [参照設定] フォルダーが、Web アプリケーションで実際のフォルダーを表していないため、Visual Studio 2005 もソリューション エクスプ ローラーから、[参照] フォルダーを削除します。</span><span class="sxs-lookup"><span data-stu-id="1e987-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="1e987-139">Visual Studio 2005 でプロジェクトの参照にアクセスするには、プロジェクトのプロパティ ページを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="1e987-140">Web プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="1e987-140">Creating Web Projects</span></span>

<span data-ttu-id="1e987-141">Web 開発者では、Visual Studio 2005 でプロジェクトの作成に使用できる多くの新しいオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="1e987-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="1e987-142">Web サイトは、できるようになりましたどこにでも作成、ファイル システム内としデバッグすることができます。 または新しい ASP.NET 開発サーバーを使用して参照します。</span><span class="sxs-lookup"><span data-stu-id="1e987-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="1e987-143">開発者は、FTP を使用して、新しい Web サイトを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="1e987-144">ここをクリックすると、Visual Studio 2005 で Web プロジェクトの作成のビデオ チュートリアルを表示します。</span><span class="sxs-lookup"><span data-stu-id="1e987-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="1e987-145">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="1e987-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="1e987-146">ファイル システムのプロジェクト</span><span class="sxs-lookup"><span data-stu-id="1e987-146">File System Projects</span></span>

<span data-ttu-id="1e987-147">ビデオ チュートリアルで説明するように、ローカル コンピューターまたはファイル共有経由でリモートの場所のいずれかのファイル システム上の Web サイトを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="1e987-148">ファイル システム上に作成される web サイトを参照し、ASP.NET 開発サーバーを使用してデバッグします。</span><span class="sxs-lookup"><span data-stu-id="1e987-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-149">ASP.NET 開発サーバーお客様には、いくつか混乱が生じる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="1e987-150">IISs ディレクトリ構造 (例: c:/inetpub/wwwroot) にファイル システムに Web プロジェクトを作成する場合、Visual Studio 2005 内から起動すると、ASP.NET 開発サーバーを使用して、Web サイトが参照も。</span><span class="sxs-lookup"><span data-stu-id="1e987-150">If a Web project is created on the file system in IISs directory structure (i.e. c:/inetpub/wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="1e987-151">そのため、IIS 構成 (つまり、認証メソッド) は適用されません。</span><span class="sxs-lookup"><span data-stu-id="1e987-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="1e987-152">既定の web プロジェクトがたくさん削除もでオーバーヘッドののみが含まれています、Default.aspx ページ、default.cs ファイル、およびアプリ/_Data フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1e987-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App/_Data folder.</span></span> <span data-ttu-id="1e987-153">Web.config および特別なフォルダー (つまりアプリ/(_c)) は、必要とされるために追加されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-153">The web.config and special folders (i.e. app/_code) are added as they are needed.</span></span> <span data-ttu-id="1e987-154">Web プロジェクトには、必要なフォルダー、ファイルのみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1e987-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="1e987-155">HTTP プロジェクト</span><span class="sxs-lookup"><span data-stu-id="1e987-155">HTTP Projects</span></span>

<span data-ttu-id="1e987-156">HTTP のプロジェクトには、プロジェクトのローカル IIS Web サイトまたはリモートの Web サイトに作成されるかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1e987-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="1e987-157">既定のプロジェクトの場所が`http://localhost`します。</span><span class="sxs-lookup"><span data-stu-id="1e987-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="1e987-158">[参照] ボタンをクリックする場合は、2 つの HTTP オプション: ローカル IIS サイトとリモート サイト。</span><span class="sxs-lookup"><span data-stu-id="1e987-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="1e987-159">これら 2 つのオプションの主な違いは、Web サーバーにファイルをコピーする方法と場所の選択 ダイアログ ボックスに web サイトの情報が表示される方法です。</span><span class="sxs-lookup"><span data-stu-id="1e987-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="1e987-160">ローカル IIS のオプションが、メタベースをローカル コンピューターからサイト情報を読み取るし、ファイル システムを使用してファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="1e987-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="1e987-161">FrontPage サーバー拡張機能とサイトについては、リモート サイトのオプションを使用して HTTP を使用してファイルをコピーおよび FrontPage Server Extensions の RPC を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1e987-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-162">Vs###/_tmp.htm ファイルと get/_aspx/_ver.aspx はバージョン情報を決定するのには使用されません。</span><span class="sxs-lookup"><span data-stu-id="1e987-162">The vs###/_tmp.htm file and get/_aspx/_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="1e987-163">HTTP の既定のオプションは、ローカルの IIS です。</span><span class="sxs-lookup"><span data-stu-id="1e987-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="1e987-164">このオプションは、どのサイトが利用できるかを判断する IIS メタベースとコンテンツを作成する場所を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="1e987-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="1e987-165">ツリー ビューで選択して、別のフォルダーまたは仮想ディレクトリを選択できます。</span><span class="sxs-lookup"><span data-stu-id="1e987-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="1e987-166">ことができますも、新しい仮想ディレクトリを作成、フォルダー、アプリケーションとしてのマークを付けるだけでなくこのダイアログ ボックスから既存の仮想ディレクトリを削除します。</span><span class="sxs-lookup"><span data-stu-id="1e987-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![場所 ダイアログ ボックスの選択](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="1e987-168">**図 1**: の場所 ダイアログ ボックスの選択</span><span class="sxs-lookup"><span data-stu-id="1e987-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="1e987-169">異なりをチェックする場合、Visual Studio の以前のバージョンで、 **Secure Sockets Layer を使用して**チェック ボックスをオンし、SSL 証明書が参照する URL と一致しませんで求めるかどうかは、セキュリティの警告 ダイアログが表示されます続行するようにします。</span><span class="sxs-lookup"><span data-stu-id="1e987-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="1e987-170">証明書が一致するものではない場合は、Visual Studio .NET 2003 を使用して、プロジェクトの作成は失敗します。</span><span class="sxs-lookup"><span data-stu-id="1e987-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![セキュリティ アラートに関する SSL 証明書](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="1e987-172">**図 2**: SSL 証明書に関するセキュリティの警告</span><span class="sxs-lookup"><span data-stu-id="1e987-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="1e987-173">ホスト ヘッダーに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="1e987-173">Note on Host Headers</span></span>

<span data-ttu-id="1e987-174">特定の IP にバインドされているサイトで Web アプリケーションを作成する場合は、ホスト ヘッダーが構成されていることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="1e987-175">それ以外の場合、Visual Studio では、サイトは作成`http://localhost`が、サイトを閲覧したり、IDE 内からデバッグするときに、IP アドレスは正しく解決されません。</span><span class="sxs-lookup"><span data-stu-id="1e987-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="1e987-176">リモート サイトのオプションを選択した場合、新しい Web サイトの送信先 URL を入力するように、ダイアログ ボックスを変更します。</span><span class="sxs-lookup"><span data-stu-id="1e987-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="1e987-177">有効になっている FrontPage Server Extensions をあるサーバーで URL を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="1e987-178">FrontPage Server Extensions を使用してローカル Web サーバーで動作する場合は、リモートのサイト オプションを使用し、ローカル URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="1e987-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![リモート サーバー上の Web サイトの作成](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="1e987-180">**図 3**: リモート サーバー上の Web サイトの作成</span><span class="sxs-lookup"><span data-stu-id="1e987-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="1e987-181">SSL 証明書が一致しない場合は、SSL 経由でリモート サイトでのアプリケーションを作成するときに、確認のダイアログ ボックスはローカル IIS オプションを使用するときに表示されるダイアログよりも若干異なります。</span><span class="sxs-lookup"><span data-stu-id="1e987-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![リモート サイトのセキュリティの警告](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="1e987-183">**図 4**: リモート サイトのセキュリティの警告</span><span class="sxs-lookup"><span data-stu-id="1e987-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="1e987-184">FTP</span><span class="sxs-lookup"><span data-stu-id="1e987-184">FTP</span></span>

<span data-ttu-id="1e987-185">Visual Studio 2005 では、FTP 経由で Web サイトを作成するオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1e987-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="1e987-186">このオプションを使用するときにユーザーの temp フォルダーにローカル ファイルが作成され、FTP を使用して、ファイルを FTP サイトに移動します。</span><span class="sxs-lookup"><span data-stu-id="1e987-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-187">一時フォルダーの場所が c:/Documents and Settings/&lt;ユーザー&gt;/ローカルの設定/Temp/VWDWebCache/&lt;Server&gt;/_&lt;アプリケーション名&gt;</span><span class="sxs-lookup"><span data-stu-id="1e987-187">The temp folder location is c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>


<span data-ttu-id="1e987-188">FTP のオプションを使用する場合の場所の選択 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="1e987-189">このダイアログを次に示すように、必要な FTP 接続情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="1e987-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![FTP の場所 ダイアログ ボックスの選択](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="1e987-191">**図 5**: FTP の場所 ダイアログ ボックスの選択</span><span class="sxs-lookup"><span data-stu-id="1e987-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="1e987-192">ラボ: セットアップ FTP サイトおよびプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1e987-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="1e987-193">次の手順は、ユーザーがある FTP 経由でをアップロードできるのみ、場所を持つように、FTP サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="1e987-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="1e987-194">FTP サービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1e987-194">Install the FTP Service</span></span>

1. <span data-ttu-id="1e987-195">プログラムの追加と削除を開き、Windows コンポーネントの追加/削除を選択します。</span><span class="sxs-lookup"><span data-stu-id="1e987-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="1e987-196">インターネット インフォメーション サービス (Windows 2003 上のアプリケーション サーバー) を選択し、クリックして**詳細**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="1e987-197">確認**ファイル転送プロトコル (FTP) サービス** をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="1e987-198">クリックして**次**FTP サービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1e987-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="1e987-199">コンテンツの新しいフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1e987-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="1e987-200">Windows エクスプ ローラーでという新しいフォルダーを作成**User1** wwwroot/c:/inetpub 内。</span><span class="sxs-lookup"><span data-stu-id="1e987-200">In Windows Explorer, create a new folder called **User1** inside of c:/inetpub/wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="1e987-201">フォルダーでフォルダーおよびアクセス許可を構成します。</span><span class="sxs-lookup"><span data-stu-id="1e987-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="1e987-202">管理ツール から、インターネット インフォメーション サービス スナップインを開きます。</span><span class="sxs-lookup"><span data-stu-id="1e987-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="1e987-203">コンピューター名のノードの下、FTP サイトのフォルダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="1e987-204">展開**FTP サイト**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="1e987-205">右クリックし、 **FTP サイトの既定の**を選択します**新規**、し**仮想ディレクトリ**、順にクリックします**次**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="1e987-206">入力**User1**仮想ディレクトリ名とクリック**次**。</span><span class="sxs-lookup"><span data-stu-id="1e987-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="1e987-207">入力**c: inetpub/wwwroot/User1**クリックとパスの**次**。</span><span class="sxs-lookup"><span data-stu-id="1e987-207">Enter **c:/inetpub/wwwroot/User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="1e987-208">をクリックして **[次へ]** し**完了**ウィザードを完了します。</span><span class="sxs-lookup"><span data-stu-id="1e987-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="1e987-209">右クリックし、 **User1**既定の FTP サイトと選択対象の仮想ディレクトリ**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="1e987-210">チェック、**書き込み**チェック ボックスをオン をクリック**OK**ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1e987-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="1e987-211">右クリック**FTP サイトの既定の**選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="1e987-212">**セキュリティ アカウント** タブで、オフに**匿名接続を許可する**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="1e987-213">をクリックして**はい**で続行するかどうかたずねるダイアログ。</span><span class="sxs-lookup"><span data-stu-id="1e987-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="1e987-214">クリックして**OK**ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1e987-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="1e987-215">展開、**既定の Web サイト**下、 **Websites**ノード。</span><span class="sxs-lookup"><span data-stu-id="1e987-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="1e987-216">右クリックし、 **User1**ディレクトリおよび選択**プロパティ**</span><span class="sxs-lookup"><span data-stu-id="1e987-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="1e987-217">**アプリケーション設定**セクションで、**作成**アプリケーションとしてフォルダーをマークします。</span><span class="sxs-lookup"><span data-stu-id="1e987-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="1e987-218">クリックして**OK**ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1e987-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="1e987-219">インターネット インフォメーション サービス スナップインを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1e987-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="1e987-220">Web プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1e987-220">Create web project</span></span>

1. <span data-ttu-id="1e987-221">Visual Studio 2005 を開きます。</span><span class="sxs-lookup"><span data-stu-id="1e987-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="1e987-222">**ファイル**メニューの **新しい Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="1e987-223">**場所**] ドロップダウンで、[ **FTP**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="1e987-224">**[参照]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1e987-224">Click **Browse**.</span></span>
5. <span data-ttu-id="1e987-225">入力**localhost**で、 **Server**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="1e987-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="1e987-226">入力**User1**ディレクトリ ボックスに、します。</span><span class="sxs-lookup"><span data-stu-id="1e987-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="1e987-227">クリックして**オープン**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-227">Click **Open**.</span></span> <span data-ttu-id="1e987-228">FTP の場所は、新しい Web サイト ダイアログに入力されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="1e987-229">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1e987-229">Click **OK**.</span></span>
9. <span data-ttu-id="1e987-230">オフに**匿名でログオン**FTP ログオン ダイアログ ボックスで、資格情報を入力し、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="1e987-231">プロジェクトの URL とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="1e987-231">What is the URL for the project?</span></span> <span data-ttu-id="1e987-232">(プロジェクトの URL はソリューション エクスプ ローラーで表示されます)。</span><span class="sxs-lookup"><span data-stu-id="1e987-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="1e987-233">**ビルド**メニューの  **Web サイトのビルド**または**ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="1e987-234">ソリューション エクスプ ローラーで Default.aspx を右クリックし、選択**ブラウザーで表示**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="1e987-235">Web サイトの URL が必要です ダイアログ ボックスで、次のように入力します。 `http://localhost/user1` URL をクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1e987-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-236">読み込む型/_Default できないことを示すエラーが発生する場合は、Web サイトを以前のバージョンではなく ASP.NET 2.0 を実行していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1e987-236">If you get a error indicating an inability to load the type /_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="1e987-237">インターネット インフォメーション サービスで、[ASP.NET] タブから行うことができます。</span><span class="sxs-lookup"><span data-stu-id="1e987-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="1e987-238">Web プロジェクトを開く</span><span class="sxs-lookup"><span data-stu-id="1e987-238">Opening Web Projects</span></span>

<span data-ttu-id="1e987-239">Web プロジェクトを開くと、プロジェクトの作成と似ています。</span><span class="sxs-lookup"><span data-stu-id="1e987-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="1e987-240">次のセクションでは、IDE 内で作業しているの監視する領域を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1e987-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="1e987-241">HTTP と FTP を使用して Web プロジェクトでの作業についても説明します。</span><span class="sxs-lookup"><span data-stu-id="1e987-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="1e987-242">Web プロジェクトを開くには、[ファイル] メニューから開く Web サイトを選択します。</span><span class="sxs-lookup"><span data-stu-id="1e987-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="1e987-243">使用できるのと同じ 4 つのオプションを使用して前の場所の選択ダイアログと同じことが表示されます。 ファイル システム、ローカル IIS、FTP、およびリモート サイト。</span><span class="sxs-lookup"><span data-stu-id="1e987-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="1e987-244">ファイル システム</span><span class="sxs-lookup"><span data-stu-id="1e987-244">File System</span></span>

<span data-ttu-id="1e987-245">このモジュールの前に示したよう Visual Studio は不要になったプロジェクト ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="1e987-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="1e987-246">そのため、ファイル システムから Web サイトを開くには選択した場合、選択したフォルダーが Visual Studio で最初に Web プロジェクトとして作成されていない場合でも、任意のフォルダーを選択するオプションがある実際にします。</span><span class="sxs-lookup"><span data-stu-id="1e987-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="1e987-247">たとえば、Web サイトとマイ ドキュメント フォルダーを開くことができ、Visual Studio が問題なくそれを開き、次に示すように、ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="1e987-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![Web サイトとして開いたマイ ドキュメント](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="1e987-249">**図 6**:*マイ ドキュメント*として Web サイトを開く</span><span class="sxs-lookup"><span data-stu-id="1e987-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="1e987-250">Visual Studio では、追加のファイルとフォルダーが必要な場合にのみ作成するため、追加のファイルまたはフォルダーは追加されませんを開いた場所にします。</span><span class="sxs-lookup"><span data-stu-id="1e987-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="1e987-251">このアーキテクチャの副作用は、ことはできないので、ファイル システム上の Web サイトの入れ子です。</span><span class="sxs-lookup"><span data-stu-id="1e987-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="1e987-252">たとえば、次のディレクトリ構造を検討してください。</span><span class="sxs-lookup"><span data-stu-id="1e987-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="1e987-253">C:/mywebsite web プロジェクト</span><span class="sxs-lookup"><span data-stu-id="1e987-253">Web project at C:/MyWebSite</span></span>

<span data-ttu-id="1e987-254">入れ子になった c:/MyWebSite/にある別の web プロジェクト</span><span class="sxs-lookup"><span data-stu-id="1e987-254">Another web project at C:/MyWebSite/Nested</span></span>

<span data-ttu-id="1e987-255">C:/mywebsite という Web サイトを開くと、入れ子になったフォルダーは、そのアプリケーションのサブ フォルダーとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-255">When you open the Web site at c:/MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="1e987-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="1e987-256">HTTP</span></span>

<span data-ttu-id="1e987-257">IIS メタベース (ローカル IIS) または FrontPage Server Extensions (リモート サイト) を使用してのいずれかに設定を読み取る HTTP 経由で Web サイトを開くときに入れ子になった web アプリケーションがある場合もアプリケーションとして指定するアイコンが表示された表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="1e987-258">Frontpage 2003 での web アプリケーションの操作について熟知する場合は、Visual Studio 2005 での動作は似ています。</span><span class="sxs-lookup"><span data-stu-id="1e987-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="1e987-259">場合でも、Visual Studio IDE 内で現在開かれているアプリケーションの下に入れ子になっているアプリケーションのアイコンが表示されます、ことはできません、コンテンツを参照するように展開します。</span><span class="sxs-lookup"><span data-stu-id="1e987-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="1e987-260">ただしにそれらを開くためにダブルクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="1e987-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="1e987-261">操作を実行するとするか、web アプリケーションを開く (および現在開いているソリューションを置き換える) を求めるダイアログが表示されますか、現在のソリューションに Web アプリケーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="1e987-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![入れ子になったアプリケーション アイコンをダブルクリックするとこのダイアログ ボックスが表示されます。](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="1e987-263">**図 7**: 入れ子になったアプリケーション アイコンをダブルクリックするとこのダイアログ ボックスが表示されます</span><span class="sxs-lookup"><span data-stu-id="1e987-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="1e987-264">FTP サイト</span><span class="sxs-lookup"><span data-stu-id="1e987-264">FTP Site</span></span>

<span data-ttu-id="1e987-265">FTP 経由でサイトを開くと、ファイルはすべてローカルにコピー temp フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1e987-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="1e987-266">ローカル ストレージの場所の完全なパスでは、プロジェクトのプロパティ ペインに表示され、次の形式を使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="1e987-267">C:/Documents and Settings/&lt;ユーザー&gt;/ローカルの設定/Temp/VWDWebCache/&lt;Server&gt;/_&lt;アプリケーション名&gt;</span><span class="sxs-lookup"><span data-stu-id="1e987-267">C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>

<span data-ttu-id="1e987-268">FTP を使用する場合、Visual Studio は、次に示すように参照できるように、プロジェクトのベース URL を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="1e987-269">ベース URL を指定しない場合は、Visual Studio 求められますの初めての Web サイトでページを参照しようとしました。</span><span class="sxs-lookup"><span data-stu-id="1e987-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![FTP サイトのベース URL を指定します。](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="1e987-271">**図 8**: FTP サイトのベース URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="1e987-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="1e987-272">コンパイルの機能強化</span><span class="sxs-lookup"><span data-stu-id="1e987-272">Improvements in Compilation</span></span>

<span data-ttu-id="1e987-273">Visual Studio 2005 で Web アプリケーションの操作は、以前のバージョンよりも著しく短縮です。</span><span class="sxs-lookup"><span data-stu-id="1e987-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="1e987-274">これは、なしのごく一部のコンパイルのアーキテクチャでの変更に期限。</span><span class="sxs-lookup"><span data-stu-id="1e987-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="1e987-275">Visual Studio 2002 および 2003 の場合に、Web アプリケーションは、/bin フォルダーに存在する 1 つのプライマリ アセンブリにコンパイルされました。</span><span class="sxs-lookup"><span data-stu-id="1e987-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="1e987-276">Visual Studio 2005 で、アプリ/(_c) フォルダーが追加されました。</span><span class="sxs-lookup"><span data-stu-id="1e987-276">In Visual Studio 2005, an App/_Code folder was added.</span></span> <span data-ttu-id="1e987-277">クラスとその他の非 UI コードは、アプリ/(_c) フォルダーに追加されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-277">Classes and other non-UI code are added to the App/_Code folder.</span></span> <span data-ttu-id="1e987-278">Visual Studio がプロジェクトをビルドするときは、アプリ/(_c) フォルダーのすべてのファイルが 1 つの App/_Code.dll ファイルにコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="1e987-278">When Visual Studio builds the project, all files in the App/_Code folder are compiled into a single App/_Code.dll file.</span></span> <span data-ttu-id="1e987-279">この変更の結果は、後続のビルドが以前のバージョンよりも高速です。</span><span class="sxs-lookup"><span data-stu-id="1e987-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-280">MSBuild コマンド ライン ユーティリティを ASP.NET Web アプリケーションを構築することもできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="1e987-281">そのツールについては、第 9 章で説明します。</span><span class="sxs-lookup"><span data-stu-id="1e987-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="1e987-282">もう 1 つのコンパイルの拡張機能は、ビルド メニューで新しいビルド ページ オプションです。</span><span class="sxs-lookup"><span data-stu-id="1e987-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="1e987-283">この機能は、変更をよりすばやくコンパイルできるように、現在ページ (と共に、コース、および依存関係の) のみを再構築する開発者を使用できます。</span><span class="sxs-lookup"><span data-stu-id="1e987-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="1e987-284">C# の IntelliSense などの更新のためのバック グラウンド コンパイルを提供しません、ためような利点が非常にこの機能から、単に 1 つのページを再構築が簡単に更新する IntelliSense を使用するためです。</span><span class="sxs-lookup"><span data-stu-id="1e987-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="1e987-285">プロジェクトのビルド プロパティを使用して、スタートアップ ページが実行される前に発生するビルドの種類を構成できます。</span><span class="sxs-lookup"><span data-stu-id="1e987-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="1e987-286">開発者は、Visual Studio はコードの変更後のアプリケーションをより迅速にデバッグを開始できるように、現在のページをビルドのみを選択できます。</span><span class="sxs-lookup"><span data-stu-id="1e987-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![ビルド ページ開始アクション](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="1e987-288">**図 9**: ビルドのページの開始アクション</span><span class="sxs-lookup"><span data-stu-id="1e987-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="1e987-289">Visual Studio および ASP.NET のアーキテクチャにもう 1 つの優れた機能強化は編集の領域であり、続行します。</span><span class="sxs-lookup"><span data-stu-id="1e987-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="1e987-290">Visual Studio 2005 で開発者がプロジェクトのデバッグを開始およびデバッガーのデタッチなくプロジェクトでコード変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="1e987-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="1e987-291">実際には、新しいクラスを追加、プロジェクトのデバッグを開始することが文字どおり、そのクラスにコードを追加、そのクラスの新しいインスタンスを作成するページにコードを追加およびデバッガーをデタッチすることがなく、クラスのメソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1e987-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="1e987-292">新しいコードを実行することは、文字どおり、ブラウザーの更新としてほど簡単です。</span><span class="sxs-lookup"><span data-stu-id="1e987-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="1e987-293">ここをクリックすると、編集のビデオ チュートリアルを参照してください。 して Visual Studio 2005 の機能を続行します。</span><span class="sxs-lookup"><span data-stu-id="1e987-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="1e987-294">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="1e987-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="1e987-295">堅牢なエディット コンティニュの機能で ASP.NET 2.0 と Visual Studio 2005 は、ASP.NET アプリケーションのアーキテクチャの変更が原因です。</span><span class="sxs-lookup"><span data-stu-id="1e987-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="1e987-296">Asp.net 1.x では、Visual Studio 2002/2003 で作成されたアプリケーションは、/bin フォルダーに格納されたプライマリ アセンブリにコンパイルされました。</span><span class="sxs-lookup"><span data-stu-id="1e987-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="1e987-297">すべてクラスをページなど、アプリケーションがその 1 つの DLL にコンパイルされたのです。</span><span class="sxs-lookup"><span data-stu-id="1e987-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="1e987-298">時に、ASP.NET はすべてのコントロール、マークアップ、およびページ内での ASP.NET コードをコンパイルし、ASP.NET 一時フォルダーにそれらの Dll をコピーします。</span><span class="sxs-lookup"><span data-stu-id="1e987-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="1e987-299">Visual Studio 2005 が ASP.NET 2.0 を使用して、実行時に 2 つのコンパイル モデルの概要 (Visual Studio のいずれか) と ASP.NET のいずれかの上にある 1 つの一般的なコンパイル モデルにマージされました。</span><span class="sxs-lookup"><span data-stu-id="1e987-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="1e987-300">すべてのコンパイル問題が実行時に開発段階の代わりにキャッチようになりましたことを意味します。</span><span class="sxs-lookup"><span data-stu-id="1e987-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="1e987-301">デザイナーと IntelliSense のサポートをユーザー コントロールやマスター ページなどの機能のこともできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="1e987-302">ここをクリックすると、ユーザー コントロールのデザイナー サポートのビデオ チュートリアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1e987-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="1e987-303">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="1e987-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="1e987-304">ユーザー コントロールがページから削除されたときに、@Registerディレクティブは、マークアップに保持し、ユーザー コントロールが Web サイトから削除された場合、パーサー エラーを回避するために手動で削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="1e987-305">Visual Studio のコンパイル モデルの別の機能強化は、Web サイトの発行機能です。</span><span class="sxs-lookup"><span data-stu-id="1e987-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="1e987-306">発行機能では、Web サイトをプリコンパイル、ために、開発者は、何も要求時にコンパイルする必要があるの追加のパフォーマンスを利用できます。</span><span class="sxs-lookup"><span data-stu-id="1e987-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="1e987-307">展開するソース コードを持たないように、アプリ/(_c) フォルダー内のすべてのソース コードは、dll の生成にもプリコンパイルにします。</span><span class="sxs-lookup"><span data-stu-id="1e987-307">It also precompiles all source code in the App/_Code folder into a DLL so that no source code has to be deployed.</span></span>


![Web サイトの発行 ダイアログ](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="1e987-309">**図 10**: Web サイトの発行 ダイアログ</span><span class="sxs-lookup"><span data-stu-id="1e987-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="1e987-310">Aspnet/_compile.exe ユーティリティは、ASP.NET Web アプリケーションを事前にコンパイルすることもできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-310">The aspnet/_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="1e987-311">そのツールについては、第 9 章で説明します。</span><span class="sxs-lookup"><span data-stu-id="1e987-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="1e987-312">ときに次に示すよう、Temporary ASP.NET Files フォルダーに格納する Web サイトでは、プリコンパイル済みファイルを発行されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="1e987-313">使用するファイル、 *.compiled*ファイル拡張子が dll の特定の依存関係を定義する XML ファイル。</span><span class="sxs-lookup"><span data-stu-id="1e987-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="1e987-314">Web フォームまたはユーザー コントロールが始まるランダムの Dll にコンパイルされる*アプリ/_Web/_* します。</span><span class="sxs-lookup"><span data-stu-id="1e987-314">Any Webform or user controls are compiled into random DLLs that begin with *App/_Web/_*.</span></span>

<span data-ttu-id="1e987-315">ままにする場合、*このプリコンパイル済みサイトを更新可能*チェック ボックスをオン、Webforms およびユーザー コントロールの内部マークアップをすると、デプロイ後に変更を加えることができます、DLL にコンパイル済みにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="1e987-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="1e987-316">展開されたコンテンツへの変更は許可されませんように、マークアップをロックダウンする場合は、このボックスをオフにします。</span><span class="sxs-lookup"><span data-stu-id="1e987-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="1e987-317">*固定名およびシングル ページ アセンブリを使用する*チェック ボックスを使用すると、各ページは固定という名前のアセンブリにコンパイルできるように、バッチのコンパイルを無効にします。</span><span class="sxs-lookup"><span data-stu-id="1e987-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="1e987-318">このボックスをオフにしたままバッチのコンパイルを利用することができます。</span><span class="sxs-lookup"><span data-stu-id="1e987-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="1e987-319">*プリコンパイル済みアセンブリで厳密な名前を有効にする*チェック ボックスでは厳密な名前に、プリコンパイル済みアセンブリ。</span><span class="sxs-lookup"><span data-stu-id="1e987-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-320">Asp.net 1.x では、厳密な名前付きアセンブリをグローバル アセンブリ キャッシュ (GAC) にインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="1e987-321">ASP.NET 2.0 で厳密な名前付きアセンブリを GAC にインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="1e987-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![ASP.NET アプリケーションのコンパイル済みファイル](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="1e987-323">**図 11**: ASP.NET アプリケーションのコンパイル済みファイル</span><span class="sxs-lookup"><span data-stu-id="1e987-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="1e987-324">上記のアプリケーションでは、web.config ファイルはありませんでした。</span><span class="sxs-lookup"><span data-stu-id="1e987-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="1e987-325">あるされていた場合にが呼び出された*PrecompiledApp.config* Web の発行後にサイトのプロセス。</span><span class="sxs-lookup"><span data-stu-id="1e987-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="1e987-326">展開の強化</span><span class="sxs-lookup"><span data-stu-id="1e987-326">Improvements in Deployment</span></span>

<span data-ttu-id="1e987-327">Visual Studio 2002 および 2003 でとして、Visual Studio 2005 は、プロジェクトのコピー機能します。</span><span class="sxs-lookup"><span data-stu-id="1e987-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="1e987-328">ただし、機能は、Visual Studio 2005 で強化されていますが、Web サイトのコピーと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1e987-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="1e987-329">Web サイトのコピー ダイアログは、左側のフレームと右側のフレームに分割されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="1e987-330">左側のフレームは、ソース Web サイトと呼ばれ、右側のフレームは、リモートの Web サイトと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1e987-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="1e987-331">一部の開発者が混乱ことの 1 つが、右側のフレームに表示されているサイトが必ずしもリモート サイトです。</span><span class="sxs-lookup"><span data-stu-id="1e987-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="1e987-332">ローカル ファイル システムまたは IIS のローカル インスタンス上のサイトがある可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="1e987-333">さらに、左側のフレームに表示されているサイトとは限りませんをソース Web サイトできないためは、ダイアログ ボックスでは、リモートの Web サイトから発行できます。*に*をソース Web サイト。</span><span class="sxs-lookup"><span data-stu-id="1e987-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="1e987-334">リモート Web サイトにプロジェクトをコピーする場合、そのサイトは、FrontPage Server Extensions がインストールされていることが必要です。</span><span class="sxs-lookup"><span data-stu-id="1e987-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="1e987-335">そうでない場合は、FTP を使用して接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="1e987-336">その一方で、プロジェクトをローカルの IIS のインスタンスにコピーする場合は FrontPage Server Extensions は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="1e987-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-337">ローカルの IIS インスタンスで新しい Web サイトを作成するとき、FrontPage 2002 Server Extensions がインストールされている場合は、SharePoint サーバーの Web サイトの作成はサポートされていないことを示すエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="1e987-338">その場合は、FrontPage 2000 Server の拡張機能をインストールまたは FrontPage サーバー拡張機能を削除するオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="1e987-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="1e987-339">Web サイトのコピー機能のビデオ チュートリアルについては、ここをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1e987-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="1e987-340">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="1e987-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="1e987-341">デバッグ機能の強化</span><span class="sxs-lookup"><span data-stu-id="1e987-341">Improvements in Debugging</span></span>

<span data-ttu-id="1e987-342">Visual Studio 2005 でのデバッグで 4 つ主な改善点があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="1e987-343">非管理者としてローカルでのデバッグは、すぐ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="1e987-344">Compilation 要素の Debug 属性は、既定値はようになりました。</span><span class="sxs-lookup"><span data-stu-id="1e987-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="1e987-345">リモート デバッグのセットアップと構成は、前より簡単です。</span><span class="sxs-lookup"><span data-stu-id="1e987-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="1e987-346">FTP の場所を使用して開いた Web サイトをデバッグできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="1e987-347">非管理者としてデバッグ</span><span class="sxs-lookup"><span data-stu-id="1e987-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="1e987-348">ASP.NET 開発サーバーの追加には、非管理者によるをすぐに使える ASP.NET アプリケーションを簡単にデバッグができます。</span><span class="sxs-lookup"><span data-stu-id="1e987-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="1e987-349">ローカル ファイル システムで実行されている ASP.NET アプリケーションをデバッグすると、Visual Studio は、ログオン ユーザーのコンテキストで ASP.NET 開発サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="1e987-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="1e987-350">そのユーザーは、追加の構成なしには、そのアプリケーションをデバッグできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="1e987-351">デバッグ False は、既定では</span><span class="sxs-lookup"><span data-stu-id="1e987-351">Debug is False by Default</span></span>

<span data-ttu-id="1e987-352">Asp.net 1.x では、*デバッグ*属性、*コンパイル*に web.config ファイルの要素が設定された*true*既定。</span><span class="sxs-lookup"><span data-stu-id="1e987-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="1e987-353">これが常に推奨開発者では、この属性を設定*false*運用環境にアプリケーションを展開する前に、ほとんどの開発者がデバッグ属性を設定する保持するための結果を理解しない完全true の場合が単純に左として-です。</span><span class="sxs-lookup"><span data-stu-id="1e987-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="1e987-354">最も重大な問題を true に設定が ASP.NETs バッチのコンパイル モデル無効にすることは debug 属性を持つを使用します。</span><span class="sxs-lookup"><span data-stu-id="1e987-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="1e987-355">そのため、各ページは、別個の DLL にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="1e987-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="1e987-356">意味 Web アプリケーションの構成 (いない前例の任意の方法で)、ページの何千もの場合は、そのアプリケーションで数千のいくつかの小さな Dll が作成されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="1e987-357">これらの Dll はサイズが小さいですが、メモリ内の特定の位置にアンロードされます。</span><span class="sxs-lookup"><span data-stu-id="1e987-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="1e987-358">そのため、システム メモリ内で断片化が発生して、OutOfMemoryException 出現させることができます。</span><span class="sxs-lookup"><span data-stu-id="1e987-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="1e987-359">ASP.NET 2.0 では、debug 属性は既定で false に設定されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="1e987-360">説明したように既に、開発者が Visual Studio 2005 で ASP.NET アプリケーションをデバッグするときにデバッグを有効に web.config ファイルを追加するように求められます。</span><span class="sxs-lookup"><span data-stu-id="1e987-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="1e987-361">ASP.NET で存在していた同じ欠点を負担そう 1.x では、開発者が明確に通知する警告が運用環境にアプリケーションを移行する前に、false に、属性をリセットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="1e987-362">リモート デバッグのセットアップと構成</span><span class="sxs-lookup"><span data-stu-id="1e987-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="1e987-363">Visual Studio 2002/2003、リモート デバッグ マシン デバッグ マネージャー (mdm.exe) および vs7jit.exe プロセスに依存します。</span><span class="sxs-lookup"><span data-stu-id="1e987-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="1e987-364">そのため、多くの場合、お客様の黒いボックスではリモート デバッグの問題のトラブルシューティングと PSS のほうが多くの場合。</span><span class="sxs-lookup"><span data-stu-id="1e987-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="1e987-365">Visual Studio 2005 では、mdm.exe と vs7jit.exe プロセスへの依存を削除します。</span><span class="sxs-lookup"><span data-stu-id="1e987-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="1e987-366">代わりに、今すぐサービス使用して、リモート デバッグ モニター (msvsmon.exe。)</span><span class="sxs-lookup"><span data-stu-id="1e987-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="1e987-367">Visual Studio 2005 でのリモート デバッグの要件は、非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="1e987-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="1e987-368">デバッグする前に、リモート サーバーで msvsmon.exe を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="1e987-369">リモート デバッグ モニターをインストールするには、Visual Studio CD から、または Web サーバーにまったく何もインストールしなくても、共有から msvsmon.exe を実行することだけことができます。</span><span class="sxs-lookup"><span data-stu-id="1e987-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="1e987-370">Msvsmon.exe を実行するときに、リモート デバッグ用にブロックされるポートに関するエラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="1e987-371">幸いにも、次に示すようは、警告ダイアログ ボックスで右からポート簡単に解除します。</span><span class="sxs-lookup"><span data-stu-id="1e987-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Windows ファイアウォールは、リモート デバッグをブロックして、通知](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="1e987-373">**図 12**: 通知を Windows ファイアウォールがリモート デバッグをブロック</span><span class="sxs-lookup"><span data-stu-id="1e987-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="1e987-374">デバッグに必要なポートをブロック解除が後、は、次に示すように、リモート デバッグ モニターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="1e987-375">このインターフェイスから接続を監視し、変更するアクセス許可を簡単にデバッグできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![リモート デバッグ モニター](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="1e987-377">**図 13**: リモート デバッグ モニター</span><span class="sxs-lookup"><span data-stu-id="1e987-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="1e987-378">FTP 経由で開かれた Web アプリケーションをリモートでデバッグすることもできます。</span><span class="sxs-lookup"><span data-stu-id="1e987-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="1e987-379">手順は、について説明したものと同じです。</span><span class="sxs-lookup"><span data-stu-id="1e987-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="1e987-380">ただし、このモジュールで前述したように FTP プロジェクトの参照のベース URL を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="1e987-381">ラボ 2</span><span class="sxs-lookup"><span data-stu-id="1e987-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="1e987-382">Visual Studio 2005 でのリモート デバッグ</span><span class="sxs-lookup"><span data-stu-id="1e987-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="1e987-383">このラボで Visual Studio 2005 でのリモート デバッグを説明します。</span><span class="sxs-lookup"><span data-stu-id="1e987-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="1e987-384">このラボのビデオ チュートリアルについては、ここをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1e987-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="1e987-385">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="1e987-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="1e987-386">このラボでは、1 つの実行中の Visual Studio 2005 と、その他の実行されている IIS 5 以上の 2 つのマシンがある必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="1e987-387">Visual Studio 2005 を開き、リモート サーバーで新しい Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1e987-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="1e987-388">IIS のリモート インスタンス、または FTP 経由で Web サイトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="1e987-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="1e987-389">リモートの Web サーバーからは、UNC パスを使用して開発用コンピューターで msvsmon.exe を検索し、実行します。</span><span class="sxs-lookup"><span data-stu-id="1e987-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="1e987-390">Msvsmon.exe の既定の場所は、//server/c$/Program ファイル/Microsoft Visual Studio 8/Common7/IDE/リモート デバッガー x86 です。</span><span class="sxs-lookup"><span data-stu-id="1e987-390">The default location for msvsmon.exe is //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span></span>
2. <span data-ttu-id="1e987-391">リモート デバッグ用のポートのブロックを解除するメッセージが表示されたら、そのためです。</span><span class="sxs-lookup"><span data-stu-id="1e987-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="1e987-392">開発用コンピューターからは、Default.aspx の分離コードを開き、ページ/(_l) メソッドにブレークポイントを設定します。</span><span class="sxs-lookup"><span data-stu-id="1e987-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page/_Load method.</span></span>
4. <span data-ttu-id="1e987-393">開発用コンピューターからデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="1e987-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="1e987-394">予想どおり、ブレークポイントにヒットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1e987-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="1e987-395">ASP.NET 開発サーバー</span><span class="sxs-lookup"><span data-stu-id="1e987-395">ASP.NET Development Server</span></span>

<span data-ttu-id="1e987-396">既に説明したという、としては、ASP.NET 開発サーバーと呼ばれる Web サーバーと Visual Studio 2005 が提供されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="1e987-397">(ASP.NET 開発サーバーとも呼ば Cassini。)この Web サーバーを参照し、ファイル システムで実行されている Web アプリケーションをデバッグする便利な手段です。</span><span class="sxs-lookup"><span data-stu-id="1e987-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="1e987-398">ASP.NET 開発サーバーとは、制限付きの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="1e987-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="1e987-399">リモート接続を許可しないこと、Web サーバーを開始したユーザー以外のユーザーからの要求を許可しません。</span><span class="sxs-lookup"><span data-stu-id="1e987-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="1e987-400">これもが ASP ページを提供する機能。</span><span class="sxs-lookup"><span data-stu-id="1e987-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="1e987-401">ASP.NET リソースと HTML のリソース (画像、CSS ファイルなどを含む) のみが提供されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="1e987-402">ある c:/Windows/Microsoft.NET/Framework/v2.0./ WebDev.WebServer.exe ファイルを実行して、コマンドラインを使用して、ASP.NET 開発サーバーを起動できる*/*  /  */*/\*.</span><span class="sxs-lookup"><span data-stu-id="1e987-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/\*.</span></span> <span data-ttu-id="1e987-403">次のダイアログ ボックスでは、使用可能なパラメーターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1e987-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="1e987-404">**図 14**</span><span class="sxs-lookup"><span data-stu-id="1e987-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="1e987-405">ASP.NET 開発サーバーは、コマンドラインを使用して明示的に起動すると、サポートされていません。</span><span class="sxs-lookup"><span data-stu-id="1e987-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
