---
title: "ASP.NET Core Razor ページへの検索の追加"
author: rick-anderson
description: "ASP.NET Core Razor ページに検索を追加する方法を紹介します"
keywords: "ASP.NET Core,検索、Razor ページ"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/search
ms.openlocfilehash: f70f1e9b0e085f5aa90fcca499526588662c3cfd
ms.sourcegitcommit: ffac7e195bd7f99364f3aab45e491eeaf8f173b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2017
---
# <a name="adding-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="89479-104">ASP.NET Core MVC アプリへの検索の追加</span><span class="sxs-lookup"><span data-stu-id="89479-104">Adding search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="89479-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="89479-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="89479-106">この記事では、ムービーを*ジャンル*や*題名*で検索できるように検索機能を索引ページに追加しています。</span><span class="sxs-lookup"><span data-stu-id="89479-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="89479-107">索引ページの `OnGetAsync` メソッドを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="89479-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

<span data-ttu-id="89479-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]</span><span class="sxs-lookup"><span data-stu-id="89479-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]</span></span>

<span data-ttu-id="89479-109">`OnGetAsync` メソッドの最初の行により、ムービーを選択する [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) クエリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="89479-109">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
 var movies = from m in _context.Movie
              select m;
```

<span data-ttu-id="89479-110">このクエリはこの時点で*のみ*定義されます。データベースに対して**実行されていません**。</span><span class="sxs-lookup"><span data-stu-id="89479-110">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="89479-111">`searchString` パラメーターに文字列が含まれる場合、検索文字列で絞り込むようにムービークエリが変更されます。</span><span class="sxs-lookup"><span data-stu-id="89479-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

<span data-ttu-id="89479-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]</span><span class="sxs-lookup"><span data-stu-id="89479-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]</span></span>

<span data-ttu-id="89479-113">`s => s.Title.Contains()` コードは[ラムダ式](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)です。</span><span class="sxs-lookup"><span data-stu-id="89479-113">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="89479-114">ラムダは、メソッド ベースの [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) クエリで、[Where](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) メソッドや `Contains` (先のコードで使用されています) など、標準クエリ演算子メソッドの引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="89479-114">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="89479-115">LINQ クエリは、メソッド (`Where`、`Contains`、`OrderBy` など) の呼び出しで定義または変更されたときには実行されません。</span><span class="sxs-lookup"><span data-stu-id="89479-115">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="89479-116">クエリ実行は先送りされます。</span><span class="sxs-lookup"><span data-stu-id="89479-116">Rather, query execution is deferred.</span></span> <span data-ttu-id="89479-117">つまり、その具体値が繰り返されるか、`ToListAsync` メソッドが呼び出されるまで、式の評価が延ばされます。</span><span class="sxs-lookup"><span data-stu-id="89479-117">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="89479-118">詳細については、「[クエリ実行](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/ef/language-reference/query-execution)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="89479-118">See [Query Execution](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="89479-119">**注:** [Contains](http://msdn.microsoft.com/library/bb155125.aspx) メソッドは C# コードではなく、データベースで実行されます。</span><span class="sxs-lookup"><span data-stu-id="89479-119">**Note:** The [Contains](http://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="89479-120">クエリの大文字と小文字の区別は、データベースや照合順序に依存します。</span><span class="sxs-lookup"><span data-stu-id="89479-120">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="89479-121">SQL Server では、`Contains` は大文字/小文字の区別がない [SQL LIKE](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql) にマッピングされます。</span><span class="sxs-lookup"><span data-stu-id="89479-121">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="89479-122">SQLite では、既定の照合順序で、大文字と小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="89479-122">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="89479-123">ムービーページに移動し、`?searchString=Ghost` のようなクエリ文字列を URL に追加します (例: `http://localhost:5000/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="89479-123">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="89479-124">フィルターされたムービーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="89479-124">The filtered movies are displayed.</span></span>

![インデックス ビュー](search/_static/ghost.png)

<span data-ttu-id="89479-126">次のルート テンプレートが索引ページに追加される場合、検索文字列を URL セグメントとして渡すことができます (例: `http://localhost:5000/Movies/ghost`)。</span><span class="sxs-lookup"><span data-stu-id="89479-126">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="89479-127">先のルート制約では、クエリ文字列値の代わりに、ルート データ (URL セグメント) として題名を検索できます。</span><span class="sxs-lookup"><span data-stu-id="89479-127">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="89479-128">`"{searchString?}"` の `?` は、これが任意のルート パラメーターであることを意味します。</span><span class="sxs-lookup"><span data-stu-id="89479-128">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![ghost という単語が URL に追加された索引ビュー。Ghostbusters と Ghostbusters 2 という 2 本のムービーからなるムービーリストが返されています。](search/_static/g2.png)

<span data-ttu-id="89479-130">ただし、URL を変更してムービーを検索することをユーザーに求めることはできません。</span><span class="sxs-lookup"><span data-stu-id="89479-130">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="89479-131">この手順では、ムービーを絞り込むための UI を追加します。</span><span class="sxs-lookup"><span data-stu-id="89479-131">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="89479-132">ルート制約 `"{searchString?}"` を追加した場合、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="89479-132">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="89479-133">*Pages/Movies/Index.cshtml* ファイルを開き、次のコードで強調表示されている `<form>` マークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="89479-133">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

<span data-ttu-id="89479-134">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]</span><span class="sxs-lookup"><span data-stu-id="89479-134">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]</span></span>

<span data-ttu-id="89479-135">HTML `<form>` タグは[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)を使用します。</span><span class="sxs-lookup"><span data-stu-id="89479-135">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="89479-136">フォームが提出されると、フィルター文字列が*ページ/ムービー/索引*ページに送信されます。</span><span class="sxs-lookup"><span data-stu-id="89479-136">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="89479-137">変更を保存し、フィルターをテストします。</span><span class="sxs-lookup"><span data-stu-id="89479-137">Save the changes and test the filter.</span></span>

![タイトル フィルター テキストボックスに ghost という単語が入力されたインデックス ビュー](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="89479-139">ジャンルで検索する</span><span class="sxs-lookup"><span data-stu-id="89479-139">Search by genre</span></span>

<span data-ttu-id="89479-140">強調表示されている次のプロパティを *Pages/Movies/Index.cshtml.cs* に追加します。</span><span class="sxs-lookup"><span data-stu-id="89479-140">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

<span data-ttu-id="89479-141">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]</span><span class="sxs-lookup"><span data-stu-id="89479-141">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]</span></span>

<span data-ttu-id="89479-142">`SelectList Genres` には、ジャンル一覧が含まれます。</span><span class="sxs-lookup"><span data-stu-id="89479-142">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="89479-143">これにより、ユーザーは一覧からジャンルを選択できます。</span><span class="sxs-lookup"><span data-stu-id="89479-143">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="89479-144">`MovieGenre` プロパティには、"Western (西部劇)" など、ユーザーが選択する特定のジャンルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="89479-144">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="89479-145">`OnGetAsync` メソッドを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="89479-145">Update the `OnGetAsync` method with the following code:</span></span>

<span data-ttu-id="89479-146">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]</span><span class="sxs-lookup"><span data-stu-id="89479-146">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]</span></span>

<span data-ttu-id="89479-147">次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。</span><span class="sxs-lookup"><span data-stu-id="89479-147">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

<span data-ttu-id="89479-148">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]</span><span class="sxs-lookup"><span data-stu-id="89479-148">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]</span></span>

<span data-ttu-id="89479-149">ジャンルの `SelectList` は、別個のジャンルを推定することで作成されます。</span><span class="sxs-lookup"><span data-stu-id="89479-149">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="89479-150">ジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="89479-150">Adding search by genre</span></span>

<span data-ttu-id="89479-151">*Index.cshtml* を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="89479-151">Update *Index.cshtml* as follows:</span></span>

<span data-ttu-id="89479-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]</span><span class="sxs-lookup"><span data-stu-id="89479-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]</span></span>

<span data-ttu-id="89479-153">ジャンルまたはムービーのタイトル、あるいはその両方で検索して、アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="89479-153">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="89479-154">[前: ページの更新](xref:tutorials/razor-pages/da1)
[次: 新しいフィールドの追加](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="89479-154">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>