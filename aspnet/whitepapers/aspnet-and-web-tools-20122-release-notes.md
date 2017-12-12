---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: "ASP.NET および Web ツール 2012.2 のリリース ノート |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET および Web ツール 2012.2 リリース ノートです。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: e6c940aa507d72928d71019070ded5197458a763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-and-web-tools-20122-release-notes"></a><span data-ttu-id="cbf41-103">ASP.NET および Web ツール 2012.2 リリース ノートには</span><span class="sxs-lookup"><span data-stu-id="cbf41-103">ASP.NET and Web Tools 2012.2 Release Notes</span></span>
====================
> <span data-ttu-id="cbf41-104">このドキュメントでは、ASP.NET および Web ツール 2012.2 のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-104">This document describes the release of ASP.NET and Web Tools 2012.2.</span></span> <span data-ttu-id="cbf41-105">Visual Studio Web ツールと ASP.NET を更新することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-105">It is an update to Visual Studio Web Tooling and ASP.NET.</span></span>


- [<span data-ttu-id="cbf41-106">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="cbf41-106">Installation Notes</span></span>](#_Installation)
- [<span data-ttu-id="cbf41-107">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="cbf41-107">Documentation</span></span>](#_Documentation)
- [<span data-ttu-id="cbf41-108">サポート</span><span class="sxs-lookup"><span data-stu-id="cbf41-108">Support</span></span>](#_Support)
- [<span data-ttu-id="cbf41-109">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="cbf41-109">Software Requirements</span></span>](#_Software_Requirements)
- [<span data-ttu-id="cbf41-110">ASP.NET および Web ツール 2012.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="cbf41-110">New Features in ASP.NET and Web Tools 2012.2</span></span>](#_New_Features_in)

    - [<span data-ttu-id="cbf41-111">ツール</span><span class="sxs-lookup"><span data-stu-id="cbf41-111">Tooling</span></span>](#_Tooling)
    - [<span data-ttu-id="cbf41-112">Web の発行</span><span class="sxs-lookup"><span data-stu-id="cbf41-112">Web Publishing</span></span>](#_Web_Publishing)
    - [<span data-ttu-id="cbf41-113">ASP.NET MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="cbf41-113">ASP.NET MVC Templates</span></span>](#_Templates)
    - [<span data-ttu-id="cbf41-114">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="cbf41-114">ASP.NET Web API</span></span>](#_ASP.NET_Web_API)

    - [<span data-ttu-id="cbf41-115">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="cbf41-115">ASP.NET SignalR</span></span>](#_ASP.NET_SignalR)
    - [<span data-ttu-id="cbf41-116">ASP.NET Friendly Url</span><span class="sxs-lookup"><span data-stu-id="cbf41-116">ASP.NET Friendly URLs</span></span>](#_ASP.NET_Friendly_URLs)
- [<span data-ttu-id="cbf41-117">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="cbf41-117">Known Issues and Breaking Changes</span></span>](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a><span data-ttu-id="cbf41-118">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="cbf41-118">Installation Notes</span></span>

<span data-ttu-id="cbf41-119">使用して、ASP.NET および Web ツール 2012.2 for Visual Studio 2012 をインストールできる[Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-119">ASP.NET and Web Tools 2012.2 for Visual Studio 2012 can be installed using [Web Platform installer](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).</span></span> <span data-ttu-id="cbf41-120">これは、更新プログラムを Visual Studio 2012 または Visual Studio Express 2012 for Web が必要です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-120">This is an update to Visual Studio 2012 or Visual Studio Express 2012 for Web, which is required.</span></span> <span data-ttu-id="cbf41-121">Visual Studio がインストールされていない場合は、Visual Studio Express 2012 for Web がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-121">If you do not have Visual Studio installed, Visual Studio Express 2012 for Web will be installed.</span></span>

<span data-ttu-id="cbf41-122">ASP.NET および Web ツール 2012.2 を手動でインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-122">You can also install ASP.NET and Web Tools 2012.2 manually.</span></span> <span data-ttu-id="cbf41-123">Visual Studio 2012 または Visual Studio Express 2012 for Web をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-123">You must have Visual Studio 2012 or Visual Studio Express 2012 for Web installed.</span></span> <span data-ttu-id="cbf41-124">次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-124">Then use the following instructions:</span></span> 

1. <span data-ttu-id="cbf41-125">ダウンロード[ASP.NET および Frameworks 2012.2 の Web](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe)インストーラーをダウンロード センターからです。</span><span class="sxs-lookup"><span data-stu-id="cbf41-125">Download [ASP.NET and Web Frameworks 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) installer from Download Center.</span></span>
2. <span data-ttu-id="cbf41-126">ときにメッセージが表示されたらクリックを実行します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-126">When prompted click Run.</span></span> <span data-ttu-id="cbf41-127">後で実行するファイルを保存することもできます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-127">You can also save the file to run it later.</span></span>
3. <span data-ttu-id="cbf41-128">更新が表示される Visual Studio のバージョンを確認します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-128">Verify the version of Visual Studio you will update.</span></span> <span data-ttu-id="cbf41-129">更新する場合、Visual Studio を起動して、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-129">You can do this by launching the Visual Studio you wish to update.</span></span> <span data-ttu-id="cbf41-130">ヘルプ メニューの項目 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-130">Then click the Help menu item.</span></span>   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. <span data-ttu-id="cbf41-131">メニュー項目を表示する場合は&quot;に関する Microsoft Visual Studio 2012 for Web&quot;ダウンロード[Web Developer Tools 2012.2 - Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-131">If you see the menu item &quot;About Microsoft Visual Studio 2012 for Web&quot; then download [Web Developer Tools 2012.2 - Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkID=282228).</span></span> <span data-ttu-id="cbf41-132">それ以外の場合にダウンロード[Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-132">Otherwise download [Web Developer Tools 2012.2 - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).</span></span>
5. <span data-ttu-id="cbf41-133">ときにメッセージが表示されたらクリックを実行します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-133">When prompted click Run.</span></span> <span data-ttu-id="cbf41-134">後で実行するファイルを保存することもできます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-134">You can also save the file to run it later.</span></span>

> [!NOTE]
> <span data-ttu-id="cbf41-135">ASP.NET および Web ツール 2012.2 のリリースでは、SQL Server Data Tools は含まれません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-135">ASP.NET and Web Tools 2012.2 release does not include SQL Server Data Tools.</span></span> <span data-ttu-id="cbf41-136">SQL Server および Windows Azure SQL データベースは、豊富な一連のデータベース プロジェクトに基づくオフラインの開発、スキーマ比較の展開機能の強化されたデータベースなどのツールを提供します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-136">SQL Server and Windows Azure SQL Databases provides a richer set of database tooling including offline project-backed development, schema comparison and enhanced database deployment capabilities.</span></span> <span data-ttu-id="cbf41-137">または SQL Server Data Tools をインストールする場合の詳細については、次を参照してください。 [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-137">For more information or to install SQL Server Data Tools visit [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).</span></span>

<a id="_Documentation"></a>
## <a name="documentation"></a><span data-ttu-id="cbf41-138">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="cbf41-138">Documentation</span></span>

<span data-ttu-id="cbf41-139">チュートリアルおよびその他の情報は、ASP.NET および Web ツール 2012.2 は ASP.NET web サイト (https://www.asp.net) から入手できます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-139">Tutorials and other information about ASP.NET and Web Tools 2012.2 are available from ASP.NET web site ( https://www.asp.net).</span></span>

<a id="_Support"></a>
## <a name="support"></a><span data-ttu-id="cbf41-140">Support</span><span class="sxs-lookup"><span data-stu-id="cbf41-140">Support</span></span>

<span data-ttu-id="cbf41-141">ASP.NET および Web ツール 2012.2 正式にリリースされた、サポートします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-141">ASP.NET and Web Tools 2012.2 is officially released and supported.</span></span> <span data-ttu-id="cbf41-142">通常のサポート チャネルを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-142">You can use your normal support channel.</span></span> <span data-ttu-id="cbf41-143">ASP.NET フォーラムに質問を投稿することもできます ([https://forums.asp.net/](https://forums.asp.net/)) では、ASP.NET コミュニティのメンバーは頻繁に、非公式のサポートを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-143">You can also post questions to the ASP.NET forums ([https://forums.asp.net/](https://forums.asp.net/)), where members of the ASP.NET community are frequently able to provide informal support.</span></span>

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="cbf41-144">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="cbf41-144">Software Requirements</span></span>

<span data-ttu-id="cbf41-145">ASP.NET および Web ツール 2012.2 Web の Visual Studio 2012 または Visual Studio Express 2012 が必要です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-145">The ASP.NET and Web Tools 2012.2 requires Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a><span data-ttu-id="cbf41-146">ASP.NET および Web ツール 2012.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="cbf41-146">New Features in ASP.NET and Web Tools 2012.2</span></span>

<span data-ttu-id="cbf41-147">このセクションでは、ASP.NET および Web ツール 2012.2 リリースで導入された機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-147">This section describes features that have been introduced in the ASP.NET and Web Tools 2012.2 release.</span></span>

<a id="_Tooling"></a>
### <a name="tooling"></a><span data-ttu-id="cbf41-148">ツール</span><span class="sxs-lookup"><span data-stu-id="cbf41-148">Tooling</span></span>

- <span data-ttu-id="cbf41-149">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="cbf41-149">Page Inspector</span></span> 

    - <span data-ttu-id="cbf41-150">マップ ページに対応する JavaScript コードに動的に追加されたアイテムに Page Inspector を許可する JavaScript の選択のマッピングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-150">Support JavaScript selection mapping allowing Page Inspector to map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span>
    - <span data-ttu-id="cbf41-151">CSS を表示する機能は、リアルタイムで更新されます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-151">The ability to see CSS updates in real-time.</span></span>
    - <span data-ttu-id="cbf41-152">詳細については、「 [CSS 自動同期と Page Inspector 内で JavaScript 選択マッピング](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-152">For more information, read [CSS Auto-Sync and JavaScript Selection Mapping in Page Inspector](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).</span></span>
- <span data-ttu-id="cbf41-153">エディター</span><span class="sxs-lookup"><span data-stu-id="cbf41-153">Editor</span></span> 

    - <span data-ttu-id="cbf41-154">CoffeeScript、見える、ハンドル、および JsRender の構文の強調表示をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-154">Support syntax highlighting for CoffeeScript, Mustache, Handlebars, and JsRender.</span></span>
    - <span data-ttu-id="cbf41-155">HTML エディターは、Knockout バインドの Intellisense を提供します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-155">The HTML editor provides Intellisense for Knockout bindings.</span></span>
    - <span data-ttu-id="cbf41-156">以下の編集およびコンパイラがサポート以下を使用して動的な CSS のビルドを有効にします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-156">LESS editing and compiler support to enable building dynamic CSS using LESS.</span></span>
    - <span data-ttu-id="cbf41-157">.NET クラスとして JSON ペーストしてください。</span><span class="sxs-lookup"><span data-stu-id="cbf41-157">Paste JSON as a .NET class.</span></span> <span data-ttu-id="cbf41-158">この特別な貼り付けコマンドを使用して、c#、VB.NET コード ファイル、および Visual Studio に JSON を貼り付けると、JSON から推論しようとする .NET クラスが自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-158">Using this Special Paste command to paste JSON into a C# or VB.NET code file, and Visual Studio will automatically generate .NET classes inferred from the JSON.</span></span>
- <span data-ttu-id="cbf41-159">モバイル エミュレーターのサポートは、VSIX としてサード パーティ製のエミュレーターをインストールできるように機能拡張フックを追加します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-159">Mobile Emulator support adds extensibility hooks so that third-party emulators can be installed as a VSIX.</span></span> <span data-ttu-id="cbf41-160">開発者がさまざまなモバイル デバイス上の web サイトをプレビューできるように、インストールされているエミュレーターが f5 キーを押して、ドロップダウン リストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-160">The installed emulators will show up in the F5 dropdown, so that developers can preview their websites on a variety of mobile devices.</span></span> <span data-ttu-id="cbf41-161">詳しくは、この機能に関するブログ エントリの Scott Hanselman に[Visual Studio で新しい BrowserStack 統合](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-161">Read more about this feature in Scott Hanselman's blog entry on [the new BrowserStack integration with Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).</span></span>

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a><span data-ttu-id="cbf41-162">Web の発行</span><span class="sxs-lookup"><span data-stu-id="cbf41-162">Web Publishing</span></span>

- <span data-ttu-id="cbf41-163">Web サイト プロジェクトでは、Windows Azure Web サイトへの発行などの Web アプリケーション プロジェクトと同じ公開機能があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="cbf41-163">Web site projects now have the same publishing experience as Web Application projects including publishing to Windows Azure Web Sites.</span></span>
- <span data-ttu-id="cbf41-164">選択的発行 &#8211;です。1 つまたは複数のファイル (Web 配置のエンドポイントへの発行) した後、次の操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-164">Selective publish &#8211; for one or more files you can perform the following actions (after publishing to a Web Deploy endpoint):</span></span> 

    - <span data-ttu-id="cbf41-165">選択したファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-165">Publish selected files.</span></span>
    - <span data-ttu-id="cbf41-166">ローカル ファイルとリモートのファイルの差分を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cbf41-166">See the difference between a local file and a remote file.</span></span>
    - <span data-ttu-id="cbf41-167">リモート ファイルをローカル ファイルを更新するか、ローカル ファイルで、リモート ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-167">Update the local file with the remote file or update the remote file with the local file.</span></span>

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a><span data-ttu-id="cbf41-168">ASP.NET MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="cbf41-168">ASP.NET MVC Templates</span></span>

- <span data-ttu-id="cbf41-169">新しい Facebook Application テンプレートは、Facebook Canvas に簡単なアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-169">The new Facebook Application template makes writing Facebook Canvas applications easy.</span></span> <span data-ttu-id="cbf41-170">いくつかの簡単な手順では、ログインしているユーザーからデータを取得し、その友達と統合し、Facebook アプリケーションを作成できます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-170">In a few simple steps, you can create a Facebook application that gets data from a logged in user and integrates with their friends.</span></span> <span data-ttu-id="cbf41-171">テンプレートには、認証、アクセス許可、Facebook データへのアクセスなど、Facebook アプリケーションのビルドに関連するすべての機能を処理する新しいライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cbf41-171">The template includes a new library to take care of all the plumbing involved in building a Facebook app, including authentication, permissions, accessing Facebook data and more.</span></span> <span data-ttu-id="cbf41-172">Facebook アプリケーション テンプレートを使用する方法については「 [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-172">For more information on using the Facebook Application template see [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).</span></span>
- <span data-ttu-id="cbf41-173">新しい Single Page Application MVC テンプレートは、HTML 5、CSS 3、および人気のある Knockout、jQuery JavaScript ライブラリを ASP.NET Web API を使用して対話型のクライアント側 web アプリをビルドする開発者を使用できます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-173">A new Single Page Application MVC template allows developers to build interactive client-side web apps using HTML 5, CSS 3, and the popular Knockout and jQuery JavaScript libraries, on top of ASP.NET Web API.</span></span> <span data-ttu-id="cbf41-174">テンプレートには、RESTful サーバー API を使用する JavaScript HTML5 アプリケーションを構築するための一般的なプラクティスを示す"todo"リスト アプリケーションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cbf41-174">The template includes a "todo" list application that demonstrates common practices for building a JavaScript HTML5 application that uses a RESTful server API.</span></span> <span data-ttu-id="cbf41-175">詳細は、「を読み取ることができます[https://www.asp.net/single-page-application](../single-page-application/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-175">You can read more at [https://www.asp.net/single-page-application](../single-page-application/index.md).</span></span>
- <span data-ttu-id="cbf41-176">ASP.NET MVC の新しいプロジェクト ダイアログ ボックスに新しいテンプレートを追加するための VSIX を作成できます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-176">You can now create a VSIX that adds new templates to the ASP.NET MVC New Project dialog.</span></span> <span data-ttu-id="cbf41-177">方法について説明します[https://go.microsoft.com/fwlink/?LinkId=275019。](https://go.microsoft.com/fwlink/?LinkId=275019)</span><span class="sxs-lookup"><span data-stu-id="cbf41-177">Learn how here: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)</span></span>
- <span data-ttu-id="cbf41-178">FixedDisplayModes パッケージ &#8211;です。MVC 4 のバグの回避策を含む新しい 'FixedDisplayModes' NuGet パッケージを含めるには、MVC プロジェクト テンプレートが更新されました。</span><span class="sxs-lookup"><span data-stu-id="cbf41-178">FixedDisplayModes package &#8211; MVC project templates have been updated to include the new ‘FixedDisplayModes' NuGet package, which contains a workaround for a bug in MVC 4.</span></span> <span data-ttu-id="cbf41-179">パッケージに含まれている修正プログラムの詳細については、このブログの投稿を参照してください ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) MVC チームからです。</span><span class="sxs-lookup"><span data-stu-id="cbf41-179">For more information on the fix contained in the package, refer to this blog post ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) from the MVC team.</span></span>

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a><span data-ttu-id="cbf41-180">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="cbf41-180">ASP.NET Web API</span></span>

<span data-ttu-id="cbf41-181">ASP.NET Web API は、いくつかの新しい機能で強化されています。</span><span class="sxs-lookup"><span data-stu-id="cbf41-181">ASP.NET Web API has been enhanced with several new features:</span></span>

- <span data-ttu-id="cbf41-182">ASP.NET Web API OData</span><span class="sxs-lookup"><span data-stu-id="cbf41-182">ASP.NET Web API OData</span></span>
- <span data-ttu-id="cbf41-183">ASP.NET Web API Tracing</span><span class="sxs-lookup"><span data-stu-id="cbf41-183">ASP.NET Web API Tracing</span></span>
- <span data-ttu-id="cbf41-184">ASP.NET Web API Help Page</span><span class="sxs-lookup"><span data-stu-id="cbf41-184">ASP.NET Web API Help Page</span></span>

#### <a name="aspnet-web-api-odata"></a><span data-ttu-id="cbf41-185">ASP.NET Web API OData</span><span class="sxs-lookup"><span data-stu-id="cbf41-185">ASP.NET Web API OData</span></span>

<span data-ttu-id="cbf41-186">ASP.NET Web API OData の柔軟性を任意のデータ ソースでも豊富なビジネス ロジックで OData エンドポイントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-186">ASP.NET Web API OData gives you the flexibility you need to build OData endpoints with rich business logic over any data source.</span></span> <span data-ttu-id="cbf41-187">ASP.NET Web API OData を使用して、公開する OData セマンティクスの量を制御します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-187">With ASP.NET Web API OData you control the amount of OData semantics that you want to expose.</span></span> <span data-ttu-id="cbf41-188">ASP.NET Web API OData、ASP.NET MVC 4 プロジェクト テンプレートに含まれても NuGet から ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata))。</span><span class="sxs-lookup"><span data-stu-id="cbf41-188">ASP.NET Web API OData is included with the ASP.NET MVC 4 project templates and is also available from NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).</span></span>

<span data-ttu-id="cbf41-189">現在、ASP.NET Web API OData には、次の機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="cbf41-189">ASP.NET Web API OData currently supports the following features:</span></span>

- <span data-ttu-id="cbf41-190">[Queryable] 属性を適用することにより、OData クエリのセマンティクスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-190">Enable OData query semantics by applying the [Queryable] attribute.</span></span>
- <span data-ttu-id="cbf41-191">簡単に OData のクエリを検証し、サポートされるクエリ オプション、演算子と関数のセットを制限します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-191">Easily validate OData queries and restrict the set of supported query options, operators and functions.</span></span>
- <span data-ttu-id="cbf41-192">パラメーターは、検証、および、IQueryable または IEnumerable に適用できるクエリの抽象構文ツリー表現を取得するには、直接 ODataQueryOptions にバインドします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-192">Parameter bind to ODataQueryOptions directly to get an abstract syntax tree representation of the query that can then be validated and applied to an IQueryable or IEnumerable.</span></span>
- <span data-ttu-id="cbf41-193">[Queryable] 属性で結果の制限を指定することによってサービス ドリブン ページングと次のページ リンクの生成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-193">Enable service-driven paging and next page link generation by specifying result limits on [Queryable] attribute.</span></span>
- <span data-ttu-id="cbf41-194">$Inlinecount を使用して一致するリソースの合計数のインライン展開の数を要求します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-194">Request an inlined count of the total number of matching resources using $inlinecount.</span></span>
- <span data-ttu-id="cbf41-195">Null の伝達を制御します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-195">Control null propagation.</span></span>
- <span data-ttu-id="cbf41-196">$Filter ですべて/すべての演算子。</span><span class="sxs-lookup"><span data-stu-id="cbf41-196">Any/All operators in $filter.</span></span>
- <span data-ttu-id="cbf41-197">Entity data model を規約によって推定または明示的に同様に Entity Framework コード優先の方法でモデルをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-197">Infer an entity data model by convention or explicitly customize a model in a manner similar to Entity Framework Code-First.</span></span>
- <span data-ttu-id="cbf41-198">EntitySetController から派生することで公開してエンティティを設定します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-198">Expose entity sets by deriving from EntitySetController.</span></span>
- <span data-ttu-id="cbf41-199">ナビゲーション プロパティを公開する、リンクを操作および OData アクションを実装するため、単純なカスタマイズ可能な規則です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-199">Simple, customizable conventions for exposing navigation properties, manipulating links and implementing OData actions.</span></span>
- <span data-ttu-id="cbf41-200">MapODataRoute 拡張メソッドを使用してルーティングを簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-200">Simplified routing using the MapODataRoute extension method.</span></span>
- <span data-ttu-id="cbf41-201">複数の EDM モデルを公開することでバージョン管理をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-201">Support for versioning by exposing multiple EDM models.</span></span>
- <span data-ttu-id="cbf41-202">Web api サービス ドキュメントおよび $metadata クライアント (.NET、Windows Phone、Windows ストアなど) を生成するようにを公開します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-202">Expose service document and $metadata so you can generate clients (.NET, Windows Phone, Windows Store, etc.) for your Web API.</span></span>
- <span data-ttu-id="cbf41-203">OData Atom、JSON、および JSON の冗長形式をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-203">Support for the OData Atom, JSON, and JSON verbose formats.</span></span>
- <span data-ttu-id="cbf41-204">作成、更新、部分的に更新 (PATCH) し、エンティティを削除します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-204">Create, update, partially update (PATCH) and delete entities.</span></span>
- <span data-ttu-id="cbf41-205">クエリおよびエンティティ間のリレーションシップを操作します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-205">Query and manipulate relationships between entities.</span></span>
- <span data-ttu-id="cbf41-206">ネットワーク上で、ルートまでリレーションシップ リンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-206">Create relationship links that wire up to your routes.</span></span>
- <span data-ttu-id="cbf41-207">複合型。</span><span class="sxs-lookup"><span data-stu-id="cbf41-207">Complex types.</span></span>
- <span data-ttu-id="cbf41-208">エンティティ型の継承します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-208">Entity Type Inheritance.</span></span>
- <span data-ttu-id="cbf41-209">コレクションのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="cbf41-209">Collection properties.</span></span>
- <span data-ttu-id="cbf41-210">列挙型。</span><span class="sxs-lookup"><span data-stu-id="cbf41-210">Enums.</span></span>
- <span data-ttu-id="cbf41-211">OData アクション。</span><span class="sxs-lookup"><span data-stu-id="cbf41-211">OData actions.</span></span>
- <span data-ttu-id="cbf41-212">WCF Data Services、つまり ODataLib として同じ基盤に構築された ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata))。</span><span class="sxs-lookup"><span data-stu-id="cbf41-212">Built upon the same foundation as WCF Data Services, namely ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).</span></span>

<span data-ttu-id="cbf41-213">ASP.NET Web API OData の詳細については、次を参照してください。 [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-213">For more information on ASP.NET Web API OData see [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).</span></span>

#### <a name="aspnet-web-api-tracing"></a><span data-ttu-id="cbf41-214">ASP.NET Web API Tracing</span><span class="sxs-lookup"><span data-stu-id="cbf41-214">ASP.NET Web API Tracing</span></span>

<span data-ttu-id="cbf41-215">ASP.NET Web API のトレースは、.NET トレースと web Api からのトレース データを統合できます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-215">ASP.NET Web API Tracing integrates tracing data from your web APIs with .NET Tracing.</span></span> <span data-ttu-id="cbf41-216">これは既定では、Web API プロジェクト テンプレートでは有効になりました。</span><span class="sxs-lookup"><span data-stu-id="cbf41-216">It is now enabled by default in the Web API project template.</span></span> <span data-ttu-id="cbf41-217">Web のデータをトレース Api は、出力ウィンドウに送信され、IntelliTrace をとおして利用可能な。</span><span class="sxs-lookup"><span data-stu-id="cbf41-217">Tracing data for your web APIs is sent to the Output window and is made available through IntelliTrace.</span></span> <span data-ttu-id="cbf41-218">統合により、Windows Azure でホストされている場合は、Web API に関するトレース情報を有効にする ASP.NET Web API Tracing [Windows Azure 診断](https://msdn.microsoft.com/en-us/library/windowsazure/hh411529.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-218">ASP.NET Web API Tracing enables you to trace information about your Web API when hosted on Windows Azure through integration with [Windows Azure Diagnostics](https://msdn.microsoft.com/en-us/library/windowsazure/hh411529.aspx).</span></span> <span data-ttu-id="cbf41-219">インストールし、ASP.NET Web API Tracing NuGet パッケージを使用して任意のアプリケーションで ASP.NET Web API のトレースを有効にすることができます ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing))。</span><span class="sxs-lookup"><span data-stu-id="cbf41-219">You can also install and enable ASP.NET Web API Tracing in any application using the ASP.NET Web API Tracing NuGet package ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).</span></span>

<span data-ttu-id="cbf41-220">詳細については、構成して、ASP.NET Web API Tracing を使用して、次を参照してください。 [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-220">For more information on configuring and using ASP.NET Web API Tracing see [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).</span></span>

#### <a name="aspnet-web-api-help-page"></a><span data-ttu-id="cbf41-221">ASP.NET Web API Help Page</span><span class="sxs-lookup"><span data-stu-id="cbf41-221">ASP.NET Web API Help Page</span></span>

<span data-ttu-id="cbf41-222">既定では、Web API プロジェクト テンプレートで、ASP.NET Web API のヘルプ ページが含まれているようになりました。</span><span class="sxs-lookup"><span data-stu-id="cbf41-222">The ASP.NET Web API Help Page is now included by default in the Web API project template.</span></span> <span data-ttu-id="cbf41-223">ASP.NET Web API のヘルプ ページには、web Api HTTP エンドポイント、サポートされる HTTP メソッド、パラメーターおよび例要求と応答メッセージのペイロードをなどのドキュメントが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-223">The ASP.NET Web API Help Page automatically generates documentation for web APIs including the HTTP endpoints, the supported HTTP methods, parameters and example request and response message payloads.</span></span> <span data-ttu-id="cbf41-224">ドキュメントは自動的に、コードのコメントからプルされなくなります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-224">Documentation is automatically pulled from comments in your code.</span></span> <span data-ttu-id="cbf41-225">ASP.NET Web API のヘルプ ページ NuGet パッケージを使用しているアプリケーションを ASP.NET Web API のヘルプ ページを追加することもできます ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage))。</span><span class="sxs-lookup"><span data-stu-id="cbf41-225">You can also add the ASP.NET Web API Help Page to any application using the ASP.NET Web API Help Page NuGet package ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).</span></span>

<span data-ttu-id="cbf41-226">詳細については、セットアップと ASP.NET Web API のヘルプ ページを参照してくださいをカスタマイズ[https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-226">For more information on setting up and customizing the ASP.NET Web API Help Page see [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).</span></span>

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a><span data-ttu-id="cbf41-227">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="cbf41-227">ASP.NET SignalR</span></span>

<span data-ttu-id="cbf41-228">ASP.NET SignalR を簡単に、ASP.NET アプリケーションにリアルタイム web 機能を追加可能な場合は Websocket を使用して、自動的にフォールバックし他の手法でない場合。</span><span class="sxs-lookup"><span data-stu-id="cbf41-228">ASP.NET SignalR makes it simple to add real-time web capabilities to your ASP.NET application, using WebSockets if available and automatically falling back to other techniques when it isn't.</span></span>

<span data-ttu-id="cbf41-229">ASP.NET SignalR を使用する方法については「 [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-229">For more information on using ASP.NET SignalR see [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).</span></span>

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a><span data-ttu-id="cbf41-230">ASP.NET Friendly Url</span><span class="sxs-lookup"><span data-stu-id="cbf41-230">ASP.NET Friendly URLs</span></span>

<span data-ttu-id="cbf41-231">ASP.NET FriendlyURLs なりますクリーナー Url を検索 (.aspx 拡張子なし) を生成する web フォーム開発者にとって非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-231">ASP.NET FriendlyURLs makes it very easy for web forms developers to generate cleaner looking URLs(without the .aspx extension).</span></span> <span data-ttu-id="cbf41-232">No にほとんどの構成が必要ですし、既存の ASP.NET v4.0 アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-232">It requires little to no configuration and can be used with existing ASP.NET v4.0 applications.</span></span> <span data-ttu-id="cbf41-233">FriendlyURLs 機能では、デスクトップおよびモバイル ビュー間の切り替えをサポートすることにより、アプリケーションにモバイル デバイスのサポートを追加する開発者を簡単にします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-233">The FriendlyURLs feature also makes it easier for developers to add mobile support to their applications, by supporting switching between desktop and mobile views.</span></span>

<span data-ttu-id="cbf41-234">詳細については、インストールして、ASP.NET Friendly Url を使用して、次を参照してください。 [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-234">For more information on installing and using ASP.NET Friendly URLs see [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).</span></span>

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="cbf41-235">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="cbf41-235">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="cbf41-236">このセクションでは、既知の問題と ASP.NET および Web ツール 2012.2 リリースでは重大な変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-236">This section describes known issues and breaking changes that are in the ASP.NET and Web Tools 2012.2 release.</span></span>

### <a name="installation-issues"></a><span data-ttu-id="cbf41-237">インストールの問題</span><span class="sxs-lookup"><span data-stu-id="cbf41-237">Installation Issues</span></span>

#### <a name="out-of-order-installs-of-visual-studio-2012"></a><span data-ttu-id="cbf41-238">Visual Studio 2012 を順序どおりにインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-238">Out of order installs of Visual Studio 2012</span></span>

<span data-ttu-id="cbf41-239">修復操作が必要になります、ASP.NET および Web ツール 2012.2 をインストールした後、追加の SKU の Visual Studio 2012 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-239">Installing an additional SKU of Visual Studio 2012 after installing the ASP.NET and Web Tools 2012.2 will require a repair operation.</span></span> <span data-ttu-id="cbf41-240">次の順序を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="cbf41-240">Consider the following sequence:</span></span>

1. <span data-ttu-id="cbf41-241">Visual Studio 2012 Express for Web をインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-241">Install Visual Studio 2012 Express for Web</span></span>
2. <span data-ttu-id="cbf41-242">ASP.NET および Web ツール 2012.2 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-242">Install ASP.NET and Web Tools 2012.2</span></span>
3. <span data-ttu-id="cbf41-243">Visual Studio 2012 Professional、Premium または Ultimate をインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-243">Install Visual Studio 2012 Professional, Premium or Ultimate</span></span>

<span data-ttu-id="cbf41-244">手順 2 は、Express for Web の更新プログラムのインストールでのみなります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-244">Step 2 would only result in installing updates for Express for Web.</span></span> <span data-ttu-id="cbf41-245">手順 3 をインストールする追加の SKU に、更新プログラムが含まれていることを確認するには、インストールされている最後の SKU の更新プログラムをインストールするには、ASP.NET および Web ツール 2012.2 を修復する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-245">To ensure that the additional SKU installed during step 3 contains the update you will need to repair the ASP.NET and Web Tools 2012.2 to install the updates for the last SKU installed.</span></span> <span data-ttu-id="cbf41-246">これは、手順 1. で Sku と 3 が取り消された場合にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-246">This also applies if the SKUs in Step 1 and 3 are reversed.</span></span>

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a><span data-ttu-id="cbf41-247">Visual Studio を開いている場合は、Microsoft ASP.NET および Web ツール 2012.2 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-247">Installing Microsoft ASP.NET and Web Tools 2012.2 when Visual Studio is open</span></span>

<span data-ttu-id="cbf41-248">Microsoft ASP.NET および Web ツール 2012.2 のインストール中に VS が開かれている場合、Visual Studio が正しくない状態に至る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-248">If VS is open during installation of Microsoft ASP.NET and Web Tools 2012.2, Visual Studio might end up in a bad state.</span></span> <span data-ttu-id="cbf41-249">ユーザーがインストールを続行する前に Visual Studio のすべてのインスタンスを閉じることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-249">It is recommended that users close all instances of Visual Studio before proceeding with install.</span></span>

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a><span data-ttu-id="cbf41-250">インストールの途中で ASP.NET および Web ツール 2012.2 のセットアップを取り消す</span><span class="sxs-lookup"><span data-stu-id="cbf41-250">Canceling ASP.NET and Web Tools 2012.2 setup in the middle of installation</span></span>

<span data-ttu-id="cbf41-251">ASP.NET と Web ツール 2012.2 取り消しセットアップ インストールの途中では状態のままに Visual Studio が正しくありません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-251">Canceling ASP.NET and Web Tools 2012.2 setup in the middle of installation will leave Visual Studio in a bad state.</span></span> <span data-ttu-id="cbf41-252">次の手順をこの問題に従ってくださいに対処します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-252">To address this problem follow these steps:</span></span> 

- <span data-ttu-id="cbf41-253">プログラムの削除 を追加するには、移動してください。</span><span class="sxs-lookup"><span data-stu-id="cbf41-253">Go to Add Remove Programs</span></span>
- <span data-ttu-id="cbf41-254">存在する場合は、Microsoft ASP.NET および Web ツール 2012.2 をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-254">Uninstall Microsoft ASP.NET and Web Tools 2012.2, if present.</span></span>
- <span data-ttu-id="cbf41-255">Microsoft ASP.NET および Web ツール 2012.2 を再インストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-255">Reinstall Microsoft ASP.NET and Web Tools 2012.2</span></span>

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a><span data-ttu-id="cbf41-256">ASP.NET および Web ツール 2012.2 ASP.NET MVC 4 をアンインストールした後にテンプレートおよび Razor v2 の Web サイト テンプレートが表示されません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-256">After uninstalling ASP.NET and Web Tools 2012.2 the ASP.NET MVC 4 templates and Razor v2 Web Site templates are missing</span></span>

<span data-ttu-id="cbf41-257">ASP.NET および Web ツール 2012.2 のアンインストールもアンインストールされますすべて ASP.NET MVC 4、Razor v2 の Web サイト テンプレートの Visual Studio 2012 からです。</span><span class="sxs-lookup"><span data-stu-id="cbf41-257">Uninstalling ASP.NET and Web Tools 2012.2 will also uninstall all of ASP.NET MVC 4 and Razor v2 Web Site templates from Visual Studio 2012.</span></span>

<span data-ttu-id="cbf41-258">回避策では、ASP.NET MVC 4、Razor v2 の Web サイト テンプレートを再インストール、Visual Studio 2012 インストールを修復します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-258">The workaround is to repair your Visual Studio 2012 installation to reinstall ASP.NET MVC 4 and Razor v2 Web Site templates.</span></span>

### <a name="tooling-issues"></a><span data-ttu-id="cbf41-259">ツールの問題</span><span class="sxs-lookup"><span data-stu-id="cbf41-259">Tooling Issues</span></span>

#### <a name="nuget-error-reported-during-project-creation"></a><span data-ttu-id="cbf41-260">プロジェクト作成時に NuGet のエラー</span><span class="sxs-lookup"><span data-stu-id="cbf41-260">NuGet error reported during project creation</span></span>

<span data-ttu-id="cbf41-261">ASP.NET および Web ツール 2012.2 をインストールした後、MVC 4 プロジェクトを作成するときに、次のエラーを表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-261">After installing ASP.NET and Web Tools 2012.2 you may see the following error when creating an MVC 4 project</span></span>

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

<span data-ttu-id="cbf41-262">ASP.NET および Web ツール 2012.2 NuGet 2.1 に付属し、Visual Studio 2012 での拡張機能がアップグレードされます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-262">The ASP.NET and Web Tools 2012.2 ships NuGet 2.1 and will upgrade the extension in Visual Studio 2012.</span></span> <span data-ttu-id="cbf41-263">場合によっては、VSIX インストーラーは、VSIX を正しく更新に失敗します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-263">In some cases, the VSIX installer will fail to correctly update the VSIX.</span></span> <span data-ttu-id="cbf41-264">次の手順ではこの問題に対処することを許可します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-264">The following steps will allow you to address this problem:</span></span>

1. <span data-ttu-id="cbf41-265">管理者として Visual Studio 2012 を起動します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-265">Start Visual Studio 2012 as an Administrator</span></span>
2. <span data-ttu-id="cbf41-266">ツールに移動して&gt;拡張機能と更新プログラムおよび NuGet をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-266">Go to Tools-&gt;Extensions and Updates and uninstall NuGet.</span></span>
3. <span data-ttu-id="cbf41-267">Visual Studio を閉じます。</span><span class="sxs-lookup"><span data-stu-id="cbf41-267">Close Visual Studio</span></span>
4. <span data-ttu-id="cbf41-268">ASP.NET および Web ツール 2012.2 インストール フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-268">Navigate to the ASP.NET and Web Tools 2012.2 installation folder:</span></span>

    1. <span data-ttu-id="cbf41-269">Visual Studio 2012: **\microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012 のプログラミング**</span><span class="sxs-lookup"><span data-stu-id="cbf41-269">For Visual Studio 2012: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**</span></span>
    2. <span data-ttu-id="cbf41-270">Visual Studio 2012 Express for Web の: **Program files \microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**</span><span class="sxs-lookup"><span data-stu-id="cbf41-270">For Visual Studio 2012 Express for Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 for Web**</span></span>
5. <span data-ttu-id="cbf41-271">NuGet を再インストールする NuGet.Tools.vsix をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-271">Double click on the NuGet.Tools.vsix to reinstall NuGet</span></span>

### <a name="web-api-issues"></a><span data-ttu-id="cbf41-272">Web API の問題</span><span class="sxs-lookup"><span data-stu-id="cbf41-272">Web API Issues</span></span>

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a><span data-ttu-id="cbf41-273">$Filter と日付時刻リテラルの解析を行って問題</span><span class="sxs-lookup"><span data-stu-id="cbf41-273">Parsing issues in $filter and DateTime literals</span></span>

<span data-ttu-id="cbf41-274">OData URI パーサーは、部分的な datetime リテラルを正しく解析に失敗します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-274">The OData URI parser fails to parse partial datetime literals properly.</span></span> <span data-ttu-id="cbf41-275">たとえば、$filter = 開始 eq datetime'2012-12-31T12:00' を正しく解析できない場合します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-275">For example, $filter=start eq datetime'2012-12-31T12:00' fails to parse properly.</span></span> <span data-ttu-id="cbf41-276">回避策は、完全リテラル $filter を使用する = 開始 eq datetime'2012-12-31T12:00:00' です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-276">A workaround is to use the full literal, $filter=start eq datetime'2012-12-31T12:00:00'.</span></span>

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a><span data-ttu-id="cbf41-277">OData は、大文字と小文字のプロパティ名をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-277">OData doesn't support case-insensitive property names.</span></span>

<span data-ttu-id="cbf41-278">OData は、OData クエリおよび odata パスの大文字と小文字のプロパティ名をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-278">OData doesn't support case-insensitive property names in OData queries and odata path.</span></span> <span data-ttu-id="cbf41-279">作業項目を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cbf41-279">See workitems:</span></span>

- [<span data-ttu-id="cbf41-280">http://aspnetwebstack.codeplex.com/workitem/366</span><span class="sxs-lookup"><span data-stu-id="cbf41-280">http://aspnetwebstack.codeplex.com/workitem/366</span></span>](http://aspnetwebstack.codeplex.com/workitem/366)
- [<span data-ttu-id="cbf41-281">http://aspnetwebstack.codeplex.com/workitem/704</span><span class="sxs-lookup"><span data-stu-id="cbf41-281">http://aspnetwebstack.codeplex.com/workitem/704</span></span>](http://aspnetwebstack.codeplex.com/workitem/704)

<span data-ttu-id="cbf41-282">ユーザーが javascript クライアント側とサーバー側で大文字小文字が異なる場合、おそらくこの問題は発生します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-282">If users have different casing on javascript client side and server side, they probably will encounter this issue.</span></span> <span data-ttu-id="cbf41-283">この問題は odata プロトコルの仕様です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-283">This issue is by design in odata protocol.</span></span> <span data-ttu-id="cbf41-284">ただし、多くのユーザーは、この問題を報告します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-284">However, many users reports this issue.</span></span> <span data-ttu-id="cbf41-285">問題を回避するには、ユーザーは URL 内のケースを修正する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-285">To work around it, users have to correct their cases in URL.</span></span>

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a><span data-ttu-id="cbf41-286">既定の OData ルーティング規約は、ナビゲーション プロパティに POST または PUT をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-286">Default OData routing conventions doesn't support POST/PUT on navigation property.</span></span>

<span data-ttu-id="cbf41-287">既定の OData ルーティング規約は、ナビゲーション プロパティに POST または PUT をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-287">Default OData routing conventions doesn't support POST/PUT on navigation property.</span></span> <span data-ttu-id="cbf41-288">作業項目を参照してください[http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-288">See workitem [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366).</span></span> <span data-ttu-id="cbf41-289">既定の規則でよく使用されるこの規約おがありません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-289">We are missing this commonly used convention in default conventions.</span></span>

<span data-ttu-id="cbf41-290">これを回避するには、ユーザーをサポートするように新しいルーティング規約を拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-290">To work around it, users need to extend new routing convention to support it.</span></span>

### <a name="facebook-template-issues"></a><span data-ttu-id="cbf41-291">Facebook テンプレートの問題</span><span class="sxs-lookup"><span data-stu-id="cbf41-291">Facebook Template Issues</span></span>

#### <a name="facebook-application-template-only-works-using-net-45"></a><span data-ttu-id="cbf41-292">.NET 4.5 を使用する Facebook Application テンプレートとのみ動作します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-292">Facebook Application template only works using .NET 4.5</span></span>

<span data-ttu-id="cbf41-293">.NET 4.5 を選択して、フレームワーク ドロップダウン リストで、新しいプロジェクト ダイアログ ボックスに ASP.NET MVC 4 の Facebook アプリケーション テンプレートを参照してください。</span><span class="sxs-lookup"><span data-stu-id="cbf41-293">You must select .NET 4.5 in the framework dropdown list in the New Project dialog to see the Facebook Application template in ASP.NET MVC 4.</span></span>

#### <a name="real-time-update-controller"></a><span data-ttu-id="cbf41-294">コント ローラーのリアルタイムの更新</span><span class="sxs-lookup"><span data-stu-id="cbf41-294">Real-time Update Controller</span></span>

<span data-ttu-id="cbf41-295">Facebook Application テンプレートにより、簡単にユーザーの Facebook からのリアルタイムの更新を処理する Web API コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-295">The Facebook Application template allows user easily create a Web API Controller to handle real-time updates from Facebook.</span></span> <span data-ttu-id="cbf41-296">開発用コンピューターが NAT の内側にある場合は、コント ローラーがネットワーク構成を変更しない動作しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cbf41-296">If your development machine is behind NAT, your Controller may not work without further network configuration.</span></span> <span data-ttu-id="cbf41-297">詳細については、ここを参照してください: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)</span><span class="sxs-lookup"><span data-stu-id="cbf41-297">See here for details: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)</span></span>

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a><span data-ttu-id="cbf41-298">文字列値が競合して Facebook OAuth パラメーターを持つクエリ</span><span class="sxs-lookup"><span data-stu-id="cbf41-298">Query string values conflict with Facebook OAuth parameters</span></span>

<span data-ttu-id="cbf41-299">Facebook OAuth ダイアログ ボックスの呼び出しと競合する、次のフィールドの URL をバックアップします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-299">The following fields conflict with Facebook OAuth dialog's call back URL.</span></span> <span data-ttu-id="cbf41-300">次の名前、独自のクエリ文字列値を追加しないでください。 コード、エラー、エラー\_説明、エラー\_理由。</span><span class="sxs-lookup"><span data-stu-id="cbf41-300">Do not add your own query string values with the following names: code, error, error\_description, error\_reason.</span></span>

#### <a name="using-page-inspector-with-facebook-template"></a><span data-ttu-id="cbf41-301">Facebook テンプレートでの Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="cbf41-301">Using Page Inspector with Facebook Template</span></span>

<span data-ttu-id="cbf41-302">Facebook アプリケーションのデバッグ中に Visual Studio 2012 では、Page Inspector の機能を使うことはできません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-302">You can't use the Page Inspector feature in Visual Studio 2012 while debugging your Facebook Application.</span></span> <span data-ttu-id="cbf41-303">Page Inspector は、iframe を現在サポートしていません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-303">The Page Inspector does not currently support iframes.</span></span>

### <a name="single-page-application-template-issues"></a><span data-ttu-id="cbf41-304">単一ページ アプリケーション テンプレートの問題</span><span class="sxs-lookup"><span data-stu-id="cbf41-304">Single Page Application Template Issues</span></span>

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a><span data-ttu-id="cbf41-305">JQuery 1.9/Knockout 2.2.1 update を既定の MVC SPA プロジェクト、新しい todo 項目の編集を実行している入力のフォーカス イベントが正しく処理されません。</span><span class="sxs-lookup"><span data-stu-id="cbf41-305">With JQuery 1.9/Knockout 2.2.1 update, when running default MVC SPA project, new todo item edit enter focus event is not handled properly.</span></span>

<span data-ttu-id="cbf41-306">JQuery 1.9/Knockout 2.2.1 更新プログラムが既定の MVC SPA プロジェクトを実行するときに新しい todo 項目の編集を不要になったフォーカス新しい todo 項目の編集ボックスに戻る、todo リストに、新しい todo 項目を入力した後にします。</span><span class="sxs-lookup"><span data-stu-id="cbf41-306">With JQuery 1.9/Knockout 2.2.1 update, when running default MVC SPA project, new todo item edit enter no longer focus back to the new todo item edit box after entering the new todo item to the todo list.</span></span>

<span data-ttu-id="cbf41-307">回避策の参照を[http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)のような修正プログラムを次のサンプル コード。</span><span class="sxs-lookup"><span data-stu-id="cbf41-307">To workaround reference [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html), and make similar fix to the following sample code:</span></span>

<span data-ttu-id="cbf41-308">ファイル todo.model.js</span><span class="sxs-lookup"><span data-stu-id="cbf41-308">File todo.model.js</span></span>  
 <span data-ttu-id="cbf41-309">関数の todolist(data)、追加以下。</span><span class="sxs-lookup"><span data-stu-id="cbf41-309">function todolist(data), add following:</span></span>  
 <span data-ttu-id="cbf41-310">**self.isSelected ko.observable(false); を =**</span><span class="sxs-lookup"><span data-stu-id="cbf41-310">**self.isSelected = ko.observable(false);**</span></span>

<span data-ttu-id="cbf41-311">次の黒くテキストを追加、todoList.prototype.addTodo を関数します。</span><span class="sxs-lookup"><span data-stu-id="cbf41-311">function todoList.prototype.addTodo, add the following blacked text:</span></span>  
 <span data-ttu-id="cbf41-312">**self.isSelected(true) です。**</span><span class="sxs-lookup"><span data-stu-id="cbf41-312">**self.isSelected(true);**</span></span>  
 <span data-ttu-id="cbf41-313">self.newTodoTitle (&quot;&quot;) です。</span><span class="sxs-lookup"><span data-stu-id="cbf41-313">self.newTodoTitle(&quot;&quot;);</span></span>

<span data-ttu-id="cbf41-314">Index.cshtml をファイルに追加し、次の黒くテキスト。</span><span class="sxs-lookup"><span data-stu-id="cbf41-314">File index.cshtml, add the following blacked text:</span></span>  
 <span data-ttu-id="cbf41-315">&lt;データ バインド フォーム =&quot;送信: addTodo&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="cbf41-315">&lt;form data-bind=&quot;submit: addTodo&quot;&gt;</span></span>  
 <span data-ttu-id="cbf41-316">&lt;クラスの入力 =&quot;addTodo&quot;型 =&quot;テキスト&quot;データ バインド =&quot;値: newTodoTitle、プレース ホルダー: '型は、ここに追加する'、blurOnEnter: true の場合、**できる: isSelected**、イベント: {ぼかし: addTodo}&quot; /&gt;</span><span class="sxs-lookup"><span data-stu-id="cbf41-316">&lt;input class=&quot;addTodo&quot; type=&quot;text&quot; data-bind=&quot;value: newTodoTitle, placeholder: 'Type here to add', blurOnEnter: true, **hasfocus: isSelected**, event: { blur: addTodo }&quot; /&gt;</span></span>  
 <span data-ttu-id="cbf41-317">&lt;/form&gt;</span><span class="sxs-lookup"><span data-stu-id="cbf41-317">&lt;/form&gt;</span></span>
