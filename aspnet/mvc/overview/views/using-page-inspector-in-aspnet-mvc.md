---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "ASP.NET MVC での Page Inspector の使用 |Microsoft ドキュメント"
author: rick-anderson
description: "Visual Studio 2012 での Page Inspector は、統合されたブラウザーと web 開発ツールです。 統合のブラウザーと Page Inspector i でいずれかの要素を選択してください."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="b02ec-104">ASP.NET MVC フォーム内での Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="b02ec-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="b02ec-105">Tim Ammann によって</span><span class="sxs-lookup"><span data-stu-id="b02ec-105">by Tim Ammann</span></span>

> <span data-ttu-id="b02ec-106">Visual Studio 2012 での Page Inspector は、統合されたブラウザーと web 開発ツールです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="b02ec-107">統合のブラウザー内で任意の要素を選択し、Page Inspector は、要素のソースと CSS に即座に強調表示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="b02ec-108">任意の MVC ビューの参照、すばやくのレンダリングされたマークアップのソースを検索し、右、Visual Studio 環境内でブラウザー ツールを使用します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="b02ec-109">ビデオを見る</span><span class="sxs-lookup"><span data-stu-id="b02ec-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="b02ec-110">このチュートリアルでは、検査モードを有効にしてからすばやく検索して、web プロジェクト内のマークアップと CSS を編集する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="b02ec-111">チュートリアルは、MVC プロジェクトを使用しますの Page Inspector を使用することもできます。 [Web フォーム](https://go.microsoft.com/?linkid=9802001)およびその他の ASP.NET アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="b02ec-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="b02ec-112">このチュートリアルでは、次のセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="b02ec-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="b02ec-113">前提条件</span><span class="sxs-lookup"><span data-stu-id="b02ec-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="b02ec-114">Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="b02ec-115">ビューへの参照に Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="b02ec-116">検査モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="b02ec-117">Page Inspector を使用するマークアップを変更する</span><span class="sxs-lookup"><span data-stu-id="b02ec-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - <span data-ttu-id="b02ec-118">[検査モードと [HTML] ウィンドウ](#_6_inspection_mode)</span><span class="sxs-lookup"><span data-stu-id="b02ec-118">[Inspection Mode and the HTML Window](#_6_inspection_mode)</span></span>
> - [<span data-ttu-id="b02ec-119">スタイルのウィンドウに CSS 変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="b02ec-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="b02ec-120">CSS 自動同期</span><span class="sxs-lookup"><span data-stu-id="b02ec-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="b02ec-121">CSS カラー ピッカーを使用します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="b02ec-122">JavaScript への動的なページ要素のマッピング</span><span class="sxs-lookup"><span data-stu-id="b02ec-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="b02ec-123">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b02ec-123">Prerequisites</span></span>

- <span data-ttu-id="b02ec-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)または[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="b02ec-125">Page Inspector の最新バージョンを取得する[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) for .NET 2.0、Windows Azure SDK をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="b02ec-126">Page Inspector には、Microsoft Web Developer Tools が付属しています。</span><span class="sxs-lookup"><span data-stu-id="b02ec-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="b02ec-127">最新バージョンでは、1.3 です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-127">The latest version is 1.3.</span></span> <span data-ttu-id="b02ec-128">どのバージョンを確認する、Visual Studio を実行して選択**に関する Microsoft クトリ**から、**ヘルプ**メニュー。</span><span class="sxs-lookup"><span data-stu-id="b02ec-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="b02ec-129">Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-129">Create a Web Application</span></span>

<span data-ttu-id="b02ec-130">最初に、Page Inspector を使用する web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="b02ec-131">Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="b02ec-132">展開し、左側の**Visual c#** **Web**、し、 **ASP.NET MVC4 Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="b02ec-134">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-134">Click **OK**.</span></span>

<span data-ttu-id="b02ec-135">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="b02ec-136">ままにして**Razor**既定のビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="b02ec-136">Leave **Razor** as the default view engine.</span></span>

![新しい ASP.NET MVC プロジェクトのインターネット アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="b02ec-138">アプリケーションが開かれた**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="b02ec-138">The application opens in **Source** view.</span></span>

![ソース ビューで新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="b02ec-140">Page Inspector を使用するとを使用するアプリケーションがある場合は、これで、ことを確認し、それを変更します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="b02ec-141">ビューへの参照に Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="b02ec-142">Visual Studio 2012 での任意のビューを右プロジェクトで、 **Page Inspector で表示**、Page Inspector は、ルートを図し、ページを表示するとします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="b02ec-143">**ソリューション エクスプ ローラー**、展開、**ビュー**フォルダーし、**ホーム**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="b02ec-144">Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Page Inspector 内でビュー Index.cshtml](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="b02ec-146">既定では、Page Inspector がドッキングされるウィンドウとして、Visual Studio 環境の左側にあります。</span><span class="sxs-lookup"><span data-stu-id="b02ec-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="b02ec-147">場合は、他の場所にドッキングしたり、ウィンドウのドッキングを解除できます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="b02ec-148">参照してください[する方法: を整列およびドッキング Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="b02ec-149">Page Inspector ウィンドウの上部ペインには、ブラウザーのウィンドウで、現在のページを示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="b02ec-150">下のペインは、ページのさまざまな側面を調査することのできるいくつかのタブと、HTML マークアップ内のページを示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="b02ec-151">下のペインがに似ていますが、 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)Internet Explorer でします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Page Inspector 内での ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="b02ec-153">このチュートリアルでは使用して、 **HTML**と**スタイル**のタブにすばやく移動し、アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="b02ec-154">EnableInspection モード</span><span class="sxs-lookup"><span data-stu-id="b02ec-154">EnableInspection Mode</span></span>

<span data-ttu-id="b02ec-155">Page Inspector を検査モードにする をクリックして、**検査**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="b02ec-156">検査モードで表示されたページの任意の部分にマウス ポインターを置いた場合、対応するソース マークアップまたはコードが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![検査モードを切り替える](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="b02ec-158">これで Page Inspector 内のページのさまざまな部分にマウスを移動します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="b02ec-159">マウス ポインターが大きなのプラス記号を変更し、下にある要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![ポインターを置いた div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="b02ec-161">マウス ポインターを移動すると、Visual Studio には、ソース ファイル内の対応する、Razor 構文が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="b02ec-162">別のソース ファイルから HTML 要素の場合、Visual Studio は自動的にファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="b02ec-163">Page Inspector 内で、 **HTML**タブは、Razor 構文から生成された HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="b02ec-164">マウス ポインターを移動すると、HTML 要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="b02ec-165">**スタイル** タブは、要素に対する CSS 規則を示しています。</span><span class="sxs-lookup"><span data-stu-id="b02ec-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="b02ec-166">Page Inspector を使用するマークアップを変更する</span><span class="sxs-lookup"><span data-stu-id="b02ec-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="b02ec-167">Page Inspector では、その位置を判断できない可能性がありますマークアップを検索できます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="b02ec-168">マークアップを変更し、結果として得られる変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="b02ec-169">これを見るには、をクリックして**検査**し、Page Inspector ウィンドウ内のページの下部までスクロールします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="b02ec-170">Page Inspector が開き、フッター領域にマウス ポインターを移動すると、 \_Layout.cshtml ファイルし、選択したレイアウト ページのセクションを強調表示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="b02ec-171">表示されるフッターはレイアウト ファイルおよびビュー自体で定義されています。</span><span class="sxs-lookup"><span data-stu-id="b02ec-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![[フッター]](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="b02ec-173">今すぐ著作権では、行にマウス ポインターを移動<a id="a"></a>に注意してください。</span><span class="sxs-lookup"><span data-stu-id="b02ec-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="b02ec-174">\_Layout.cshtml ページで、対応する行が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![ページ フッターの著作権行が強調表示されます。](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="b02ec-176">内の行の末尾にいくつかのテキストを追加、 \_Layout.cshtml ファイル。</span><span class="sxs-lookup"><span data-stu-id="b02ec-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="b02ec-177">&lt;p&gt;&amp;コピーです。@DateTime.Now.Year -My ASP.NET MVC アプリケーションの!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="b02ec-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="b02ec-178">ここで、Ctrl + Alt + Enter を押すか、Page Inspector のブラウザーのウィンドウで結果を表示する更新バー をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET アプリケーションのものです。](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="b02ec-180">Index.cshtml で定義されているフッターが内にあるになっていることを考えたかもしれません、 \_Layout.cshtml、および Page Inspector を検出します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="b02ec-181">検査モードと [HTML] ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="b02ec-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="b02ec-182">次に、簡単に見て、HTML ウィンドウとどのようにするための要素をマップするがあります。</span><span class="sxs-lookup"><span data-stu-id="b02ec-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="b02ec-183">をクリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b02ec-184">"Logohere、"と表示されているページの上部をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="b02ec-185">詳細については、マウス ポインターを移動すると、ブラウザー ウィンドウに表示が不要になった変更の特定の要素を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="b02ec-186">これで、マウス ポインターを移動する、 **HTML**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="b02ec-187">Page Inspector 内の要素の概要、マウス ポインターを移動すると、 **HTML**ウィンドウとブラウザーのウィンドウで、対応する要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML ウィンドウ](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="b02ec-189">Page Inspector を開く前に、 \_Layout.cshtml ファイルが一時的なタブにします。クリックして、\_で、Layout.cshtml 一時 タブ、および対応するマークアップが強調表示されますが、&lt;ヘッダー&gt;のセクション。</span><span class="sxs-lookup"><span data-stu-id="b02ec-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![強調表示されたマークアップ](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="b02ec-191">スタイルのウィンドウに CSS 変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="b02ec-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="b02ec-192">Page Inspector を使用して、次に、**スタイル**CSS に変更をプレビューするウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="b02ec-193">をクリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b02ec-194">Page Inspector のブラウザーのウィンドウでマウス ポインターを移動まで「ホーム ページ」セクションの上、 **div.content ラッパー**ラベルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![ポインターを置いた div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="b02ec-196">Div.content ラッパー セクション内に 1 回クリックしにマウス ポインターを移動、**スタイル**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="b02ec-197">**Syles**すべてこの要素に対する CSS 規則のウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="b02ec-198">検索 .featured <model>.content ラッパー クラス セレクターまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="b02ec-199">背景色のプロパティのチェック ボックスをオフようになりました。</span><span class="sxs-lookup"><span data-stu-id="b02ec-199">Now clear the checkbox for the background-color property.</span></span>

![オフの背景色](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="b02ec-201">どの変更をプレビュー即座に Page Inspector のブラウザー ウィンドウに注目してください。</span><span class="sxs-lookup"><span data-stu-id="b02ec-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="b02ec-202">もう一度チェック ボックスをオン、プロパティ値をダブルクリックして、赤に変更します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="b02ec-203">変更をすぐに示しています。</span><span class="sxs-lookup"><span data-stu-id="b02ec-203">The change shows immediately:</span></span>

![赤の背景色](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="b02ec-205">**スタイル**スタイルに変更をコミットする前に変更が簡単にテストし、CSS のプレビューにウィンドウがシート自体です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="b02ec-206">CSS 自動同期</span><span class="sxs-lookup"><span data-stu-id="b02ec-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="b02ec-207">この機能には、Page Inspector のバージョン 1.3 が必要です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="b02ec-208">CSS 自動同期機能を使用すると、CSS ファイルを直接編集し、Page Inspector のブラウザーですぐに変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="b02ec-209">をクリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b02ec-210">Page Inspector のブラウザーの「ホーム ページ」セクションまでマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="b02ec-211">この要素を選択するには、1 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-211">Click once to select this element.</span></span>

<span data-ttu-id="b02ec-212">**Syles**すべてこの要素に対する CSS 規則のウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="b02ec-213">検索 .featured <model>.content ラッパー クラス セレクターまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="b02ec-214">「.Featured <model>.content ラッパー」をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="b02ec-215">Page Inspector は、このスタイル (Site.css) を定義して対応する CSS スタイルを強調表示する CSS ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="b02ec-216">値を変更するようになりました`background-color`"red"にします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="b02ec-217">変更は、Page Inspector のブラウザーにすぐに表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="b02ec-218">CSS カラー ピッカーを使用します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="b02ec-219">Visual Studio 2012 で CSS エディターには、カラー ピッカーを容易に選択し、色を挿入するものがあります。</span><span class="sxs-lookup"><span data-stu-id="b02ec-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="b02ec-220">カラー ピッカーは、標準色パレットが含まれています、標準的な色の名前、ハッシュ コード、RGB、RGBA、HSL、および HSLA の色をサポートし、ドキュメントで最も最近使用した色の一覧を保持します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="b02ec-221">値を変更して、前のセクションで、`background-color`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="b02ec-222">カラー ピッカーを起動するには、プロパティの名前と型の後にカーソルを置きます **#** または**rgb (**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![CSS カラー ピッカー バー](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="b02ec-224">色を選択して、または下方向キーを押してクリックし、左と右方向キーを使用して、色を走査します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="b02ec-225">アクセスすると、色、対応する 16 進値には次のプレビューが行われます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![プレビューの背景色プロパティ値](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="b02ec-227">カラー バーには、目的の正確な色が割り当てられていない、カラー ピッカーの pop ダウンを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="b02ec-228">開くにはをカラー バーの右端にある二重山かっこをクリックしてまたはキーボードの下矢印を 1、2 回押します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS カラー ピッカーの Pop ダウン](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="b02ec-230">右側の垂直バーから色をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="b02ec-231">これにより、その色のグラデーションがメイン ウィンドウで表示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="b02ec-232">、Enter キーを押して、垂直バーから直接色を選択するか、任意の時点より高い精度でを選択するメイン ウィンドウでをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="b02ec-233">使用するコンピューターの画面で、色があるかどうか (がない、Visual Studio ユーザー インターフェイスの中にある)、右下にあるスポイト ツールを使用してその値をキャプチャすることができます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="b02ec-234">また、色の不透明度をカラー ピッカーの下部にあるスライダーを移動することによって変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="b02ec-235">そうカラー RGBA 値に値を変更、RGBA 形式は、不透明度を表すことができるためです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="b02ec-236">色を選択したら、Enter キーを押しますし、セミコロンの背景色の入力を補完する、 *Site.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b02ec-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="b02ec-237">ページのインスペクター更新バー</span><span class="sxs-lookup"><span data-stu-id="b02ec-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="b02ec-238">Page Inspector への変更をすぐに検出、 *Site.css*更新バーで、アラートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![更新バー](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="b02ec-240">すべてのファイルを保存して Page Inspector のブラウザーを更新して、Ctrl + Alt + Enter キーを押します。 または [更新バー] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="b02ec-241">強調表示色の変更は、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="b02ec-242">JavaScript への動的なページ要素のマッピング</span><span class="sxs-lookup"><span data-stu-id="b02ec-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="b02ec-243">最新の web アプリケーションで、ページ内の要素は、多くの場合、動的に生成 JavaScript で。</span><span class="sxs-lookup"><span data-stu-id="b02ec-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="b02ec-244">つまり、これらのページ要素に対応する static のマークアップ (HTML または Razor) はありません。</span><span class="sxs-lookup"><span data-stu-id="b02ec-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="b02ec-245">バージョン 1.3 では、Page Inspector はページを対応する JavaScript コードに動的に追加された項目を今すぐマップできます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="b02ec-246">この機能を示すためを使用、[単一ページ アプリケーション (SPA) テンプレート](../../../single-page-application/overview/introduction/knockoutjs-template.md)です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b02ec-247">SPA テンプレートが必要です、 [ASP.NET および Web ツール 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)を更新します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="b02ec-248">Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="b02ec-249">展開し、左側の**Visual c#** **Web**、し、 **ASP.NET MVC4 Web アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="b02ec-250">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-250">Click **OK**.</span></span>

<span data-ttu-id="b02ec-251">**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、 **Single Page Application**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="b02ec-252">ソリューション エクスプ ローラーで、展開、**ビュー**フォルダーし、**ホーム**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="b02ec-253">Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**です。</span><span class="sxs-lookup"><span data-stu-id="b02ec-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="b02ec-254">Page Inspector のブラウザーでは最初に表示されるには、ログイン ページを示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="b02ec-255">「サインアップ」 をクリックし、ユーザー名とパスワードを作成します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="b02ec-256">サインアップした後、アプリケーション ログに記録して、to do リスト項目を作成するいくつかのサンプルです。</span><span class="sxs-lookup"><span data-stu-id="b02ec-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="b02ec-257">をクリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="b02ec-258">Page Inspector のブラウザーでは、作業項目のいずれかをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="b02ec-259">青で強調表示されているの代わりに、要素、強調表示される"JS"では、オレンジ色で、要素名の横にあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="b02ec-260">これは、スクリプトによって、要素が動的に作成されたことを示します。</span><span class="sxs-lookup"><span data-stu-id="b02ec-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="b02ec-261">さらに、オレンジ色の下線が表示されます、**呼び出し履歴**タブです。これが示す、**呼び出し履歴**ウィンドウの詳細については、要素があります。</span><span class="sxs-lookup"><span data-stu-id="b02ec-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="b02ec-262">をクリックして、**呼び出し履歴**タブです。**呼び出し履歴**ウィンドウ要素を作成した JavaScript の呼び出しの呼び出し履歴を示しています。</span><span class="sxs-lookup"><span data-stu-id="b02ec-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="b02ec-263">呼び出す外部ライブラリ jQuery が折りたたまれている場合など、アプリケーションのスクリプトへの呼び出しを簡単に認識できるようにします。</span><span class="sxs-lookup"><span data-stu-id="b02ec-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="b02ec-264">外部のライブラリへの呼び出しなど、完全なスタックを表示するには、「外部ライブラリ」というラベルの付いたノードを展開することができます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="b02ec-265">呼び出し履歴内の項目をクリックすると、Visual Studio はコード ファイルが表示され、対応するスクリプトが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b02ec-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="b02ec-266">参照</span><span class="sxs-lookup"><span data-stu-id="b02ec-266">See Also</span></span>

<span data-ttu-id="b02ec-267">[Visual Studio での ASP.NET MVC 4 に出だし](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)(ASP.net web サイト)</span><span class="sxs-lookup"><span data-stu-id="b02ec-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="b02ec-268">[Page Inspector を導入](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/)(Channel 9 ビデオ)</span><span class="sxs-lookup"><span data-stu-id="b02ec-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="b02ec-269">[Page Inspector エラー メッセージ](https://go.microsoft.com/?linkid=9813062)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="b02ec-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
