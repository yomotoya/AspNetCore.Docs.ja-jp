<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="14184-101">ここで検索を送信すると、URL に検索クエリ文字列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="14184-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="14184-102">`HttpPost Index` メソッドがある場合でも、検索時には `HttpGet Index` アクション メソッドにも移動します。</span><span class="sxs-lookup"><span data-stu-id="14184-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![URL に searchString=ghost が表示されたブラウザー ウィンドウ。返された Ghostbusters および Ghostbusters 2 というムービーには ghost という単語が含まれています](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="14184-104">次のマークアップは `form` タグの変更を示しています。</span><span class="sxs-lookup"><span data-stu-id="14184-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="14184-105">ジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="14184-105">Adding Search by genre</span></span>

<span data-ttu-id="14184-106">次の `MovieGenreViewModel` クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="14184-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="14184-107">ムービージャンルのビュー モデルには以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="14184-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="14184-108">ムービーのリスト。</span><span class="sxs-lookup"><span data-stu-id="14184-108">A list of movies.</span></span>
   * <span data-ttu-id="14184-109">ジャンルのリストを含む `SelectList`。</span><span class="sxs-lookup"><span data-stu-id="14184-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="14184-110">これにより、ユーザーは一覧からジャンルを選択できます。</span><span class="sxs-lookup"><span data-stu-id="14184-110">This allows the user to select a genre from the list.</span></span>
   * <span data-ttu-id="14184-111">選択されたジャンルを含む、`MovieGenre`。</span><span class="sxs-lookup"><span data-stu-id="14184-111">`MovieGenre`, which contains the selected genre.</span></span>
   * <span data-ttu-id="14184-112">ユーザーが検索テキスト ボックスに入力したテキストが含まれる `SearchString`。</span><span class="sxs-lookup"><span data-stu-id="14184-112">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="14184-113">`MoviesController.cs` の `Index` メソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="14184-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="14184-114">次のコードは、データベースからすべてのジャンルを取得する `LINQ` クエリです。</span><span class="sxs-lookup"><span data-stu-id="14184-114">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="14184-115">ジャンルの `SelectList` は、個々のジャンルを投影して作成します (選択リストでジャンルが重複しないようにします)。</span><span class="sxs-lookup"><span data-stu-id="14184-115">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="14184-116">ユーザーが項目を検索すると、検索値が検索ボックスに保持されます。</span><span class="sxs-lookup"><span data-stu-id="14184-116">When the user searches for the item, the search value is retained in the search box.</span></span> <span data-ttu-id="14184-117">検索値を保持するには、`SearchString` プロパティに検索値を設定します。</span><span class="sxs-lookup"><span data-stu-id="14184-117">To retain the search value,  populate the `SearchString` property with the search value.</span></span> <span data-ttu-id="14184-118">検索値は、`Index` コントローラー アクションに対する `searchString` パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="14184-118">The search value is the `searchString` parameter for the `Index` controller action.</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="14184-119">インデックス ビューへのジャンルによる検索の追加</span><span class="sxs-lookup"><span data-stu-id="14184-119">Adding search by genre to the Index view</span></span>

<span data-ttu-id="14184-120">次のように `Index.cshtml` を更新します。</span><span class="sxs-lookup"><span data-stu-id="14184-120">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="14184-121">次の HTML ヘルパーで使用されるラムダ式を確認します。</span><span class="sxs-lookup"><span data-stu-id="14184-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
<span data-ttu-id="14184-122">上のコードでは、`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。</span><span class="sxs-lookup"><span data-stu-id="14184-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="14184-123">ラムダ式は評価されるのではなく、検査されるため、`model`、`model.Movies`、または `model.Movies[0]` が `null` または空である場合にアクセス違反が発生することはありません。</span><span class="sxs-lookup"><span data-stu-id="14184-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="14184-124">ラムダ式が評価される場合 (`@Html.DisplayFor(modelItem => item.Title)` など)、モデルのプロパティ値が評価されます。</span><span class="sxs-lookup"><span data-stu-id="14184-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="14184-125">ジャンルまたはムービーのタイトル、あるいはその両方で検索して、アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="14184-125">Test the app by searching by genre, by movie title, and by both.</span></span>
