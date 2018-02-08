[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="ba007-101">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="ba007-101">The preceding code:</span></span>

* <span data-ttu-id="ba007-102">空のコントローラー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="ba007-102">Defines an empty controller class.</span></span> <span data-ttu-id="ba007-103">次のセクションでは、API を実装するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba007-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="ba007-104">コンストラクターでは、[依存性の注入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`TodoContext `) がコントローラーに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="ba007-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="ba007-105">データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。</span><span class="sxs-lookup"><span data-stu-id="ba007-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="ba007-106">アイテムが存在しない場合は、コンストラクターがメモリ内データベースにアイテムを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba007-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="ba007-107">To Do アイテムの取得</span><span class="sxs-lookup"><span data-stu-id="ba007-107">Getting to-do items</span></span>

<span data-ttu-id="ba007-108">To Do アイテムを取得するには、`TodoController` クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ba007-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="ba007-109">これらのメソッドでは、以下の 2 つの GET メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="ba007-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="ba007-110">`GetAll` メソッドの HTTP 応答例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="ba007-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="ba007-111">[Postman](https://www.getpostman.com/) または [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) を使用して HTTP 応答を表示する方法については、後で説明します。</span><span class="sxs-lookup"><span data-stu-id="ba007-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="ba007-112">ルーティングと URL パス</span><span class="sxs-lookup"><span data-stu-id="ba007-112">Routing and URL paths</span></span>

<span data-ttu-id="ba007-113">`[HttpGet]` 属性では HTTP GET メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="ba007-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="ba007-114">各メソッドの URL パスは次のように構成されます。</span><span class="sxs-lookup"><span data-stu-id="ba007-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="ba007-115">コントローラーの `Route` 属性でテンプレート文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="ba007-115">Take the template string in the controller's `Route` attribute:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="ba007-116">`[controller]` をコントローラーの名前 ("Controller" サフィックスを除くコントローラー クラス名) に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ba007-116">Replaces `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="ba007-117">このサンプルでは、コントローラー クラス名は **Todo**Controller で、ルート名は "todo" です。</span><span class="sxs-lookup"><span data-stu-id="ba007-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="ba007-118">ASP.NET Core の[ルーティング](xref:mvc/controllers/routing)では大文字と小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="ba007-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="ba007-119">`[HttpGet]` 属性にルート テンプレート (`[HttpGet("/products")]` など) がある場合は、それをパスに追加します。</span><span class="sxs-lookup"><span data-stu-id="ba007-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="ba007-120">このサンプルではテンプレートを使用しません。</span><span class="sxs-lookup"><span data-stu-id="ba007-120">This sample doesn't use a template.</span></span> <span data-ttu-id="ba007-121">詳細については、「[Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)」 (Http[Verb] 属性を使用する属性のルーティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ba007-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="ba007-122">`GetById` メソッドの場合:</span><span class="sxs-lookup"><span data-stu-id="ba007-122">In the `GetById` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="ba007-123">`"{id}"` は、`todo` アイテムの ID に対するプレースホルダー変数です。</span><span class="sxs-lookup"><span data-stu-id="ba007-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="ba007-124">`GetById` が呼び出されると、メソッドの `id` パラメーターに URL の "{id}" の値が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="ba007-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="ba007-125">`Name = "GetTodo"` により名前付きのルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="ba007-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="ba007-126">名前付きのルートの役割は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ba007-126">Named routes:</span></span>

* <span data-ttu-id="ba007-127">アプリで、ルート名を使用して HTTP リンクを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ba007-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="ba007-128">説明は後ほど行います。</span><span class="sxs-lookup"><span data-stu-id="ba007-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="ba007-129">戻り値</span><span class="sxs-lookup"><span data-stu-id="ba007-129">Return values</span></span>

<span data-ttu-id="ba007-130">`GetAll` メソッドは `IEnumerable` を返します。</span><span class="sxs-lookup"><span data-stu-id="ba007-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="ba007-131">MVC は自動的にオブジェクトを [JSON](http://www.json.org/) にシリアル化して、応答メッセージの本文に JSON を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="ba007-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="ba007-132">このメソッドの応答コードは 200 で、ハンドルされない例外がないものと想定します </span><span class="sxs-lookup"><span data-stu-id="ba007-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="ba007-133">(ハンドルされない例外は 5xx エラーに変換されます)。</span><span class="sxs-lookup"><span data-stu-id="ba007-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="ba007-134">これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、より一般的な `IActionResult` 型を返します。</span><span class="sxs-lookup"><span data-stu-id="ba007-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="ba007-135">`GetById` には、次の 2 つの異なる戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="ba007-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="ba007-136">要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="ba007-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="ba007-137">戻り値が `NotFound` の場合、HTTP 404 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="ba007-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="ba007-138">それ以外の場合、メソッドは JSON 応答本文で 200 を返します。</span><span class="sxs-lookup"><span data-stu-id="ba007-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="ba007-139">戻り値が `ObjectResult` の場合、HTTP 200 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="ba007-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
