---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: "ビュー (VB) を追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a><span data-ttu-id="03d1f-103">ビュー (VB) を追加します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-103">Adding a View (VB)</span></span>
====================
<span data-ttu-id="03d1f-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="03d1f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="03d1f-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="03d1f-106">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="03d1f-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="03d1f-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="03d1f-108">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="03d1f-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="03d1f-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="03d1f-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="03d1f-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="03d1f-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="03d1f-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="03d1f-113">VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="03d1f-114">[バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="03d1f-115">C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-a-view.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-115">If you prefer C#, switch to the [C# version](../cs/adding-a-view.md) of this tutorial.</span></span>


<span data-ttu-id="03d1f-116">このセクションではことを変更する、`HelloWorldController`クリーンにするビュー テンプレート ファイルを使用するクラスがクライアントに HTML 応答を生成するプロセスをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-116">In this section we're going to modify the `HelloWorldController` class to use a view template file to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="03d1f-117">ビューのテンプレートを使用して、まず、`Index`メソッドで、`HelloWorldController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-117">Let's start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="03d1f-118">現在、`Index`メソッドがコント ローラー クラス内でハードコーディングされているメッセージに文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-118">Currently the `Index` method returns a string with a message that is hard-coded within the controller class.</span></span> <span data-ttu-id="03d1f-119">変更、`Index`を返すメソッドを`View`オブジェクトを次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="03d1f-119">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

<span data-ttu-id="03d1f-120">呼び出すことのできるおプロジェクトにビュー テンプレートを追加してみましょうようになりました、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-120">Let's now add a view template to our project that we can invoke with the `Index` method.</span></span> <span data-ttu-id="03d1f-121">これを行うには、内部を右クリックして、`Index`メソッドとクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-121">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

<span data-ttu-id="03d1f-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="03d1f-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span></span>

<span data-ttu-id="03d1f-123">**ビューの追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-123">The **Add View** dialog box appears.</span></span> <span data-ttu-id="03d1f-124">既定のエントリのままにし、をクリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="03d1f-124">Leave the default entries and click the **Add** button.</span></span>

<span data-ttu-id="03d1f-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="03d1f-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span></span>

<span data-ttu-id="03d1f-126">*MvcMovie\Views\HelloWorld*フォルダーおよび*MvcMovie\Views\HelloWorld\Index.vbhtml*ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-126">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.vbhtml* file are created.</span></span> <span data-ttu-id="03d1f-127">ご覧**ソリューション エクスプ ローラー**:</span><span class="sxs-lookup"><span data-stu-id="03d1f-127">You can see them in **Solution Explorer**:</span></span>

<span data-ttu-id="03d1f-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="03d1f-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span></span>

<span data-ttu-id="03d1f-129">HTML を追加、`<h2>`タグ。</span><span class="sxs-lookup"><span data-stu-id="03d1f-129">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="03d1f-130">変更された*MvcMovie\Views\HelloWorld\Index.vbhtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-130">The modified *MvcMovie\Views\HelloWorld\Index.vbhtml* file is shown below.</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

<span data-ttu-id="03d1f-131">アプリケーションを実行しを参照、 &quot;hello world&quot;コント ローラー (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="03d1f-131">Run the application and browse to the &quot;hello world&quot; controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="03d1f-132">`Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`クライアントへの応答を表示するためにテンプレート ファイルの表示を使用したいことが示されます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-132">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which indicated that we wanted to use a view template file to render a response to the client.</span></span> <span data-ttu-id="03d1f-133">ASP.NET MVC の既定値を使用してに明示的を使用するビューのテンプレート ファイルの名前を指定していない、ため、 *Index.vbhtml*内でファイルの表示、 *\Views\HelloWorld*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-133">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.vbhtml* view file within the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="03d1f-134">次の図は、ビューで、ハードコーディング文字列を示しています。</span><span class="sxs-lookup"><span data-stu-id="03d1f-134">The image below shows the string hard-coded in the view.</span></span>

<span data-ttu-id="03d1f-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="03d1f-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="03d1f-136">なかなか良いを検索します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-136">Looks pretty good.</span></span> <span data-ttu-id="03d1f-137">ただし、ブラウザーのタイトル バーに表示されることを確認&quot;インデックス&quot; ページで大きなタイトルという&quot;マイ MVC アプリケーション。&quot;これらの名前を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="03d1f-137">However, notice that the browser's title bar says &quot;Index&quot; and the big title on the page says &quot;My MVC Application.&quot; Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="03d1f-138">ビューとレイアウト ページの変更</span><span class="sxs-lookup"><span data-stu-id="03d1f-138">Changing views and layout pages</span></span>

<span data-ttu-id="03d1f-139">最初に、テキストを変更してみましょう&quot;マイ MVC アプリケーション。&quot;そのテキストを使用して、共有は、すべてのページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-139">First, let's change the text &quot;My MVC Application.&quot; That text is shared and appears on every page.</span></span> <span data-ttu-id="03d1f-140">アプリケーション内の各ページ上にあるいても実際には、プロジェクト内の 1 か所に表示します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-140">It actually appears in only one place in our project, even though it's on every page in our application.</span></span> <span data-ttu-id="03d1f-141">移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.vbhtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="03d1f-141">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.vbhtml* file.</span></span> <span data-ttu-id="03d1f-142">このファイルし、呼ばれ、レイアウト ページ、共有は&quot;シェル&quot;他のすべてのページを使用します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-142">This file is called a layout page and it's the shared &quot;shell&quot; that all other pages use.</span></span>

<span data-ttu-id="03d1f-143">注、`@RenderBody()`ファイルの下部にあるコードの行。</span><span class="sxs-lookup"><span data-stu-id="03d1f-143">Note the `@RenderBody()` line of code near the bottom of the file.</span></span> <span data-ttu-id="03d1f-144">`RenderBody`場所を作成するすべてのページは表示のプレース ホルダー&quot;ラップ&quot;レイアウト ページでします。</span><span class="sxs-lookup"><span data-stu-id="03d1f-144">`RenderBody` is a placeholder where all the pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="03d1f-145">変更、`<h1>`から見出し **&quot;** マイ MVC アプリケーション&quot;に&quot;MVC ムービー アプリ&quot;です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-145">Change the `<h1>` heading from **&quot;** My MVC Application&quot; to &quot;MVC Movie App&quot;.</span></span>

[!code-html[Main](adding-a-view/samples/sample3.html)]

<span data-ttu-id="03d1f-146">アプリケーションを実行し、今すぐということに注意してください&quot;MVC ムービー アプリ&quot;です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-146">Run the application and note it now says &quot;MVC Movie App&quot;.</span></span> <span data-ttu-id="03d1f-147">をクリックして、**に関する**リンク、およびページ表示&quot;MVC ムービー アプリ&quot;、すぎます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-147">Click the **About** link, and that page shows &quot;MVC Movie App&quot;, too.</span></span>

<span data-ttu-id="03d1f-148">完全な *\_Layout.vbhtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-148">The complete *\_Layout.vbhtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="03d1f-149">ここで、インデックス ページ (ビュー) のタイトルを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="03d1f-149">Now, let's change the title of the Index page (view).</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

<span data-ttu-id="03d1f-150">開いている*MvcMovie\Views\HelloWorld\Index.vbhtml*です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-150">Open *MvcMovie\Views\HelloWorld\Index.vbhtml*.</span></span> <span data-ttu-id="03d1f-151">2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。</span><span class="sxs-lookup"><span data-stu-id="03d1f-151">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="03d1f-152">しましょうに若干異なるため、どのビット コードの変更、アプリのどの部分を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03d1f-152">We'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

<span data-ttu-id="03d1f-153">アプリケーションを実行しを参照`http://localhost:xx/HelloWorld`です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-153">Run the application and browse to`http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="03d1f-154">ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください </span><span class="sxs-lookup"><span data-stu-id="03d1f-154">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="03d1f-155">ビューに小さな変更を使用してアプリケーションに大きな変更を加えるには簡単です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-155">It's easy to make big changes in your application with small changes to a view.</span></span> <span data-ttu-id="03d1f-156">(ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="03d1f-156">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="03d1f-157">ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。</span><span class="sxs-lookup"><span data-stu-id="03d1f-157">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="03d1f-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="03d1f-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span></span>

<span data-ttu-id="03d1f-159">当社ほんの少しの&quot;データ&quot;(ここでは、 &quot;Hello World!&quot;メッセージ) は、ハードコードです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-159">Our little bit of &quot;data&quot; (in this case the &quot;Hello World!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="03d1f-160">MVC アプリケーションは V (views) を持ち、C (コント ローラー) がないの M (モデル)。 まだお手伝いします。</span><span class="sxs-lookup"><span data-stu-id="03d1f-160">Our MVC application has V (views) and we've got C (controllers), but no M (model) yet.</span></span> <span data-ttu-id="03d1f-161">間もなく、方法を説明するデータベースを作成し、モデル データを取得します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-161">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="03d1f-162">コントローラーからビューへのデータの受け渡し</span><span class="sxs-lookup"><span data-stu-id="03d1f-162">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="03d1f-163">データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-163">Before we go to a database and talk about models, though, let's first talk about passing information from the Controller to a View.</span></span> <span data-ttu-id="03d1f-164">クライアントへの HTML 応答を表示するために必要とビューのテンプレートを通過することができます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-164">We want to pass what a view template requires in order to render an HTML response to a client.</span></span> <span data-ttu-id="03d1f-165">これらのオブジェクトが通常作成され、コント ローラー クラスによって、ビュー テンプレートに渡され、テンプレートの表示を必要とするデータのみを含んでいる必要があります: とは。</span><span class="sxs-lookup"><span data-stu-id="03d1f-165">These objects are typically created and passed by a controller class to a view template, and they should contain only the data that the view template requires — and no more.</span></span>

<span data-ttu-id="03d1f-166">以前、`HelloWorldController`クラス、`Welcome`アクション メソッドがかかりました、`name`と`numTimes`パラメーターと、パラメーターの値をブラウザーにし、出力します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-166">Previously with the `HelloWorldController` class, the `Welcome` action method took a `name` and a `numTimes` parameter and then output the parameter values to the browser.</span></span> <span data-ttu-id="03d1f-167">はなくこの応答を直接表示するために続行コント ローラーがあるより代わりにおを見ていきましょうデータ バッグに、ビューの。</span><span class="sxs-lookup"><span data-stu-id="03d1f-167">Rather than have the controller continue to render this response directly, let's instead we'll put that data in a bag for the View.</span></span> <span data-ttu-id="03d1f-168">コント ローラーとビューを使用できる、`ViewBag`をそのデータを保持するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="03d1f-168">Controllers and Views can use a `ViewBag` object to hold that data.</span></span> <span data-ttu-id="03d1f-169">経由で渡されるビュー テンプレートに自動的に、しするデータとしてバッグの内容を使用して、HTML 応答を表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-169">That will be passed over to a view template automatically, and used to render the HTML response using the contents of the bag as data.</span></span> <span data-ttu-id="03d1f-170">このようにして、1 つの点とそのと他のビュー テンプレートに関しては、コント ローラー: クリーンを維持することを有効にする&quot;関心の分離&quot;アプリケーション内で。</span><span class="sxs-lookup"><span data-stu-id="03d1f-170">That way the controller is concerned with one thing and the view template with another — enabling us to maintain clean &quot;separation of concerns&quot; within the application.</span></span>

<span data-ttu-id="03d1f-171">代わりに、おでしたカスタム クラスを定義、独自にそのオブジェクトのインスタンスを作成、データを入力し、ビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-171">Alternatively, we could define a custom class, then create an instance of that object on our own, fill it with data and pass it to the View.</span></span> <span data-ttu-id="03d1f-172">カスタム ビューのモデルになっているため、ViewModel 多くの場合、呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-172">That is often called a ViewModel, because it's a custom Model for the View.</span></span> <span data-ttu-id="03d1f-173">少量のデータは、ただし、ViewBag うまく動作します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-173">For small amounts of data, however, the ViewBag works great.</span></span>

<span data-ttu-id="03d1f-174">戻り、 *HelloWorldController.vb*ファイルの変更、 `Welcome` ViewBag をメッセージと NumTimes にコント ローラーの内部メソッドです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-174">Return to the *HelloWorldController.vb* file change the `Welcome` method inside the controller to put the Message and NumTimes into the ViewBag.</span></span> <span data-ttu-id="03d1f-175">ViewBag は、動的なオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-175">The ViewBag is a dynamic object.</span></span> <span data-ttu-id="03d1f-176">つまりに自由に配置することができます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-176">That means you can put whatever you want in to it.</span></span> <span data-ttu-id="03d1f-177">その中のものを配置するまで、ViewBag は定義されているプロパティがありません。</span><span class="sxs-lookup"><span data-stu-id="03d1f-177">The ViewBag has no defined properties until you put something inside it.</span></span>

<span data-ttu-id="03d1f-178">完全な`HelloWorldController.vb`同じファイルで、新しいクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-178">The complete `HelloWorldController.vb` with the new class in the same file.</span></span>

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

<span data-ttu-id="03d1f-179">これで、ViewBag には、渡される経由でのビューに自動的にデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="03d1f-179">Now our ViewBag contains data that will be passed over to the View automatically.</span></span> <span data-ttu-id="03d1f-180">もう一度、またはでしたが渡された次のように、独自のオブジェクトは良かった場合。</span><span class="sxs-lookup"><span data-stu-id="03d1f-180">Again, alternatively we could have passed in our own object like this if we liked:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="03d1f-181">いく必要があります、`WelcomeView`テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-181">Now we need a `WelcomeView` template!</span></span> <span data-ttu-id="03d1f-182">新しいコードがコンパイルされるために、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-182">Run the application so the new code is compiled.</span></span> <span data-ttu-id="03d1f-183">ブラウザーを閉じるには、内部を右クリックして、`Welcome`メソッド、およびクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="03d1f-183">Close the browser, right-click inside the `Welcome` method, and then click **Add View**.</span></span>

<span data-ttu-id="03d1f-184">ここでは、どのような**ビューの追加** ダイアログ ボックスはようになります。</span><span class="sxs-lookup"><span data-stu-id="03d1f-184">Here's what your **Add View** dialog box looks like.</span></span>

<span data-ttu-id="03d1f-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="03d1f-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="03d1f-186">次のコードを追加、 `<h2>` 、新しい要素*へようこそ* 。vbhtml ファイルです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-186">Add the following code under the `<h2>` element in the new *Welcome.*vbhtml file.</span></span> <span data-ttu-id="03d1f-187">うまくループを作成し、言う&quot;こんにちは&quot;ユーザーの質問がお回数だけです。</span><span class="sxs-lookup"><span data-stu-id="03d1f-187">We'll make a loop and say &quot;Hello&quot; as many times as the user says we should!</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

<span data-ttu-id="03d1f-188">アプリケーションを実行しを参照`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span><span class="sxs-lookup"><span data-stu-id="03d1f-188">Run the application and browse to `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span></span>

<span data-ttu-id="03d1f-189">今すぐデータが URL から取得され、コント ローラーに自動的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-189">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="03d1f-190">コント ローラーへのデータをパッケージ化、`Model`オブジェクトをビューにオブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="03d1f-190">The controller packages up the data into a `Model` object and passes that object to the view.</span></span> <span data-ttu-id="03d1f-191">ビューよりも、ユーザーに html 形式でデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03d1f-191">The view than displays the data as HTML to the user.</span></span>

<span data-ttu-id="03d1f-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="03d1f-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span></span>

<span data-ttu-id="03d1f-193">種類もの&quot;M&quot;モデルがデータベースの種類ではありません。</span><span class="sxs-lookup"><span data-stu-id="03d1f-193">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="03d1f-194">学習したことを確認し、ムービーのデータベースを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="03d1f-194">Let's take what we've learned and create a database of movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="03d1f-195">[前へ](adding-a-controller.md)
[次へ](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="03d1f-195">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
