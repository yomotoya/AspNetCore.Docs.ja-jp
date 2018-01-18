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
ms.openlocfilehash: 3729f351ba7d1e5f566046a619c17e9e1e6614cb
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2018
---
# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="7c004-104">Razor ページ アプリへの検索の追加</span><span class="sxs-lookup"><span data-stu-id="7c004-104">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="7c004-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c004-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c004-106">この記事では、ムービーを*ジャンル*や*題名*で検索できるように検索機能を索引ページに追加しています。</span><span class="sxs-lookup"><span data-stu-id="7c004-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="7c004-107">索引ページの `OnGetAsync` メソッドを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="7c004-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="7c004-108">`OnGetAsync` メソッドの最初の行により、ムービーを選択する [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) クエリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-108">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="7c004-109">このクエリはこの時点で*のみ*定義されます。データベースに対して**実行されていません**。</span><span class="sxs-lookup"><span data-stu-id="7c004-109">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="7c004-110">`searchString` パラメーターに文字列が含まれる場合、検索文字列で絞り込むようにムービークエリが変更されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-110">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="7c004-111">`s => s.Title.Contains()` コードは[ラムダ式](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)です。</span><span class="sxs-lookup"><span data-stu-id="7c004-111">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="7c004-112">ラムダは、メソッド ベースの [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) クエリで、[Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) メソッドや `Contains` (先のコードで使用されています) など、標準クエリ演算子メソッドの引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-112">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="7c004-113">LINQ クエリは、メソッド (`Where`、`Contains`、`OrderBy` など) の呼び出しで定義または変更されたときには実行されません。</span><span class="sxs-lookup"><span data-stu-id="7c004-113">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="7c004-114">クエリ実行は先送りされます。</span><span class="sxs-lookup"><span data-stu-id="7c004-114">Rather, query execution is deferred.</span></span> <span data-ttu-id="7c004-115">つまり、その具体値が繰り返されるか、`ToListAsync` メソッドが呼び出されるまで、式の評価が延ばされます。</span><span class="sxs-lookup"><span data-stu-id="7c004-115">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="7c004-116">詳細については、「[クエリ実行](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7c004-116">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="7c004-117">**注:** [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) メソッドは C# コードではなく、データベースで実行されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-117">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="7c004-118">クエリの大文字と小文字の区別は、データベースや照合順序に依存します。</span><span class="sxs-lookup"><span data-stu-id="7c004-118">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="7c004-119">SQL Server では、`Contains` は大文字/小文字の区別がない [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql) にマッピングされます。</span><span class="sxs-lookup"><span data-stu-id="7c004-119">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="7c004-120">SQLite では、既定の照合順序で、大文字と小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-120">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="7c004-121">ムービーページに移動し、`?searchString=Ghost` のようなクエリ文字列を URL に追加します (例: `http://localhost:5000/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="7c004-121">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="7c004-122">フィルターされたムービーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-122">The filtered movies are displayed.</span></span>

![インデックス ビュー](search/_static/ghost.png)

<span data-ttu-id="7c004-124">次のルート テンプレートが索引ページに追加される場合、検索文字列を URL セグメントとして渡すことができます (例: `http://localhost:5000/Movies/ghost`)。</span><span class="sxs-lookup"><span data-stu-id="7c004-124">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="7c004-125">先のルート制約では、クエリ文字列値の代わりに、ルート データ (URL セグメント) として題名を検索できます。</span><span class="sxs-lookup"><span data-stu-id="7c004-125">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="7c004-126">`"{searchString?}"` の `?` は、これが任意のルート パラメーターであることを意味します。</span><span class="sxs-lookup"><span data-stu-id="7c004-126">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![ghost という単語が URL に追加された索引ビュー。Ghostbusters と Ghostbusters 2 という 2 本のムービーからなるムービーリストが返されています。](search/_static/g2.png)

<span data-ttu-id="7c004-128">ただし、URL を変更してムービーを検索することをユーザーに求めることはできません。</span><span class="sxs-lookup"><span data-stu-id="7c004-128">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="7c004-129">この手順では、ムービーを絞り込むための UI を追加します。</span><span class="sxs-lookup"><span data-stu-id="7c004-129">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="7c004-130">ルート制約 `"{searchString?}"` を追加した場合、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="7c004-130">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="7c004-131">*Pages/Movies/Index.cshtml* ファイルを開き、次のコードで強調表示されている `<form>` マークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="7c004-131">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="7c004-132">HTML `<form>` タグは[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)を使用します。</span><span class="sxs-lookup"><span data-stu-id="7c004-132">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="7c004-133">フォームが提出されると、フィルター文字列が*ページ/ムービー/索引*ページに送信されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-133">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="7c004-134">変更を保存し、フィルターをテストします。</span><span class="sxs-lookup"><span data-stu-id="7c004-134">Save the changes and test the filter.</span></span>

![タイトル フィルター テキストボックスに ghost という単語が入力されたインデックス ビュー](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="7c004-136">ジャンルで検索する</span><span class="sxs-lookup"><span data-stu-id="7c004-136">Search by genre</span></span>

<span data-ttu-id="7c004-137">強調表示されている次のプロパティを *Pages/Movies/Index.cshtml.cs* に追加します。</span><span class="sxs-lookup"><span data-stu-id="7c004-137">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="7c004-138">`SelectList Genres` には、ジャンル一覧が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7c004-138">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="7c004-139">これにより、ユーザーは一覧からジャンルを選択できます。</span><span class="sxs-lookup"><span data-stu-id="7c004-139">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="7c004-140">`MovieGenre` プロパティには、"Western (西部劇)" など、ユーザーが選択する特定のジャンルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7c004-140">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="7c004-141">`OnGetAsync` メソッドを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="7c004-141">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="7c004-142">次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。</span><span class="sxs-lookup"><span data-stu-id="7c004-142">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="7c004-143">ジャンルの `SelectList` は、別個のジャンルを推定することで作成されます。</span><span class="sxs-lookup"><span data-stu-id="7c004-143">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="7c004-144">ジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="7c004-144">Adding search by genre</span></span>

<span data-ttu-id="7c004-145">*Index.cshtml* を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="7c004-145">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="7c004-146">ジャンルまたはムービーのタイトル、あるいはその両方で検索して、アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="7c004-146">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7c004-147">[前: ページの更新](xref:tutorials/razor-pages/da1)
[次: 新しいフィールドの追加](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="7c004-147">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
