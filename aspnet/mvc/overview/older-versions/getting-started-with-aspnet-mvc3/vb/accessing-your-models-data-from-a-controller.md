---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: コント ローラー (VB) から、モデルのデータにアクセス |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 030e7cc966078b76b5f96229d437c9990f17656a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller-vb"></a><span data-ttu-id="76dc0-103">コント ローラー (VB) から、モデルのデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="76dc0-103">Accessing your Model's Data from a Controller (VB)</span></span>
====================
<span data-ttu-id="76dc0-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="76dc0-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="76dc0-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="76dc0-106">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="76dc0-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="76dc0-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="76dc0-108">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="76dc0-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="76dc0-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="76dc0-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="76dc0-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="76dc0-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="76dc0-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="76dc0-113">VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="76dc0-114">[バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="76dc0-115">C# を使用する場合に切り替え、 [c# バージョン](../cs/accessing-your-models-data-from-a-controller.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-115">If you prefer C#, switch to the [C# version](../cs/accessing-your-models-data-from-a-controller.md) of this tutorial.</span></span>


<span data-ttu-id="76dc0-116">このセクションで作成、新しい`MoviesController`クラスし、ムービー データが取得され、ビュー テンプレートを使用してブラウザーに表示するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-116">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span> <span data-ttu-id="76dc0-117">続行する前にアプリケーションをビルドすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-117">Be sure to build your application before proceeding.</span></span>

<span data-ttu-id="76dc0-118">右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="76dc0-118">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="76dc0-119">次のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-119">Select the following options:</span></span>

- <span data-ttu-id="76dc0-120">コント ローラー名: **MoviesController**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-120">Controller name: **MoviesController**.</span></span> <span data-ttu-id="76dc0-121">(これは、既定値です)。</span><span class="sxs-lookup"><span data-stu-id="76dc0-121">(This is the default.)</span></span>
- <span data-ttu-id="76dc0-122">テンプレート: **Entity Framework を使用して読み取り/書き込みアクションと、ビュー、コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-122">Template: **Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="76dc0-123">モデル クラス:**ムービー (MvcMovie.Models)**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-123">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="76dc0-124">データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-124">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="76dc0-125">ビュー: **Razor (CSHTML)**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-125">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="76dc0-126">(既定値です。)</span><span class="sxs-lookup"><span data-stu-id="76dc0-126">(The default.)</span></span>

<span data-ttu-id="76dc0-127">[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-127">[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="76dc0-128">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="76dc0-128">Click **Add**.</span></span> <span data-ttu-id="76dc0-129">Visual Web Developer では、次のファイルとフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-129">Visual Web Developer creates the following files and folders:</span></span>

- <span data-ttu-id="76dc0-130">*MoviesController.vb*ファイルはプロジェクトの*コント ローラー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-130">*A MoviesController.vb* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="76dc0-131">A*映画*プロジェクトのフォルダーに*ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-131">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="76dc0-132">*Create.vbhtml、Delete.vbhtml、Details.vbhtml、Edit.vbhtml*、および*Index.vbhtml*新しい*Views\Movies*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-132">*Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, and *Index.vbhtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="76dc0-133">[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-133">[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="76dc0-134">ASP.NET MVC 3 のスキャフォールディング機構自動的に作成された、CRUD (作成、読み取り、更新、および削除) のアクション メソッドとするためのビューです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-134">The ASP.NET MVC 3 scaffolding mechanism automatically created the CRUD (create, read, update, and delete) action methods and views for you.</span></span> <span data-ttu-id="76dc0-135">作成、一覧表示、編集、およびムービーのエントリを削除することができますを完全に機能の web アプリケーションがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="76dc0-135">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="76dc0-136">アプリケーションを実行しを参照、`Movies`コント ローラーを追加して*/Movies*お使いのブラウザーのアドレス バーの URL にします。</span><span class="sxs-lookup"><span data-stu-id="76dc0-136">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="76dc0-137">既定のルーティングにアプリケーションが依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`は既定値にルーティング`Index`のアクション メソッド、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="76dc0-137">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="76dc0-138">ブラウザー要求言い換えれば、`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-138">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="76dc0-139">いずれかがまだ追加されているため、ムービーの空のリストになります。</span><span class="sxs-lookup"><span data-stu-id="76dc0-139">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a><span data-ttu-id="76dc0-140">ムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-140">Creating a Movie</span></span>

<span data-ttu-id="76dc0-141">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-141">Select the **Create New** link.</span></span> <span data-ttu-id="76dc0-142">ムービーの詳細を入力し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="76dc0-142">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="76dc0-143">クリックすると、**作成**と、ムービー情報がデータベースに保存されているサーバーにポストするフォームのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="76dc0-143">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="76dc0-144">リダイレクトしている、 */Movies* URL、一覧に新しく作成したムービーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-144">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="76dc0-145">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-145">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)</span></span>

<span data-ttu-id="76dc0-146">いくつかのムービー エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-146">Create a couple more movie entries.</span></span> <span data-ttu-id="76dc0-147">**[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-147">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="76dc0-148">生成されたコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-148">Examining the Generated Code</span></span>

<span data-ttu-id="76dc0-149">開く、 *Controllers\MoviesController.vb*ファイルし、生成された確認`Index`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-149">Open the *Controllers\MoviesController.vb* file and examine the generated `Index` method.</span></span> <span data-ttu-id="76dc0-150">ムービーのコント ローラーの一部、`Index`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-150">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

<span data-ttu-id="76dc0-151">次の行から、`MoviesController`クラスは、前述のようにムービーのデータベース コンテキストをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-151">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="76dc0-152">ムービーのデータベース コンテキストを使用して、クエリ、編集、および映画を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-152">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

<span data-ttu-id="76dc0-153">要求を`Movies`コント ローラーのすべてのエントリが返されます、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="76dc0-153">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="76dc0-154">モデルを厳密に型指定と@modelキーワード</span><span class="sxs-lookup"><span data-stu-id="76dc0-154">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="76dc0-155">このチュートリアルで既に説明しましたコント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートに、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="76dc0-155">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="76dc0-156">`ViewBag`ビューに情報を渡す便利な遅延バインディングされた方法を提供する動的オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-156">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="76dc0-157">ASP.NET MVC では、厳密に渡せるように入力データまたはオブジェクトをビュー テンプレートも用意されています。</span><span class="sxs-lookup"><span data-stu-id="76dc0-157">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="76dc0-158">これには、アプローチにより向上コンパイル時のコードと、Visual Web Developer エディターでは、豊富な IntelliSense のチェック厳密型指定されました。</span><span class="sxs-lookup"><span data-stu-id="76dc0-158">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Web Developer editor.</span></span> <span data-ttu-id="76dc0-159">このアプローチでを使用して、`MoviesController`クラスと*Index.vbhtml*ビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="76dc0-159">We're using this approach with the `MoviesController` class and *Index.vbhtml* view template.</span></span>

<span data-ttu-id="76dc0-160">コードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すとき、`View`内のヘルパー メソッド、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="76dc0-160">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="76dc0-161">コードが、これを渡します`Movies`表示するリストのコント ローラーから。</span><span class="sxs-lookup"><span data-stu-id="76dc0-161">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

<span data-ttu-id="76dc0-162">含めることによって、`@ModelType`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが期待しているオブジェクトの種類を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-162">By including a `@ModelType` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="76dc0-163">Visual Web Developer が自動的に、次を含め、ムービーのコント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.vbhtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="76dc0-163">When you created the movie controller, Visual Web Developer automatically included the following `@model` statement at the top of the *Index.vbhtml* file:</span></span>

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

<span data-ttu-id="76dc0-164">これは、`@ModelType`ディレクティブでは、コント ローラーを使用してビューに渡されるムービーの一覧にアクセスすることができます、`Model`厳密に型指定されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="76dc0-164">This `@ModelType` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="76dc0-165">たとえば、 *Index.vbhtml*テンプレート、ループ、映画を深める、 `foreach` 、厳密に型指定されたステートメント`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="76dc0-165">For example, in the *Index.vbhtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

<span data-ttu-id="76dc0-166">`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-166">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="76dc0-167">その他の利点にはコードのコンパイル時のチェックを取得して、完全なコード エディターで IntelliSense をサポートすることを意味します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-167">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

<span data-ttu-id="76dc0-168">[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-168">[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)</span></span>

## <a name="working-with-sql-server-compact"></a><span data-ttu-id="76dc0-169">SQL Server Compact の使用</span><span class="sxs-lookup"><span data-stu-id="76dc0-169">Working with SQL Server Compact</span></span>

<span data-ttu-id="76dc0-170">指定されたデータベース接続文字列が指す、entity Framework Code First の検出、`Movies`データベースをまだ、コードの最初にデータベースを自動的に作成するために存在しませんでした。</span><span class="sxs-lookup"><span data-stu-id="76dc0-170">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="76dc0-171">作成されたことを確認することができます、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-171">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="76dc0-172">表示されない場合、 *Movies.sdf*ファイルをクリックして、 **すべてのファイル**ボタンをクリックして、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-172">If you don't see the *Movies.sdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

<span data-ttu-id="76dc0-173">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-173">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)</span></span>

<span data-ttu-id="76dc0-174">ダブルクリックして*Movies.sdf*を開くには**サーバー エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-174">Double-click *Movies.sdf* to open **Server Explorer**.</span></span> <span data-ttu-id="76dc0-175">展開し、**テーブル**フォルダーをデータベースに作成されているテーブルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="76dc0-175">Then expand the **Tables** folder to see the tables that have been created in the database.</span></span>

> [!NOTE]
> <span data-ttu-id="76dc0-176">ダブルクリックすると、エラーが発生した場合*Movies.sdf*、インストールされていることを確認**Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-176">If you get an error when you double-click *Movies.sdf*, make sure you've installed **Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**.</span></span> <span data-ttu-id="76dc0-177">(ソフトウェアへのリンク、このチュートリアル シリーズの第 1 部での前提条件の一覧を参照してください)。リリースを今すぐインストールする場合は、閉じてから再度開いて Visual Web Developer する必要があります。</span><span class="sxs-lookup"><span data-stu-id="76dc0-177">(For links to the software, see the list of prerequisites in part 1 of this tutorial series.) If you install the release now, you'll have to close and re-open Visual Web Developer.</span></span>


<span data-ttu-id="76dc0-178">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-178">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)</span></span>

<span data-ttu-id="76dc0-179">1 つずつ、2 つのテーブルがある、`Movie`エンティティ セットし、`EdmMetadata`テーブル。</span><span class="sxs-lookup"><span data-stu-id="76dc0-179">There are two tables, one for the `Movie` entity set and then the `EdmMetadata` table.</span></span> <span data-ttu-id="76dc0-180">`EdmMetadata` Entity Framework では、モデルとデータベースの同期を決定するテーブルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-180">The `EdmMetadata` table is used by the Entity Framework to determine when the model and the database are out of sync.</span></span>

<span data-ttu-id="76dc0-181">右クリックし、`Movies`テーブルを選択して**テーブル データの表示**作成したデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="76dc0-181">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

<span data-ttu-id="76dc0-182">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-182">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)</span></span>

<span data-ttu-id="76dc0-183">右クリックし、`Movies`テーブルを選択して**テーブル スキーマの編集**です。</span><span class="sxs-lookup"><span data-stu-id="76dc0-183">Right-click the `Movies` table and select **Edit Table Schema**.</span></span>

<span data-ttu-id="76dc0-184">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-184">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)</span></span>

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

<span data-ttu-id="76dc0-186">通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`先ほど作成したクラスです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-186">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="76dc0-187">Entity Framework Code First 自動的に作成されたこのスキーマに基づく、`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="76dc0-187">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="76dc0-188">完了したら、接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-188">When you're finished, close the connection.</span></span> <span data-ttu-id="76dc0-189">(接続を終了しないで場合がありますエラーが発生した、次回プロジェクトを実行する)。</span><span class="sxs-lookup"><span data-stu-id="76dc0-189">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="76dc0-190">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="76dc0-190">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)</span></span>

<span data-ttu-id="76dc0-191">データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="76dc0-191">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="76dc0-192">チュートリアルでは、[次へ]、うまくスキャフォールディング コードの残りの部分を確認し、追加、`SearchIndex`メソッドおよび`SearchIndex`ビューをこのデータベースで映画を検索できます。</span><span class="sxs-lookup"><span data-stu-id="76dc0-192">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="76dc0-193">[前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="76dc0-193">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
