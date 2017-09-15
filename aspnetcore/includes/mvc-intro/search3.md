<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="247db-101">ここで検索を送信すると、URL に検索クエリ文字列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="247db-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="247db-102">`HttpPost Index` メソッドがある場合でも、検索時には `HttpGet Index` アクション メソッドにも移動します。</span><span class="sxs-lookup"><span data-stu-id="247db-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![URL に searchString=ghost が表示されたブラウザー ウィンドウ。返された Ghostbusters および Ghostbusters 2 というムービーには ghost という単語が含まれています](../../tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="247db-104">次のマークアップは `form` タグの変更を示しています。</span><span class="sxs-lookup"><span data-stu-id="247db-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="247db-105">ジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="247db-105">Adding Search by genre</span></span>

<span data-ttu-id="247db-106">次の `MovieGenreViewModel` クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="247db-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

<span data-ttu-id="247db-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="247db-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span></span>

<span data-ttu-id="247db-108">ムービージャンルのビュー モデルには以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="247db-108">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="247db-109">ムービーのリスト。</span><span class="sxs-lookup"><span data-stu-id="247db-109">A list of movies.</span></span>
   * <span data-ttu-id="247db-110">ジャンルのリストを含む `SelectList`。</span><span class="sxs-lookup"><span data-stu-id="247db-110">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="247db-111">これにより、ユーザーはリストからジャンルを選択できます。</span><span class="sxs-lookup"><span data-stu-id="247db-111">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="247db-112">選択されたジャンルを含む、`movieGenre`。</span><span class="sxs-lookup"><span data-stu-id="247db-112">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="247db-113">`MoviesController.cs` の `Index` メソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="247db-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

<span data-ttu-id="247db-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span><span class="sxs-lookup"><span data-stu-id="247db-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span></span>

<span data-ttu-id="247db-115">次のコードは、データベースからすべてのジャンルを取得する `LINQ` クエリです。</span><span class="sxs-lookup"><span data-stu-id="247db-115">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

<span data-ttu-id="247db-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span><span class="sxs-lookup"><span data-stu-id="247db-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span></span>

<span data-ttu-id="247db-117">ジャンルの `SelectList` は、個々のジャンルを投影して作成します (選択リストでジャンルが重複しないようにします)。</span><span class="sxs-lookup"><span data-stu-id="247db-117">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="247db-118">インデックス ビューへのジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="247db-118">Adding search by genre to the Index view</span></span>

<span data-ttu-id="247db-119">次のように `Index.cshtml` を更新します。</span><span class="sxs-lookup"><span data-stu-id="247db-119">Update `Index.cshtml` as follows:</span></span>

<span data-ttu-id="247db-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span><span class="sxs-lookup"><span data-stu-id="247db-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span></span>

<span data-ttu-id="247db-121">次の HTML ヘルパーで使用されるラムダ式を確認します。</span><span class="sxs-lookup"><span data-stu-id="247db-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="247db-122">上のコードでは、`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。</span><span class="sxs-lookup"><span data-stu-id="247db-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="247db-123">ラムダ式は評価されるのではなく、検査されるため、`model`、`model.movies`、または `model.movies[0]` が `null` または空である場合にアクセス違反が発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="247db-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="247db-124">ラムダ式が評価される場合 (`@Html.DisplayFor(modelItem => item.Title)` など)、モデルのプロパティ値が評価されます。</span><span class="sxs-lookup"><span data-stu-id="247db-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="247db-125">ジャンルまたはムービーのタイトル、あるいはその両方で検索して、アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="247db-125">Test the app by searching by genre, by movie title, and by both.</span></span>
