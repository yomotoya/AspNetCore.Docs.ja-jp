---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: "ビュー (c#) を追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 46d5494e668dfe156aeb6647ded83e6ce5366714
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-c"></a><span data-ttu-id="80099-103">ビュー (c#) を追加します。</span><span class="sxs-lookup"><span data-stu-id="80099-103">Adding a View (C#)</span></span>
====================
<span data-ttu-id="80099-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="80099-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="80099-105">このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="80099-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="80099-106">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="80099-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="80099-107">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="80099-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="80099-108">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="80099-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="80099-109">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="80099-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="80099-110">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="80099-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="80099-111">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="80099-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="80099-112">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="80099-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="80099-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="80099-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="80099-114">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="80099-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="80099-115">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="80099-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="80099-116">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="80099-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="80099-117">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="80099-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="80099-118">このセクションでしようとしている変更、`HelloWorldController`クリーンにするテンプレート ファイルは、クライアントに HTML 応答を生成するプロセスをカプセル化するビューを使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="80099-118">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="80099-119">使用して、新しいビュー テンプレート ファイルを作成する[Razor ビュー エンジン](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)ASP.NET MVC 3 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="80099-119">You'll create a view template file using the new [Razor view engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduced with ASP.NET MVC 3.</span></span> <span data-ttu-id="80099-120">Razor ベースのビューのテンプレートが、 *.cshtml*ファイル拡張子、および HTML を使用して C# の場合の出力を作成する簡潔な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="80099-120">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="80099-121">Razor 文字とビュー テンプレートの作成時に必要なキーの数を最小化され、ワークフローをコーディングする高速で、流体できます。</span><span class="sxs-lookup"><span data-stu-id="80099-121">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="80099-122">ビューのテンプレートを使用して、開始、`Index`メソッドで、`HelloWorldController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="80099-122">Start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="80099-123">現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="80099-123">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="80099-124">変更、`Index`を返すメソッドを`View`オブジェクトを次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="80099-124">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

<span data-ttu-id="80099-125">このコードでは、ビュー テンプレートを使用して、ブラウザーに HTML 応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="80099-125">This code uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="80099-126">プロジェクトで、とともに使用できるビュー テンプレートの追加、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="80099-126">In the project, add a view template that you can use with the `Index` method.</span></span> <span data-ttu-id="80099-127">これを行うには、内部を右クリックして、`Index`メソッドとクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="80099-127">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

![](adding-a-view/_static/image1.png)

<span data-ttu-id="80099-128">**ビューの追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80099-128">The **Add View** dialog box appears.</span></span> <span data-ttu-id="80099-129">既定値は、をクリックする方法のままにして、**追加**ボタン。</span><span class="sxs-lookup"><span data-stu-id="80099-129">Leave the defaults the way they are and click the **Add** button:</span></span>

![](adding-a-view/_static/image2.png)

<span data-ttu-id="80099-130">*MvcMovie\Views\HelloWorld*フォルダーおよび*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="80099-130">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.cshtml* file are created.</span></span> <span data-ttu-id="80099-131">ご覧**ソリューション エクスプ ローラー**:</span><span class="sxs-lookup"><span data-stu-id="80099-131">You can see them in **Solution Explorer**:</span></span>

![](adding-a-view/_static/image3.png)

<span data-ttu-id="80099-132">次に示す、 *Index.cshtml*作成されたファイル。</span><span class="sxs-lookup"><span data-stu-id="80099-132">The following shows the *Index.cshtml* file that was created:</span></span>

<span data-ttu-id="80099-133">[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="80099-133">[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)</span></span>

<span data-ttu-id="80099-134">HTML を追加、`<h2>`タグ。</span><span class="sxs-lookup"><span data-stu-id="80099-134">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="80099-135">変更された*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="80099-135">The modified *MvcMovie\Views\HelloWorld\Index.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

<span data-ttu-id="80099-136">アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="80099-136">Run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="80099-137">`Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`、指定されている、メソッドが、ブラウザーへの応答を表示するためにテンプレート ファイルの表示を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="80099-137">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="80099-138">ASP.NET MVC の既定値を使用してに明示的に使用するビューのテンプレート ファイルの名前を指定しませんでしたので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="80099-138">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="80099-139">次の図は、ビューで、ハードコーディング文字列を示しています。</span><span class="sxs-lookup"><span data-stu-id="80099-139">The image below shows the string hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="80099-140">なかなか良いを検索します。</span><span class="sxs-lookup"><span data-stu-id="80099-140">Looks pretty good.</span></span> <span data-ttu-id="80099-141">ただし、"Index"ブラウザーのタイトル バーに表示し、ページで、大きなタイトルは「My MVC アプリケーションです」ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="80099-141">However, notice that the browser's title bar says "Index" and the big title on the page says "My MVC Application."</span></span> <span data-ttu-id="80099-142">これらの名前を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80099-142">Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="80099-143">ビューとレイアウト ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="80099-143">Changing Views and Layout Pages</span></span>

<span data-ttu-id="80099-144">最初に、ページの上部にある「マイ MVC アプリケーション」タイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="80099-144">First, you want to change the "My MVC Application" title at the top of the page.</span></span> <span data-ttu-id="80099-145">テキストがすべてのページに共通します。</span><span class="sxs-lookup"><span data-stu-id="80099-145">That text is common to every page.</span></span> <span data-ttu-id="80099-146">実際に実装するもの、プロジェクト内の 1 か所で、アプリケーション内の各ページに表示される場合でも。</span><span class="sxs-lookup"><span data-stu-id="80099-146">It actually is implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="80099-147">移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80099-147">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="80099-148">このファイルが呼び出され、*レイアウト ページ*は共有「シェル」その他のすべてのページを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="80099-148">This file is called a *layout page* and it's the shared "shell" that all other pages use.</span></span>

<span data-ttu-id="80099-149">[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="80099-149">[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="80099-150">レイアウト テンプレートを使用すると、1 か所で、コンテナーの HTML レイアウトは、サイトを指定し、サイト内の複数のページにわたって適用できます。</span><span class="sxs-lookup"><span data-stu-id="80099-150">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="80099-151">注、`@RenderBody()`ファイルの下部にある行。</span><span class="sxs-lookup"><span data-stu-id="80099-151">Note the `@RenderBody()` line near the bottom of the file.</span></span> <span data-ttu-id="80099-152">`RenderBody`場所を作成するすべてのビューに固有のページを表示する、レイアウト ページで「ラップされた」プレース ホルダー。</span><span class="sxs-lookup"><span data-stu-id="80099-152">`RenderBody` is a placeholder where all the view-specific pages you create show up, "wrapped" in the layout page.</span></span> <span data-ttu-id="80099-153">「MVC ムービー アプリ」に「マイ MVC アプリケーション」からレイアウト テンプレートのタイトルの見出しを変更します。</span><span class="sxs-lookup"><span data-stu-id="80099-153">Change the title heading in the layout template from "My MVC Application" to "MVC Movie App".</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

<span data-ttu-id="80099-154">アプリケーションを実行し、今すぐ"MVC ムービー App"をということに注意してください。</span><span class="sxs-lookup"><span data-stu-id="80099-154">Run the application and notice that it now says "MVC Movie App".</span></span> <span data-ttu-id="80099-155">クリックして、**に関する**リンク、および、そのページがすぎます"MVC ムービー App"を表示を参照してください。</span><span class="sxs-lookup"><span data-stu-id="80099-155">Click the **About** link, and you see how that page shows "MVC Movie App", too.</span></span> <span data-ttu-id="80099-156">レイアウト テンプレートに 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.</span><span class="sxs-lookup"><span data-stu-id="80099-156">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="80099-157">完全な *\_Layout.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="80099-157">The complete *\_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="80099-158">ここで、インデックス ページ (ビュー) のタイトルを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80099-158">Now, let's change the title of the Index page (view).</span></span>

<span data-ttu-id="80099-159">開いている*MvcMovie\Views\HelloWorld\Index.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="80099-159">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="80099-160">2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。</span><span class="sxs-lookup"><span data-stu-id="80099-160">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="80099-161">これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="80099-161">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

<span data-ttu-id="80099-162">HTML 表示するタイトルをセットの上に、コードを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (である、 *Index.cshtml*テンプレートの表示)。</span><span class="sxs-lookup"><span data-stu-id="80099-162">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="80099-163">戻るレイアウト テンプレートのソース コードを見ることがわかりますテンプレートがこの値を使用する、`<title>`要素の一部として、 `<head>` HTML の「します。</span><span class="sxs-lookup"><span data-stu-id="80099-163">If you look back at the source code of the layout template, you'll notice that the template uses this value in the `<title>` element as part of the `<head>` section of the HTML.</span></span> <span data-ttu-id="80099-164">このアプローチを使用して、ビューのテンプレートと、レイアウト ファイルの間での他のパラメーターを簡単に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="80099-164">Using this approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="80099-165">アプリケーションを実行しを参照`http://localhost:xx/HelloWorld`です。</span><span class="sxs-lookup"><span data-stu-id="80099-165">Run the application and browse to `http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="80099-166">ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください </span><span class="sxs-lookup"><span data-stu-id="80099-166">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="80099-167">(ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="80099-167">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="80099-168">ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。</span><span class="sxs-lookup"><span data-stu-id="80099-168">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="80099-169">また方法でコンテンツ、 *Index.cshtml*ビュー テンプレートのトピックとマージされました、  *\_Layout.cshtml*ビュー テンプレートおよび 1 つの HTML 応答をブラウザーに送信されました。</span><span class="sxs-lookup"><span data-stu-id="80099-169">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="80099-170">レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="80099-170">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image10.png)

<span data-ttu-id="80099-171">ここでは、"データ" のごく一部 (この場合は "Hello from our View Template!" というメッセージ) を</span><span class="sxs-lookup"><span data-stu-id="80099-171">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="80099-172">ハード コーディングしました。</span><span class="sxs-lookup"><span data-stu-id="80099-172">message) is hard-coded, though.</span></span> <span data-ttu-id="80099-173">MVC アプリケーションには "V" (ビュー) があり、"C" (コントローラー) もありますが、"M" (モデル) はまだありません。</span><span class="sxs-lookup"><span data-stu-id="80099-173">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span> <span data-ttu-id="80099-174">間もなく、方法を説明するデータベースを作成し、モデル データを取得します。</span><span class="sxs-lookup"><span data-stu-id="80099-174">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="80099-175">コントローラーからビューへのデータの受け渡し</span><span class="sxs-lookup"><span data-stu-id="80099-175">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="80099-176">データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。</span><span class="sxs-lookup"><span data-stu-id="80099-176">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="80099-177">受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="80099-177">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="80099-178">コント ローラー クラスでは、受信パラメーターを処理、データがデータベースから取得、および最終的には、ブラウザーに返送する応答の種類を決定するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="80099-178">A controller class is where you write the code that handles the incoming parameters, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="80099-179">テンプレートの表示は、生成し、ブラウザーに HTML 応答の書式を設定するコント ローラーから使用できます。</span><span class="sxs-lookup"><span data-stu-id="80099-179">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="80099-180">コント ローラーは、ブラウザーへの応答をレンダリングするビュー テンプレートの順に必要なすべてのデータまたはオブジェクトを提供します。</span><span class="sxs-lookup"><span data-stu-id="80099-180">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="80099-181">ビュー テンプレートは、ビジネス ロジックを実行したり、データベースと直接やり取りする必要がありますしないでください。</span><span class="sxs-lookup"><span data-stu-id="80099-181">A view template should never perform business logic or interact with a database directly.</span></span> <span data-ttu-id="80099-182">代わりに、コント ローラーに設定されているデータでのみ処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="80099-182">Instead, it should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="80099-183">この「関心の分離」を維持することは、クリーンと保守の容易なコードを維持に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="80099-183">Maintaining this "separation of concerns" helps keep your code clean and more maintainable.</span></span>

<span data-ttu-id="80099-184">現時点では、`Welcome`でアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターと、ブラウザーに直接値を出力します。</span><span class="sxs-lookup"><span data-stu-id="80099-184">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="80099-185">この応答を文字列としてのレンダリング コント ローラーがあるのではなくビュー テンプレートを代わりに使用するコント ローラーに変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80099-185">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="80099-186">このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="80099-186">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="80099-187">コント ローラーにテンプレートの表示に必要な動的なデータをまとめることによってこれを行う、`ViewBag`ビュー テンプレートにアクセスできるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="80099-187">You can do this by having the controller put the dynamic data that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="80099-188">戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="80099-188">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="80099-189">`ViewBag`動的なオブジェクトは、つまりに自由を配置する;`ViewBag`オブジェクトを持たないプロパティに定義されたものと、その内部に配置するまでです。</span><span class="sxs-lookup"><span data-stu-id="80099-189">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="80099-190">完全な *HelloWorldController.cs* ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="80099-190">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

<span data-ttu-id="80099-191">これで、`ViewBag`オブジェクトには、自動的にビューに渡されるデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="80099-191">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span>

<span data-ttu-id="80099-192">次に、開始ビュー テンプレートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="80099-192">Next, you need a Welcome view template!</span></span> <span data-ttu-id="80099-193">**デバッグ**メニューの **ビルド MvcMovie**プロジェクトをコンパイルするかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="80099-193">In the **Debug** menu, select **Build MvcMovie** to make sure the project is compiled.</span></span>

<span data-ttu-id="80099-194">[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="80099-194">[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="80099-195">内で右クリックし、`Welcome`メソッドとクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="80099-195">Then right-click inside the `Welcome` method and click **Add View**.</span></span> <span data-ttu-id="80099-196">ここでは、どのような**ビューの追加** ダイアログ ボックスはようになります。</span><span class="sxs-lookup"><span data-stu-id="80099-196">Here's what the **Add View** dialog box looks like:</span></span>

![](adding-a-view/_static/image13.png)

<span data-ttu-id="80099-197">をクリックして**追加**、下にある次のコードを追加し、 `<h2>` 、新しい要素*Welcome.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="80099-197">Click **Add**, and then add the following code under the `<h2>` element in the new *Welcome.cshtml* file.</span></span> <span data-ttu-id="80099-198">数は、ユーザーの質問と同じ回数「こんにちは」と表示されているループを作成します。</span><span class="sxs-lookup"><span data-stu-id="80099-198">You'll create a loop that says "Hello" as many times as the user says it should.</span></span> <span data-ttu-id="80099-199">完全な*Welcome.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="80099-199">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

<span data-ttu-id="80099-200">アプリケーションを実行し、次の URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="80099-200">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="80099-201">今すぐデータが URL から取得され、コント ローラーに自動的に渡されます。</span><span class="sxs-lookup"><span data-stu-id="80099-201">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="80099-202">コント ローラーへのデータをパッケージ化、`ViewBag`オブジェクトをビューにオブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="80099-202">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="80099-203">ビューは、データを HTML として、ユーザーにし、表示します。</span><span class="sxs-lookup"><span data-stu-id="80099-203">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image14.png)

<span data-ttu-id="80099-204">"M" (モデル) については学習しましたが、データベースについてはまだです。</span><span class="sxs-lookup"><span data-stu-id="80099-204">Well, that was a kind of an "M" for model, but not the database kind.</span></span> <span data-ttu-id="80099-205">学習したことを確認し、ムービーのデータベースを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80099-205">Let's take what we've learned and create a database of movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="80099-206">[前へ](adding-a-controller.md)
[次へ](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="80099-206">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
