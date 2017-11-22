<span data-ttu-id="b8f6d-101">上のコードで強調表示されている部分は、(*Startup.cs* ファイルの) [依存性の注入](xref:fundamentals/dependency-injection)コンテナーに追加されているムービー データベース コンテキストを示しています。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="b8f6d-102">`services.AddDbContext<MvcMovieContext>(options =>` では、使用するデータベースと接続文字列を指定します。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="b8f6d-103">`=>` は[ラムダ演算子](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator)です。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-103">`=>` is a [lambda operator](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="b8f6d-104">*Controllers/MoviesController.cs* ファイルを開いて、コンストラクターを調べます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="b8f6d-105">コンストラクターでは、[依存性の注入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`MvcMovieContext `) がコントローラーに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="b8f6d-106">データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="b8f6d-107">厳密に型指定されたモデルと @model キーワード</span><span class="sxs-lookup"><span data-stu-id="b8f6d-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="b8f6d-108">コントローラーで `ViewData` ディクショナリを使ってビューにデータまたはオブジェクトを渡す方法を前に示しました。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="b8f6d-109">`ViewData` ディクショナリは動的オブジェクトであり、ビューに情報を渡すための便利な遅延バインディングの方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="b8f6d-110">MVC にも、厳密に型指定されたモデル オブジェクトをビューに渡す機能があります。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="b8f6d-111">この厳密に型指定された方法を使うと、コンパイル時のコードのチェックが向上します。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="b8f6d-112">スキャフォールディング メカニズムは、メソッドとビューを作成するときに、`MoviesController` クラスとビューでこの方法 (つまり、厳密に型指定されたモデルを渡すこと) を使いました。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="b8f6d-113">*Controllers/MoviesController.cs* ファイルで生成された `Details` メソッドを調べてください。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="b8f6d-114">通常、`id` パラメーターはルート データとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-114">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="b8f6d-115">たとえば、`http://localhost:5000/movies/details/1` は次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-115">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="b8f6d-116">コントローラーを `movies` コントローラーに (最初の URL セグメント)。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-116">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="b8f6d-117">アクションを `details` に (2 番目の URL セグメント)。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-117">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="b8f6d-118">ID を 1 に (最後の URL セグメント)。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-118">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="b8f6d-119">次のようにクエリ文字列で `id` を渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-119">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="b8f6d-120">ID 値が指定されない場合、`id` パラメーターは [null 許容型](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) として定義されます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-120">The `id` parameter is defined as a [nullable type](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value is not provided.</span></span>

<span data-ttu-id="b8f6d-121">ルート データまたはクエリ文字列の値と一致するムービー エンティティを選択するため、[ラムダ式](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)が `SingleOrDefaultAsync` に渡されます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-121">A [lambda expression](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

<span data-ttu-id="b8f6d-122">ムービーが見つかった場合、`Movie` モデルのインスタンスが `Details` ビューに渡されます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-122">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="b8f6d-123">*Views/Movies/Details.cshtml* ファイルの内容を確認してください。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-123">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="b8f6d-124">ビュー ファイルの先頭に `@model` ステートメントを含めることで、ビューが期待するオブジェクトの型を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-124">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="b8f6d-125">ムービー コントローラーを作成したとき、Visual Studio によって *Details.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-125">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="b8f6d-126">この `@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-126">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b8f6d-127">たとえば、*Details.cshtml* ビューでは、コードで厳密に型指定された `Model` オブジェクトを使って、`DisplayNameFor` および `DisplayFor` HTML ヘルパーに各ムービー フィールドを渡しています。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-127">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="b8f6d-128">`Create` および `Edit` のメソッドとビューも、`Movie` モデル オブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-128">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="b8f6d-129">Movies コントローラーの *Index.cshtml* ビューと `Index` メソッドを確認してください。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-129">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="b8f6d-130">コードで `View` メソッドを呼び出すときの `List` オブジェクトの作成方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-130">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="b8f6d-131">コードでは、この `Movies` リストを `Index` アクション メソッドからビューに渡しています。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-131">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="b8f6d-132">ムービー コントローラーを作成したとき、スキャフォールディングによって *Index.cshtml* ファイルの先頭に `@model` ステートメントが自動的に追加されています。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-132">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="b8f6d-133">`@model` ディレクティブにより、厳密に型指定された `Model` オブジェクトを使って、コントローラーがビューに渡したムービーのリストにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-133">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="b8f6d-134">たとえば、*Index.cshtml* ビューのコードでは、`foreach` ステートメントを使って厳密に型指定された `Model` オブジェクトのムービーをループ処理しています。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-134">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="b8f6d-135">`Model` オブジェクトは厳密に型指定されているので (`IEnumerable<Movie>` オブジェクトとして)、ループ内の各項目は `Movie` として型指定されます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-135">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="b8f6d-136">それ以外の利点としては、コンパイル時にコードのチェックが行われます。</span><span class="sxs-lookup"><span data-stu-id="b8f6d-136">Among other benefits, this means that you get compile-time checking of the code:</span></span>
