---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: コント ローラー (c#) から、モデルのデータへのアクセス |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a1a39cf06b7ab9109117e89051c8adf47062a926
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805614"
---
<a name="accessing-your-models-data-from-a-controller-c"></a><span data-ttu-id="42c80-103">コント ローラー (c#) から、モデルのデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="42c80-103">Accessing your Model's Data from a Controller (C#)</span></span>
====================
<span data-ttu-id="42c80-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="42c80-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="42c80-105">このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="42c80-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="42c80-106">より安全ではるかに簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="42c80-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="42c80-107">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="42c80-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="42c80-108">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="42c80-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="42c80-109">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="42c80-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="42c80-110">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="42c80-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="42c80-111">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="42c80-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="42c80-112">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="42c80-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="42c80-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="42c80-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="42c80-114">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="42c80-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="42c80-115">C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="42c80-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="42c80-116">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。</span><span class="sxs-lookup"><span data-stu-id="42c80-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="42c80-117">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="42c80-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

<span data-ttu-id="42c80-118">このセクションで作成します新しい`MoviesController`クラスし、映画のデータを取得し、ビュー テンプレートを使用してブラウザーで表示するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="42c80-118">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span> <span data-ttu-id="42c80-119">続行する前に、アプリケーションを構築することを確認します。</span><span class="sxs-lookup"><span data-stu-id="42c80-119">Be sure to build your application before proceeding.</span></span>

<span data-ttu-id="42c80-120">右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="42c80-120">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="42c80-121">次のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="42c80-121">Select the following options:</span></span>

- <span data-ttu-id="42c80-122">コント ローラー名: **MoviesController**します。</span><span class="sxs-lookup"><span data-stu-id="42c80-122">Controller name: **MoviesController**.</span></span> <span data-ttu-id="42c80-123">(これは、既定値です。</span><span class="sxs-lookup"><span data-stu-id="42c80-123">(This is the default.</span></span> <span data-ttu-id="42c80-124">)</span><span class="sxs-lookup"><span data-stu-id="42c80-124">)</span></span>
- <span data-ttu-id="42c80-125">テンプレート: **Entity Framework を使用して、読み取り/書き込み操作とビューとコント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="42c80-125">Template: **Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="42c80-126">モデル クラス: **Movie (MvcMovie.Models)** します。</span><span class="sxs-lookup"><span data-stu-id="42c80-126">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="42c80-127">データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)** します。</span><span class="sxs-lookup"><span data-stu-id="42c80-127">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="42c80-128">ビュー: **Razor (CSHTML)** します。</span><span class="sxs-lookup"><span data-stu-id="42c80-128">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="42c80-129">(既定値です。)</span><span class="sxs-lookup"><span data-stu-id="42c80-129">(The default.)</span></span>

<span data-ttu-id="42c80-130">[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-130">[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="42c80-131">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="42c80-131">Click **Add**.</span></span> <span data-ttu-id="42c80-132">Visual Web Developer には、次のファイルとフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="42c80-132">Visual Web Developer creates the following files and folders:</span></span>

- <span data-ttu-id="42c80-133">*MoviesController.cs*プロジェクトのファイル*コント ローラー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="42c80-133">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="42c80-134">A*映画*プロジェクトのフォルダー*ビュー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="42c80-134">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="42c80-135">*Create.cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml*、および*Index.cshtml*新しい*views \movies*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="42c80-135">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="42c80-136">[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-136">[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="42c80-137">ASP.NET MVC 3 のスキャフォールディング機能は自動的に作成、CRUD (作成、読み取り、更新、および削除) アクション メソッドとビューの。</span><span class="sxs-lookup"><span data-stu-id="42c80-137">The ASP.NET MVC 3 scaffolding mechanism automatically created the CRUD (create, read, update, and delete) action methods and views for you.</span></span> <span data-ttu-id="42c80-138">完全に機能する web アプリケーションを作成、一覧表示、編集、およびムービー エントリを削除することができますがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="42c80-138">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="42c80-139">アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL にします。</span><span class="sxs-lookup"><span data-stu-id="42c80-139">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="42c80-140">アプリケーションが既定のルーティングに依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`が既定値にルーティングされる`Index`のアクション メソッド、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="42c80-140">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="42c80-141">つまり、ブラウザーの要求`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`します。</span><span class="sxs-lookup"><span data-stu-id="42c80-141">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="42c80-142">まだ追加していないため、映画の空のリストになります。</span><span class="sxs-lookup"><span data-stu-id="42c80-142">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a><span data-ttu-id="42c80-143">ムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="42c80-143">Creating a Movie</span></span>

<span data-ttu-id="42c80-144">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="42c80-144">Select the **Create New** link.</span></span> <span data-ttu-id="42c80-145">ムービーの詳細を入力し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="42c80-145">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="42c80-146">クリックすると、**作成**ボタンにより、ムービー情報がデータベースに保存されているサーバーに投稿するフォーム。</span><span class="sxs-lookup"><span data-stu-id="42c80-146">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="42c80-147">リダイレクトし、 */Movies* URL の一覧で、新しく作成したムービーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="42c80-147">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="42c80-148">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-148">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)</span></span>

<span data-ttu-id="42c80-149">いくつかのムービー エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="42c80-149">Create a couple more movie entries.</span></span> <span data-ttu-id="42c80-150">**[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。</span><span class="sxs-lookup"><span data-stu-id="42c80-150">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="42c80-151">生成されたコードを調べる</span><span class="sxs-lookup"><span data-stu-id="42c80-151">Examining the Generated Code</span></span>

<span data-ttu-id="42c80-152">開く、 *Controllers\MoviesController.cs*ファイルを開き、生成された確認`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="42c80-152">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="42c80-153">ムービー コント ローラーの一部、`Index`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="42c80-153">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="42c80-154">次の行から、`MoviesController`クラスは、前述のようにムービー データベース コンテキストをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="42c80-154">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="42c80-155">ムービー データベース コンテキストを使用して、クエリ、編集、およびムービーを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="42c80-155">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="42c80-156">要求、`Movies`コント ローラーは、すべてのエントリを返します、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="42c80-156">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="42c80-157">厳密に型指定されたモデルと@modelキーワード</span><span class="sxs-lookup"><span data-stu-id="42c80-157">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="42c80-158">このチュートリアルでは、以前では、コント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートを説明しました、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="42c80-158">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="42c80-159">`ViewBag`はビューに情報を渡す便利な遅延バインディング方法を提供する動的オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="42c80-159">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="42c80-160">ASP.NET MVC では、ビュー テンプレートにオブジェクトやデータに型指定を厳密に渡す機能も提供します。</span><span class="sxs-lookup"><span data-stu-id="42c80-160">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="42c80-161">これにより、アプローチにより優れたコンパイル時のコードと Visual Web Developer のエディターでの高度な IntelliSense のチェックが厳密に型指定します。</span><span class="sxs-lookup"><span data-stu-id="42c80-161">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Web Developer editor.</span></span> <span data-ttu-id="42c80-162">このアプローチを使用しています、`MoviesController`クラスと*Index.cshtml*テンプレートの表示。</span><span class="sxs-lookup"><span data-stu-id="42c80-162">We're using this approach with the `MoviesController` class and *Index.cshtml* view template.</span></span>

<span data-ttu-id="42c80-163">このコードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すときに、`View`ヘルパー メソッドで、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="42c80-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="42c80-164">このコードで、これは`Movies`コント ローラーからビューの一覧。</span><span class="sxs-lookup"><span data-stu-id="42c80-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="42c80-165">含めることによって、`@model`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが必要とするオブジェクトの種類を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="42c80-165">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="42c80-166">Visual Web Developer で、次が自動的に含めムービー コント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="42c80-166">When you created the movie controller, Visual Web Developer automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="42c80-167">これは、`@model`ディレクティブを使用すると、コント ローラーを使用して、ビューに渡されるムービーの一覧へのアクセス、`Model`厳密に型指定されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="42c80-167">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="42c80-168">たとえば、 *Index.cshtml*テンプレート コードをムービーでループ処理、`foreach`経由で、厳密に型指定されたステートメント`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="42c80-168">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

<span data-ttu-id="42c80-169">`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`します。</span><span class="sxs-lookup"><span data-stu-id="42c80-169">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="42c80-170">その他の利点は、コードのコンパイル時のチェックを取得し、完全なコード エディターで IntelliSense のサポート、つまり。</span><span class="sxs-lookup"><span data-stu-id="42c80-170">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

<span data-ttu-id="42c80-171">[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-171">[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)</span></span>

## <a name="working-with-sql-server-compact"></a><span data-ttu-id="42c80-172">SQL Server Compact の使用</span><span class="sxs-lookup"><span data-stu-id="42c80-172">Working with SQL Server Compact</span></span>

<span data-ttu-id="42c80-173">Entity Framework Code First が検出されたデータベース接続文字列が指す、 `Movies` Code First にデータベースを自動的に作成するためにまだ存在していないデータベース。</span><span class="sxs-lookup"><span data-stu-id="42c80-173">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="42c80-174">作成されたことを確認することができます、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="42c80-174">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="42c80-175">表示されない場合、 *Movies.sdf*ファイルで、をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="42c80-175">If you don't see the *Movies.sdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

<span data-ttu-id="42c80-176">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-176">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)</span></span>

<span data-ttu-id="42c80-177">ダブルクリック*Movies.sdf*を開く**サーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="42c80-177">Double-click *Movies.sdf* to open **Server Explorer**.</span></span> <span data-ttu-id="42c80-178">展開し、**テーブル**フォルダーに、データベースで作成されたテーブルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="42c80-178">Then expand the **Tables** folder to see the tables that have been created in the database.</span></span>

> [!NOTE]
> <span data-ttu-id="42c80-179">ダブルクリックすると、エラーが発生した場合*Movies.sdf*、インストールされていることを確認[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)。</span><span class="sxs-lookup"><span data-stu-id="42c80-179">If you get an error when you double-click *Movies.sdf*, make sure you've installed [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support).</span></span> <span data-ttu-id="42c80-180">(ソフトウェアへのリンク、このチュートリアル シリーズのパート 1 での前提条件の一覧を参照してください)。リリースを今すぐインストールする場合を閉じて Visual Web Developer を再び開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="42c80-180">(For links to the software, see the list of prerequisites in part 1 of this tutorial series.) If you install the release now, you'll have to close and re-open Visual Web Developer.</span></span>


<span data-ttu-id="42c80-181">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-181">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)</span></span>

<span data-ttu-id="42c80-182">1 つずつ、2 つのテーブルがある、`Movie`エンティティ セットをクリックし、`EdmMetadata`テーブル。</span><span class="sxs-lookup"><span data-stu-id="42c80-182">There are two tables, one for the `Movie` entity set and then the `EdmMetadata` table.</span></span> <span data-ttu-id="42c80-183">`EdmMetadata`テーブルは、モデルとデータベースの同期を確認する Entity Framework によって使用されます。</span><span class="sxs-lookup"><span data-stu-id="42c80-183">The `EdmMetadata` table is used by the Entity Framework to determine when the model and the database are out of sync.</span></span>

<span data-ttu-id="42c80-184">右クリックし、`Movies`テーブルを選択**テーブル データの表示**作成したデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="42c80-184">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

<span data-ttu-id="42c80-185">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-185">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)</span></span>

<span data-ttu-id="42c80-186">右クリックし、`Movies`テーブルを選択**テーブル スキーマの編集**します。</span><span class="sxs-lookup"><span data-stu-id="42c80-186">Right-click the `Movies` table and select **Edit Table Schema**.</span></span>

<span data-ttu-id="42c80-187">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-187">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)</span></span>

<span data-ttu-id="42c80-188">[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-188">[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)</span></span>

<span data-ttu-id="42c80-189">通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`前に作成したクラス。</span><span class="sxs-lookup"><span data-stu-id="42c80-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="42c80-190">Entity Framework Code First 自動的に作成されたこのスキーマに基づいて、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="42c80-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="42c80-191">完了したら、接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="42c80-191">When you're finished, close the connection.</span></span> <span data-ttu-id="42c80-192">(接続を終了しない場合がありますエラーが表示、次回プロジェクトを実行する)。</span><span class="sxs-lookup"><span data-stu-id="42c80-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="42c80-193">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="42c80-193">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)</span></span>

<span data-ttu-id="42c80-194">データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="42c80-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="42c80-195">次のチュートリアルがスキャフォールディングされたコードの残りの部分を調べるされ追加、`SearchIndex`メソッドと`SearchIndex`ビューをこのデータベースのムービーを検索することができます。</span><span class="sxs-lookup"><span data-stu-id="42c80-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="42c80-196">[前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="42c80-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
