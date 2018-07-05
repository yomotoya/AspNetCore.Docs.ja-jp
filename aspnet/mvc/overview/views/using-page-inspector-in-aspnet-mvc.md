---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: ASP.NET MVC で Page Inspector の使用 |Microsoft Docs
author: rick-anderson
description: Visual Studio 2012 での Page Inspector は、統合ブラウザーの web 開発ツールです。 統合のブラウザーと Page Inspector i で任意の要素を選択してください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 032be78e744bdf2c74337c72afb4efb7471ae4f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399144"
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="f7b4f-104">ASP.NET MVC フォーム内での Page Inspector の使用</span><span class="sxs-lookup"><span data-stu-id="f7b4f-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="f7b4f-105">Tim Ammann、</span><span class="sxs-lookup"><span data-stu-id="f7b4f-105">by Tim Ammann</span></span>

> <span data-ttu-id="f7b4f-106">Visual Studio 2012 での Page Inspector は、統合ブラウザーの web 開発ツールです。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="f7b4f-107">統合されたブラウザーで任意の要素を選択し、Page Inspector は、要素のソースと CSS にすぐに強調表示します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="f7b4f-108">任意の MVC ビューを参照、すばやくの出力されるマークアップは、のソースを検索し、Visual Studio 環境内で直接ブラウザー ツールを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="f7b4f-109">ビデオを見る</span><span class="sxs-lookup"><span data-stu-id="f7b4f-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="f7b4f-110">このチュートリアルでは、検査モードを有効にして、すばやく見つけて web プロジェクト内でマークアップと CSS を編集する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="f7b4f-111">チュートリアルは、MVC プロジェクトを使用してがの Page Inspector を使用することもできます。 [Web フォーム](https://go.microsoft.com/?linkid=9802001)と他の ASP.NET アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="f7b4f-112">このチュートリアルでは、次のセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="f7b4f-113">前提条件</span><span class="sxs-lookup"><span data-stu-id="f7b4f-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="f7b4f-114">Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="f7b4f-115">ビューを参照する Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="f7b4f-116">検査モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="f7b4f-117">Page Inspector を使用して、マークアップを変更する</span><span class="sxs-lookup"><span data-stu-id="f7b4f-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - <span data-ttu-id="f7b4f-118">[検査モードと [HTML] ウィンドウ](#_6_inspection_mode)</span><span class="sxs-lookup"><span data-stu-id="f7b4f-118">[Inspection Mode and the HTML Window](#_6_inspection_mode)</span></span>
> - [<span data-ttu-id="f7b4f-119">スタイルのウィンドウに CSS 変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="f7b4f-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="f7b4f-120">CSS 自動同期</span><span class="sxs-lookup"><span data-stu-id="f7b4f-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="f7b4f-121">CSS カラー ピッカーを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="f7b4f-122">Javascript の動的なページ要素のマッピング</span><span class="sxs-lookup"><span data-stu-id="f7b4f-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f7b4f-123">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f7b4f-123">Prerequisites</span></span>

- <span data-ttu-id="f7b4f-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)または[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="f7b4f-125">Page Inspector の最新バージョンを取得する[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) for .NET 2.0、Windows Azure SDK をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="f7b4f-126">Page Inspector には、Microsoft Web Developer Tools が付属しています。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="f7b4f-127">最新バージョンは、1.3 です。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-127">The latest version is 1.3.</span></span> <span data-ttu-id="f7b4f-128">どのバージョンを確認するが、Visual Studio を実行して**Microsoft Visual Studio**から、**ヘルプ**メニュー。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="f7b4f-129">Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-129">Create a Web Application</span></span>

<span data-ttu-id="f7b4f-130">最初に、Page Inspector を使用する web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="f7b4f-131">Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f7b4f-132">左側で、展開**Visual c#** を選択します**Web**、し、 **ASP.NET MVC4 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="f7b4f-134">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-134">Click **OK**.</span></span>

<span data-ttu-id="f7b4f-135">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、**インターネット アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="f7b4f-136">まま**Razor**既定のビュー エンジンとして。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-136">Leave **Razor** as the default view engine.</span></span>

![新しい ASP.NET MVC プロジェクトのインターネット アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="f7b4f-138">アプリケーションを開いた**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-138">The application opens in **Source** view.</span></span>

![ソース ビューで新しい ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="f7b4f-140">使用するアプリケーションがあるようになりましたことを確認および変更 Page Inspector を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="f7b4f-141">ビューを参照する Page Inspector を使用します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="f7b4f-142">Visual Studio 2012 で、プロジェクトで任意のビューを右できます**Page Inspector で表示**、Page Inspector は、ルートを図し、ページを表示するとします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="f7b4f-143">**ソリューション エクスプ ローラー**、展開、**ビュー**フォルダーをクリックし、**ホーム**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="f7b4f-144">Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Page Inspector 内での Index.cshtml を表示します。](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="f7b4f-146">既定では、Page Inspector は、Visual Studio 環境の左側にあるウィンドウとしてドッキングします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="f7b4f-147">場合は、他の場所にドッキングします。 または、ウィンドウをドッキング解除できます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="f7b4f-148">参照してください[方法: の整列し、固定 Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="f7b4f-149">Page Inspector ウィンドウの上部のペインでは、ブラウザーのウィンドウで、現在のページを示しています。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="f7b4f-150">下部のウィンドウと共に使用して、ページのさまざまな側面を検査できますいくつかのタブの HTML マークアップでページを示します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="f7b4f-151">下のペインと似ています、 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)Internet Explorer でします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Page Inspector 内での ASP.NET MVC アプリケーション](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="f7b4f-153">このチュートリアルでは使用して、 **HTML**と**スタイル**をすばやく移動し、アプリケーションに変更を加えるタブ。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="f7b4f-154">EnableInspection モード</span><span class="sxs-lookup"><span data-stu-id="f7b4f-154">EnableInspection Mode</span></span>

<span data-ttu-id="f7b4f-155">Page Inspector を検査モードにする をクリックして、**検査**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="f7b4f-156">検査モードで表示されたページの任意の部分にマウス ポインターを置くと、対応するソース マークアップまたはコードが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![検査モードの切り替え](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="f7b4f-158">これで Page Inspector 内のページのさまざまな部分にマウスを移動します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="f7b4f-159">同様、大規模のプラス記号にマウス ポインターを変更し、下にある要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![ポインターを合わせると div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="f7b4f-161">マウス ポインターを移動すると、Visual Studio には、ソース ファイル内の対応する、Razor 構文が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="f7b4f-162">別のソース ファイルから HTML 要素の場合、Visual Studio では、ファイルが自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="f7b4f-163">Page Inspector、内で、 **HTML**  タブには、Razor 構文から生成された HTML が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="f7b4f-164">マウス ポインターを移動すると、HTML 要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="f7b4f-165">**スタイル**タブには、要素に対する CSS 規則が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="f7b4f-166">Page Inspector を使用して、マークアップを変更する</span><span class="sxs-lookup"><span data-stu-id="f7b4f-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="f7b4f-167">Page Inspector を使用して、その位置を判断できない可能性がありますマークアップを検索できます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="f7b4f-168">マークアップを変更し、結果として得られる変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="f7b4f-169">次のようにクリックします。**検査**、Page Inspector ウィンドウで、ページの一番下までスクロールします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="f7b4f-170">Page Inspector を開きますフッター領域にマウス ポインターを移動すると、 \_Layout.cshtml ファイルして、選択したレイアウト ページのセクションを強調表示します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="f7b4f-171">フッターには、ご覧のとおり、レイアウト ファイル、およびビュー自体で定義されています。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![[フッター]](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="f7b4f-173">著作権では、行にマウス ポインターを移動ようになりました<a id="a"></a>に注意してください。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="f7b4f-174">\_Layout.cshtml ページで、対応する行が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![強調表示されている著作権行のフッター](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="f7b4f-176">内の行の末尾にテキストを追加、 \_Layout.cshtml ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="f7b4f-177">&lt;p&gt;&amp;コピーです。@DateTime.Now.Year -My ASP.NET MVC アプリケーション Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="f7b4f-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="f7b4f-178">ここで、Ctrl + Alt + Enter を押すか、Page Inspector のブラウザー ウィンドウで結果を表示する更新バーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET Application Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="f7b4f-180">Index.cshtml で定義されているフッターがであることが判明ことを考えたかもしれません、 \_Layout.cshtml、および Page Inspector を検出します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="f7b4f-181">検査モードと [HTML] ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="f7b4f-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="f7b4f-182">次に、簡単に見て、HTML ウィンドウとする要素をマップする方法があります。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="f7b4f-183">クリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f7b4f-184">"Logohere、"と書かれたページの上部をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="f7b4f-185">詳細については、マウス ポインターを移動すると、ブラウザー ウィンドウの表示が不要になった変更の特定の要素を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="f7b4f-186">今すぐにマウス ポインターを移動、 **HTML**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="f7b4f-187">Page Inspector 内の要素の概要、マウス ポインターを移動すると、 **HTML**ウィンドウとブラウザーのウィンドウで、対応する要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML ウィンドウ](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="f7b4f-189">前に、Page Inspector が開くと、 \_Layout.cshtml ファイルの一時的なタブ。をクリックして、\_で、Layout.cshtml の一時的なタブと、対応するマークアップが強調表示されますが、&lt;ヘッダー&gt;のセクション。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![強調表示されたマークアップ](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="f7b4f-191">スタイルのウィンドウに CSS 変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="f7b4f-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="f7b4f-192">Page Inspector を使用して、次に、**スタイル**CSS の変更をプレビュー ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="f7b4f-193">クリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f7b4f-194">Page Inspector のブラウザーのウィンドウでまでの「ホーム ページ」セクションにマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![ポインターを合わせると div.content ラッパー](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="f7b4f-196">Div.content ラッパー セクション内で 1 回クリックしにマウス ポインターを移動、**スタイル**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="f7b4f-197">**スタイル**ウィンドウには、この要素に対する CSS 規則のすべてが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f7b4f-198">検索 .featured の .content ラッパー クラス セレクターまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f7b4f-199">背景色のプロパティのチェック ボックスをオフにようになりました。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-199">Now clear the checkbox for the background-color property.</span></span>

![オフの背景色](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="f7b4f-201">Page Inspector のブラウザー ウィンドウで変更がどのすぐにプレビューに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="f7b4f-202">もう一度チェック ボックスをオン、プロパティ値をダブルクリックして、赤に変更します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="f7b4f-203">変更がすぐには示しています。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-203">The change shows immediately:</span></span>

![赤の背景色](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="f7b4f-205">**スタイル**シート自体のウィンドウでのスタイルに変更をコミットする前に変更をテストおよび CSS をプレビューすることが容易です。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="f7b4f-206">CSS 自動同期</span><span class="sxs-lookup"><span data-stu-id="f7b4f-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="f7b4f-207">この機能には、Page Inspector のバージョン 1.3 が必要です。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="f7b4f-208">CSS 自動同期機能を使用すると、CSS ファイルを直接編集して、Page Inspector のブラウザー内ですぐに変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="f7b4f-209">クリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f7b4f-210">Page Inspector のブラウザーでまでの「ホーム ページ」セクションにマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="f7b4f-211">この要素を選択するには、1 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-211">Click once to select this element.</span></span>

<span data-ttu-id="f7b4f-212">**スタイル**ウィンドウには、この要素に対する CSS 規則のすべてが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f7b4f-213">検索 .featured の .content ラッパー クラス セレクターまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f7b4f-214">「.Featured .content ラッパー」をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="f7b4f-215">Page Inspector は、このスタイル (Site.css) を定義して対応する CSS スタイルを強調表示、CSS ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="f7b4f-216">値を変更するようになりました`background-color`"red"にします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="f7b4f-217">Page Inspector のブラウザーで、変更がすぐに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="f7b4f-218">CSS カラー ピッカーを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="f7b4f-219">Visual Studio 2012 CSS エディターを選択し、色の挿入を容易にするカラー ピッカーがあります。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="f7b4f-220">カラー ピッカーは、標準カラー パレットが含まれています、標準の色の名前、ハッシュ コード、RGB、RGBA、HSL、および HSLA のカラーをサポートし、ドキュメントで最も最近使用した色のリストを保持します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="f7b4f-221">値を変更する前のセクションで、`background-color`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="f7b4f-222">カラー ピッカーを起動するには、プロパティの名前と型の後にカーソルを置きます**#** または**rgb (** します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![CSS カラー ピッカーのバー](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="f7b4f-224">選択し、または下方向キーを押しての色をクリックし、左と右方向キーを使用して、色を走査します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="f7b4f-225">色を閲覧するときに、対応する 16 進値のプレビューします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![プレビューの背景色のプロパティ値](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="f7b4f-227">カラー バーが希望する正確な色を持っていない場合は、カラー ピッカーの pop ダウンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="f7b4f-228">開くにはをカラー バーの右端にある二重シェブロンをクリックします。 または、キーボードの下向き矢印を 1 ~ 2 回押します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS カラー ピッカーの Pop ダウン](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="f7b4f-230">右側の垂直バーの色をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="f7b4f-231">メイン ウィンドウにその色のグラデーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="f7b4f-232">、Enter キーを押して、垂直バーから直接色を選択するか、さらに高い精度を選択するメイン ウィンドウの任意の時点をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="f7b4f-233">使用するコンピューターの画面で、色があるかどうか (必要はありません、Visual Studio ユーザー インターフェイスの内部にある)、右下にスポイト ツールを使用して、その値をキャプチャすることができます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="f7b4f-234">カラー ピッカーの下部にあるスライダーを移動することによって、色の不透明度を変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="f7b4f-235">そう、RGBA 値を値の色ため、RGBA 形式は、不透明度を表すことができます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="f7b4f-236">色を選択した後、Enter キーを押し、背景色のエントリを完了するセミコロンを入力し、 *Site.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="f7b4f-237">ページのインスペクター更新バー</span><span class="sxs-lookup"><span data-stu-id="f7b4f-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="f7b4f-238">Page Inspector がすぐに変更を検出、 *Site.css*更新バーで、アラートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![更新バー](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="f7b4f-240">すべてのファイルを保存して Page Inspector のブラウザーを更新して、Ctrl + Alt + Enter キーを押します。 または [更新バー] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="f7b4f-241">強調表示色の変更は、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="f7b4f-242">Javascript の動的なページ要素のマッピング</span><span class="sxs-lookup"><span data-stu-id="f7b4f-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="f7b4f-243">最新の web アプリケーションでページ内の要素は多くの場合、動的に生成 JavaScript で。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="f7b4f-244">これらのページ要素に対応する static のマークアップ (HTML または Razor) がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="f7b4f-245">バージョン 1.3 では、Page Inspector は今すぐページを対応する JavaScript コードに動的に追加された項目をマップできます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="f7b4f-246">この機能を示すためには、使用して、 [Single Page Application (SPA) テンプレート](../../../single-page-application/overview/introduction/knockoutjs-template.md)します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f7b4f-247">SPA テンプレートが必要です、 [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)を更新します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="f7b4f-248">Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f7b4f-249">左側で、展開**Visual c#** を選択します**Web**、し、 **ASP.NET MVC4 Web アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="f7b4f-250">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-250">Click **OK**.</span></span>

<span data-ttu-id="f7b4f-251">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、 **Single Page Application**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="f7b4f-252">ソリューション エクスプ ローラーで、**ビュー**フォルダーをクリックし、**ホーム**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="f7b4f-253">Index.cshtml ファイルを右クリックし、選択**Page Inspector で表示**します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="f7b4f-254">Page Inspector のブラウザーでは最初に表示されるは、ログイン ページです。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="f7b4f-255">「サインアップ」をクリックし、ユーザー名とパスワードを作成します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="f7b4f-256">サインアップして、アプリケーション ログに記録するいくつかのサンプルの項目を含む、to do リストを作成します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="f7b4f-257">クリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="f7b4f-258">Page Inspector のブラウザーでは、to do 項目のいずれかをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="f7b4f-259">青で強調表示されているのではなく、要素がハイライトされている"JS"では、オレンジ色で、要素名の横にあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="f7b4f-260">これは、スクリプトによって、要素が動的に作成されたことを示します。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="f7b4f-261">さらに、オレンジ色の下線が表示されます、**呼び出し履歴**タブ。これが示す、**呼び出し履歴**ウィンドウには、要素の詳細について。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="f7b4f-262">をクリックして、**呼び出し履歴**タブ。**呼び出し履歴**ウィンドウには、要素を作成した JavaScript の呼び出しの呼び出し履歴が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="f7b4f-263">呼び出しを外部ライブラリなど、jQuery が折りたたまれている場合、アプリケーションのスクリプトへの呼び出しを簡単に確認できるように。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="f7b4f-264">呼び出しを外部ライブラリを含む、完全なスタックを表示するには、「外部ライブラリ」というラベルの付いたノードを展開することができます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="f7b4f-265">呼び出し履歴内の項目をクリックすると、Visual Studio はコード ファイルが表示され、対応するスクリプトを強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7b4f-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="f7b4f-266">関連項目</span><span class="sxs-lookup"><span data-stu-id="f7b4f-266">See Also</span></span>

<span data-ttu-id="f7b4f-267">[Visual Studio を使用した ASP.NET MVC 4 の概要](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)(ASP.net web サイト)</span><span class="sxs-lookup"><span data-stu-id="f7b4f-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="f7b4f-268">[Page Inspector を導入](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/)(Channel 9 ビデオ)</span><span class="sxs-lookup"><span data-stu-id="f7b4f-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="f7b4f-269">[Page Inspector エラー メッセージ](https://go.microsoft.com/?linkid=9813062)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="f7b4f-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
