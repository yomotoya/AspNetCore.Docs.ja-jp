---
title: "MVC アプリケーションにビューの追加"
author: Rick-Anderson
description: "MVC アプリケーションにビューの追加"
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 52f15784f16d355791360021f045cf4f3c467897
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2017
---
<a name="adding-a-view"></a><span data-ttu-id="94693-103">ビューの追加</span><span class="sxs-lookup"><span data-stu-id="94693-103">Adding a View</span></span>
====================
<span data-ttu-id="94693-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="94693-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="94693-105">このセクションでしようとしている変更、`HelloWorldController`クリーンにするテンプレート ファイルは、クライアントに HTML 応答を生成するプロセスをカプセル化するビューを使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="94693-105">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span> 

<span data-ttu-id="94693-106">使用してビュー テンプレート ファイルを作成、 [Razor ビュー エンジン](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md)です。</span><span class="sxs-lookup"><span data-stu-id="94693-106">You'll create a view template file using the [Razor view engine](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md).</span></span> <span data-ttu-id="94693-107">Razor ベースのビューのテンプレートが、 *.cshtml*ファイル拡張子、および HTML を使用して C# の場合の出力を作成する簡潔な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="94693-107">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="94693-108">Razor 文字とビュー テンプレートの作成時に必要なキーの数を最小化され、ワークフローをコーディングする高速で、流体できます。</span><span class="sxs-lookup"><span data-stu-id="94693-108">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="94693-109">現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="94693-109">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="94693-110">変更、`Index`を返すメソッドを`View`オブジェクトを次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="94693-110">Change the `Index` method to return a `View` object, as shown in the following code:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

<span data-ttu-id="94693-111">`Index`上記のメソッドでは、ビュー テンプレートを使用して、ブラウザーに HTML 応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="94693-111">The `Index` method above uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="94693-112">コント ローラーのメソッド (とも呼ばれる[アクション メソッド](http://rachelappel.com/asp.net-mvc-actionresults-explained))、ように、`Index`一般に、上記のメソッドが返す、 [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (から派生したクラスまたは[ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)) などの文字列のないプリミティブ型。</span><span class="sxs-lookup"><span data-stu-id="94693-112">Controller methods (also known as [action methods](http://rachelappel.com/asp.net-mvc-actionresults-explained)), such as the `Index` method above, generally return an [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (or a class derived from [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)), not primitive types like string.</span></span>

<span data-ttu-id="94693-113">右クリックして、 *Views\HelloWorld*フォルダーをクリック**追加**をクリックし、 **MVC 5 ビュー ページ (Razor レイアウト)**です。</span><span class="sxs-lookup"><span data-stu-id="94693-113">Right click the *Views\HelloWorld* folder and click **Add**, then click **MVC 5 View Page with (Layout Razor)**.</span></span>
  
![](adding-a-view/_static/image1.png)   
  
<span data-ttu-id="94693-114">**項目の名前を指定** ダイアログ ボックスに、入力*インデックス*、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="94693-114">In the **Specify Name for Item** dialog box, enter *Index*, and then click **OK**.</span></span>  
  
![](adding-a-view/_static/image2.png)  
  
<span data-ttu-id="94693-115">**レイアウト ページの選択**ダイアログ ボックスで、既定値をそのまま使用 **\_Layout.cshtml**  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="94693-115">In the **Select a Layout Page** dialog, accept the default **\_Layout.cshtml** and click **OK**.</span></span>  
  
![](adding-a-view/_static/image3.png)  
  
<span data-ttu-id="94693-116">上記のダイアログ ボックスで、 *\shared*左側のウィンドウでフォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="94693-116">In the dialog above, the *Views\Shared* folder is selected in the left pane.</span></span> <span data-ttu-id="94693-117">別のフォルダーで、カスタム レイアウト ファイルをした場合を選択する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="94693-117">If you had a custom layout file in another folder, you could select it.</span></span> <span data-ttu-id="94693-118">レイアウト ファイルは、チュートリアルの後半で取り上げます</span><span class="sxs-lookup"><span data-stu-id="94693-118">We'll talk about the layout file later in the tutorial</span></span>

<span data-ttu-id="94693-119">*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="94693-119">The *MvcMovie\Views\HelloWorld\Index.cshtml* file is created.</span></span>

![](adding-a-view/_static/image4.png)

<span data-ttu-id="94693-120">次の強調表示されているマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="94693-120">Add the following highlighted markup.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

<span data-ttu-id="94693-121">右クリックして、 *Index.cshtml*ファイルおよび選択した**ブラウザーで表示**です。</span><span class="sxs-lookup"><span data-stu-id="94693-121">Right click the *Index.cshtml* file and select **View in Browser**.</span></span>

![PI](adding-a-view/_static/image5.png)

<span data-ttu-id="94693-123">右クリック、 *Index.cshtml*ファイルおよび選択した**Page Inspector で表示します。**</span><span class="sxs-lookup"><span data-stu-id="94693-123">You can also right click the *Index.cshtml* file and select **View in Page Inspector.**</span></span> <span data-ttu-id="94693-124">参照してください、 [Page Inspector チュートリアル](../../views/using-page-inspector-in-aspnet-mvc.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="94693-124">See the [Page Inspector tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) for more information.</span></span>

<span data-ttu-id="94693-125">また、アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="94693-125">Alternatively, run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="94693-126">`Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`、指定されている、メソッドが、ブラウザーへの応答を表示するためにテンプレート ファイルの表示を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94693-126">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="94693-127">ASP.NET MVC の既定値を使用してに明示的に使用するビューのテンプレート ファイルの名前を指定しませんでしたので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="94693-127">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="94693-128">次の図は、文字列を示しています。 &quot;、ビュー テンプレートから Hello!&quot;ビューで、ハードコーディングします。</span><span class="sxs-lookup"><span data-stu-id="94693-128">The image below shows the string &quot;Hello from our View Template!&quot; hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="94693-129">なかなか良いを検索します。</span><span class="sxs-lookup"><span data-stu-id="94693-129">Looks pretty good.</span></span> <span data-ttu-id="94693-130">ただし、ブラウザーのタイトル バーが表示されていることを確認&quot;インデックス - マイ ASP.NET アプリケーション"とページの上部の大規模なリンクは、"Application name"を表示</span><span class="sxs-lookup"><span data-stu-id="94693-130">However, notice that the browser's title bar shows &quot;Index - My ASP.NET Appli" and the big link on the top of the page says "Application name."</span></span> <span data-ttu-id="94693-131">によってどのように、ブラウザー ウィンドウを行うと、表示する右上の 3 つのバーをクリックする必要がありますに、**ホーム**、**に関する**、**連絡先**、 **登録**と**ログイン**リンクします。</span><span class="sxs-lookup"><span data-stu-id="94693-131">Depending on how small you make your browser window, you might need to click the three bars in the upper right to see the to the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="94693-132">ビューとレイアウト ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="94693-132">Changing Views and Layout Pages</span></span>

<span data-ttu-id="94693-133">最初に、変更する、&quot;アプリケーション名&quot;ページの上部にリンクします。</span><span class="sxs-lookup"><span data-stu-id="94693-133">First, you want to change the &quot;Application name&quot; link at the top of the page.</span></span> <span data-ttu-id="94693-134">テキストがすべてのページに共通します。</span><span class="sxs-lookup"><span data-stu-id="94693-134">That text is common to every page.</span></span> <span data-ttu-id="94693-135">アプリケーション内の各ページに表示される場合でも、プロジェクト内の 1 か所で実際に実装されます。</span><span class="sxs-lookup"><span data-stu-id="94693-135">It's actually implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="94693-136">移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="94693-136">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="94693-137">このファイルが呼び出され、*レイアウト ページ*他のすべてのページを使用する共有フォルダー内にあるとします。</span><span class="sxs-lookup"><span data-stu-id="94693-137">This file is called a *layout page* and it's in the shared folder that all other pages use.</span></span>

![_LayoutCshtml](adding-a-view/_static/image7.png)

<span data-ttu-id="94693-139">レイアウト テンプレートを使用すると、1 か所で、コンテナーの HTML レイアウトは、サイトを指定し、サイト内の複数のページにわたって適用できます。</span><span class="sxs-lookup"><span data-stu-id="94693-139">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="94693-140">`@RenderBody()` という行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="94693-140">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="94693-141">`RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに&quot;ラップ&quot;されます。</span><span class="sxs-lookup"><span data-stu-id="94693-141">`RenderBody` is a placeholder where all the view-specific pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="94693-142">たとえば、選択した場合、**に関する**リンク、 *Views\Home\About.cshtml*内部ビューが表示される、`RenderBody`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="94693-142">For example, if you select the **About** link, the *Views\Home\About.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<span data-ttu-id="94693-143">タイトル要素の内容を変更します。</span><span class="sxs-lookup"><span data-stu-id="94693-143">Change the contents of the title element.</span></span> <span data-ttu-id="94693-144">変更、 [ActionLink](https://msdn.microsoft.com/en-us/library/dd504972(v=vs.108).aspx)からレイアウト テンプレートに&quot;アプリケーション名&quot;に&quot;MVC ムービー&quot;とコント ローラーから`Home`に`Movies`です。</span><span class="sxs-lookup"><span data-stu-id="94693-144">Change the [ActionLink](https://msdn.microsoft.com/en-us/library/dd504972(v=vs.108).aspx) in the layout template from &quot;Application name&quot; to &quot;MVC Movie&quot; and the controller from `Home` to `Movies`.</span></span> <span data-ttu-id="94693-145">完全なレイアウト ファイルは、次に示します。</span><span class="sxs-lookup"><span data-stu-id="94693-145">The complete layout file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

<span data-ttu-id="94693-146">アプリケーションとに注意してください、今すぐ実行&quot;MVC ムービー&quot;です。</span><span class="sxs-lookup"><span data-stu-id="94693-146">Run the application and notice that it now says &quot;MVC Movie &quot;.</span></span> <span data-ttu-id="94693-147">をクリックして、**に関する**リンク、およびするは、そのページに表示する方法を参照してください。 &quot;MVC ムービー&quot;、すぎます。</span><span class="sxs-lookup"><span data-stu-id="94693-147">Click the **About** link, and you see how that page shows &quot;MVC Movie&quot;, too.</span></span> <span data-ttu-id="94693-148">レイアウト テンプレートに 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.</span><span class="sxs-lookup"><span data-stu-id="94693-148">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image8.png)

<span data-ttu-id="94693-149">作成したときに、 *Views\HelloWorld\Index.cshtml*ファイルでは、次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="94693-149">When we first created the *Views\HelloWorld\Index.cshtml* file, it contained the following code:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="94693-150">上記の Razor コードは、レイアウト ページを明示的に設定です。</span><span class="sxs-lookup"><span data-stu-id="94693-150">The Razor code above is explicitly setting the layout page.</span></span> <span data-ttu-id="94693-151">確認、*ビュー\\は _viewstart.vbhtml*ファイル、正確な同じ Razor マークアップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="94693-151">Examine the *Views\\_ViewStart.cshtml* file, it contains the exact same Razor markup.</span></span> <span data-ttu-id="94693-152">*[ビュー\\は _viewstart.vbhtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* ファイルは、すべてのビューを使用する一般的なレイアウトを定義、ためアウトまたは削除からそのコードをコメント、 *Views\HelloWorld\Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="94693-152">The *[Views\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* file defines the common layout that all views will use, therefore you can comment out or remove that code from the *Views\HelloWorld\Index.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

<span data-ttu-id="94693-153">`Layout` プロパティを使用すれば、別のレイアウト ビューを設定することができます。また、`null` に設定して、レイアウト ファイルが使用されないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="94693-153">You can use the `Layout` property to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="94693-154">ここで、インデックス ビューのタイトルを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="94693-154">Now, let's change the title of the Index view.</span></span>

<span data-ttu-id="94693-155">開いている*MvcMovie\Views\HelloWorld\Index.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="94693-155">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="94693-156">2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。</span><span class="sxs-lookup"><span data-stu-id="94693-156">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="94693-157">これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="94693-157">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

<span data-ttu-id="94693-158">HTML 表示するタイトルをセットの上に、コードを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (である、 *Index.cshtml*テンプレートの表示)。</span><span class="sxs-lookup"><span data-stu-id="94693-158">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="94693-159">注意してレイアウト テンプレート ( *\shared\\_Layout.cshtml* ) の値を使用して、`<title>`要素の一部として、`<head>`以前に変更した HTML の「します。</span><span class="sxs-lookup"><span data-stu-id="94693-159">Notice that the layout template ( *Views\Shared\\_Layout.cshtml* ) uses this value in the `<title>` element as part of the `<head>` section of the HTML that we modified previously.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

<span data-ttu-id="94693-160">これを使用して`ViewBag`アプローチでは、簡単に渡せる他のパラメーター、ビュー テンプレートと、レイアウト ファイルの間です。</span><span class="sxs-lookup"><span data-stu-id="94693-160">Using this `ViewBag` approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="94693-161">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="94693-161">Run the application.</span></span> <span data-ttu-id="94693-162">ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください </span><span class="sxs-lookup"><span data-stu-id="94693-162">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="94693-163">(ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="94693-163">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="94693-164">ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルが作成、`ViewBag.Title`設定、 *Index.cshtml*テンプレートおよびその他の表示&quot;-ムービー アプリ&quot;レイアウト ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="94693-164">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with the `ViewBag.Title` we set in the *Index.cshtml* view template and the additional &quot;- Movie App&quot; added in the layout file.</span></span>

<span data-ttu-id="94693-165">また方法でコンテンツ、 *Index.cshtml*ビュー テンプレートのトピックとマージされました、  *\_Layout.cshtml*ビュー テンプレートおよび 1 つの HTML 応答をブラウザーに送信されました。</span><span class="sxs-lookup"><span data-stu-id="94693-165">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="94693-166">レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="94693-166">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="94693-167">ほんの少しの&quot;データ&quot;(ここでは、 &quot;、ビュー テンプレートからこんにちは!&quot;メッセージ) は、ハードコードです。</span><span class="sxs-lookup"><span data-stu-id="94693-167">Our little bit of &quot;data&quot; (in this case the &quot;Hello from our View Template!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="94693-168">MVC アプリケーションには、 &quot;V&quot; (ビュー) を取得したら、 &quot;C&quot; (コント ローラー) が&quot;M&quot;まだ (モデル)。</span><span class="sxs-lookup"><span data-stu-id="94693-168">The MVC application has a &quot;V&quot; (view) and you've got a &quot;C&quot; (controller), but no &quot;M&quot; (model) yet.</span></span> <span data-ttu-id="94693-169">間もなく、方法を説明するデータベースを作成し、モデル データを取得します。</span><span class="sxs-lookup"><span data-stu-id="94693-169">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="94693-170">コントローラーからビューへのデータの受け渡し</span><span class="sxs-lookup"><span data-stu-id="94693-170">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="94693-171">データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。</span><span class="sxs-lookup"><span data-stu-id="94693-171">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="94693-172">受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="94693-172">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="94693-173">コント ローラー クラスでは、受信ブラウザーを処理するコードは、要求をデータベースからデータを取得し、最終的には、ブラウザーに返送する応答の種類を決定を記述します。</span><span class="sxs-lookup"><span data-stu-id="94693-173">A controller class is where you write the code that handles the incoming browser requests, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="94693-174">テンプレートの表示は、生成し、ブラウザーに HTML 応答の書式を設定するコント ローラーから使用できます。</span><span class="sxs-lookup"><span data-stu-id="94693-174">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="94693-175">コント ローラーは、ブラウザーへの応答をレンダリングするビュー テンプレートの順に必要なすべてのデータまたはオブジェクトを提供します。</span><span class="sxs-lookup"><span data-stu-id="94693-175">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="94693-176">ベスト プラクティス:**ビュー テンプレートのビジネス ロジックを実行またはデータベースと直接やり取りする必要がありますしない**です。</span><span class="sxs-lookup"><span data-stu-id="94693-176">A best practice: **A view template should never perform business logic or interact with a database directly**.</span></span> <span data-ttu-id="94693-177">代わりに、ビュー テンプレートは、コント ローラーに設定されているデータのみを操作する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94693-177">Instead, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="94693-178">これを維持する&quot;関心の分離&quot;完全テスト可能でありより優れたコードに保ちます。</span><span class="sxs-lookup"><span data-stu-id="94693-178">Maintaining this &quot;separation of concerns&quot; helps keep your code clean, testable and more maintainable.</span></span>

<span data-ttu-id="94693-179">現時点では、`Welcome`でアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターと、ブラウザーに直接値を出力します。</span><span class="sxs-lookup"><span data-stu-id="94693-179">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="94693-180">この応答を文字列としてのレンダリング コント ローラーがあるのではなくビュー テンプレートを代わりに使用するコント ローラーに変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="94693-180">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="94693-181">このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="94693-181">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="94693-182">コント ローラーにテンプレートの表示に必要な動的なデータ (パラメーター) を置くことによってこれを行う、`ViewBag`ビュー テンプレートにアクセスできるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="94693-182">You can do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="94693-183">戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="94693-183">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="94693-184">`ViewBag`動的なオブジェクトは、つまりに自由を配置する;`ViewBag`オブジェクトを持たないプロパティに定義されたものと、その内部に配置するまでです。</span><span class="sxs-lookup"><span data-stu-id="94693-184">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="94693-185">[ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)名前付きパラメーターが自動的にマップ (`name`と`numTimes`)、アドレス バーに、メソッドのパラメーターのクエリ文字列から。</span><span class="sxs-lookup"><span data-stu-id="94693-185">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="94693-186">完全な *HelloWorldController.cs* ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="94693-186">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

<span data-ttu-id="94693-187">これで、`ViewBag`オブジェクトには、自動的にビューに渡されるデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="94693-187">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span> <span data-ttu-id="94693-188">次に、開始ビュー テンプレートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="94693-188">Next, you need a Welcome view template!</span></span> <span data-ttu-id="94693-189">**ビルド**メニューの **ソリューションのビルド**(または Ctrl + Shift + B)、プロジェクトをコンパイルするかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="94693-189">In the **Build** menu, select **Build Solution** (or Ctrl+Shift+B) to make sure the project is compiled.</span></span> <span data-ttu-id="94693-190">右クリックして、 *Views\HelloWorld*フォルダーをクリック**追加**をクリックし、 **MVC 5 ビュー ページ (Razor) のレイアウト**です。</span><span class="sxs-lookup"><span data-stu-id="94693-190">Right click the *Views\HelloWorld* folder and click **Add**, then click **MVC 5 View Page with Layout (Razor)**.</span></span>
  
![](adding-a-view/_static/image10.png)   
  
<span data-ttu-id="94693-191">**項目の名前を指定** ダイアログ ボックスに、入力*へようこそ*、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="94693-191">In the **Specify Name for Item** dialog box, enter *Welcome*, and then click **OK**.</span></span>   
  
<span data-ttu-id="94693-192">**レイアウト ページの選択**ダイアログ ボックスで、既定値をそのまま使用 **\_Layout.cshtml**  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="94693-192">In the **Select a Layout Page** dialog, accept the default **\_Layout.cshtml** and click **OK**.</span></span>  
  
![](adding-a-view/_static/image11.png)   

<span data-ttu-id="94693-193">*MvcMovie\Views\HelloWorld\Welcome.cshtml*ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="94693-193">The *MvcMovie\Views\HelloWorld\Welcome.cshtml* file is created.</span></span>

<span data-ttu-id="94693-194">内のマークアップを置き換える、 *Welcome.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="94693-194">Replace the markup in the *Welcome.cshtml* file.</span></span> <span data-ttu-id="94693-195">表示されているループを作成します&quot;こんにちは&quot;として何度でも、ユーザーの質問にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="94693-195">You'll create a loop that says &quot;Hello&quot; as many times as the user says it should.</span></span> <span data-ttu-id="94693-196">完全な*Welcome.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="94693-196">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

<span data-ttu-id="94693-197">アプリケーションを実行し、次の URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="94693-197">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="94693-198">データが URL から取得され、コント ローラーを使用して、渡されるようになりました、[モデル バインダー](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="94693-198">Now data is taken from the URL and passed to the controller using the [model binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span></span> <span data-ttu-id="94693-199">コント ローラーへのデータをパッケージ化、`ViewBag`オブジェクトをビューにオブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="94693-199">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="94693-200">ビューは、データを HTML として、ユーザーにし、表示します。</span><span class="sxs-lookup"><span data-stu-id="94693-200">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image12.png)

<span data-ttu-id="94693-201">上記のサンプルで使用して、`ViewBag`コント ローラーからビューにデータを渡すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="94693-201">In the sample above, we used a `ViewBag` object to pass data from the controller to a view.</span></span> <span data-ttu-id="94693-202">チュートリアルの後半で、ビュー モデルを使用して、コントローラーからビューにデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="94693-202">Later in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="94693-203">モデルの表示方法でデータを渡すには、ビュー バッグ アプローチ経由で一般にはるかに優先です。</span><span class="sxs-lookup"><span data-stu-id="94693-203">The view model approach to passing data is generally much preferred over the view bag approach.</span></span> <span data-ttu-id="94693-204">ブログ記事を参照してください[動的 V 厳密に型指定されたビュー](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="94693-204">See the blog entry [Dynamic V Strongly Typed Views](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) for more information.</span></span> 

<span data-ttu-id="94693-205">種類もの&quot;M&quot;モデルがデータベースの種類ではありません。</span><span class="sxs-lookup"><span data-stu-id="94693-205">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="94693-206">学習したことを確認し、ムービーのデータベースを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="94693-206">Let's take what we've learned and create a database of movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="94693-207">[前へ](adding-a-controller.md)
[次へ](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="94693-207">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
