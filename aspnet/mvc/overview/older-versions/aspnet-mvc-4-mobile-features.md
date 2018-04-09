---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: ASP.NET MVC 4 のモバイル機能 |Microsoft ドキュメント
author: Rick-Anderson
description: ASP.NET MVC 5 モバイル Web アプリケーションでは、Azure Web サイトの展開でのコード サンプルでは、このチュートリアルの MVC 5 のバージョンがようになりました。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 5f38fcdd8e71ce12f7899214b6b2133e21f9910c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-4-mobile-features"></a><span data-ttu-id="950a5-103">ASP.NET MVC 4 モバイル機能</span><span class="sxs-lookup"><span data-stu-id="950a5-103">ASP.NET MVC 4 Mobile Features</span></span>
====================
<span data-ttu-id="950a5-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="950a5-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="950a5-105">このチュートリアルでのコード サンプルでの MVC 5 のバージョンがないようになりました[ASP.NET MVC 5 モバイル Web アプリケーションでは、Azure Web サイトを展開](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-105">There is now an MVC 5 version of this tutorial with code samples at [Deploy an ASP.NET MVC 5 Mobile Web Application on Azure Web Sites](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).</span></span>


<span data-ttu-id="950a5-106">このチュートリアルでは、ASP.NET MVC 4 Web アプリケーションでのモバイル機能を使用する方法の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="950a5-106">This tutorial will teach you the basics of how to work with mobile features in an ASP.NET MVC 4 Web application.</span></span> <span data-ttu-id="950a5-107">このチュートリアルで行うこともできます[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)または Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer または VWD&quot;)。</span><span class="sxs-lookup"><span data-stu-id="950a5-107">For this tutorial, you can use [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) or Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer or VWD&quot;).</span></span> <span data-ttu-id="950a5-108">既にある場合は、プロフェッショナルなバージョンの Visual Studio を使用できます。</span><span class="sxs-lookup"><span data-stu-id="950a5-108">You can use the professional version of Visual Studio if you already have that.</span></span>

<span data-ttu-id="950a5-109">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="950a5-109">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="950a5-110">[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (推奨) または Visual Studio Web Developer Express SP1。</span><span class="sxs-lookup"><span data-stu-id="950a5-110">[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recommended) or Visual Studio Web Developer Express SP1.</span></span> <span data-ttu-id="950a5-111">Visual Studio 2012 contains ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="950a5-111">Visual Studio 2012 contains ASP.NET MVC 4.</span></span> <span data-ttu-id="950a5-112">Visual Web Developer 2010 を使用している場合は、インストールする必要あります[ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-112">If you are using Visual Web Developer 2010, you must install [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).</span></span>

<span data-ttu-id="950a5-113">モバイル ブラウザー エミュレーターも必要になります。</span><span class="sxs-lookup"><span data-stu-id="950a5-113">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="950a5-114">次のいずれか、動作します。</span><span class="sxs-lookup"><span data-stu-id="950a5-114">Any of the following will work:</span></span>

- <span data-ttu-id="950a5-115">[Windows 7 Phone エミュレーター](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-115">[Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx).</span></span> <span data-ttu-id="950a5-116">(これは、このチュートリアルでは、スクリーン ショットのほとんどで使用されているエミュレーターです)。</span><span class="sxs-lookup"><span data-stu-id="950a5-116">(This is the emulator that's used in most of the screen shots in this tutorial.)</span></span>
- <span data-ttu-id="950a5-117">IPhone をエミュレートするためにユーザー エージェント文字列を変更します。</span><span class="sxs-lookup"><span data-stu-id="950a5-117">Change the user agent string to emulate an iPhone.</span></span> <span data-ttu-id="950a5-118">参照してください[この](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)ブログ エントリです。</span><span class="sxs-lookup"><span data-stu-id="950a5-118">See [this](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) blog entry.</span></span>
- [<span data-ttu-id="950a5-119">元の opera Mobile エミュレーター</span><span class="sxs-lookup"><span data-stu-id="950a5-119">Opera Mobile Emulator</span></span>](http://www.opera.com/developer/tools/mobile/)
- <span data-ttu-id="950a5-120">[Apple Safari](http://www.apple.com/safari/download/)ユーザー エージェント iPhone に設定します。</span><span class="sxs-lookup"><span data-stu-id="950a5-120">[Apple Safari](http://www.apple.com/safari/download/) with the user agent set to iPhone.</span></span> <span data-ttu-id="950a5-121">Safari で、ユーザー エージェントを"iPhone"に設定する方法については、次を参照してください。 [IE が見かけ上 Safari させる方法について](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)David Alison ブログ。</span><span class="sxs-lookup"><span data-stu-id="950a5-121">For instructions on how to set the user agent in Safari to "iPhone", see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) on David Alison's blog.</span></span>

<span data-ttu-id="950a5-122">C# ソース コードの visual Studio プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="950a5-122">Visual Studio projects with C# source code are available to accompany this topic:</span></span>

- [<span data-ttu-id="950a5-123">スタート プロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="950a5-123">Starter project download</span></span>](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [<span data-ttu-id="950a5-124">プロジェクトのダウンロードが完了しました</span><span class="sxs-lookup"><span data-stu-id="950a5-124">Completed project download</span></span>](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a><span data-ttu-id="950a5-125">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="950a5-125">What You'll Build</span></span>

<span data-ttu-id="950a5-126">このチュートリアルでは、モバイルの機能を追加会議一覧の簡単なアプリケーションで提供される、[スタート プロジェクト](https://go.microsoft.com/fwlink/?LinkId=228307)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-126">For this tutorial, you'll add mobile features to the simple conference-listing application that's provided in the [starter project](https://go.microsoft.com/fwlink/?LinkId=228307).</span></span> <span data-ttu-id="950a5-127">次のスクリーン ショットに示すように、完成したアプリケーションの [タグ] ページを示しています、 [Windows 7 Phone エミュレーター](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-127">The following screenshot shows the tags page of the completed application as seen in the [Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx).</span></span> <span data-ttu-id="950a5-128">参照してください[キーボード マッピングの Windows Phone エミュレーター](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx)をキーボード入力を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="950a5-128">See [Keyboard Mapping for Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) to simplify keyboard input.</span></span>

<span data-ttu-id="950a5-129">[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-129">[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)</span></span>

<span data-ttu-id="950a5-130">Internet Explorer バージョン 9、または 10 に設定して、モバイル アプリケーションを開発するには、FireFox または Chrome を使用することができます、[ユーザー エージェント文字列](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-130">You can use Internet Explorer version 9 or 10, FireFox or Chrome to develop your mobile application by setting the [user agent string](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/).</span></span> <span data-ttu-id="950a5-131">次の図は、iPhone をエミュレートする Internet Explorer を使用して、完了したチュートリアルを示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-131">The following image shows the completed tutorial using Internet Explorer emulating an iPhone.</span></span> <span data-ttu-id="950a5-132">Internet Explorer F-12 開発者ツールを使用することができます、 [Fiddler ツール](http://www.fiddler2.com/fiddler2/)アプリケーションをデバッグする際にします。</span><span class="sxs-lookup"><span data-stu-id="950a5-132">You can use the Internet Explorer F-12 developer tools and the [Fiddler tool](http://www.fiddler2.com/fiddler2/) to help debug your application.</span></span>

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a><span data-ttu-id="950a5-133">スキルの学習</span><span class="sxs-lookup"><span data-stu-id="950a5-133">Skills You'll Learn</span></span>

<span data-ttu-id="950a5-134">学習する内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-134">Here's what you'll learn:</span></span>

- <span data-ttu-id="950a5-135">ASP.NET MVC 4 のテンプレートが HTML5 に使用して`viewport`属性とアダプティブ レンダリング向上させるためにモバイル デバイスに表示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-135">How the ASP.NET MVC 4 templates use the HTML5 `viewport` attribute and adaptive rendering to improve display on mobile devices.</span></span>
- <span data-ttu-id="950a5-136">Mobile に固有のビューを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="950a5-136">How to create mobile-specific views.</span></span>
- <span data-ttu-id="950a5-137">ビュー スイッチャーにそのによりユーザーを切り替えるモバイル ビューと、アプリケーションのデスクトップ ビューを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="950a5-137">How to create a view switcher that lets users toggle between a mobile view and a desktop view of the application.</span></span>

### <a name="getting-started"></a><span data-ttu-id="950a5-138">作業の開始</span><span class="sxs-lookup"><span data-stu-id="950a5-138">Getting Started</span></span>

<span data-ttu-id="950a5-139">会議リスト アプリケーションをダウンロードして、次のリンクを使用して初期プロジェクト:[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=228307)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-139">Download the conference-listing application for the starter project using the following link: [Download](https://go.microsoft.com/fwlink/?LinkId=228307).</span></span> <span data-ttu-id="950a5-140">Windows エクスプ ローラーで右クリック、 *MvcMobile.zip*ファイルして選択**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="950a5-140">Then in Windows Explorer, right-click the *MvcMobile.zip* file and choose **Properties**.</span></span> <span data-ttu-id="950a5-141">**MvcMobile.zip プロパティ** ダイアログ ボックスで、選択、**ブロックを解除する**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="950a5-141">In the **MvcMobile.zip Properties** dialog box, choose the **Unblock** button.</span></span> <span data-ttu-id="950a5-142">(ブロック解除により、セキュリティの警告を使用しようとしたときに発生する、 *.zip* web からダウンロードしたファイルです)。</span><span class="sxs-lookup"><span data-stu-id="950a5-142">(Unblocking prevents a security warning that occurs when you try to use a *.zip* file that you've downloaded from the web.)</span></span>

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

<span data-ttu-id="950a5-144">右クリックし、 *MvcMobile.zip*ファイルおよび選択した**すべて展開**ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="950a5-144">Right-click the *MvcMobile.zip* file and select **Extract All** to unzip the file.</span></span> <span data-ttu-id="950a5-145">Visual Studio で開く、 *MvcMobile.sln*ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-145">In Visual Studio, open the *MvcMobile.sln* file.</span></span>

<span data-ttu-id="950a5-146">CTRL + f5 キーを押してデスクトップ ブラウザーで表示されるアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="950a5-146">Press CTRL+F5 to run the application, which will display it in your desktop browser.</span></span> <span data-ttu-id="950a5-147">モバイル ブラウザー エミュレーターを起動し、エミュレーターに会議アプリケーションの URL をコピーして、をクリックして、**タグによってブラウズ**リンクします。</span><span class="sxs-lookup"><span data-stu-id="950a5-147">Start your mobile browser emulator, copy the URL for the conference application into the emulator, and then click the **Browse by tag** link.</span></span> <span data-ttu-id="950a5-148">Windows Phone エミュレーターを使用している場合の URL バーをクリックし、一時停止キーを押してキーボード アクセスを取得します。</span><span class="sxs-lookup"><span data-stu-id="950a5-148">If you are using the Windows Phone Emulator, click in the URL bar and press the Pause key to get keyboard access.</span></span> <span data-ttu-id="950a5-149">下の例のイメージ、 *AllTags*ビュー (選択**タグによってブラウズ**)。</span><span class="sxs-lookup"><span data-stu-id="950a5-149">The image below shows the *AllTags* view (from choosing **Browse by tag**).</span></span>

<span data-ttu-id="950a5-150">[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-150">[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)</span></span>

<span data-ttu-id="950a5-151">表示は、モバイル デバイスで非常に読み取り可能です。</span><span class="sxs-lookup"><span data-stu-id="950a5-151">The display is very readable on a mobile device.</span></span> <span data-ttu-id="950a5-152">ASP.NET は、リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="950a5-152">Choose the ASP.NET link.</span></span>

<span data-ttu-id="950a5-153">[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-153">[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)</span></span>

<span data-ttu-id="950a5-154">ASP.NET タグ表示は非常に混雑です。</span><span class="sxs-lookup"><span data-stu-id="950a5-154">The ASP.NET tag view is very cluttered.</span></span> <span data-ttu-id="950a5-155">たとえば、**日付**列は読み取りを非常に困難です。</span><span class="sxs-lookup"><span data-stu-id="950a5-155">For example, the **Date** column is very difficult to read.</span></span> <span data-ttu-id="950a5-156">このチュートリアルで後ほどのバージョンを作成します、 *AllTags*モバイル ブラウザーについて具体的には、読み取り可能な表示を構成するはビューです。</span><span class="sxs-lookup"><span data-stu-id="950a5-156">Later in the tutorial you'll create a version of the *AllTags* view that's specifically for mobile browsers and that will make the display readable.</span></span>

<span data-ttu-id="950a5-157">注: バグに現在存在するモバイル キャッシュ エンジン。</span><span class="sxs-lookup"><span data-stu-id="950a5-157">Note: Currently a bug exists in the mobile caching engine.</span></span> <span data-ttu-id="950a5-158">実稼働アプリケーションのインストールする必要があります、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes)注目パッケージです。</span><span class="sxs-lookup"><span data-stu-id="950a5-158">For production applications, you must install the [Fixed DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget package.</span></span> <span data-ttu-id="950a5-159">参照してください[ASP.NET MVC 4 Mobile キャッシュ バグ固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)詳細については、修正します。</span><span class="sxs-lookup"><span data-stu-id="950a5-159">See [ASP.NET MVC 4 Mobile Caching Bug Fixed](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) for details on the fix.</span></span>

## <a name="css-media-queries"></a><span data-ttu-id="950a5-160">CSS のメディア クエリ</span><span class="sxs-lookup"><span data-stu-id="950a5-160">CSS Media Queries</span></span>

<span data-ttu-id="950a5-161">[CSS のメディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)メディアの種類の CSS を拡張します。</span><span class="sxs-lookup"><span data-stu-id="950a5-161">[CSS media queries](http://www.w3.org/TR/css3-mediaqueries/) are an extension to CSS for media types.</span></span> <span data-ttu-id="950a5-162">これらによって、特定のブラウザー (ユーザー エージェント) の既定の CSS 規則を上書きするルールを作成できます。</span><span class="sxs-lookup"><span data-stu-id="950a5-162">They allow you to create rules that override the default CSS rules for specific browsers (user agents).</span></span> <span data-ttu-id="950a5-163">モバイル ブラウザーを対象とする CSS の一般的なルールでは、画面の最大サイズを定義します。</span><span class="sxs-lookup"><span data-stu-id="950a5-163">A common rule for CSS that targets mobile browsers is defining the maximum screen size.</span></span> <span data-ttu-id="950a5-164">*Content\Site.css*新しい ASP.NET MVC 4 インターネット プロジェクトを作成するときに作成されるファイルには、次のメディア クエリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="950a5-164">The *Content\Site.css* file that's created when you create a new ASP.NET MVC 4 Internet project contains the following media query:</span></span>

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

<span data-ttu-id="950a5-165">ブラウザー ウィンドウが 850 ピクセル以下の場合は、このメディア ブロック内で CSS ルールが使用されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-165">If the browser window is 850 pixels wide or less, it will use the CSS rules inside this media block.</span></span> <span data-ttu-id="950a5-166">このような CSS メディア クエリを使用するよう設計されている既定の CSS 規則よりも (モバイル ブラウザーなど) の小さなブラウザーに HTML コンテンツをよりわかりやすく表示をデスクトップ ブラウザーの幅の表示を提供します。</span><span class="sxs-lookup"><span data-stu-id="950a5-166">You can use CSS media queries like this to provide a better display of HTML content on small browsers (like mobile browsers) than the default CSS rules that are designed for the wider displays of desktop browsers.</span></span>

## <a name="the-viewport-meta-tag"></a><span data-ttu-id="950a5-167">ビューポートのメタ タグ</span><span class="sxs-lookup"><span data-stu-id="950a5-167">The Viewport Meta Tag</span></span>

<span data-ttu-id="950a5-168">ほとんどのモバイル ブラウザー仮想ブラウザー ウィンドウの幅を定義する (、*ビューポート*) は、モバイル デバイスの実際の幅よりも大きくします。</span><span class="sxs-lookup"><span data-stu-id="950a5-168">Most mobile browsers define a virtual browser window width (the *viewport*) that's much larger than the actual width of the mobile device.</span></span> <span data-ttu-id="950a5-169">これにより、仮想の表示内の web ページ全体に合わせてモバイル ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="950a5-169">This allows mobile browsers to fit the entire web page inside the virtual display.</span></span> <span data-ttu-id="950a5-170">ユーザーは、興味深い内容でにズームできます。</span><span class="sxs-lookup"><span data-stu-id="950a5-170">Users can then zoom in on interesting content.</span></span> <span data-ttu-id="950a5-171">ただし、実際のデバイスの幅をビューポートの幅を設定する場合は、ズームは必要ありませんモバイル ブラウザーで、コンテンツを収めるためです。</span><span class="sxs-lookup"><span data-stu-id="950a5-171">However, if you set the viewport width to the actual device width, no zooming is required, because the content fits in the mobile browser.</span></span>

<span data-ttu-id="950a5-172">ビューポート`<meta>`ASP.NET MVC 4 レイアウト ファイル内のタグは、デバイスの幅に、ビューポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="950a5-172">The viewport `<meta>` tag in the ASP.NET MVC 4 layout file sets the viewport to the device width.</span></span> <span data-ttu-id="950a5-173">次の行は、ビューポートを示しています。 `<meta>` ASP.NET MVC 4 レイアウト ファイル内のタグ。</span><span class="sxs-lookup"><span data-stu-id="950a5-173">The following line shows the viewport `<meta>` tag in the ASP.NET MVC 4 layout file.</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a><span data-ttu-id="950a5-174">CSS のメディア クエリと、ビューポートのメタ タグの影響を確認します。</span><span class="sxs-lookup"><span data-stu-id="950a5-174">Examining the Effect of CSS Media Queries and the Viewport Meta Tag</span></span>

<span data-ttu-id="950a5-175">開く、 *\shared\\_Layout.cshtml*をコメント アウト、ビューポート、エディターでファイル`<meta>`タグ。</span><span class="sxs-lookup"><span data-stu-id="950a5-175">Open the *Views\Shared\\_Layout.cshtml* file in the editor and comment out the viewport `<meta>` tag.</span></span> <span data-ttu-id="950a5-176">次のマークアップは、コメント アウトされた行を示しています。</span><span class="sxs-lookup"><span data-stu-id="950a5-176">The following markup shows the commented-out line.</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

<span data-ttu-id="950a5-177">開く、 *MvcMobile\Content\Site.css*エディターでファイルを開き 0 ピクセルにメディア クエリで最大の幅を変更します。</span><span class="sxs-lookup"><span data-stu-id="950a5-177">Open the *MvcMobile\Content\Site.css* file in the editor and change the maximum width in the media query to zero pixels.</span></span> <span data-ttu-id="950a5-178">これは、ため、CSS 規則はモバイル ブラウザーで使用されているできなくなります。</span><span class="sxs-lookup"><span data-stu-id="950a5-178">This will prevent the CSS rules from being used in mobile browsers.</span></span> <span data-ttu-id="950a5-179">次の行は、変更されたメディア クエリを示しています。</span><span class="sxs-lookup"><span data-stu-id="950a5-179">The following line shows the modified media query:</span></span>

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

<span data-ttu-id="950a5-180">変更を保存し、モバイル ブラウザー エミュレーターで会議アプリケーションを参照します。</span><span class="sxs-lookup"><span data-stu-id="950a5-180">Save your changes and browse to the Conference application in a mobile browser emulator.</span></span> <span data-ttu-id="950a5-181">ビューポートを削除するは、次の図の小さなテキスト`<meta>`タグ。</span><span class="sxs-lookup"><span data-stu-id="950a5-181">The tiny text in the following image is the result of removing the viewport `<meta>` tag.</span></span> <span data-ttu-id="950a5-182">ありませんビューポートと`<meta>`タグ、ブラウザーがズーム アウトする既定のビューポートの幅 (850 ピクセルのほとんどのモバイル ブラウザーで広くしたりします)。</span><span class="sxs-lookup"><span data-stu-id="950a5-182">With no viewport `<meta>` tag, the browser is zooming out to the default viewport width (850 pixels or wider for most mobile browsers.)</span></span>

<span data-ttu-id="950a5-183">[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-183">[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)</span></span>

<span data-ttu-id="950a5-184">変更を取り消して、-、ビューポートのコメントを解除`<meta>`レイアウト ファイルにタグを付けるし、850 ピクセルにメディア クエリを復元、 *Site.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-184">Undo your changes — uncomment the viewport `<meta>` tag in the layout file and restore the media query to 850 pixels in the *Site.css* file.</span></span> <span data-ttu-id="950a5-185">変更内容を保存し、モバイル対応の表示が復元されていることを確認するモバイル ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="950a5-185">Save your changes and refresh the mobile browser to verify that the mobile-friendly display has been restored.</span></span>

<span data-ttu-id="950a5-186">ビューポート`<meta>`タグと CSS のメディア クエリは ASP.NET MVC 4 に固有ではないと、任意の web アプリケーションこれらの機能のメリットを利用することができます。</span><span class="sxs-lookup"><span data-stu-id="950a5-186">The viewport `<meta>` tag and the CSS media query are not specific to ASP.NET MVC 4, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="950a5-187">それらは、新しい ASP.NET MVC 4 プロジェクトを作成するときに生成されるファイルに組み込まれてようになりました。</span><span class="sxs-lookup"><span data-stu-id="950a5-187">But they are now built into the files that are generated when you create a new ASP.NET MVC 4 project.</span></span>

<span data-ttu-id="950a5-188">ビューポートの詳細については`<meta>`タグは、「 [A tale 2 つのビューポートの-パート 2](http://www.quirksmode.org/mobile/viewports2.html)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-188">For more information about the viewport `<meta>` tag, see [A tale of two viewports — part two](http://www.quirksmode.org/mobile/viewports2.html).</span></span>

<span data-ttu-id="950a5-189">次のセクションでは、mobile ブラウザーの特定のビューを提供する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-189">In the next section you'll see how to provide mobile-browser specific views.</span></span>

## <a name="overriding-views-layouts-and-partial-views"></a><span data-ttu-id="950a5-190">ビュー、レイアウト、および部分ビューをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="950a5-190">Overriding Views, Layouts, and Partial Views</span></span>

<span data-ttu-id="950a5-191">ASP.NET MVC 4 において重要な新しい機能は、オーバーライドできます (レイアウトや部分ビューを含む) の任意のビューの個々 のモバイル ブラウザーでの一般に、または、特定のブラウザーのモバイル ブラウザーでを単純なメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="950a5-191">A significant new feature in ASP.NET MVC 4 is a simple mechanism that lets you override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="950a5-192">Mobile に固有のビューを提供するには、ビュー ファイルをコピーし、追加することができます*です。Mobile*ファイル名にします。</span><span class="sxs-lookup"><span data-stu-id="950a5-192">To provide a mobile-specific view, you can copy a view file and add *.Mobile* to the file name.</span></span> <span data-ttu-id="950a5-193">たとえば、モバイルを作成する*インデックス*ビューで、コピー *Views\Home\Index.cshtml*に*Views\Home\Index.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-193">For example, to create a mobile *Index* view, copy *Views\Home\Index.cshtml* to *Views\Home\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="950a5-194">このセクションでは、mobile に固有のレイアウト ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="950a5-194">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="950a5-195">を開始するコピー *\shared\\_Layout.cshtml*に*\shared\\_Layout.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-195">To start, copy *Views\Shared\\_Layout.cshtml* to *Views\Shared\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="950a5-196">開いている *\_Layout.Mobile.cshtml*からタイトルを変更**MVC4 会議**に**会議 (モバイル)**です。</span><span class="sxs-lookup"><span data-stu-id="950a5-196">Open *\_Layout.Mobile.cshtml* and change the title from **MVC4 Conference** to **Conference (Mobile)**.</span></span>

<span data-ttu-id="950a5-197">各`Html.ActionLink`呼び出し、それぞれのリンクに「によって参照」を削除*ActionLink*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-197">In each `Html.ActionLink` call, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="950a5-198">次のコードは、モバイルのレイアウト ファイルの完了の body セクションを示しています。</span><span class="sxs-lookup"><span data-stu-id="950a5-198">The following code shows the completed body section of the mobile layout file.</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

<span data-ttu-id="950a5-199">コピー、 *Views\Home\AllTags.cshtml*ファイルの名前を*Views\Home\AllTags.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-199">Copy the *Views\Home\AllTags.cshtml* file to *Views\Home\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="950a5-200">新しいファイルを開き、変更、 `<h2>` "Tags"からの要素に"タグ (M)"。</span><span class="sxs-lookup"><span data-stu-id="950a5-200">Open the new file and change the `<h2>` element from "Tags" to "Tags (M)":</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

<span data-ttu-id="950a5-201">デスクトップのブラウザーを使用し、モバイル ブラウザーのエミュレーターを使用して、タグ ページを参照します。</span><span class="sxs-lookup"><span data-stu-id="950a5-201">Browse to the tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="950a5-202">モバイル ブラウザー エミュレーターは、次の 2 つの変更を示しています。</span><span class="sxs-lookup"><span data-stu-id="950a5-202">The mobile browser emulator shows the two changes you made.</span></span>

<span data-ttu-id="950a5-203">[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-203">[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)</span></span>

<span data-ttu-id="950a5-204">これに対し、デスクトップの表示は変更されていません。</span><span class="sxs-lookup"><span data-stu-id="950a5-204">In contrast, the desktop display has not changed.</span></span>

<span data-ttu-id="950a5-205">[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-205">[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)</span></span>

## <a name="browser-specific-views"></a><span data-ttu-id="950a5-206">ブラウザー固有のビュー</span><span class="sxs-lookup"><span data-stu-id="950a5-206">Browser-Specific Views</span></span>

<span data-ttu-id="950a5-207">モバイルおよびデスクトップに固有のビューだけでなく個々 のブラウザーのビューを作成できます。</span><span class="sxs-lookup"><span data-stu-id="950a5-207">In addition to mobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="950a5-208">たとえば、iPhone ブラウザーのための特別なビューを作成できます。</span><span class="sxs-lookup"><span data-stu-id="950a5-208">For example, you can create views that are specifically for the iPhone browser.</span></span> <span data-ttu-id="950a5-209">ここでの iPhone ブラウザーと iPhone のバージョンのレイアウトを作成、 *AllTags*ビュー。</span><span class="sxs-lookup"><span data-stu-id="950a5-209">In this section, you'll create a layout for the iPhone browser and an iPhone version of the *AllTags* view.</span></span>

<span data-ttu-id="950a5-210">開く、 *Global.asax*ファイルし、次のコードを追加、`Application_Start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="950a5-210">Open the *Global.asax* file and add the following code to the `Application_Start` method.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

<span data-ttu-id="950a5-211">このコードでは、各入力方向の要求と一致する"iPhone"という名前の新しい表示モードを定義します。</span><span class="sxs-lookup"><span data-stu-id="950a5-211">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="950a5-212">受信要求には、(ユーザー エージェントには、文字列"iPhone"が含まれている) 場合は、定義した条件が一致すると、ASP.NET MVC ビューの名前持つにはには、"iPhone"サフィックスが含まれています。 検索されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-212">If the incoming request matches the condition you defined (that is, if the user agent contains the string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

<span data-ttu-id="950a5-213">コードを右クリックして`DefaultDisplayMode`、選択**解決**を選択し`using System.Web.WebPages;`です。</span><span class="sxs-lookup"><span data-stu-id="950a5-213">In the code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="950a5-214">参照を追加、`System.Web.WebPages`が名前空間、`DisplayModes`と`DefaultDisplayMode`型が定義されています。</span><span class="sxs-lookup"><span data-stu-id="950a5-214">This adds a reference to the `System.Web.WebPages` namespace, which is where the `DisplayModes` and `DefaultDisplayMode` types are defined.</span></span>

<span data-ttu-id="950a5-215">[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-215">[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)</span></span>

<span data-ttu-id="950a5-216">次の行を手動で追加する代わりに、`using`ファイルのセクションです。</span><span class="sxs-lookup"><span data-stu-id="950a5-216">Alternatively, you can just manually add the following line to the `using` section of the file.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

<span data-ttu-id="950a5-217">完全な内容、 *Global.asax*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-217">The complete contents of the *Global.asax* file is shown below.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

<span data-ttu-id="950a5-218">変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="950a5-218">Save the changes.</span></span> <span data-ttu-id="950a5-219">コピー、 *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルの名前を*MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-219">Copy the *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file to *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="950a5-220">新しいファイルを開き、変更、`h1`から見出し`Conference (Mobile)`に`Conference (iPhone)`です。</span><span class="sxs-lookup"><span data-stu-id="950a5-220">Open the new file and then change the `h1` heading from `Conference (Mobile)` to `Conference (iPhone)`.</span></span>

<span data-ttu-id="950a5-221">コピー、 *MvcMobile\Views\Home\AllTags.Mobile.cshtml*ファイルの名前を*MvcMobile\Views\Home\AllTags.iPhone.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-221">Copy the *MvcMobile\Views\Home\AllTags.Mobile.cshtml* file to *MvcMobile\Views\Home\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="950a5-222">新しいファイルで、変更、`<h2>`要素から"タグ (M)"を「タグ (iPhone)」です。</span><span class="sxs-lookup"><span data-stu-id="950a5-222">In the new file, change the `<h2>` element from "Tags (M)" to "Tags (iPhone)".</span></span>

<span data-ttu-id="950a5-223">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="950a5-223">Run the application.</span></span> <span data-ttu-id="950a5-224">モバイル ブラウザー エミュレーターの実行でそのユーザー エージェントが"iPhone"に設定されていることを確認しを参照、 *AllTags*ビュー。</span><span class="sxs-lookup"><span data-stu-id="950a5-224">Run a mobile browser emulator, make sure its user agent is set to "iPhone", and browse to the *AllTags* view.</span></span> <span data-ttu-id="950a5-225">次のスクリーン ショットでは、 *AllTags*でビューが表示される、 [Safari](http://www.apple.com/safari/download/)ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="950a5-225">The following screenshot shows the *AllTags* view rendered in the [Safari](http://www.apple.com/safari/download/) browser.</span></span> <span data-ttu-id="950a5-226">Safari for Windows をダウンロードする[ここ](https://support.apple.com/kb/DL1531)です。</span><span class="sxs-lookup"><span data-stu-id="950a5-226">You can download Safari for Windows [here](https://support.apple.com/kb/DL1531).</span></span>

<span data-ttu-id="950a5-227">[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-227">[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)</span></span>

<span data-ttu-id="950a5-228">このセクションのレイアウトや iPhone などの特定のデバイス用のビューを作成する方法と、モバイルのレイアウトやビューを作成する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="950a5-228">In this section we've seen how to create mobile layouts and views and how to create layouts and views for specific devices such as the iPhone.</span></span> <span data-ttu-id="950a5-229">次のセクションでは、説得力のある他のモバイル ビューの jQuery Mobile を活用する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-229">In the next section you'll see how to leverage jQuery Mobile for more compelling mobile views.</span></span>

## <a name="using-jquery-mobile"></a><span data-ttu-id="950a5-230">JQuery Mobile を使用します。</span><span class="sxs-lookup"><span data-stu-id="950a5-230">Using jQuery Mobile</span></span>

<span data-ttu-id="950a5-231">[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html)ライブラリには、すべての主要なモバイル ブラウザーでも機能するユーザー インターフェイスのフレームワークが用意されています。</span><span class="sxs-lookup"><span data-stu-id="950a5-231">The [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) library provides a user interface framework that works on all the major mobile browsers.</span></span> <span data-ttu-id="950a5-232">jQuery Mobile 適用*段階的な強化*CSS および JavaScript をサポートするモバイル ブラウザー。</span><span class="sxs-lookup"><span data-stu-id="950a5-232">jQuery Mobile applies *progressive enhancement* to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="950a5-233">段階的な強化より強力なブラウザーおよびデバイスが豊富な表示にしながら、web ページの基本的な内容を表示するすべてのブラウザーを許可します。</span><span class="sxs-lookup"><span data-stu-id="950a5-233">Progressive enhancement allows all browsers to display the basic content of a web page, while allowing more powerful browsers and devices to have a richer display.</span></span> <span data-ttu-id="950a5-234">JavaScript および CSS ファイル jQuery Mobile に含まれているモバイル ブラウザーをマークアップ変更を加えずに適合するさまざまな要素のスタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="950a5-234">The JavaScript and CSS files that are included with jQuery Mobile style many elements to fit mobile browsers without making any markup changes.</span></span>

<span data-ttu-id="950a5-235">このセクションでをインストールし、 *jQuery.Mobile.MVC* NuGet パッケージは、jQuery Mobile、およびビュー スイッチャー ウィジェットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="950a5-235">In this section you'll install the *jQuery.Mobile.MVC* NuGet package, which installs jQuery Mobile and a view-switcher widget.</span></span>

<span data-ttu-id="950a5-236">開始するには、削除、 *Shared\\_Layout.Mobile.cshtml*と*Shared\\_Layout.iPhone.cshtml*先ほど作成したファイルです。</span><span class="sxs-lookup"><span data-stu-id="950a5-236">To start, delete the *Shared\\_Layout.Mobile.cshtml* and *Shared\\_Layout.iPhone.cshtml* files that you created earlier.</span></span>

<span data-ttu-id="950a5-237">名前を変更*Views\Home\AllTags.Mobile.cshtml*と*Views\Home\AllTags.iPhone.cshtml*ファイルを*Views\Home\AllTags.iPhone.cshtml.hide*と*Views\Home\AllTags.Mobile.cshtml.hide*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-237">Rename *Views\Home\AllTags.Mobile.cshtml* and *Views\Home\AllTags.iPhone.cshtml* files to *Views\Home\AllTags.iPhone.cshtml.hide* and *Views\Home\AllTags.Mobile.cshtml.hide*.</span></span> <span data-ttu-id="950a5-238">ファイルが必要がなくなるので、 *.cshtml*拡張機能、表示するために、ASP.NET MVC ランタイムによって使用されない、 *AllTags*ビュー。</span><span class="sxs-lookup"><span data-stu-id="950a5-238">Because the files no longer have a *.cshtml* extension, they won't be used by the ASP.NET MVC runtime to render the *AllTags* view.</span></span>

<span data-ttu-id="950a5-239">インストール、 *jQuery.Mobile.MVC*これにより、NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="950a5-239">Install the *jQuery.Mobile.MVC* NuGet package by doing this:</span></span>

1. <span data-ttu-id="950a5-240">**ツール**メニューの **ライブラリ パッケージ マネージャー**、し、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="950a5-240">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

    <span data-ttu-id="950a5-241">[![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-241">[![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)</span></span>
2. <span data-ttu-id="950a5-242">**Package Manager Console**を入力してください `Install-Package jQuery.Mobile.MVC -version 1.0.0`</span><span class="sxs-lookup"><span data-stu-id="950a5-242">In the **Package Manager Console**, enter `Install-Package jQuery.Mobile.MVC -version 1.0.0`</span></span>

<span data-ttu-id="950a5-243">次の図は、追加および NuGet jQuery.Mobile.MVC パッケージによって MvcMobile プロジェクトに変更したファイルを示しています。</span><span class="sxs-lookup"><span data-stu-id="950a5-243">The following image shows the files added and changed to the MvcMobile project by the NuGet jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="950a5-244">ファイル名の後に追加された [追加] が追加されるファイルです。</span><span class="sxs-lookup"><span data-stu-id="950a5-244">Files which are added have [add] appended after the file name.</span></span> <span data-ttu-id="950a5-245">イメージは、GIF を表示しないおよび PNG ファイルに追加、 *Content\images*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="950a5-245">The image does not show the GIF and PNG files added to the *Content\images* folder.</span></span>

![](aspnet-mvc-4-mobile-features/_static/image21.png)

<span data-ttu-id="950a5-246">次に、jQuery.Mobile.MVC NuGet パッケージがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="950a5-246">The jQuery.Mobile.MVC NuGet package installs the following:</span></span>

- <span data-ttu-id="950a5-247">*アプリ\_Start\BundleMobileConfig.cs*追加 jQuery JavaScript および CSS ファイルを参照するために必要なファイルです。</span><span class="sxs-lookup"><span data-stu-id="950a5-247">The *App\_Start\BundleMobileConfig.cs* file, which is needed to reference the jQuery JavaScript and CSS files added.</span></span> <span data-ttu-id="950a5-248">次の手順に従ってし、このファイルで定義されているモバイルのバンドルを参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="950a5-248">You must follow the instructions below and reference the mobile bundle defined in this file.</span></span>
- <span data-ttu-id="950a5-249">jQuery Mobile CSS ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-249">jQuery Mobile CSS files.</span></span>
- <span data-ttu-id="950a5-250">A`ViewSwitcher`ウィジェットのコント ローラー (*Controllers\ViewSwitcherController.cs*)。</span><span class="sxs-lookup"><span data-stu-id="950a5-250">A `ViewSwitcher` controller widget (*Controllers\ViewSwitcherController.cs*).</span></span>
- <span data-ttu-id="950a5-251">jQuery モバイルの JavaScript ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-251">jQuery Mobile JavaScript files.</span></span>
- <span data-ttu-id="950a5-252">JQuery Mobile スタイル レイアウト ファイル (*\shared\\_Layout.Mobile.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="950a5-252">A jQuery Mobile-styled layout file (*Views\Shared\\_Layout.Mobile.cshtml*).</span></span>
- <span data-ttu-id="950a5-253">ビュー スイッチャー部分ビュー *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) モバイル ビューおよびその逆のデスクトップの表示からを切り替えるには、各ページの上部にあるリンクを提供します。</span><span class="sxs-lookup"><span data-stu-id="950a5-253">A view-switcher partial view *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) that provides a link at the top of each page to switch from desktop view to mobile view and vice versa.</span></span>
- <span data-ttu-id="950a5-254">いくつか<em>.png</em>と<em>.gif</em>イメージ ファイル、 <em>Content\images</em>フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="950a5-254">Several<em>.png</em> and <em>.gif</em> image files in the <em>Content\images</em> folder.</span></span>

<span data-ttu-id="950a5-255">開く、 *Global.asax*ファイルし、の最後の行として次のコードを追加、`Application_Start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="950a5-255">Open the *Global.asax* file and add the following code as the last line of the `Application_Start` method.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

<span data-ttu-id="950a5-256">次のコードは、完全な*Global.asax*ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-256">The following code shows the complete *Global.asax* file.</span></span>

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> <span data-ttu-id="950a5-257">Internet Explorer 9 を使用しているし、表示されない場合、`BundleMobileConfig`黄色のハイライト上線をクリックして、[互換表示 ボタン](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![(オフ) 互換表示 ボタンの画像](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(オフ) 互換表示 ボタンの画像")アウトラインから変更アイコン IE で![(オフ) 互換表示 ボタンの画像](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(オフ) 互換表示 ボタンの画像")を純色![(on) 互換表示 ボタンの画像](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(on) 互換表示 ボタンの画像")します。</span><span class="sxs-lookup"><span data-stu-id="950a5-257">If you are using Internet Explorer 9 and you don't see the `BundleMobileConfig` line above in yellow highlight, click the [Compatibility View button](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![Picture of the Compatibility View button (off)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Picture of the Compatibility View button (off)") in IE to make the icon change from an outline ![Picture of the Compatibility View button (off)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Picture of the Compatibility View button (off)") to a solid color ![Picture of the Compatibility View button (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Picture of the Compatibility View button (on)").</span></span> <span data-ttu-id="950a5-258">また FireFox または Chrome で、このチュートリアルを参照できます。</span><span class="sxs-lookup"><span data-stu-id="950a5-258">Alternatively you can view this tutorial in FireFox or Chrome.</span></span>


<span data-ttu-id="950a5-259">開く、 *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルし、次のマークアップを直接後の追加、`Html.Partial`呼び出し。</span><span class="sxs-lookup"><span data-stu-id="950a5-259">Open the *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file and add the following markup directly after the `Html.Partial` call:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

<span data-ttu-id="950a5-260">完全な*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-260">The complete *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* file is shown below:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

<span data-ttu-id="950a5-261">アプリケーションをビルドし、モバイル ブラウザー エミュレーターを参照、 *AllTags*ビュー。</span><span class="sxs-lookup"><span data-stu-id="950a5-261">Build the application, and in your mobile browser emulator browse to the *AllTags* view.</span></span> <span data-ttu-id="950a5-262">次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="950a5-262">You see the following:</span></span>

<span data-ttu-id="950a5-263">[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-263">[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)</span></span>

> [!NOTE]
> <span data-ttu-id="950a5-264">モバイルの特定のコードをデバッグする[ユーザー エージェント文字列を設定](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)IE または iPhone および F-12 開発者ツールを使用するには Chrome です。</span><span class="sxs-lookup"><span data-stu-id="950a5-264">You can debug the mobile specific code by [setting the user agent string](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) for IE or Chrome to iPhone and then using the F-12 developer tools.</span></span> <span data-ttu-id="950a5-265">モバイル ブラウザーで表示されない場合は、**ホーム**、**スピーカー**、**タグ**、および**日付**ボタンとしてリンク、jQuery Mobile への参照スクリプトおよび CSS ファイルが正しくない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="950a5-265">If your mobile browser doesn't display the **Home**, **Speaker**, **Tag**, and **Date** links as buttons, the references to jQuery Mobile scripts and CSS files are probably not correct.</span></span>


<span data-ttu-id="950a5-266">表示スタイルの変更だけでなく**モバイル ビューを表示する**およびモバイル ビューからデスクトップ ビューに切り替えることができますをリンクします。</span><span class="sxs-lookup"><span data-stu-id="950a5-266">In addition to the style changes, you see **Displaying mobile view** and a link that lets you switch from mobile view to desktop view.</span></span> <span data-ttu-id="950a5-267">選択、**デスクトップ ビュー**リンク、およびデスクトップ ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-267">Choose the **Desktop view** link, and the desktop view is displayed.</span></span>

<span data-ttu-id="950a5-268">[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-268">[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)</span></span>

<span data-ttu-id="950a5-269">デスクトップのビューには、モバイル ビューに直接移動する方法を提供しません。</span><span class="sxs-lookup"><span data-stu-id="950a5-269">The desktop view doesn't provide a way to directly navigate back to the mobile view.</span></span> <span data-ttu-id="950a5-270">修正するようになりました。</span><span class="sxs-lookup"><span data-stu-id="950a5-270">You'll fix that now.</span></span> <span data-ttu-id="950a5-271">開く、 *\shared\\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-271">Open the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="950a5-272">ページのすぐ下`body`要素ビュー スイッチャー ウィジェットを表示する次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="950a5-272">Just under the page `body` element, add the following code, which renders the view-switcher widget:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

<span data-ttu-id="950a5-273">更新、 *AllTags*モバイル ブラウザーで表示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-273">Refresh the *AllTags* view in the mobile browser.</span></span> <span data-ttu-id="950a5-274">デスクトップおよびモバイル ビュー間を移動するようになりました。</span><span class="sxs-lookup"><span data-stu-id="950a5-274">You can now navigate between desktop and mobile views.</span></span>

<span data-ttu-id="950a5-275">[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-275">[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)</span></span>

> [!NOTE]
> <span data-ttu-id="950a5-276">注のデバッグ: \shared の末尾に次のコードを追加することができます\\_ViewSwitcher.cshtml ブラウザーのユーザー エージェント文字列を使用してモバイル デバイスに設定すると、ビューをデバッグする際にします。</span><span class="sxs-lookup"><span data-stu-id="950a5-276">Debug note: You can add the following code to the end of the Views\Shared\\_ViewSwitcher.cshtml to help debug views when using a browser the user agent string set to a mobile device.</span></span>
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  <span data-ttu-id="950a5-277">次の見出しを追加して、 *\shared\\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-277">and adding the following heading to the *Views\Shared\\_Layout.cshtml* file.</span></span>  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


<span data-ttu-id="950a5-278">参照、 *AllTags*デスクトップ ブラウザーでページ。</span><span class="sxs-lookup"><span data-stu-id="950a5-278">Browse to the *AllTags* page in a desktop browser.</span></span> <span data-ttu-id="950a5-279">モバイルのレイアウト ページにのみ追加されるため、ビュー スイッチャー ウィジェットはデスクトップ ブラウザーで表示されません。</span><span class="sxs-lookup"><span data-stu-id="950a5-279">The view-switcher widget is not displayed in a desktop browser because it's added only to the mobile layout page.</span></span> <span data-ttu-id="950a5-280">このチュートリアルで後ほど、デスクトップ ビューに、ビュー スイッチャー ウィジェットを追加する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-280">Later in the tutorial you'll see how you can add the view-switcher widget to the desktop view.</span></span>

<span data-ttu-id="950a5-281">[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-281">[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)</span></span>

## <a name="improving-the-speakers-list"></a><span data-ttu-id="950a5-282">スピーカーのリストを向上させる</span><span class="sxs-lookup"><span data-stu-id="950a5-282">Improving the Speakers List</span></span>

<span data-ttu-id="950a5-283">モバイル ブラウザーで、選択、**スピーカー**リンクします。</span><span class="sxs-lookup"><span data-stu-id="950a5-283">In the mobile browser, select the **Speakers** link.</span></span> <span data-ttu-id="950a5-284">モバイル ビューがないため (*AllSpeakers.Mobile.cshtml*)、既定のスピーカーの表示 (*AllSpeakers.cshtml*)、モバイルのレイアウト ビューを使用してレンダリング ( *\_Layout.Mobile.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="950a5-284">Because there's no mobile view(*AllSpeakers.Mobile.cshtml*), the default speakers display (*AllSpeakers.cshtml*) is rendered using the mobile layout view (*\_Layout.Mobile.cshtml*).</span></span>

<span data-ttu-id="950a5-285">[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-285">[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)</span></span>

<span data-ttu-id="950a5-286">設定してモバイル レイアウト内の表示から既定の (非モバイル) ビューを無効にすることができますグローバル`RequireConsistentDisplayMode`に`true`で、*ビュー\\は _viewstart.vbhtml*次のように、ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-286">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in the *Views\\_ViewStart.cshtml* file, like this:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

<span data-ttu-id="950a5-287">ときに`RequireConsistentDisplayMode`に設定されている`true`、モバイルのレイアウト (<em>\_Layout.Mobile.cshtml</em>) はモバイル ビューに対してのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="950a5-287">When `RequireConsistentDisplayMode` is set to `true`, the mobile layout (<em>\_Layout.Mobile.cshtml</em>) is used only for mobile views.</span></span> <span data-ttu-id="950a5-288">(そのファイルの表示は、フォームの<em>* * ViewName</em><em>です。Mobile.cshtml</em>)。設定することができます`RequireConsistentDisplayMode`に`true`場合は、非モバイル ビューに対して、モバイルのレイアウトは動作しません。</span><span class="sxs-lookup"><span data-stu-id="950a5-288">(That is, the view file is of the form <em>**ViewName</em><em>.Mobile.cshtml</em>.) You might want to set `RequireConsistentDisplayMode` to `true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="950a5-289">次のスクリーン ショット方法、<em>スピーカー</em>ページが表示されるときに`RequireConsistentDisplayMode`に設定されている`true`です。</span><span class="sxs-lookup"><span data-stu-id="950a5-289">The screenshot below shows how the <em>Speakers</em> page renders when `RequireConsistentDisplayMode` is set to `true`.</span></span>

<span data-ttu-id="950a5-290">[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-290">[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)</span></span>

<span data-ttu-id="950a5-291">設定して、ビューの一貫した表示モードを無効にすることができます`RequireConsistentDisplayMode`に`false`ファイルの表示にします。</span><span class="sxs-lookup"><span data-stu-id="950a5-291">You can disable consistent display mode in a view by setting `RequireConsistentDisplayMode` to `false` in the view file.</span></span> <span data-ttu-id="950a5-292">次のマークアップ、 *Views\Home\AllSpeakers.cshtml*ファイルのセットを`RequireConsistentDisplayMode`に`false`:</span><span class="sxs-lookup"><span data-stu-id="950a5-292">The following markup in the *Views\Home\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` to `false`:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a><span data-ttu-id="950a5-293">スピーカーがモバイル ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="950a5-293">Creating a Mobile Speakers View</span></span>

<span data-ttu-id="950a5-294">説明したとおり、*スピーカー*ビューは読み取り可能なですが、リンクは、小さな、モバイル デバイスをタップするが困難です。</span><span class="sxs-lookup"><span data-stu-id="950a5-294">As you just saw, the *Speakers* view is readable, but the links are small and are difficult to tap on a mobile device.</span></span> <span data-ttu-id="950a5-295">このセクションでモバイル固有の仕様を作成します*スピーカー*ビューを最新のモバイル アプリケーションのようになります: 大規模なが表示されます、tap 簡単なリンクしスピーカーをすばやく検索する検索ボックスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="950a5-295">In this section, you'll create a mobile-specific *Speakers* view that looks like a modern mobile application — it displays large, easy-to-tap links and contains a search box to quickly find speakers.</span></span>

<span data-ttu-id="950a5-296">コピー *AllSpeakers.cshtml*に*AllSpeakers.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-296">Copy *AllSpeakers.cshtml* to *AllSpeakers.Mobile.cshtml*.</span></span> <span data-ttu-id="950a5-297">開く、 *AllSpeakers.Mobile.cshtml*ファイルし、削除、`<h2>`見出し要素。</span><span class="sxs-lookup"><span data-stu-id="950a5-297">Open the *AllSpeakers.Mobile.cshtml* file and remove the `<h2>` heading element.</span></span>

<span data-ttu-id="950a5-298">`<ul>`タグ、追加、`data-role`属性し、その値に設定`listview`です。</span><span class="sxs-lookup"><span data-stu-id="950a5-298">In the `<ul>` tag, add the `data-role` attribute and set its value to `listview`.</span></span> <span data-ttu-id="950a5-299">などの他の[`data-*`属性](http://html5doctor.com/html5-custom-data-attributes/)、`data-role="listview"`タップする大きなリスト項目を簡単には、します。</span><span class="sxs-lookup"><span data-stu-id="950a5-299">Like other [`data-*` attributes](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` makes the large list items easier to tap.</span></span> <span data-ttu-id="950a5-300">これは、完全なマークアップはのようになります。</span><span class="sxs-lookup"><span data-stu-id="950a5-300">This is what the completed markup looks like:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

<span data-ttu-id="950a5-301">モバイル ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="950a5-301">Refresh the mobile browser.</span></span> <span data-ttu-id="950a5-302">表示を更新は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="950a5-302">The updated view looks like this:</span></span>

<span data-ttu-id="950a5-303">[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-303">[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)</span></span>

<span data-ttu-id="950a5-304">モバイル ビューが改善はスピーカーの長いリストを移動するが困難です。</span><span class="sxs-lookup"><span data-stu-id="950a5-304">Although the mobile view has improved, it's difficult to navigate the long list of speakers.</span></span> <span data-ttu-id="950a5-305">これを修正する、`<ul>`タグ、追加、`data-filter`属性に設定し、`true`です。</span><span class="sxs-lookup"><span data-stu-id="950a5-305">To fix this, in the `<ul>` tag, add the `data-filter` attribute and set it to `true`.</span></span> <span data-ttu-id="950a5-306">次のコード、`ul`マークアップ。</span><span class="sxs-lookup"><span data-stu-id="950a5-306">The code below shows the `ul` markup.</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

<span data-ttu-id="950a5-307">次の図は、検索フィルター ボックスに起因するページの上部にある、`data-filter`属性。</span><span class="sxs-lookup"><span data-stu-id="950a5-307">The following image shows the search filter box at the top of the page that results from the `data-filter` attribute.</span></span>

<span data-ttu-id="950a5-308">[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-308">[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)</span></span>

<span data-ttu-id="950a5-309">検索ボックスに各文字を入力すると jQuery Mobile は、次の図に示すように、表示される一覧をフィルターします。</span><span class="sxs-lookup"><span data-stu-id="950a5-309">As you type each letter in the search box, jQuery Mobile filters the displayed list as shown in the image below.</span></span>

<span data-ttu-id="950a5-310">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-310">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)</span></span>

## <a name="improving-the-tags-list"></a><span data-ttu-id="950a5-311">[タグ] ボックスの向上</span><span class="sxs-lookup"><span data-stu-id="950a5-311">Improving the Tags List</span></span>

<span data-ttu-id="950a5-312">既定値と同様に*スピーカー*ビュー、*タグ*ビューが読み取り可能では、リンクは小さく、モバイル デバイスをタップするが困難です。</span><span class="sxs-lookup"><span data-stu-id="950a5-312">Like the default *Speakers* view, the *Tags* view is readable, but the links are small and difficult to tap on a mobile device.</span></span> <span data-ttu-id="950a5-313">修正、このセクションで、*タグ*修正したのと同じ方法を表示、*スピーカー*ビュー。</span><span class="sxs-lookup"><span data-stu-id="950a5-313">In this section, you'll fix the *Tags* view the same way you fixed the *Speakers* view.</span></span>

<span data-ttu-id="950a5-314">削除、&quot;を非表示に&quot;にサフィックスが付いて、 *Views\Home\AllTags.Mobile.cshtml.hide*ファイルの名前は*Views\Home\AllTags.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-314">Remove the &quot;hide&quot; suffix to the *Views\Home\AllTags.Mobile.cshtml.hide* file so the name is *Views\Home\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="950a5-315">名前が変更されたファイルを開き、削除、`<h2>`要素。</span><span class="sxs-lookup"><span data-stu-id="950a5-315">Open the renamed file and remove the `<h2>` element.</span></span>

<span data-ttu-id="950a5-316">追加、`data-role`と`data-filter`属性を`<ul>`タグを次のように。</span><span class="sxs-lookup"><span data-stu-id="950a5-316">Add the `data-role` and `data-filter` attributes to the `<ul>` tag, as shown here:</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

<span data-ttu-id="950a5-317">次の図は、アルファベットのフィルタ リング タグ ページを示しています`J`です。</span><span class="sxs-lookup"><span data-stu-id="950a5-317">The image below shows the tags page filtering on the letter `J`.</span></span>

<span data-ttu-id="950a5-318">[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-318">[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)</span></span>

## <a name="improving-the-dates-list"></a><span data-ttu-id="950a5-319">日付のリストを向上させる</span><span class="sxs-lookup"><span data-stu-id="950a5-319">Improving the Dates List</span></span>

<span data-ttu-id="950a5-320">向上できます、*日付*表示を改善する、*スピーカー*と*タグ*ビュー、モバイル デバイスを使用する方が簡単にできるようにします。</span><span class="sxs-lookup"><span data-stu-id="950a5-320">You can improve the *Dates* view like you improved the *Speakers* and *Tags* views, so that it's easier to use on a mobile device.</span></span>

<span data-ttu-id="950a5-321">コピー、 *Views\Home\AllDates.cshtml*ファイルの名前を*Views\Home\AllDates.Mobile.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="950a5-321">Copy the *Views\Home\AllDates.cshtml* file to *Views\Home\AllDates.Mobile.cshtml*.</span></span> <span data-ttu-id="950a5-322">新しいファイルを開き、削除、`<h2>`要素。</span><span class="sxs-lookup"><span data-stu-id="950a5-322">Open the new file and remove the `<h2>` element.</span></span>

<span data-ttu-id="950a5-323">追加`data-role="listview"`を`<ul>`タグは、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="950a5-323">Add `data-role="listview"` to the `<ul>` tag, like this:</span></span>

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

<span data-ttu-id="950a5-324">次の図が動作を示しています、**日付**ページのように、`data-role`インプレース属性。</span><span class="sxs-lookup"><span data-stu-id="950a5-324">The image below shows what the **Date** page looks like with the `data-role` attribute in place.</span></span>

<span data-ttu-id="950a5-325">[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png)の内容を置き換える、 *Views\Home\AllDates.Mobile.cshtml*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-325">[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Replace the contents of the *Views\Home\AllDates.Mobile.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

<span data-ttu-id="950a5-326">このコードでは、指定した日数をすべてのセッションがグループ化します。</span><span class="sxs-lookup"><span data-stu-id="950a5-326">This code groups all sessions by days.</span></span> <span data-ttu-id="950a5-327">新しい日付ごとに、リストの区分線を作成し、1 日に区分線の下のすべてのセッションが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-327">It creates a list divider for each new day, and it lists all the sessions for each day under a divider.</span></span> <span data-ttu-id="950a5-328">このコードを実行すると外観を次に示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-328">Here's what it looks like when this code runs:</span></span>

<span data-ttu-id="950a5-329">[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-329">[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)</span></span>

## <a name="improving-the-sessionstable-view"></a><span data-ttu-id="950a5-330">SessionsTable ビューの向上</span><span class="sxs-lookup"><span data-stu-id="950a5-330">Improving the SessionsTable View</span></span>

<span data-ttu-id="950a5-331">このセクションでは、セッションの mobile に固有のビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="950a5-331">In this section, you'll create a mobile-specific view of sessions.</span></span> <span data-ttu-id="950a5-332">変更を行う場合は、他のビューを作成したよりもより広範なされます。</span><span class="sxs-lookup"><span data-stu-id="950a5-332">The changes we make will be more extensive than in other views we have created.</span></span>

<span data-ttu-id="950a5-333">モバイル ブラウザーで、タップ、**スピーカー**ボタンをクリックし、入力`Sc`検索ボックスにします。</span><span class="sxs-lookup"><span data-stu-id="950a5-333">In the mobile browser, tap the **Speaker** button, then enter `Sc` in the search box.</span></span>

<span data-ttu-id="950a5-334">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-334">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)</span></span>

<span data-ttu-id="950a5-335">タップして、 **Scott Hanselman**リンクします。</span><span class="sxs-lookup"><span data-stu-id="950a5-335">Tap the **Scott Hanselman** link.</span></span>

<span data-ttu-id="950a5-336">[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-336">[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)</span></span>

<span data-ttu-id="950a5-337">ご覧のように、表示はモバイル ブラウザーで読みにくいです。</span><span class="sxs-lookup"><span data-stu-id="950a5-337">As you can see, the display is difficult to read on a mobile browser.</span></span> <span data-ttu-id="950a5-338">日付の列が読みにくくなると、タグ列は、表示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-338">The date column is hard to read and the tags column is out of the view.</span></span> <span data-ttu-id="950a5-339">この問題を解決するにはコピー *Views\Home\SessionsTable.cshtml*に*Views\Home\SessionsTable.Mobile.cshtml*、ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="950a5-339">To fix this, copy *Views\Home\SessionsTable.cshtml* to *Views\Home\SessionsTable.Mobile.cshtml*, and then replace the contents of the file with the following code:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

<span data-ttu-id="950a5-340">コードが、ルームを削除し、タグ列、およびすべての情報をモバイル ブラウザーで読み取ることができるように、タイトル、スピーカー、および日の垂直方向に書式します。</span><span class="sxs-lookup"><span data-stu-id="950a5-340">The code removes the room and tags columns, and formats the title, speaker, and date vertically, so that all this information is readable on a mobile browser.</span></span> <span data-ttu-id="950a5-341">次の図には、コードの変更が反映されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-341">The image below reflects the code changes.</span></span>

<span data-ttu-id="950a5-342">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-342">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)</span></span>

## <a name="improving-the-sessionbycode-view"></a><span data-ttu-id="950a5-343">SessionByCode ビューの向上</span><span class="sxs-lookup"><span data-stu-id="950a5-343">Improving the SessionByCode View</span></span>

<span data-ttu-id="950a5-344">最後のモバイルに固有のビューを作成、 *SessionByCode*ビュー。</span><span class="sxs-lookup"><span data-stu-id="950a5-344">Finally, you'll create a mobile-specific view of the *SessionByCode* view.</span></span> <span data-ttu-id="950a5-345">モバイル ブラウザーで、タップ、**スピーカー**ボタンをクリックし、入力`Sc`検索ボックスにします。</span><span class="sxs-lookup"><span data-stu-id="950a5-345">In the mobile browser, tap the **Speaker** button, then enter `Sc` in the search box.</span></span>

<span data-ttu-id="950a5-346">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-346">[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)</span></span>

<span data-ttu-id="950a5-347">タップして、 **Scott Hanselman**リンクします。</span><span class="sxs-lookup"><span data-stu-id="950a5-347">Tap the **Scott Hanselman** link.</span></span> <span data-ttu-id="950a5-348">Scott Hanselman のセッションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="950a5-348">Scott Hanselman's sessions are displayed.</span></span>

<span data-ttu-id="950a5-349">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-349">[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)</span></span>

<span data-ttu-id="950a5-350">選択、 **of Love の MS Web スタック概要**リンクします。</span><span class="sxs-lookup"><span data-stu-id="950a5-350">Choose the **An Overview of the MS Web Stack of Love** link.</span></span>

<span data-ttu-id="950a5-351">[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-351">[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)</span></span>

<span data-ttu-id="950a5-352">既定のデスクトップ表示ですが、それを向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="950a5-352">The default desktop view is fine, but you can improve it.</span></span>

<span data-ttu-id="950a5-353">コピー、 *Views\Home\SessionByCode.cshtml*に*Views\Home\SessionByCode.Mobile.cshtml*の内容を置き換えると、 *Views\Home\SessionByCode.Mobile.cshtml*次のマークアップ ファイル。</span><span class="sxs-lookup"><span data-stu-id="950a5-353">Copy the *Views\Home\SessionByCode.cshtml* to *Views\Home\SessionByCode.Mobile.cshtml* and replace the contents of the *Views\Home\SessionByCode.Mobile.cshtml* file with the following markup:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

<span data-ttu-id="950a5-354">新しいマークアップを使用して、`data-role`属性ビューのレイアウトを改善します。</span><span class="sxs-lookup"><span data-stu-id="950a5-354">The new markup uses the `data-role` attribute to improve the layout of the view.</span></span>

<span data-ttu-id="950a5-355">モバイル ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="950a5-355">Refresh the mobile browser.</span></span> <span data-ttu-id="950a5-356">次の図は、先ほど作成したコードの変更を反映しています。</span><span class="sxs-lookup"><span data-stu-id="950a5-356">The following image reflects the code changes that you just made:</span></span>

<span data-ttu-id="950a5-357">[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)</span><span class="sxs-lookup"><span data-stu-id="950a5-357">[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)</span></span>

## <a name="wrapup-and-review"></a><span data-ttu-id="950a5-358">(コース完了) とレビュー</span><span class="sxs-lookup"><span data-stu-id="950a5-358">Wrapup and Review</span></span>

<span data-ttu-id="950a5-359">このチュートリアルには、ASP.NET MVC 4 Developer Preview の新しいモバイル機能が導入されました。</span><span class="sxs-lookup"><span data-stu-id="950a5-359">This tutorial has introduced the new mobile features of ASP.NET MVC 4 Developer Preview.</span></span> <span data-ttu-id="950a5-360">モバイルの機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="950a5-360">The mobile features include:</span></span>

- <span data-ttu-id="950a5-361">グローバルと個々 のビューの両方に、レイアウト、ビュー、および部分ビューを上書きする権限です。</span><span class="sxs-lookup"><span data-stu-id="950a5-361">The ability to override layout, views, and partial views, both globally and for an individual view.</span></span>
- <span data-ttu-id="950a5-362">レイアウトと部分的な上書きの適用を使用して制御、`RequireConsistentDisplayMode`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="950a5-362">Control over layout and partial override enforcement using the `RequireConsistentDisplayMode` property.</span></span>
- <span data-ttu-id="950a5-363">モバイル ビュー スイッチャー ウィジェット デスクトップ ビューにも表示されるよりもを表示します。</span><span class="sxs-lookup"><span data-stu-id="950a5-363">A view-switcher widget for mobile views than can also be displayed in desktop views.</span></span>
- <span data-ttu-id="950a5-364">IPhone ブラウザーなどの特定のブラウザーをサポートするためのサポート。</span><span class="sxs-lookup"><span data-stu-id="950a5-364">Support for supporting specific browsers, such as the iPhone browser.</span></span>

## <a name="see-also"></a><span data-ttu-id="950a5-365">関連項目</span><span class="sxs-lookup"><span data-stu-id="950a5-365">See Also</span></span>

- <span data-ttu-id="950a5-366">[jQuery Mobile](http://jquerymobile.com)サイトです。</span><span class="sxs-lookup"><span data-stu-id="950a5-366">[jQuery Mobile](http://jquerymobile.com) site.</span></span>
- [<span data-ttu-id="950a5-367">jQuery Mobile の概要</span><span class="sxs-lookup"><span data-stu-id="950a5-367">jQuery Mobile Overview</span></span>](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [<span data-ttu-id="950a5-368">W3C Recommendation モバイル Web アプリケーションのベスト プラクティス</span><span class="sxs-lookup"><span data-stu-id="950a5-368">W3C Recommendation Mobile Web Application Best Practices</span></span>](http://www.w3.org/TR/mwabp/)
- [<span data-ttu-id="950a5-369">W3C Candidate Recommendation メディア クエリ</span><span class="sxs-lookup"><span data-stu-id="950a5-369">W3C Candidate Recommendation for media queries</span></span>](http://www.w3.org/TR/css3-mediaqueries/)
