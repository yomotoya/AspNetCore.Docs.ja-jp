---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: コント ローラーからモデルのデータへのアクセス |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf1d27088c1e65d55a6820825eebe63f7fdcb515
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804938"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="45c31-104">コント ローラーからモデルのデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="45c31-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="45c31-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="45c31-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="45c31-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="45c31-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="45c31-107">より安全ではるかに簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="45c31-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="45c31-108">このセクションで作成します新しい`MoviesController`クラスし、映画のデータを取得し、ビュー テンプレートを使用してブラウザーで表示するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="45c31-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="45c31-109">**アプリケーションをビルド**次の手順に進む前にします。</span><span class="sxs-lookup"><span data-stu-id="45c31-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="45c31-110">右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="45c31-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="45c31-111">アプリケーションをビルドするまでは、以下のオプションは表示されません。</span><span class="sxs-lookup"><span data-stu-id="45c31-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="45c31-112">次のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="45c31-112">Select the following options:</span></span>

- <span data-ttu-id="45c31-113">コント ローラー名: **MoviesController**します。</span><span class="sxs-lookup"><span data-stu-id="45c31-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="45c31-114">(これは、既定値です。</span><span class="sxs-lookup"><span data-stu-id="45c31-114">(This is the default.</span></span> <span data-ttu-id="45c31-115">)</span><span class="sxs-lookup"><span data-stu-id="45c31-115">)</span></span>
- <span data-ttu-id="45c31-116">テンプレート: **Entity Framework を使用して、読み取り/書き込み操作とビューがある MVC コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="45c31-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="45c31-117">モデル クラス: **Movie (MvcMovie.Models)** します。</span><span class="sxs-lookup"><span data-stu-id="45c31-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="45c31-118">データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)** します。</span><span class="sxs-lookup"><span data-stu-id="45c31-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="45c31-119">ビュー: **Razor (CSHTML)** します。</span><span class="sxs-lookup"><span data-stu-id="45c31-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="45c31-120">(既定値です。)</span><span class="sxs-lookup"><span data-stu-id="45c31-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="45c31-122">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="45c31-122">Click **Add**.</span></span> <span data-ttu-id="45c31-123">Visual Studio Express には、次のファイルとフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="45c31-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="45c31-124">*MoviesController.cs*プロジェクトのファイル*コント ローラー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="45c31-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="45c31-125">A*映画*プロジェクトのフォルダー*ビュー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="45c31-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="45c31-126">*Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*views \movies*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="45c31-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="45c31-127">ASP.NET MVC 4 では自動的に作成、CRUD (作成、読み取り、更新、および削除) アクション メソッドと (CRUD アクション メソッドとビューの自動作成はスキャフォールディングと呼ばれます) のビュー。</span><span class="sxs-lookup"><span data-stu-id="45c31-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="45c31-128">完全に機能する web アプリケーションを作成、一覧表示、編集、およびムービー エントリを削除することができますがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="45c31-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="45c31-129">アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL にします。</span><span class="sxs-lookup"><span data-stu-id="45c31-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="45c31-130">アプリケーションが既定のルーティングに依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`が既定値にルーティングされる`Index`のアクション メソッド、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="45c31-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="45c31-131">つまり、ブラウザーの要求`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`します。</span><span class="sxs-lookup"><span data-stu-id="45c31-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="45c31-132">まだ追加していないため、映画の空のリストになります。</span><span class="sxs-lookup"><span data-stu-id="45c31-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="45c31-133">ムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="45c31-133">Creating a Movie</span></span>

<span data-ttu-id="45c31-134">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="45c31-134">Select the **Create New** link.</span></span> <span data-ttu-id="45c31-135">ムービーの詳細を入力し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="45c31-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="45c31-136">クリックすると、**作成**ボタンにより、ムービー情報がデータベースに保存されているサーバーに投稿するフォーム。</span><span class="sxs-lookup"><span data-stu-id="45c31-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="45c31-137">リダイレクトし、 */Movies* URL の一覧で、新しく作成したムービーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="45c31-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="45c31-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="45c31-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="45c31-139">いくつかのムービー エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="45c31-139">Create a couple more movie entries.</span></span> <span data-ttu-id="45c31-140">**[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。</span><span class="sxs-lookup"><span data-stu-id="45c31-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="45c31-141">生成されたコードを調べる</span><span class="sxs-lookup"><span data-stu-id="45c31-141">Examining the Generated Code</span></span>

<span data-ttu-id="45c31-142">開く、 *Controllers\MoviesController.cs*ファイルを開き、生成された確認`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="45c31-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="45c31-143">ムービー コント ローラーの一部、`Index`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="45c31-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="45c31-144">次の行から、`MoviesController`クラスは、前述のようにムービー データベース コンテキストをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="45c31-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="45c31-145">ムービー データベース コンテキストを使用して、クエリ、編集、およびムービーを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="45c31-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="45c31-146">要求、`Movies`コント ローラーは、すべてのエントリを返します、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="45c31-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="45c31-147">厳密に型指定されたモデルと@modelキーワード</span><span class="sxs-lookup"><span data-stu-id="45c31-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="45c31-148">このチュートリアルでは、以前では、コント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートを説明しました、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="45c31-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="45c31-149">`ViewBag`はビューに情報を渡す便利な遅延バインディング方法を提供する動的オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="45c31-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="45c31-150">ASP.NET MVC では、ビュー テンプレートにオブジェクトやデータに型指定を厳密に渡す機能も提供します。</span><span class="sxs-lookup"><span data-stu-id="45c31-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="45c31-151">これにより、アプローチにより優れたコンパイル時のコードと Visual Studio エディターでの高度な IntelliSense のチェックが厳密に型指定します。</span><span class="sxs-lookup"><span data-stu-id="45c31-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="45c31-152">Visual Studio でスキャフォールディング メカニズムでは、このアプローチを使用する、`MoviesController`メソッドとビューの作成時に、クラスとビューのテンプレート。</span><span class="sxs-lookup"><span data-stu-id="45c31-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="45c31-153">*Controllers\MoviesController.cs*ファイルを生成された確認`Details`メソッド。</span><span class="sxs-lookup"><span data-stu-id="45c31-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="45c31-154">ムービー コント ローラーの一部、`Details`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="45c31-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="45c31-155">場合、`Movie`が見つかるのインスタンス、`Movie`モデルは、詳細ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="45c31-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="45c31-156">内容を確認、 *Views\Movies\Details.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="45c31-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="45c31-157">含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが必要とするオブジェクトの種類を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="45c31-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="45c31-158">ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。</span><span class="sxs-lookup"><span data-stu-id="45c31-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="45c31-159">この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="45c31-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="45c31-160">たとえば、 *Details.cshtml*テンプレートでは、このコードでは各ムービー フィールドを`DisplayNameFor`と[DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx)で厳密に型指定された HTML ヘルパー`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="45c31-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="45c31-161">Create と Edit メソッドとビュー テンプレートもムービー モデル オブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="45c31-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="45c31-162">確認、 *Index.cshtml*テンプレートの表示と`Index`メソッドで、 *MoviesController.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="45c31-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="45c31-163">このコードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すときに、`View`ヘルパー メソッドで、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="45c31-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="45c31-164">このコードで、これは`Movies`コント ローラーからビューの一覧。</span><span class="sxs-lookup"><span data-stu-id="45c31-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="45c31-165">ムービー コント ローラーを作成したときに Visual Studio Express に自動的に含まれている、次`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="45c31-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="45c31-166">これは、`@model`ディレクティブを使用すると、コント ローラーを使用して、ビューに渡されるムービーの一覧へのアクセス、`Model`厳密に型指定されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="45c31-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="45c31-167">たとえば、 *Index.cshtml*テンプレート コードをムービーでループ処理、`foreach`経由で、厳密に型指定されたステートメント`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="45c31-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="45c31-168">`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`します。</span><span class="sxs-lookup"><span data-stu-id="45c31-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="45c31-169">その他の利点は、コードのコンパイル時のチェックを取得し、完全なコード エディターで IntelliSense のサポート、つまり。</span><span class="sxs-lookup"><span data-stu-id="45c31-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="45c31-171">SQL Server LocalDB の使用</span><span class="sxs-lookup"><span data-stu-id="45c31-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="45c31-172">Entity Framework Code First が検出されたデータベース接続文字列が指す、 `Movies` Code First にデータベースを自動的に作成するためにまだ存在していないデータベース。</span><span class="sxs-lookup"><span data-stu-id="45c31-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="45c31-173">作成されたことを確認することができます、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="45c31-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="45c31-174">表示されない場合、 *Movies.mdf*ファイルで、をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="45c31-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="45c31-175">ダブルクリック*Movies.mdf*を開く**データベース エクスプ ローラー**の順に展開、**テーブル**映画の表を参照するフォルダー。</span><span class="sxs-lookup"><span data-stu-id="45c31-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="45c31-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="45c31-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="45c31-177">データベース エクスプ ローラーが表示されない場合から、**ツール**メニューの **データベースへの接続**、キャンセルして、**データ ソースの選択**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="45c31-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="45c31-178">これにより、開いているデータベース エクスプ ローラー。</span><span class="sxs-lookup"><span data-stu-id="45c31-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="45c31-179">VWD または Visual Studio 2010 を使用して次の次のいずれかのようなエラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="45c31-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="45c31-180">データベース ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES します。MDF' 706 のバージョンであるため、開くことができません。</span><span class="sxs-lookup"><span data-stu-id="45c31-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="45c31-181">このサーバーは、655 およびそれ以前のバージョンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="45c31-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="45c31-182">ダウン グレード パスがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="45c31-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="45c31-183">&quot;これらの列は、ユーザー コードによって処理されなかった&quot;渡された SqlConnection では、初期カタログが指定されていません。</span><span class="sxs-lookup"><span data-stu-id="45c31-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="45c31-184">インストールする必要がある、 [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)と[LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)します。</span><span class="sxs-lookup"><span data-stu-id="45c31-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="45c31-185">確認、`MovieDBContext`前のページで指定された接続文字列。</span><span class="sxs-lookup"><span data-stu-id="45c31-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="45c31-186">右クリックし、`Movies`テーブルを選択**テーブル データの表示**作成したデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="45c31-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="45c31-187">右クリックし、`Movies`テーブルを選択**テーブル定義を開く**に構造を Entity Framework Code First が自動的に作成するテーブルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="45c31-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="45c31-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="45c31-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="45c31-189">通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`前に作成したクラス。</span><span class="sxs-lookup"><span data-stu-id="45c31-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="45c31-190">Entity Framework Code First 自動的に作成されたこのスキーマに基づいて、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="45c31-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="45c31-191">完了したらを右クリックして接続を閉じる*MovieDBContext*選択**閉じる接続**します。</span><span class="sxs-lookup"><span data-stu-id="45c31-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="45c31-192">(接続を終了しない場合がありますエラーが表示、次回プロジェクトを実行する)。</span><span class="sxs-lookup"><span data-stu-id="45c31-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="45c31-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="45c31-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="45c31-194">データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="45c31-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="45c31-195">次のチュートリアルがスキャフォールディングされたコードの残りの部分を調べるされ追加、`SearchIndex`メソッドと`SearchIndex`ビューをこのデータベースのムービーを検索することができます。</span><span class="sxs-lookup"><span data-stu-id="45c31-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45c31-196">[前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="45c31-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
