---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: コント ローラーからモデルのデータへのアクセス |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 16f4a61d048e90b7962f038e90e0721883726c44
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833373"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="b7210-102">コント ローラーからモデルのデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="b7210-102">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="b7210-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b7210-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="b7210-104">このセクションで作成します新しい`MoviesController`クラスし、映画のデータを取得し、ビュー テンプレートを使用してブラウザーで表示するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="b7210-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="b7210-105">**アプリケーションをビルド**次の手順に進む前にします。</span><span class="sxs-lookup"><span data-stu-id="b7210-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="b7210-106">アプリケーションをビルドしない場合は、コント ローラーを追加中にエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b7210-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="b7210-107">ソリューション エクスプ ローラーで右クリックし、*コント ローラー*フォルダーをクリック**追加**、し**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="b7210-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="b7210-108">**スキャフォールディングの追加**ダイアログ ボックスで、をクリックして**MVC 5 コント ローラーとビュー、Entity Framework を使用して**、順にクリックします**追加**します。</span><span class="sxs-lookup"><span data-stu-id="b7210-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="b7210-109">選択**Movie (MvcMovie.Models)** モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="b7210-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="b7210-110">選択**MovieDBContext (MvcMovie.Models)** のデータ コンテキスト クラス。</span><span class="sxs-lookup"><span data-stu-id="b7210-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="b7210-111">コント ローラー名を入力**MoviesController**します。</span><span class="sxs-lookup"><span data-stu-id="b7210-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="b7210-112">次の図には、完了のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b7210-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="b7210-113">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b7210-113">Click **Add**.</span></span> <span data-ttu-id="b7210-114">(エラーが発生した場合おそらくしていないアプリケーションを構築するコント ローラーの追加を開始する前にします。)Visual Studio には、次のファイルとフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b7210-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="b7210-115">*MoviesController.cs*ファイル、*コント ローラー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b7210-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="b7210-116">A *views \movies*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b7210-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="b7210-117">*Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*views \movies*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b7210-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="b7210-118">Visual Studio が自動的に作成、 [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドと (CRUD アクション メソッドとビューの自動作成はスキャフォールディングと呼ばれます) のビュー。</span><span class="sxs-lookup"><span data-stu-id="b7210-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="b7210-119">完全に機能する web アプリケーションを作成、一覧表示、編集、およびムービー エントリを削除することができますがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="b7210-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="b7210-120">アプリケーションを実行し、をクリックして、 **MVC ムービー**リンク (するか、参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL に)。</span><span class="sxs-lookup"><span data-stu-id="b7210-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="b7210-121">アプリケーションが既定のルーティングに依存するため (で定義されている、*アプリ\_\routeconfig.cs*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`が既定値にルーティングされる`Index`のアクションメソッド`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="b7210-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="b7210-122">つまり、ブラウザーの要求`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`します。</span><span class="sxs-lookup"><span data-stu-id="b7210-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="b7210-123">まだ追加していないため、映画の空のリストになります。</span><span class="sxs-lookup"><span data-stu-id="b7210-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="b7210-124">ムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b7210-124">Creating a Movie</span></span>

<span data-ttu-id="b7210-125">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="b7210-125">Select the **Create New** link.</span></span> <span data-ttu-id="b7210-126">ムービーの詳細を入力し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="b7210-126">Enter some details about a movie and then click the **Create** button.</span></span>


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="b7210-127">[Price] フィールドに小数点またはコンマを入力することはできません。</span><span class="sxs-lookup"><span data-stu-id="b7210-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="b7210-128">コンマを使用するロケールを英語以外の jQuery の検証をサポートするために (&quot;、&quot;) する必要があります、小数点と英語 (米国) 以外の日付形式は、 *globalize.js*と特定*cultures/globalize.cultures.js*ファイル (から[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`します。</span><span class="sxs-lookup"><span data-stu-id="b7210-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="b7210-129">次のチュートリアルでこれを行う方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="b7210-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="b7210-130">ここでは、単に 10 のような整数を入力します。</span><span class="sxs-lookup"><span data-stu-id="b7210-130">For now, just enter whole numbers like 10.</span></span>


<span data-ttu-id="b7210-131">クリックすると、**作成**ボタンにより、ムービー情報がデータベースに保存されているサーバーに投稿するフォーム。</span><span class="sxs-lookup"><span data-stu-id="b7210-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="b7210-132">リダイレクトし、 */Movies* URL の一覧で、新しく作成したムービーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="b7210-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="b7210-133">いくつかのムービー エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b7210-133">Create a couple more movie entries.</span></span> <span data-ttu-id="b7210-134">**[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。</span><span class="sxs-lookup"><span data-stu-id="b7210-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="b7210-135">生成されたコードを調べる</span><span class="sxs-lookup"><span data-stu-id="b7210-135">Examining the Generated Code</span></span>

<span data-ttu-id="b7210-136">開く、 *Controllers\MoviesController.cs*ファイルを開き、生成された確認`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b7210-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="b7210-137">ムービー コント ローラーの一部、`Index`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="b7210-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="b7210-138">要求、`Movies`コント ローラーは、すべてのエントリを返します、`Movies`テーブルし、結果を渡す、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="b7210-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="b7210-139">次の行から、`MoviesController`クラスは、前述のようにムービー データベース コンテキストをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="b7210-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="b7210-140">ムービー データベース コンテキストを使用して、クエリ、編集、およびムービーを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="b7210-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="b7210-141">厳密に型指定されたモデルと@modelキーワード</span><span class="sxs-lookup"><span data-stu-id="b7210-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="b7210-142">このチュートリアルでは、以前では、コント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートを説明しました、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b7210-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="b7210-143">`ViewBag`はビューに情報を渡す便利な遅延バインディング方法を提供する動的オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="b7210-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="b7210-144">MVC を渡す機能も用意されています。*強く*型指定されたビュー テンプレートのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b7210-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="b7210-145">この厳密に型指定のアプローチによりより優れたコンパイル時、コードのチェックで豊富な[IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) Visual Studio エディターでします。</span><span class="sxs-lookup"><span data-stu-id="b7210-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="b7210-146">Visual Studio でスキャフォールディング メカニズムは、このアプローチを使用 (は、渡す、*強く*型指定されたモデル) で、`MoviesController`メソッドとビューが作成されたときに、クラスとビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="b7210-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="b7210-147">*Controllers\MoviesController.cs*ファイルを生成された確認`Details`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b7210-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="b7210-148">`Details`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="b7210-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="b7210-149">`id`一般パラメーター、ルート データとしてたとえば`http://localhost:1234/movies/details/1`ムービー コント ローラー、アクション、コント ローラーを設定`details`と`id`を 1 にします。</span><span class="sxs-lookup"><span data-stu-id="b7210-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="b7210-150">渡すこともできますクエリ文字列を含む id で、次のように。</span><span class="sxs-lookup"><span data-stu-id="b7210-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="b7210-151">場合、`Movie`が見つかるのインスタンス、`Movie`にモデルが渡される、`Details`ビュー。</span><span class="sxs-lookup"><span data-stu-id="b7210-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="b7210-152">内容を確認、 *Views\Movies\Details.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b7210-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="b7210-153">含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが必要とするオブジェクトの種類を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b7210-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="b7210-154">ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。</span><span class="sxs-lookup"><span data-stu-id="b7210-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="b7210-155">この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b7210-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b7210-156">たとえば、 *Details.cshtml*テンプレートでは、このコードでは各ムービー フィールドを`DisplayNameFor`と[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)で厳密に型指定された HTML ヘルパー`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b7210-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="b7210-157">`Create`と`Edit`メソッドとビュー テンプレート ムービー モデル オブジェクトを渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="b7210-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="b7210-158">確認、 *Index.cshtml*テンプレートの表示と`Index`メソッドで、 *MoviesController.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b7210-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="b7210-159">このコードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すときに、`View`ヘルパー メソッドで、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="b7210-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="b7210-160">このコードで、これは`Movies`一覧は、`Index`ビューにアクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="b7210-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="b7210-161">Visual Studio が、次を自動的に含まれているムービー コント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b7210-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="b7210-162">これは、`@model`ディレクティブを使用すると、コント ローラーを使用して、ビューに渡されるムービーの一覧へのアクセス、`Model`厳密に型指定されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b7210-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b7210-163">たとえば、 *Index.cshtml*テンプレート コードをムービーでループ処理、`foreach`経由で、厳密に型指定されたステートメント`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="b7210-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="b7210-164">`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`します。</span><span class="sxs-lookup"><span data-stu-id="b7210-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="b7210-165">その他の利点は、コードのコンパイル時のチェックを取得し、完全なコード エディターで IntelliSense のサポート、つまり。</span><span class="sxs-lookup"><span data-stu-id="b7210-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="b7210-167">SQL Server LocalDB の使用</span><span class="sxs-lookup"><span data-stu-id="b7210-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="b7210-168">Entity Framework Code First が検出されたデータベース接続文字列が指す、 `Movies` Code First にデータベースを自動的に作成するためにまだ存在していないデータベース。</span><span class="sxs-lookup"><span data-stu-id="b7210-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="b7210-169">作成されたことを確認することができます、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b7210-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="b7210-170">表示されない場合、 *Movies.mdf*ファイルで、をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b7210-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="b7210-171">ダブルクリック*Movies.mdf*を開く**サーバー エクスプ ローラー**の順に展開、**テーブル**映画の表を参照するフォルダー。</span><span class="sxs-lookup"><span data-stu-id="b7210-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="b7210-172">Id。 横にあるキー アイコンに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b7210-172">Note the key icon next to ID.</span></span> <span data-ttu-id="b7210-173">既定では、主キーの ID をという名前のプロパティが EF になります。</span><span class="sxs-lookup"><span data-stu-id="b7210-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="b7210-174">EF と MVC の詳細についてを参照してください Tom Dykstra の優れたチュートリアル[MVC と EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="b7210-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="b7210-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="b7210-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="b7210-176">右クリックし、`Movies`テーブルを選択**テーブル データの表示**作成したデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="b7210-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="b7210-177">右クリックし、`Movies`テーブルを選択**テーブル定義を開く**に構造を Entity Framework Code First が自動的に作成するテーブルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="b7210-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="b7210-178">通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`前に作成したクラス。</span><span class="sxs-lookup"><span data-stu-id="b7210-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="b7210-179">Entity Framework Code First 自動的に作成されたこのスキーマに基づいて、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="b7210-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="b7210-180">完了したらを右クリックして接続を閉じる*MovieDBContext*選択**閉じる接続**します。</span><span class="sxs-lookup"><span data-stu-id="b7210-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="b7210-181">(接続を終了しない場合がありますエラーが表示、次回プロジェクトを実行する)。</span><span class="sxs-lookup"><span data-stu-id="b7210-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="b7210-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="b7210-182">![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")</span></span>

<span data-ttu-id="b7210-183">これで、データを表示、編集、更新および削除できるデータベースができました。</span><span class="sxs-lookup"><span data-stu-id="b7210-183">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="b7210-184">次のチュートリアルがスキャフォールディングされたコードの残りの部分を調べるされ追加、`SearchIndex`メソッドと`SearchIndex`ビューをこのデータベースのムービーを検索することができます。</span><span class="sxs-lookup"><span data-stu-id="b7210-184">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="b7210-185">MVC を Entity Framework を使用する方法の詳細については、次を参照してください。 [、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="b7210-185">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7210-186">[前へ](creating-a-connection-string.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="b7210-186">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
