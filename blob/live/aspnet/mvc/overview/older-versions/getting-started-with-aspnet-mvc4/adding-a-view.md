---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: "ビューの追加 |Microsoft ドキュメント"
author: Rick-Anderson
description: "注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4309290294b28d4c177e0193719bcff4b3f2a8cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view"></a><span data-ttu-id="0ff76-104">ビューの追加</span><span class="sxs-lookup"><span data-stu-id="0ff76-104">Adding a View</span></span>
====================
<span data-ttu-id="0ff76-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0ff76-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="0ff76-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="0ff76-107">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="0ff76-108">このセクションでしようとしている変更、`HelloWorldController`クリーンにするテンプレート ファイルは、クライアントに HTML 応答を生成するプロセスをカプセル化するビューを使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="0ff76-108">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="0ff76-109">テンプレート ファイルを使用してビューを作成、 [Razor ビュー エンジン](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)ASP.NET MVC 3 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="0ff76-109">You'll create a view template file using the [Razor view engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduced with ASP.NET MVC 3.</span></span> <span data-ttu-id="0ff76-110">Razor ベースのビューのテンプレートが、 *.cshtml*ファイル拡張子、および HTML を使用して C# の場合の出力を作成する簡潔な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-110">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="0ff76-111">Razor 文字とビュー テンプレートの作成時に必要なキーの数を最小化され、ワークフローをコーディングする高速で、流体できます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-111">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="0ff76-112">現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-112">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="0ff76-113">変更、`Index`を返すメソッドを`View`オブジェクトを次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="0ff76-113">Change the `Index` method to return a `View` object, as shown in the following code:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

<span data-ttu-id="0ff76-114">`Index`上記のメソッドでは、ビュー テンプレートを使用して、ブラウザーに HTML 応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-114">The `Index` method above uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="0ff76-115">コント ローラーのメソッド (とも呼ばれる[アクション メソッド](http://rachelappel.com/asp.net-mvc-actionresults-explained))、ように、`Index`一般に、上記のメソッドが返す、 [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (から派生したクラスまたは[ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)) などの文字列のないプリミティブ型。</span><span class="sxs-lookup"><span data-stu-id="0ff76-115">Controller methods (also known as [action methods](http://rachelappel.com/asp.net-mvc-actionresults-explained)), such as the `Index` method above, generally return an [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (or a class derived from [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)), not primitive types like string.</span></span>

<span data-ttu-id="0ff76-116">プロジェクトで、とともに使用できるビュー テンプレートの追加、`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0ff76-116">In the project, add a view template that you can use with the `Index` method.</span></span> <span data-ttu-id="0ff76-117">これを行うには、内部を右クリックして、`Index`メソッドとクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-117">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

![](adding-a-view/_static/image1.png)

<span data-ttu-id="0ff76-118">**ビューの追加** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-118">The **Add View** dialog box appears.</span></span> <span data-ttu-id="0ff76-119">既定値は、をクリックする方法のままにして、**追加**ボタン。</span><span class="sxs-lookup"><span data-stu-id="0ff76-119">Leave the defaults the way they are and click the **Add** button:</span></span>

![](adding-a-view/_static/image2.png)

<span data-ttu-id="0ff76-120">*MvcMovie\Views\HelloWorld*フォルダーおよび*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-120">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.cshtml* file are created.</span></span> <span data-ttu-id="0ff76-121">ご覧**ソリューション エクスプ ローラー**:</span><span class="sxs-lookup"><span data-stu-id="0ff76-121">You can see them in **Solution Explorer**:</span></span>

![](adding-a-view/_static/image3.png)

<span data-ttu-id="0ff76-122">次に示す、 *Index.cshtml*作成されたファイル。</span><span class="sxs-lookup"><span data-stu-id="0ff76-122">The following shows the *Index.cshtml* file that was created:</span></span>

![HelloWorldIndex](adding-a-view/_static/image4.png)

<span data-ttu-id="0ff76-124">次の HTML を追加、`<h2>`タグ。</span><span class="sxs-lookup"><span data-stu-id="0ff76-124">Add the following HTML under the `<h2>` tag.</span></span>

[!code-html[Main](adding-a-view/samples/sample2.html)]

<span data-ttu-id="0ff76-125">完全な*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-125">The complete *MvcMovie\Views\HelloWorld\Index.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

<span data-ttu-id="0ff76-126">ソリューション エクスプ ローラーで、Visual Studio 2012 を使用している場合を右クリックして、 *Index.cshtml*ファイルおよび選択した**Page Inspector で表示**です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-126">If you are using Visual Studio 2012, in solution explorer, right click the *Index.cshtml* file and select **View in Page Inspector**.</span></span>

![PI](adding-a-view/_static/image5.png)

<span data-ttu-id="0ff76-128">[Page Inspector チュートリアル](../../views/using-page-inspector-in-aspnet-mvc.md)この新しいツールの詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="0ff76-128">The [Page Inspector tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) has more information about this new tool.</span></span>

<span data-ttu-id="0ff76-129">また、アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="0ff76-129">Alternatively, run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="0ff76-130">`Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`、指定されている、メソッドが、ブラウザーへの応答を表示するためにテンプレート ファイルの表示を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ff76-130">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="0ff76-131">ASP.NET MVC の既定値を使用してに明示的に使用するビューのテンプレート ファイルの名前を指定しませんでしたので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0ff76-131">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="0ff76-132">次の図は、文字列を示しています。 &quot;、ビュー テンプレートから Hello!&quot;ビューで、ハードコーディングします。</span><span class="sxs-lookup"><span data-stu-id="0ff76-132">The image below shows the string &quot;Hello from our View Template!&quot; hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="0ff76-133">なかなか良いを検索します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-133">Looks pretty good.</span></span> <span data-ttu-id="0ff76-134">ただし、ブラウザーのタイトル バーが表示されていることを確認&quot;My ASP.NET A をインデックス&quot;ページの上部の大規模なリンクと&quot;ここにロゴです。&quot;下、 &quot;、ロゴ。 ここで&quot;リンクの登録とリンクでと、自宅にリンクしている以下のログについては、ページにお問い合わせください。</span><span class="sxs-lookup"><span data-stu-id="0ff76-134">However, notice that the browser's title bar shows &quot;Index My ASP.NET A&quot; and the big link on the top of the page says &quot;your logo here.&quot; Below the &quot;your logo here.&quot; link are registration and log in links, and below that links to Home, About and Contact pages.</span></span> <span data-ttu-id="0ff76-135">これらのいくつかの名前を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0ff76-135">Let's change some of these.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="0ff76-136">ビューとレイアウト ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-136">Changing Views and Layout Pages</span></span>

<span data-ttu-id="0ff76-137">最初に、変更する、 &quot;、ロゴ。 ここで&quot;、ページの上部にタイトルです。</span><span class="sxs-lookup"><span data-stu-id="0ff76-137">First, you want to change the &quot;your logo here.&quot; title at the top of the page.</span></span> <span data-ttu-id="0ff76-138">テキストがすべてのページに共通します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-138">That text is common to every page.</span></span> <span data-ttu-id="0ff76-139">アプリケーション内の各ページに表示される場合でも、プロジェクト内の 1 か所で実際に実装されます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-139">It's actually implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="0ff76-140">移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0ff76-140">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="0ff76-141">このファイルが呼び出され、*レイアウト ページ*であり、共有&quot;シェル&quot;他のすべてのページを使用します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-141">This file is called a *layout page* and it's the shared &quot;shell&quot; that all other pages use.</span></span>

![_LayoutCshtml](adding-a-view/_static/image7.png)

<span data-ttu-id="0ff76-143">レイアウト テンプレートを使用すると、1 か所で、コンテナーの HTML レイアウトは、サイトを指定し、サイト内の複数のページにわたって適用できます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-143">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="0ff76-144">`@RenderBody()` という行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="0ff76-145">`RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに&quot;ラップ&quot;されます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-145">`RenderBody` is a placeholder where all the view-specific pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="0ff76-146">たとえば、バージョン情報のリンクを選択する場合、 *Views\Home\About.cshtml*内部ビューが表示される、`RenderBody`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0ff76-146">For example, if you select the About link, the *Views\Home\About.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<span data-ttu-id="0ff76-147">レイアウト テンプレートからのサイトのタイトルの見出しを変更&quot;ここにロゴ&quot;に&quot;MVC ムービー&quot;です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-147">Change the site-title heading in the layout template from &quot;your logo here&quot; to &quot;MVC Movie&quot;.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="0ff76-148">Title 要素の内容を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-148">Replace the contents of the title element with the following markup:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

<span data-ttu-id="0ff76-149">アプリケーションとに注意してください、今すぐ実行&quot;MVC ムービー&quot;です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-149">Run the application and notice that it now says &quot;MVC Movie &quot;.</span></span> <span data-ttu-id="0ff76-150">をクリックして、**に関する**リンク、およびするは、そのページに表示する方法を参照してください。 &quot;MVC ムービー&quot;、すぎます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-150">Click the **About** link, and you see how that page shows &quot;MVC Movie&quot;, too.</span></span> <span data-ttu-id="0ff76-151">レイアウト テンプレートに 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.</span><span class="sxs-lookup"><span data-stu-id="0ff76-151">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image8.png)

<span data-ttu-id="0ff76-152">ここで、インデックス ビューのタイトルを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0ff76-152">Now, let's change the title of the Index view.</span></span>

<span data-ttu-id="0ff76-153">開いている*MvcMovie\Views\HelloWorld\Index.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-153">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="0ff76-154">2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。</span><span class="sxs-lookup"><span data-stu-id="0ff76-154">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="0ff76-155">これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-155">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

<span data-ttu-id="0ff76-156">HTML 表示するタイトルをセットの上に、コードを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (である、 *Index.cshtml*テンプレートの表示)。</span><span class="sxs-lookup"><span data-stu-id="0ff76-156">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="0ff76-157">戻るレイアウト テンプレートのソース コードを見ることがわかりますテンプレートがこの値を使用する、`<title>`要素の一部として、`<head>`以前に変更した HTML の「します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-157">If you look back at the source code of the layout template, you'll notice that the template uses this value in the `<title>` element as part of the `<head>` section of the HTML that we modified previously.</span></span> <span data-ttu-id="0ff76-158">これを使用して`ViewBag`アプローチでは、簡単に渡せる他のパラメーター、ビュー テンプレートと、レイアウト ファイルの間です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-158">Using this `ViewBag` approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="0ff76-159">アプリケーションを実行しを参照`http://localhost:xx/HelloWorld`です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-159">Run the application and browse to `http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="0ff76-160">ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください </span><span class="sxs-lookup"><span data-stu-id="0ff76-160">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="0ff76-161">(ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0ff76-161">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="0ff76-162">ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルが作成、`ViewBag.Title`設定、 *Index.cshtml*テンプレートおよびその他の表示&quot;-ムービー アプリ&quot;レイアウト ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-162">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with the `ViewBag.Title` we set in the *Index.cshtml* view template and the additional &quot;- Movie App&quot; added in the layout file.</span></span>

<span data-ttu-id="0ff76-163">また方法でコンテンツ、 *Index.cshtml*ビュー テンプレートのトピックとマージされました、  *\_Layout.cshtml*ビュー テンプレートおよび 1 つの HTML 応答をブラウザーに送信されました。</span><span class="sxs-lookup"><span data-stu-id="0ff76-163">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="0ff76-164">レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-164">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="0ff76-165">ほんの少しの&quot;データ&quot;(ここでは、 &quot;、ビュー テンプレートからこんにちは!&quot;メッセージ) は、ハードコードです。</span><span class="sxs-lookup"><span data-stu-id="0ff76-165">Our little bit of &quot;data&quot; (in this case the &quot;Hello from our View Template!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="0ff76-166">MVC アプリケーションには、 &quot;V&quot; (ビュー) を取得したら、 &quot;C&quot; (コント ローラー) が&quot;M&quot;まだ (モデル)。</span><span class="sxs-lookup"><span data-stu-id="0ff76-166">The MVC application has a &quot;V&quot; (view) and you've got a &quot;C&quot; (controller), but no &quot;M&quot; (model) yet.</span></span> <span data-ttu-id="0ff76-167">間もなく、方法を説明するデータベースを作成し、モデル データを取得します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-167">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="0ff76-168">コントローラーからビューへのデータの受け渡し</span><span class="sxs-lookup"><span data-stu-id="0ff76-168">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="0ff76-169">データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-169">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="0ff76-170">受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-170">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="0ff76-171">コント ローラー クラスでは、受信ブラウザーを処理するコードは、要求をデータベースからデータを取得し、最終的には、ブラウザーに返送する応答の種類を決定を記述します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-171">A controller class is where you write the code that handles the incoming browser requests, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="0ff76-172">テンプレートの表示は、生成し、ブラウザーに HTML 応答の書式を設定するコント ローラーから使用できます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-172">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="0ff76-173">コント ローラーは、ブラウザーへの応答をレンダリングするビュー テンプレートの順に必要なすべてのデータまたはオブジェクトを提供します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-173">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="0ff76-174">ベスト プラクティス:**ビュー テンプレートのビジネス ロジックを実行またはデータベースと直接やり取りする必要がありますしない**です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-174">A best practice: **A view template should never perform business logic or interact with a database directly**.</span></span> <span data-ttu-id="0ff76-175">代わりに、ビュー テンプレートは、コント ローラーに設定されているデータのみを操作する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ff76-175">Instead, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="0ff76-176">これを維持する&quot;関心の分離&quot;完全テスト可能でありより優れたコードに保ちます。</span><span class="sxs-lookup"><span data-stu-id="0ff76-176">Maintaining this &quot;separation of concerns&quot; helps keep your code clean, testable and more maintainable.</span></span>

<span data-ttu-id="0ff76-177">現時点では、`Welcome`でアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターと、ブラウザーに直接値を出力します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-177">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="0ff76-178">この応答を文字列としてのレンダリング コント ローラーがあるのではなくビュー テンプレートを代わりに使用するコント ローラーに変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0ff76-178">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="0ff76-179">このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-179">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="0ff76-180">コント ローラーにテンプレートの表示に必要な動的なデータ (パラメーター) を置くことによってこれを行う、`ViewBag`ビュー テンプレートにアクセスできるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0ff76-180">You can do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="0ff76-181">戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0ff76-181">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="0ff76-182">`ViewBag`動的なオブジェクトは、つまりに自由を配置する;`ViewBag`オブジェクトを持たないプロパティに定義されたものと、その内部に配置するまでです。</span><span class="sxs-lookup"><span data-stu-id="0ff76-182">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="0ff76-183">[ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)名前付きパラメーターが自動的にマップ (`name`と`numTimes`)、アドレス バーに、メソッドのパラメーターのクエリ文字列から。</span><span class="sxs-lookup"><span data-stu-id="0ff76-183">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="0ff76-184">完全な *HelloWorldController.cs* ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0ff76-184">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="0ff76-185">これで、`ViewBag`オブジェクトには、自動的にビューに渡されるデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0ff76-185">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span>

<span data-ttu-id="0ff76-186">次に、開始ビュー テンプレートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ff76-186">Next, you need a Welcome view template!</span></span> <span data-ttu-id="0ff76-187">**ビルド**メニューの **ビルド MvcMovie**プロジェクトをコンパイルするかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-187">In the **Build** menu, select **Build MvcMovie** to make sure the project is compiled.</span></span>

<span data-ttu-id="0ff76-188">内で右クリックし、`Welcome`メソッドとクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-188">Then right-click inside the `Welcome` method and click **Add View**.</span></span>

![](adding-a-view/_static/image10.png)

<span data-ttu-id="0ff76-189">ここでは、どのような**ビューの追加** ダイアログ ボックスはようになります。</span><span class="sxs-lookup"><span data-stu-id="0ff76-189">Here's what the **Add View** dialog box looks like:</span></span>

![](adding-a-view/_static/image11.png)

<span data-ttu-id="0ff76-190">をクリックして**追加**、下にある次のコードを追加し、 `<h2>` 、新しい要素*Welcome.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0ff76-190">Click **Add**, and then add the following code under the `<h2>` element in the new *Welcome.cshtml* file.</span></span> <span data-ttu-id="0ff76-191">表示されているループを作成します&quot;こんにちは&quot;として何度でも、ユーザーの質問にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0ff76-191">You'll create a loop that says &quot;Hello&quot; as many times as the user says it should.</span></span> <span data-ttu-id="0ff76-192">完全な*Welcome.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-192">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

<span data-ttu-id="0ff76-193">アプリケーションを実行し、次の URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-193">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="0ff76-194">データが URL から取得され、コント ローラーを使用して、渡されるようになりました、[モデル バインダー](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-194">Now data is taken from the URL and passed to the controller using the [model binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span></span> <span data-ttu-id="0ff76-195">コント ローラーへのデータをパッケージ化、`ViewBag`オブジェクトをビューにオブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-195">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="0ff76-196">ビューは、データを HTML として、ユーザーにし、表示します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-196">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image12.png)

<span data-ttu-id="0ff76-197">上記のサンプルで使用して、`ViewBag`コント ローラーからビューにデータを渡すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0ff76-197">In the sample above, we used a `ViewBag` object to pass data from the controller to a view.</span></span> <span data-ttu-id="0ff76-198">後者のチュートリアルでは、ビュー モデルをデータを渡すコント ローラーからビューを使用します。</span><span class="sxs-lookup"><span data-stu-id="0ff76-198">Latter in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="0ff76-199">モデルの表示方法でデータを渡すには、ビュー バッグ アプローチ経由で一般にはるかに優先です。</span><span class="sxs-lookup"><span data-stu-id="0ff76-199">The view model approach to passing data is generally much preferred over the view bag approach.</span></span> <span data-ttu-id="0ff76-200">ブログ記事を参照してください[動的 V 厳密に型指定されたビュー](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="0ff76-200">See the blog entry [Dynamic V Strongly Typed Views](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) for more information.</span></span>

<span data-ttu-id="0ff76-201">種類もの&quot;M&quot;モデルがデータベースの種類ではありません。</span><span class="sxs-lookup"><span data-stu-id="0ff76-201">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="0ff76-202">学習したことを確認し、ムービーのデータベースを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0ff76-202">Let's take what we've learned and create a database of movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0ff76-203">[前へ](adding-a-controller.md)
[次へ](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="0ff76-203">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
