---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: コント ローラー (VB) から、モデルのデータへのアクセス |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 47d6e611ea79cf8c217f1f793d342b065233081f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577887"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a><span data-ttu-id="1b57e-103">コント ローラー (VB) から、モデルのデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="1b57e-103">Accessing your Model's Data from a Controller (VB)</span></span>
====================
<span data-ttu-id="1b57e-104">によって[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="1b57e-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="1b57e-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="1b57e-106">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="1b57e-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="1b57e-108">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="1b57e-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="1b57e-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="1b57e-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="1b57e-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="1b57e-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="1b57e-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="1b57e-113">VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="1b57e-114">[VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="1b57e-115">C# を使用する場合に切り替えて、 [c# バージョン](../cs/accessing-your-models-data-from-a-controller.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="1b57e-115">If you prefer C#, switch to the [C# version](../cs/accessing-your-models-data-from-a-controller.md) of this tutorial.</span></span>


<span data-ttu-id="1b57e-116">このセクションで作成します新しい`MoviesController`クラスし、映画のデータを取得し、ビュー テンプレートを使用してブラウザーで表示するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-116">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span> <span data-ttu-id="1b57e-117">続行する前に、アプリケーションを構築することを確認します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-117">Be sure to build your application before proceeding.</span></span>

<span data-ttu-id="1b57e-118">右クリックし、*コント ローラー*フォルダーを新規作成および`MoviesController`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-118">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="1b57e-119">次のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-119">Select the following options:</span></span>

- <span data-ttu-id="1b57e-120">コント ローラー名: **MoviesController**します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-120">Controller name: **MoviesController**.</span></span> <span data-ttu-id="1b57e-121">(これは、既定値です)。</span><span class="sxs-lookup"><span data-stu-id="1b57e-121">(This is the default.)</span></span>
- <span data-ttu-id="1b57e-122">テンプレート: **Entity Framework を使用して、読み取り/書き込み操作とビューとコント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-122">Template: **Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="1b57e-123">モデル クラス: **Movie (MvcMovie.Models)** します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-123">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="1b57e-124">データ コンテキスト クラス: **MovieDBContext (MvcMovie.Models)** します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-124">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="1b57e-125">ビュー: **Razor (CSHTML)** します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-125">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="1b57e-126">(既定値です。)</span><span class="sxs-lookup"><span data-stu-id="1b57e-126">(The default.)</span></span>

<span data-ttu-id="1b57e-127">[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-127">[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="1b57e-128">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1b57e-128">Click **Add**.</span></span> <span data-ttu-id="1b57e-129">Visual Web Developer には、次のファイルとフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-129">Visual Web Developer creates the following files and folders:</span></span>

- <span data-ttu-id="1b57e-130">*MoviesController.vb*プロジェクトのファイル*コント ローラー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-130">*A MoviesController.vb* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="1b57e-131">A*映画*プロジェクトのフォルダー*ビュー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-131">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="1b57e-132">*Create.vbhtml、Delete.vbhtml、Details.vbhtml、Edit.vbhtml*、および*Index.vbhtml*新しい*views \movies*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-132">*Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, and *Index.vbhtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="1b57e-133">[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-133">[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="1b57e-134">ASP.NET MVC 3 のスキャフォールディング機能は自動的に作成、CRUD (作成、読み取り、更新、および削除) アクション メソッドとビューの。</span><span class="sxs-lookup"><span data-stu-id="1b57e-134">The ASP.NET MVC 3 scaffolding mechanism automatically created the CRUD (create, read, update, and delete) action methods and views for you.</span></span> <span data-ttu-id="1b57e-135">完全に機能する web アプリケーションを作成、一覧表示、編集、およびムービー エントリを削除することができますがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="1b57e-135">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="1b57e-136">アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL にします。</span><span class="sxs-lookup"><span data-stu-id="1b57e-136">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="1b57e-137">アプリケーションが既定のルーティングに依存するため (で定義されている、 *Global.asax*ファイル)、ブラウザーの要求`http://localhost:xxxxx/Movies`が既定値にルーティングされる`Index`のアクション メソッド、`Movies`コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-137">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="1b57e-138">つまり、ブラウザーの要求`http://localhost:xxxxx/Movies`ブラウザーの要求と同じでは、事実上`http://localhost:xxxxx/Movies/Index`します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-138">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="1b57e-139">まだ追加していないため、映画の空のリストになります。</span><span class="sxs-lookup"><span data-stu-id="1b57e-139">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a><span data-ttu-id="1b57e-140">ムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-140">Creating a Movie</span></span>

<span data-ttu-id="1b57e-141">**[Create New]\(新規作成\)** リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-141">Select the **Create New** link.</span></span> <span data-ttu-id="1b57e-142">ムービーの詳細を入力し、**作成**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1b57e-142">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="1b57e-143">クリックすると、**作成**ボタンにより、ムービー情報がデータベースに保存されているサーバーに投稿するフォーム。</span><span class="sxs-lookup"><span data-stu-id="1b57e-143">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="1b57e-144">リダイレクトし、 */Movies* URL の一覧で、新しく作成したムービーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-144">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="1b57e-145">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-145">[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)</span></span>

<span data-ttu-id="1b57e-146">いくつかのムービー エントリを作成します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-146">Create a couple more movie entries.</span></span> <span data-ttu-id="1b57e-147">**[編集]**、**[詳細]**、および **[削除]** リンクを試してください。すべて機能します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-147">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="1b57e-148">生成されたコードを調べる</span><span class="sxs-lookup"><span data-stu-id="1b57e-148">Examining the Generated Code</span></span>

<span data-ttu-id="1b57e-149">開く、 *Controllers\MoviesController.vb*ファイルを開き、生成された確認`Index`メソッド。</span><span class="sxs-lookup"><span data-stu-id="1b57e-149">Open the *Controllers\MoviesController.vb* file and examine the generated `Index` method.</span></span> <span data-ttu-id="1b57e-150">ムービー コント ローラーの一部、`Index`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-150">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

<span data-ttu-id="1b57e-151">次の行から、`MoviesController`クラスは、前述のようにムービー データベース コンテキストをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-151">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="1b57e-152">ムービー データベース コンテキストを使用して、クエリ、編集、およびムービーを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-152">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

<span data-ttu-id="1b57e-153">要求、`Movies`コント ローラーは、すべてのエントリを返します、`Movies`ムービー データベースのテーブルに結果を渡します、`Index`ビュー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-153">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="1b57e-154">厳密に型指定されたモデルと@modelキーワード</span><span class="sxs-lookup"><span data-stu-id="1b57e-154">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="1b57e-155">このチュートリアルでは、以前では、コント ローラーが渡す方法データまたはオブジェクトを使用してビュー テンプレートを説明しました、`ViewBag`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1b57e-155">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="1b57e-156">`ViewBag`はビューに情報を渡す便利な遅延バインディング方法を提供する動的オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="1b57e-156">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="1b57e-157">ASP.NET MVC では、ビュー テンプレートにオブジェクトやデータに型指定を厳密に渡す機能も提供します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-157">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="1b57e-158">これにより、アプローチにより優れたコンパイル時のコードと Visual Web Developer のエディターでの高度な IntelliSense のチェックが厳密に型指定します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-158">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Web Developer editor.</span></span> <span data-ttu-id="1b57e-159">このアプローチを使用しています、`MoviesController`クラスと*Index.vbhtml*テンプレートの表示。</span><span class="sxs-lookup"><span data-stu-id="1b57e-159">We're using this approach with the `MoviesController` class and *Index.vbhtml* view template.</span></span>

<span data-ttu-id="1b57e-160">このコードを作成する方法に注意してください、 [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx)オブジェクトを呼び出すときに、`View`ヘルパー メソッドで、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="1b57e-160">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="1b57e-161">このコードで、これは`Movies`コント ローラーからビューの一覧。</span><span class="sxs-lookup"><span data-stu-id="1b57e-161">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

<span data-ttu-id="1b57e-162">含めることによって、`@ModelType`ビュー テンプレート ファイルの上部にあるステートメントでは、ビューが必要とするオブジェクトの種類を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-162">By including a `@ModelType` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="1b57e-163">Visual Web Developer で、次が自動的に含めムービー コント ローラーを作成したときに`@model`の上部にあるステートメント、 *Index.vbhtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1b57e-163">When you created the movie controller, Visual Web Developer automatically included the following `@model` statement at the top of the *Index.vbhtml* file:</span></span>

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

<span data-ttu-id="1b57e-164">これは、`@ModelType`ディレクティブを使用すると、コント ローラーを使用して、ビューに渡されるムービーの一覧へのアクセス、`Model`厳密に型指定されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1b57e-164">This `@ModelType` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="1b57e-165">たとえば、 *Index.vbhtml*テンプレート コードをムービーでループ処理、`foreach`経由で、厳密に型指定されたステートメント`Model`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1b57e-165">For example, in the *Index.vbhtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

<span data-ttu-id="1b57e-166">`Model`オブジェクトが厳密に型指定された (として、`IEnumerable<Movie>`オブジェクト)、各`item`として、ループ内のオブジェクトが型指定された`Movie`します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-166">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="1b57e-167">その他の利点は、コードのコンパイル時のチェックを取得し、完全なコード エディターで IntelliSense のサポート、つまり。</span><span class="sxs-lookup"><span data-stu-id="1b57e-167">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

<span data-ttu-id="1b57e-168">[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-168">[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)</span></span>

## <a name="working-with-sql-server-compact"></a><span data-ttu-id="1b57e-169">SQL Server Compact の使用</span><span class="sxs-lookup"><span data-stu-id="1b57e-169">Working with SQL Server Compact</span></span>

<span data-ttu-id="1b57e-170">Entity Framework Code First が検出されたデータベース接続文字列が指す、 `Movies` Code First にデータベースを自動的に作成するためにまだ存在していないデータベース。</span><span class="sxs-lookup"><span data-stu-id="1b57e-170">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="1b57e-171">作成されたことを確認することができます、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-171">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="1b57e-172">表示されない場合、 *Movies.sdf*ファイルで、をクリックして、 **すべてのファイル**ボタン、**ソリューション エクスプ ローラー**ツールバーで、をクリックして、**更新**ボタンをクリックし、展開、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1b57e-172">If you don't see the *Movies.sdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

<span data-ttu-id="1b57e-173">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-173">[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)</span></span>

<span data-ttu-id="1b57e-174">ダブルクリック*Movies.sdf*を開く**サーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-174">Double-click *Movies.sdf* to open **Server Explorer**.</span></span> <span data-ttu-id="1b57e-175">展開し、**テーブル**フォルダーに、データベースで作成されたテーブルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1b57e-175">Then expand the **Tables** folder to see the tables that have been created in the database.</span></span>

> [!NOTE]
> <span data-ttu-id="1b57e-176">ダブルクリックすると、エラーが発生した場合*Movies.sdf*、インストールされていることを確認**Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-176">If you get an error when you double-click *Movies.sdf*, make sure you've installed **Visual Studio 2010 SP1 Tools for SQL Server Compact 4.0**.</span></span> <span data-ttu-id="1b57e-177">(ソフトウェアへのリンク、このチュートリアル シリーズのパート 1 での前提条件の一覧を参照してください)。リリースを今すぐインストールする場合を閉じて Visual Web Developer を再び開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="1b57e-177">(For links to the software, see the list of prerequisites in part 1 of this tutorial series.) If you install the release now, you'll have to close and re-open Visual Web Developer.</span></span>


<span data-ttu-id="1b57e-178">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-178">[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)</span></span>

<span data-ttu-id="1b57e-179">1 つずつ、2 つのテーブルがある、`Movie`エンティティ セットをクリックし、`EdmMetadata`テーブル。</span><span class="sxs-lookup"><span data-stu-id="1b57e-179">There are two tables, one for the `Movie` entity set and then the `EdmMetadata` table.</span></span> <span data-ttu-id="1b57e-180">`EdmMetadata`テーブルは、モデルとデータベースの同期を確認する Entity Framework によって使用されます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-180">The `EdmMetadata` table is used by the Entity Framework to determine when the model and the database are out of sync.</span></span>

<span data-ttu-id="1b57e-181">右クリックし、`Movies`テーブルを選択**テーブル データの表示**作成したデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-181">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

<span data-ttu-id="1b57e-182">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-182">[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)</span></span>

<span data-ttu-id="1b57e-183">右クリックし、`Movies`テーブルを選択**テーブル スキーマの編集**します。</span><span class="sxs-lookup"><span data-stu-id="1b57e-183">Right-click the `Movies` table and select **Edit Table Schema**.</span></span>

<span data-ttu-id="1b57e-184">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-184">[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)</span></span>

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

<span data-ttu-id="1b57e-186">通知方法のスキーマ、`Movies`テーブルにマップ、`Movie`前に作成したクラス。</span><span class="sxs-lookup"><span data-stu-id="1b57e-186">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="1b57e-187">Entity Framework Code First 自動的に作成されたこのスキーマに基づいて、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="1b57e-187">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="1b57e-188">完了したら、接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-188">When you're finished, close the connection.</span></span> <span data-ttu-id="1b57e-189">(接続を終了しない場合がありますエラーが表示、次回プロジェクトを実行する)。</span><span class="sxs-lookup"><span data-stu-id="1b57e-189">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="1b57e-190">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="1b57e-190">[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)</span></span>

<span data-ttu-id="1b57e-191">データベースとそこからコンテンツを表示する単純なリスト ページがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="1b57e-191">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="1b57e-192">次のチュートリアルがスキャフォールディングされたコードの残りの部分を調べるされ追加、`SearchIndex`メソッドと`SearchIndex`ビューをこのデータベースのムービーを検索することができます。</span><span class="sxs-lookup"><span data-stu-id="1b57e-192">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1b57e-193">[前へ](adding-a-model.md)
> [次へ](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="1b57e-193">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
