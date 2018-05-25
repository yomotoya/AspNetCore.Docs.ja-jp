---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Page Inspector を使用して ASP.NET Web での Visual Studio 2012 のフォームの |Microsoft ドキュメント
author: rick-anderson
description: Visual Studio 2012 用の Page Inspector は、統合ブラウザーで web 開発ツールです。 統合のブラウザーと Page Inspector でいずれかの要素を選択してください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="3754f-104">Page Inspector を使用して ASP.NET Web フォームでの Visual Studio 2012 用</span><span class="sxs-lookup"><span data-stu-id="3754f-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="3754f-105">Tim Ammann によって</span><span class="sxs-lookup"><span data-stu-id="3754f-105">by Tim Ammann</span></span>

> <span data-ttu-id="3754f-106">Visual Studio 2012 用の Page Inspector は、統合ブラウザーで web 開発ツールです。</span><span class="sxs-lookup"><span data-stu-id="3754f-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="3754f-107">統合のブラウザー内で任意の要素を選択し、Page Inspector は、要素のソースと CSS に即座に強調表示します。</span><span class="sxs-lookup"><span data-stu-id="3754f-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="3754f-108">アプリケーションで任意のページを参照、すばやくのレンダリングされたマークアップのソースを検索し、右、Visual Studio 環境内でブラウザー ツールを使用します。</span><span class="sxs-lookup"><span data-stu-id="3754f-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="3754f-109">このチュートリアルの shwos 検査モードを有効にしてからすばやく検索して、web プロジェクト内のテキストと CSS 規則を編集する方法です。</span><span class="sxs-lookup"><span data-stu-id="3754f-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="3754f-110">チュートリアルは、Web フォーム アプリケーション プロジェクトを使用しますが、Web サイト プロジェクトの Page Inspector を使用することもでき、 [MVC](https://go.microsoft.com/?linkid=9802002)アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="3754f-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="3754f-111">このチュートリアルでは、次のセクションがあります。</span><span class="sxs-lookup"><span data-stu-id="3754f-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="3754f-112">前提条件</span><span class="sxs-lookup"><span data-stu-id="3754f-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="3754f-113">Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3754f-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="3754f-114">Page Inspector を使用して、アプリケーションの表示</span><span class="sxs-lookup"><span data-stu-id="3754f-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="3754f-115">検査モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3754f-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="3754f-116">Page Inspector を使用するマークアップを変更する</span><span class="sxs-lookup"><span data-stu-id="3754f-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> <span data-ttu-id="3754f-117">[検査モードと [HTML] ウィンドウ](#_6_inspection_mode)</span><span class="sxs-lookup"><span data-stu-id="3754f-117">[Inspection Mode and the HTML Window](#_6_inspection_mode)</span></span>
> 
> [<span data-ttu-id="3754f-118">スタイルのウィンドウに CSS 変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="3754f-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="3754f-119">CSS 自動同期</span><span class="sxs-lookup"><span data-stu-id="3754f-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="3754f-120">CSS カラー ピッカーを使用します。</span><span class="sxs-lookup"><span data-stu-id="3754f-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="3754f-121">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3754f-121">Prerequisites</span></span>

- <span data-ttu-id="3754f-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)または[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。</span><span class="sxs-lookup"><span data-stu-id="3754f-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="3754f-123">Page Inspector の最新バージョンを取得する[Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) for .NET 2.0、Azure SDK をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3754f-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="3754f-124">Page Inspector には、Microsoft Web Developer Tools が付属しています。</span><span class="sxs-lookup"><span data-stu-id="3754f-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="3754f-125">最新バージョンでは、1.3 です。</span><span class="sxs-lookup"><span data-stu-id="3754f-125">The latest version is 1.3.</span></span> <span data-ttu-id="3754f-126">どのバージョンを確認する、Visual Studio を実行して選択**に関する Microsoft クトリ**から、**ヘルプ**メニュー。</span><span class="sxs-lookup"><span data-stu-id="3754f-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="3754f-127">Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3754f-127">Create a Web Application</span></span>

<span data-ttu-id="3754f-128">最初に、Page Inspector を使用する web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3754f-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="3754f-129">Visual Studio で、次のように選択します。**ファイル** &gt; **新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="3754f-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="3754f-130">展開し、左側の**Visual c#** **Web**、し、 **ASP.NET Web フォーム アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="3754f-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![新しい Web フォーム アプリケーション](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="3754f-132">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-132">Click **OK**.</span></span>

<span data-ttu-id="3754f-133">アプリケーションが開かれた**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="3754f-133">The application opens in **Source** view.</span></span>

![ソース ビューで新しい Web フォーム アプリケーション](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="3754f-135">Page Inspector を使用するとを使用するアプリケーションがある場合は、これで、ことを確認し、それを変更します。</span><span class="sxs-lookup"><span data-stu-id="3754f-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="3754f-136">Page Inspector を使用して、アプリケーションの表示</span><span class="sxs-lookup"><span data-stu-id="3754f-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="3754f-137">次に、Page Inspector を使用してアプリケーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="3754f-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="3754f-138">**ソリューション エクスプ ローラー**、プロジェクトを右クリックしを選択し、 **Page Inspector で表示**です。</span><span class="sxs-lookup"><span data-stu-id="3754f-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Page Inspector で表示](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="3754f-140">既定では、最初に、Page Inspector が起動するときにドッキングされているナロー ウィンドウとして、Visual Studio 環境の左側にあります。</span><span class="sxs-lookup"><span data-stu-id="3754f-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="3754f-141">左側にドッキングされていると、使いやすいまたは上部、下部、または右にツール領域の 1 つの固定幅には、設定のままにします。</span><span class="sxs-lookup"><span data-stu-id="3754f-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Page Inspector のドッキング位置](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="3754f-143">場合、Page Inspector ウィンドウのドッキングを解除することができます配置する Visual Studio の外部または 2 つ目のモニターでもある場合。</span><span class="sxs-lookup"><span data-stu-id="3754f-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="3754f-144">ただし、alt キーを押しながら TAB Page Inspector と Visual Studio の間の順序で Page Inspector] ウィンドウがドッキングされていないときに移動**ツール** &gt; **オプション** &gt; **環境** &gt; **タブとウィンドウ**、し、[**タブも**チェック ボックスをオフ**フローティング ツール ウィンドウの上に常に連動して、メイン ウィンドウ**:</span><span class="sxs-lookup"><span data-stu-id="3754f-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Visual Studio と Page Inspector の非ドッキング ウィンドウの間には、ALT + TAB をフローティング ツール windows チェック ボックスをオフします。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="3754f-146">Page Inspector ウィンドウの上部ペインには、ブラウザーのウィンドウで、現在のページを示します。</span><span class="sxs-lookup"><span data-stu-id="3754f-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="3754f-147">下のペインは、左側の HTML マークアップでページを表示し、いくつかのタブを使用する右側は、ページのさまざまな側面を調査します。</span><span class="sxs-lookup"><span data-stu-id="3754f-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="3754f-148">下のペインがに似ていますが、 [F12 開発者ツール](https://msdn.microsoft.com/ie/aa740478)Internet Explorer でします。</span><span class="sxs-lookup"><span data-stu-id="3754f-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="3754f-149">(ただし、開発者ツールとは異なり使用できます Visual Studio 内で右 Page Inspector。)</span><span class="sxs-lookup"><span data-stu-id="3754f-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="3754f-151">このチュートリアルでは、Page Inspector のブラウザー ウィンドウを使用して、 **HTML**と**スタイル**に迅速に役立つタブ移動し、アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="3754f-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="3754f-152">検査モードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3754f-152">Enable Inspection Mode</span></span>

<span data-ttu-id="3754f-153">次に、Page Inspector の検査モードの動作方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="3754f-154">[Page Inspector] ウィンドウ、**検査**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="3754f-156">検査モードの動作を表示するには、Page Inspector のブラウザー ウィンドウ内のページのさまざまな部分にマウスを移動します。</span><span class="sxs-lookup"><span data-stu-id="3754f-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="3754f-157">マウス ポインターが大きなのプラス記号を変更し、下にある要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![ポインターを置いた div.content ラッパー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="3754f-159">マウス ポインターを移動するよう注意してください。</span><span class="sxs-lookup"><span data-stu-id="3754f-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="3754f-160">内容は、**ソース** ページで選択した要素に対応するマークアップを表示する変更を表示します。</span><span class="sxs-lookup"><span data-stu-id="3754f-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="3754f-161">関連するマークアップが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="3754f-162">ソースが別のファイル内にある場合は、そのはで開かれるファイル ソース ビューに関連するマークアップを強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="3754f-163">表示されるマークアップ、 **HTML** Page Inspector 内でのタブは、ページで選択した要素に対応するも変更します。</span><span class="sxs-lookup"><span data-stu-id="3754f-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="3754f-164">**HTML**  タブで、関連するマークアップをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="3754f-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="3754f-165">**スタイル** タブが現在の選択に関連する CSS 規則を示します。</span><span class="sxs-lookup"><span data-stu-id="3754f-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="3754f-166">Page Inspector を使用するマークアップを変更する</span><span class="sxs-lookup"><span data-stu-id="3754f-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="3754f-167">今すぐ検索し、マークアップまたは場所がすぐにわできない可能性がありますテキストに変更を Page Inspector を使用する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="3754f-168">検査モードで Page Inspector を配置し、ホーム ページの下部までスクロールします。</span><span class="sxs-lookup"><span data-stu-id="3754f-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="3754f-169">フッターの領域を入力するとすぐに Page Inspector を開きます、 *Site.Master*内でレイアウト ファイル**ソース**もう一方の右側に一時 タブの表示がタブし、マスターのセクションを強調表示したページ選択しました。</span><span class="sxs-lookup"><span data-stu-id="3754f-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="3754f-170">これにより表示 Page Inspector が方法を見つけるし、コンテンツ ページ上に表示を最初に開いたものの別のファイルから実際に行います。</span><span class="sxs-lookup"><span data-stu-id="3754f-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![検査モードでページ フッターを強調表示します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="3754f-172">Page Inspector のブラウザーのウィンドウで、著作権では、行にマウス ポインターを移動<a id="a"></a>に注意してください。</span><span class="sxs-lookup"><span data-stu-id="3754f-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="3754f-173">*Site.Master*  ページで、対応する行が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![ページ フッターの著作権行が強調表示されます。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="3754f-175">内の行の末尾にいくつかのテキストを追加、 *Site.Master*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3754f-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="3754f-176">&lt;p&gt;&amp;コピーです。&lt;%: DateTime.Now.Year %&gt; -マイ ASP.NET アプリケーションのものです&lt;。/p&gt;</span><span class="sxs-lookup"><span data-stu-id="3754f-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="3754f-177">ここで、Ctrl + Alt + Enter を押すか、Page Inspector のブラウザーのウィンドウで結果を表示する更新バー をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![My ASP.NET アプリケーションのものです。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="3754f-179">フッターにあったことを考えたかもしれません、 *Default.aspx*  ページで、それがマスター レイアウト ページのことがわかりましたし、Page Inspector では、これを検出するためです。</span><span class="sxs-lookup"><span data-stu-id="3754f-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="3754f-180">検査モードと [HTML] ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="3754f-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="3754f-181">次に、簡単に見て、HTML ウィンドウとどのようにするための要素をマップするがあります。</span><span class="sxs-lookup"><span data-stu-id="3754f-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="3754f-182">Page Inspector を検査モードで配置します。</span><span class="sxs-lookup"><span data-stu-id="3754f-182">Put Page Inspector in Inspection Mode.</span></span>

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="3754f-184">「ここにロゴ」と表示されているページの上部をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="3754f-185">詳細については、マウス ポインターを移動すると、ブラウザー ウィンドウに表示が不要になった変更の特定の要素を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="3754f-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="3754f-186">これで、マウス ポインターを移動する、 **HTML**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="3754f-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="3754f-187">Page Inspector 内の要素の概要、マウス ポインターを移動すると、 **HTML**ウィンドウとブラウザーのウィンドウで、対応する要素が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![HTML ウィンドウ](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="3754f-189">Page Inspector を開く前に、 *Site.Master*ファイルが一時的なタブにします。Site.Master タブをクリックし、対応するマークアップが強調表示されます、&lt;ヘッダー&gt;セクション。</span><span class="sxs-lookup"><span data-stu-id="3754f-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![強調表示されたマークアップ](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="3754f-191">スタイルのウィンドウに CSS 変更のプレビュー</span><span class="sxs-lookup"><span data-stu-id="3754f-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="3754f-192">Page Inspector を使用する方法を表示が次に、**スタイル**CSS に変更をプレビューするウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="3754f-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="3754f-193">クリックして、**検査**Page Inspector を検査モードで配置するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="3754f-194">Page Inspector のブラウザーのウィンドウでマウス ポインターを移動まで「ホーム ページ」セクションの上、 **div.content ラッパー**ラベルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![要素の上にマウス](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="3754f-196">Div.content ラッパー セクション内に 1 回クリックしにマウス ポインターを移動、**スタイル**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="3754f-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="3754f-197">.Featured <model>.content ラッパー クラス セレクターの下をオフにし、背景色のプロパティのチェック ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="3754f-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![オフの背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="3754f-199">どの変更をプレビュー即座に Page Inspector のブラウザー ウィンドウに注目してください。</span><span class="sxs-lookup"><span data-stu-id="3754f-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="3754f-200">もう一度チェック ボックスをオン、プロパティ値をダブルクリックしに変更して`red`です。</span><span class="sxs-lookup"><span data-stu-id="3754f-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="3754f-201">変更をすぐに示しています。</span><span class="sxs-lookup"><span data-stu-id="3754f-201">The change shows immediately:</span></span>

![赤の背景色](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="3754f-203">**スタイル**スタイルに変更をコミットする前に変更が簡単にテストし、CSS のプレビューにウィンドウがシート自体です。</span><span class="sxs-lookup"><span data-stu-id="3754f-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="3754f-204">CSS 自動同期</span><span class="sxs-lookup"><span data-stu-id="3754f-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="3754f-205">この機能には、Page Inspector のバージョン 1.3 が必要です。</span><span class="sxs-lookup"><span data-stu-id="3754f-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="3754f-206">CSS 自動同期機能を使用すると、CSS ファイルを直接編集し、Page Inspector のブラウザーですぐに変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="3754f-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="3754f-207">をクリックして**検査**Page Inspector を検査モードにします。</span><span class="sxs-lookup"><span data-stu-id="3754f-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="3754f-208">Page Inspector のブラウザーの「ホーム ページ」セクションまでマウス ポインターを移動、 **div.content ラッパー**ラベルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="3754f-209">この要素を選択するには、1 回クリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-209">Click once to select this element.</span></span>

<span data-ttu-id="3754f-210">**Syles**すべてこの要素に対する CSS 規則のウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="3754f-211">検索 .featured <model>.content ラッパー クラス セレクターまで下にスクロールします。</span><span class="sxs-lookup"><span data-stu-id="3754f-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="3754f-212">「.Featured <model>.content ラッパー」をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="3754f-213">Page Inspector は、このスタイル (Site.css) を定義して対応する CSS スタイルを強調表示する CSS ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="3754f-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![CSS ファイル](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="3754f-215">値を変更するようになりました`background-color`"red"にします。</span><span class="sxs-lookup"><span data-stu-id="3754f-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="3754f-216">変更は、Page Inspector のブラウザーにすぐに表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-216">The change appears immediately in the Page Inspector browser.</span></span>

![Page Inspector のブラウザー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="3754f-218">CSS カラー ピッカーを使用します。</span><span class="sxs-lookup"><span data-stu-id="3754f-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="3754f-219">次に、Page Inspector を使用してすばやく発見し、CSS の既定のアプリケーションで強調表示されたテキストを変更する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="3754f-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="3754f-220">この例では、青色の枠と同様に、別の色を変更しないと判断しました。</span><span class="sxs-lookup"><span data-stu-id="3754f-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="3754f-221">クリックして、**検査**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-221">Click the **Inspect** button.</span></span>

![要素を検査します。](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="3754f-223">Page Inspector のブラウザーのウィンドウで、マウス ポインター上に移動、強調表示された「ビデオ、チュートリアル、およびサンプル」テキストできるように、CSS「マーク」ラベルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![マーク要素の上にマウス](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="3754f-225">テキストを選択 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-225">Click the text to select it.</span></span> <span data-ttu-id="3754f-226">下に、対応する CSS マーク セレクターが表示される、**スタイル**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="3754f-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![スタイルのウィンドウで、セレクターのマークを付ける](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="3754f-228">マーク セレクターをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-228">Click the mark selector.</span></span> <span data-ttu-id="3754f-229">開き、 *Site.css* web アプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="3754f-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="3754f-230">Site.css タブをクリックし、対応する CSS セレクターが強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![スタイル シートで、セレクターのマークを付ける](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="3754f-232">選択し、背景色のプロパティでは、行を削除します。</span><span class="sxs-lookup"><span data-stu-id="3754f-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="3754f-233">新しい色を選択する、新しい Visual Studio 2012 CSS カラー ピッカーを使用、**マーク**背景色のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="3754f-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="3754f-234">Visual Studio 2012 CSS カラー ピッカーを使用します。</span><span class="sxs-lookup"><span data-stu-id="3754f-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="3754f-235">Visual Studio 2012 で CSS エディターには、カラー ピッカーを容易に選択し、色を挿入するものがあります。</span><span class="sxs-lookup"><span data-stu-id="3754f-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="3754f-236">単純なカラー バーとさらに細かく制御を提供する「pop ダウン」ピッカーいます。</span><span class="sxs-lookup"><span data-stu-id="3754f-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="3754f-237">カラー ピッカーは、標準色パレットが含まれています、標準的な色の名前、ハッシュ コード、RGB、RGBA、HSL、および HSLA の色をサポートし、ドキュメントで最も最近使用した色の一覧を保持します。</span><span class="sxs-lookup"><span data-stu-id="3754f-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="3754f-238">背景色のプロパティである、1 行には、"bc"を入力し、下向きの矢印を 1 回押します。</span><span class="sxs-lookup"><span data-stu-id="3754f-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="3754f-239">[背景色] のようにハイフンで区切ったプロパティ内の各単語の最初の文字を入力すると、IntelliSense には、一致するプロパティのみを表示するための一覧がフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="3754f-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Intellisense フィルターの値](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="3754f-241">コロンを入力します。</span><span class="sxs-lookup"><span data-stu-id="3754f-241">Now type a colon.</span></span> <span data-ttu-id="3754f-242">実行すると、全体の背景色のプロパティ名が挿入されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="3754f-243">型**#** または**rgb (**、カラー ピッカーのバーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![CSS カラー ピッカー バー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="3754f-245">カラー ピッカーのバーの動作を確認するには、マウス ポインターをその色をクリックしてまたは下方向キーを押すし、左と右方向キーを使用して、色を走査します。</span><span class="sxs-lookup"><span data-stu-id="3754f-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="3754f-246">アクセスすると、色、背景色のプロパティの対応する値には次のプレビューが行われます。</span><span class="sxs-lookup"><span data-stu-id="3754f-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![プレビューの背景色プロパティ値](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="3754f-248">この時点では、値と、CSS の入力を補完する、セミコロン (;) を選択するには Enter キーを押して可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3754f-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="3754f-249">ここでは、に移動、次のセクション、カラー ピッカー pop ダウンのしくみを認識できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3754f-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="3754f-250">カラー ピッカーの Pop ダウンを使用します。</span><span class="sxs-lookup"><span data-stu-id="3754f-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="3754f-251">カラー バーには、探している正確な色が割り当てられていない、ときに、カラー ピッカー pop ダウンを行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="3754f-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="3754f-252">開くにはをカラー バーの右端にある二重山かっこをクリックしてまたはキーボードの下矢印を 1、2 回押します。</span><span class="sxs-lookup"><span data-stu-id="3754f-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![CSS カラー ピッカーの Pop ダウン](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="3754f-254">右側の垂直バーから色をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="3754f-255">これにより、その色のグラデーションがメイン ウィンドウで表示します。</span><span class="sxs-lookup"><span data-stu-id="3754f-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="3754f-256">、Enter キーを押して、垂直バーから直接色を選択するか、任意の時点より高い精度でを選択するメイン ウィンドウでをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="3754f-257">使用するコンピューターの画面で、色があるかどうか (がない、Visual Studio ユーザー インターフェイスの中にある)、右下にあるスポイト ツールを使用してその値をキャプチャすることができます。</span><span class="sxs-lookup"><span data-stu-id="3754f-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="3754f-258">また、色の不透明度をカラー ピッカーの下部にあるスライダーを移動することによって変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="3754f-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="3754f-259">そう、RGBA 形式は、不透明度を表すことができるためにカラー RGBA 値に値を変更します。</span><span class="sxs-lookup"><span data-stu-id="3754f-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="3754f-260">色を選択したら、Enter キーを押しますし、セミコロンの背景色の入力を補完する、 *Site.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="3754f-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="3754f-261">ページのインスペクター更新バー</span><span class="sxs-lookup"><span data-stu-id="3754f-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="3754f-262">Page Inspector への変更をすぐに検出、 *Site.css*ファイル (または、アプリケーション内のファイルに) し、更新バーにアラートを表示します。</span><span class="sxs-lookup"><span data-stu-id="3754f-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![更新バー](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="3754f-264">すべてのファイルを保存して Page Inspector のブラウザーを更新して、Ctrl + Alt + Enter キーを押します。 または [更新バー] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3754f-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="3754f-265">強調表示色の変更は、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="3754f-265">The change in the highlight color appears in the browser:</span></span>

![強調表示色の変更](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="3754f-267">Visual Studio 環境内で Page Inspector のブラウザーから直接を簡単に更新されるに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3754f-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="3754f-268">外部ブラウザーではなく Page Inspector を使用すると、web アプリケーションを開発すると、エディターで状態を維持できます。</span><span class="sxs-lookup"><span data-stu-id="3754f-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="3754f-269">参照</span><span class="sxs-lookup"><span data-stu-id="3754f-269">See Also</span></span>

<span data-ttu-id="3754f-270">[Page Inspector を導入](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector)(Channel 9 ビデオ)</span><span class="sxs-lookup"><span data-stu-id="3754f-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="3754f-271">[Page Inspector エラー メッセージ](https://go.microsoft.com/?linkid=9813062)(MSDN)</span><span class="sxs-lookup"><span data-stu-id="3754f-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
