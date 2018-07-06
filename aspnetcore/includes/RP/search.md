# <a name="add-search-to-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="578f7-101">ASP.NET Core Razor ページ アプリへの検索の追加</span><span class="sxs-lookup"><span data-stu-id="578f7-101">Add search to an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="578f7-102">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="578f7-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="578f7-103">この記事では、ムービーを*ジャンル*や*題名*で検索できるように検索機能を索引ページに追加しています。</span><span class="sxs-lookup"><span data-stu-id="578f7-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="578f7-104">索引ページの `OnGetAsync` メソッドを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="578f7-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="578f7-105">`OnGetAsync` メソッドの最初の行により、ムービーを選択する [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) クエリが作成されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-105">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="578f7-106">このクエリはこの時点で*のみ*定義されます。データベースに対して**実行されていません**。</span><span class="sxs-lookup"><span data-stu-id="578f7-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="578f7-107">`searchString` パラメーターに文字列が含まれる場合、検索文字列で絞り込むようにムービークエリが変更されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="578f7-108">`s => s.Title.Contains()` コードは[ラムダ式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)です。</span><span class="sxs-lookup"><span data-stu-id="578f7-108">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="578f7-109">ラムダは、メソッド ベースの [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) クエリで、[Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) メソッドや `Contains` (先のコードで使用されています) など、標準クエリ演算子メソッドの引数として使用されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-109">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="578f7-110">LINQ クエリは、`Where`、`Contains`、`OrderBy` などのメソッドの呼び出しで定義または変更されたときには実行されません。</span><span class="sxs-lookup"><span data-stu-id="578f7-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="578f7-111">クエリ実行は先送りされます。</span><span class="sxs-lookup"><span data-stu-id="578f7-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="578f7-112">つまり、その具体値が繰り返されるか、`ToListAsync` メソッドが呼び出されるまで、式の評価が延ばされます。</span><span class="sxs-lookup"><span data-stu-id="578f7-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="578f7-113">詳細については、「[クエリ実行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="578f7-113">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="578f7-114">**注:** [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) メソッドは C# コードではなく、データベースで実行されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-114">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="578f7-115">クエリの大文字と小文字の区別は、データベースや照合順序に依存します。</span><span class="sxs-lookup"><span data-stu-id="578f7-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="578f7-116">SQL Server では、`Contains` は大文字/小文字の区別がない [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql) にマッピングされます。</span><span class="sxs-lookup"><span data-stu-id="578f7-116">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="578f7-117">SQLite では、既定の照合順序で、大文字と小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="578f7-118">ムービーページに移動し、`?searchString=Ghost` のようなクエリ文字列を URL に追加します (例: `http://localhost:5000/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="578f7-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="578f7-119">フィルターされたムービーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-119">The filtered movies are displayed.</span></span>

![インデックス ビュー](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="578f7-121">次のルート テンプレートが索引ページに追加される場合、検索文字列を URL セグメントとして渡すことができます (例: `http://localhost:5000/Movies/ghost`)。</span><span class="sxs-lookup"><span data-stu-id="578f7-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="578f7-122">先のルート制約では、クエリ文字列値の代わりに、ルート データ (URL セグメント) として題名を検索できます。</span><span class="sxs-lookup"><span data-stu-id="578f7-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="578f7-123">`"{searchString?}"` の `?` は、これが任意のルート パラメーターであることを意味します。</span><span class="sxs-lookup"><span data-stu-id="578f7-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![ghost という単語が URL に追加された索引ビュー。Ghostbusters と Ghostbusters 2 という 2 本のムービーからなるムービーリストが返されています。](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="578f7-125">ただし、URL を変更してムービーを検索することをユーザーに求めることはできません。</span><span class="sxs-lookup"><span data-stu-id="578f7-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="578f7-126">この手順では、ムービーを絞り込むための UI を追加します。</span><span class="sxs-lookup"><span data-stu-id="578f7-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="578f7-127">ルート制約 `"{searchString?}"` を追加した場合、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="578f7-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="578f7-128">*Pages/Movies/Index.cshtml* ファイルを開き、次のコードで強調表示されている `<form>` マークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="578f7-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="578f7-129">HTML `<form>` タグは[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)を使用します。</span><span class="sxs-lookup"><span data-stu-id="578f7-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="578f7-130">フォームが提出されると、フィルター文字列が*ページ/ムービー/索引*ページに送信されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="578f7-131">変更を保存し、フィルターをテストします。</span><span class="sxs-lookup"><span data-stu-id="578f7-131">Save the changes and test the filter.</span></span>

![タイトル フィルター テキストボックスに ghost という単語が入力されたインデックス ビュー](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="578f7-133">ジャンルで検索する</span><span class="sxs-lookup"><span data-stu-id="578f7-133">Search by genre</span></span>

<span data-ttu-id="578f7-134">強調表示されている次のプロパティを *Pages/Movies/Index.cshtml.cs* に追加します。</span><span class="sxs-lookup"><span data-stu-id="578f7-134">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="578f7-135">`SelectList Genres` には、ジャンル一覧が含まれます。</span><span class="sxs-lookup"><span data-stu-id="578f7-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="578f7-136">これにより、ユーザーは一覧からジャンルを選択できます。</span><span class="sxs-lookup"><span data-stu-id="578f7-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="578f7-137">`MovieGenre` プロパティには、"Western (西部劇)" など、ユーザーが選択する特定のジャンルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="578f7-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="578f7-138">`OnGetAsync` メソッドを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="578f7-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="578f7-139">次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。</span><span class="sxs-lookup"><span data-stu-id="578f7-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="578f7-140">ジャンルの `SelectList` は、別個のジャンルを推定することで作成されます。</span><span class="sxs-lookup"><span data-stu-id="578f7-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="578f7-141">ジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="578f7-141">Adding search by genre</span></span>

<span data-ttu-id="578f7-142">*Index.cshtml* を次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="578f7-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="578f7-143">ジャンルまたはムービーのタイトル、あるいはその両方で検索して、アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="578f7-143">Test the app by searching by genre, by movie title, and by both.</span></span>
