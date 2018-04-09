---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: コント ローラー (c#) から、モデルのデータにアクセスする |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 4218116eec6f177730087955b8fb69e5ecc0022f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller-c"></a><span data-ttu-id="4d68d-103">コント ローラー (c#) から、モデルのデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="4d68d-103">Accessing your Model's Data from a Controller (C#)</span></span>
====================
<span data-ttu-id="4d68d-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="4d68d-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="4d68d-105">このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="4d68d-106">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="4d68d-107">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="4d68d-108">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4d68d-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="4d68d-109">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="4d68d-110">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="4d68d-111">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="4d68d-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="4d68d-112">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="4d68d-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="4d68d-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="4d68d-114">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="4d68d-115">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="4d68d-116">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="4d68d-117">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

<span data-ttu-id="4d68d-118">このセクションで作成、新しい`MoviesController`クラスし、ムービー データが取得され、ビュー テンプレートを使用してブラウザーに表示するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-118">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span> <span data-ttu-id="4d68d-119">続行する前にアプリケーションをビルドすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-119">Be sure to build your application before proceeding.</span></span>

<span data-ttu-id="4d68d-120">右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="4d68d-120">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="4d68d-121">次のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-121">Select the following options:</span></span>

- <span data-ttu-id="4d68d-122">コント ローラー名: **MoviesController**です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-122">Controller name: **MoviesController**.</span></span> <span data-ttu-id="4d68d-123">(これは、既定値です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-123">(This is the default.</span></span> <span data-ttu-id="4d68d-124">)</span><span class="sxs-lookup"><span data-stu-id="4d68d-124">)</span></span>
- <span data-ttu-id="4d68d-125">テンプレート: **Entity Framework を使用して読み取り/書き込みアクションと、ビュー、コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-125">Template: **Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="4d68d-126">モデル クラス:**ムービー (MvcMovie.Models)**です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-126">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="4d68d-127">データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)**です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-127">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="4d68d-128">ビュー: **Razor (CSHTML)**です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-128">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="4d68d-129">(既定値です。)</span><span class="sxs-lookup"><span data-stu-id="4d68d-129">(The default.)</span></span>

<span data-ttu-id="4d68d-130">[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-130">[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="4d68d-131">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="4d68d-131">Click **Add**.</span></span> <span data-ttu-id="4d68d-132">Visual Web Developer では、次のファイルとフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-132">Visual Web Developer creates the following files and folders:</span></span>

- <span data-ttu-id="4d68d-133">*MoviesController.cs*ファイルはプロジェクトの*コント ローラー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-133">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="4d68d-134">A*映画*プロジェクトのフォルダーに*ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-134">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="4d68d-135">*Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*Views\Movies*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-135">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="4d68d-136">[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-136">[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="4d68d-137">ASP.NET MVC 3 のスキャフォールディング機構自動的に作成された、CRUD (作成、読み取り、更新、および削除) のアクション メソッドとするためのビューです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-137">The ASP.NET MVC 3 scaffolding mechanism automatically created the CRUD (create, read, update, and delete) action methods and views for you.</span></span> <span data-ttu-id="4d68d-138">作成、一覧表示、編集、およびムービーのエントリを削除することができますを完全に機能の web アプリケーションがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="4d68d-138">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="4d68d-139">アプリケーションを実行しを参照、`Movies`コント ローラーを追加して*/Movies*お使いのブラウザーのアドレス バーの URL にします。</span><span class="sxs-lookup"><span data-stu-id="4d68d-139">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="4d68d-140">既定のルーティングにアプリケーションが依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`は既定値にルーティング`Index`のアクション メソッド、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="4d68d-140">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="4d68d-141">ブラウザー要求言い換えれば、`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-141">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="4d68d-142">いずれかがまだ追加されているため、ムービーの空のリストになります。</span><span class="sxs-lookup"><span data-stu-id="4d68d-142">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a><span data-ttu-id="4d68d-143">ムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-143">Creating a Movie</span></span>

<span data-ttu-id="4d68d-144">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-144">Select the **Create New** link.</span></span> <span data-ttu-id="4d68d-145">ムービーの詳細を入力し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="4d68d-145">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="4d68d-146">クリックすると、**作成**と、ムービー情報がデータベースに保存されているサーバーにポストするフォームのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="4d68d-146">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="4d68d-147">リダイレクトしている、 */Movies* URL、一覧に新しく作成したムービーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-147">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="4d68d-148">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-148">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)</span></span>

<span data-ttu-id="4d68d-149">いくつかのムービー エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-149">Create a couple more movie entries.</span></span> <span data-ttu-id="4d68d-150">**[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-150">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="4d68d-151">生成されたコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-151">Examining the Generated Code</span></span>

<span data-ttu-id="4d68d-152">開く、 *Controllers\MoviesController.cs*ファイルし、生成された確認`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-152">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="4d68d-153">ムービーのコント ローラーの一部、`Index`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-153">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="4d68d-154">次の行から、`MoviesController`クラスは、前述のようにムービーのデータベース コンテキストをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-154">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="4d68d-155">ムービーのデータベース コンテキストを使用して、クエリ、編集、および映画を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-155">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="4d68d-156">要求を`Movies`コント ローラーのすべてのエントリが返されます、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="4d68d-156">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="4d68d-157">モデルを厳密に型指定と@modelキーワード</span><span class="sxs-lookup"><span data-stu-id="4d68d-157">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="4d68d-158">このチュートリアルで既に説明しましたコント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートに、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4d68d-158">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="4d68d-159">`ViewBag`ビューに情報を渡す便利な遅延バインディングされた方法を提供する動的オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-159">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="4d68d-160">ASP.NET MVC では、厳密に渡せるように入力データまたはオブジェクトをビュー テンプレートも用意されています。</span><span class="sxs-lookup"><span data-stu-id="4d68d-160">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="4d68d-161">これには、アプローチにより向上コンパイル時のコードと、Visual Web Developer エディターでは、豊富な IntelliSense のチェック厳密型指定されました。</span><span class="sxs-lookup"><span data-stu-id="4d68d-161">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Web Developer editor.</span></span> <span data-ttu-id="4d68d-162">このアプローチでを使用して、`MoviesController`クラスと*Index.cshtml*ビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="4d68d-162">We're using this approach with the `MoviesController` class and *Index.cshtml* view template.</span></span>

<span data-ttu-id="4d68d-163">コードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すとき、`View`内のヘルパー メソッド、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="4d68d-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="4d68d-164">コードが、これを渡します`Movies`表示するリストのコント ローラーから。</span><span class="sxs-lookup"><span data-stu-id="4d68d-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="4d68d-165">含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが期待しているオブジェクトの種類を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-165">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="4d68d-166">Visual Web Developer が自動的に、次を含め、ムービーのコント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4d68d-166">When you created the movie controller, Visual Web Developer automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="4d68d-167">これは、`@model`ディレクティブでは、コント ローラーを使用してビューに渡されるムービーの一覧にアクセスすることができます、`Model`厳密に型指定されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4d68d-167">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="4d68d-168">たとえば、 *Index.cshtml*テンプレート、ループ、映画を深める、 `foreach` 、厳密に型指定されたステートメント`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="4d68d-168">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

<span data-ttu-id="4d68d-169">`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-169">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="4d68d-170">その他の利点にはコードのコンパイル時のチェックを取得して、完全なコード エディターで IntelliSense をサポートすることを意味します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-170">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

<span data-ttu-id="4d68d-171">[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-171">[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)</span></span>

## <a name="working-with-sql-server-compact"></a><span data-ttu-id="4d68d-172">SQL Server Compact の使用</span><span class="sxs-lookup"><span data-stu-id="4d68d-172">Working with SQL Server Compact</span></span>

<span data-ttu-id="4d68d-173">指定されたデータベース接続文字列が指す、entity Framework Code First の検出、`Movies`データベースをまだ、コードの最初にデータベースを自動的に作成するために存在しませんでした。</span><span class="sxs-lookup"><span data-stu-id="4d68d-173">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="4d68d-174">作成されたことを確認することができます、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-174">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="4d68d-175">表示されない場合、 *Movies.sdf*ファイルをクリックして、 **すべてのファイル**ボタンをクリックして、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-175">If you don't see the *Movies.sdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

<span data-ttu-id="4d68d-176">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-176">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)</span></span>

<span data-ttu-id="4d68d-177">ダブルクリックして*Movies.sdf*を開くには**サーバー エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-177">Double-click *Movies.sdf* to open **Server Explorer**.</span></span> <span data-ttu-id="4d68d-178">展開し、**テーブル**フォルダーをデータベースに作成されているテーブルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4d68d-178">Then expand the **Tables** folder to see the tables that have been created in the database.</span></span>

> [!NOTE]
> <span data-ttu-id="4d68d-179">ダブルクリックすると、エラーが発生した場合*Movies.sdf*、インストールされていることを確認[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)。</span><span class="sxs-lookup"><span data-stu-id="4d68d-179">If you get an error when you double-click *Movies.sdf*, make sure you've installed [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support).</span></span> <span data-ttu-id="4d68d-180">(ソフトウェアへのリンク、このチュートリアル シリーズの第 1 部での前提条件の一覧を参照してください)。リリースを今すぐインストールする場合は、閉じてから再度開いて Visual Web Developer する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4d68d-180">(For links to the software, see the list of prerequisites in part 1 of this tutorial series.) If you install the release now, you'll have to close and re-open Visual Web Developer.</span></span>


<span data-ttu-id="4d68d-181">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-181">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)</span></span>

<span data-ttu-id="4d68d-182">1 つずつ、2 つのテーブルがある、`Movie`エンティティ セットし、`EdmMetadata`テーブル。</span><span class="sxs-lookup"><span data-stu-id="4d68d-182">There are two tables, one for the `Movie` entity set and then the `EdmMetadata` table.</span></span> <span data-ttu-id="4d68d-183">`EdmMetadata` Entity Framework では、モデルとデータベースの同期を決定するテーブルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-183">The `EdmMetadata` table is used by the Entity Framework to determine when the model and the database are out of sync.</span></span>

<span data-ttu-id="4d68d-184">右クリックし、`Movies`テーブルを選択して**テーブル データの表示**作成したデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="4d68d-184">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

<span data-ttu-id="4d68d-185">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-185">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)</span></span>

<span data-ttu-id="4d68d-186">右クリックし、`Movies`テーブルを選択して**テーブル スキーマの編集**です。</span><span class="sxs-lookup"><span data-stu-id="4d68d-186">Right-click the `Movies` table and select **Edit Table Schema**.</span></span>

<span data-ttu-id="4d68d-187">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-187">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)</span></span>

<span data-ttu-id="4d68d-188">[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-188">[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)</span></span>

<span data-ttu-id="4d68d-189">通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`先ほど作成したクラスです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="4d68d-190">Entity Framework Code First 自動的に作成されたこのスキーマに基づく、`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="4d68d-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="4d68d-191">完了したら、接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-191">When you're finished, close the connection.</span></span> <span data-ttu-id="4d68d-192">(接続を終了しないで場合がありますエラーが発生した、次回プロジェクトを実行する)。</span><span class="sxs-lookup"><span data-stu-id="4d68d-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="4d68d-193">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="4d68d-193">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)</span></span>

<span data-ttu-id="4d68d-194">データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="4d68d-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="4d68d-195">チュートリアルでは、[次へ]、うまくスキャフォールディング コードの残りの部分を確認し、追加、`SearchIndex`メソッドおよび`SearchIndex`ビューをこのデータベースで映画を検索できます。</span><span class="sxs-lookup"><span data-stu-id="4d68d-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4d68d-196">[前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="4d68d-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
