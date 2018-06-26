::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="f3042-101">上記のコードでは、メソッドを使用せず、API コントローラー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="f3042-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="f3042-102">次のセクションでは、API を実装するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f3042-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="f3042-103">上記のコードでは、メソッドを使用せず、API コントローラー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="f3042-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="f3042-104">次のセクションでは、API を実装するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f3042-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="f3042-105">いくつかの便利な機能を有効にするには、`[ApiController]` 属性でクラスに注釈を付けます。</span><span class="sxs-lookup"><span data-stu-id="f3042-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="f3042-106">属性によって有効にする機能の詳細については、「[ApiControllerAttribute でクラスに注釈を付ける](xref:web-api/index#annotate-class-with-apicontrollerattribute)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f3042-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="f3042-107">コントローラーのコンストラクターでは、[依存性の挿入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`TodoContext`) がコントローラーに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="f3042-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f3042-108">データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。</span><span class="sxs-lookup"><span data-stu-id="f3042-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="f3042-109">アイテムが存在しない場合は、コンストラクターがメモリ内データベースにアイテムを追加します。</span><span class="sxs-lookup"><span data-stu-id="f3042-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="f3042-110">To Do アイテムの取得</span><span class="sxs-lookup"><span data-stu-id="f3042-110">Get to-do items</span></span>

<span data-ttu-id="f3042-111">To Do アイテムを取得するには、`TodoController` クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f3042-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="f3042-112">これらのメソッドでは、以下の 2 つの GET メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="f3042-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="f3042-113">`GetAll` メソッドの HTTP 応答例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="f3042-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="f3042-114">[Postman](https://www.getpostman.com/) または [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) を使用して HTTP 応答を表示する方法については、後で説明します。</span><span class="sxs-lookup"><span data-stu-id="f3042-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="f3042-115">ルーティングと URL パス</span><span class="sxs-lookup"><span data-stu-id="f3042-115">Routing and URL paths</span></span>

<span data-ttu-id="f3042-116">`[HttpGet]` 属性は、HTTP GET 要求に応答するメソッドを表します。</span><span class="sxs-lookup"><span data-stu-id="f3042-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f3042-117">各メソッドの URL パスは次のように構成されます。</span><span class="sxs-lookup"><span data-stu-id="f3042-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f3042-118">コントローラーの `Route` 属性でテンプレート文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="f3042-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="f3042-119">`[controller]` をコントローラーの名前 ("Controller" サフィックスを除くコントローラー クラス名) に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f3042-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f3042-120">このサンプルでは、コントローラー クラス名は **Todo**Controller で、ルート名は "todo" です。</span><span class="sxs-lookup"><span data-stu-id="f3042-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="f3042-121">ASP.NET Core の[ルーティング](xref:mvc/controllers/routing)では、大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="f3042-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f3042-122">`[HttpGet]` 属性にルート テンプレート (`[HttpGet("/products")]` など) がある場合は、それをパスに追加します。</span><span class="sxs-lookup"><span data-stu-id="f3042-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="f3042-123">このサンプルではテンプレートを使用しません。</span><span class="sxs-lookup"><span data-stu-id="f3042-123">This sample doesn't use a template.</span></span> <span data-ttu-id="f3042-124">詳細については、「[Http[Verb] 属性を使用する属性ルーティング](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f3042-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f3042-125">次の `GetById` メソッドで、`"{id}"` は To Do アイテムの一意識別子に使用するプレースホルダーの変数です。</span><span class="sxs-lookup"><span data-stu-id="f3042-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f3042-126">`GetById` が呼び出されると、メソッドの `id` パラメーターに URL の `"{id}"` の値が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="f3042-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="f3042-127">`Name = "GetTodo"` により名前付きのルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="f3042-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="f3042-128">名前付きのルートの役割は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f3042-128">Named routes:</span></span>

* <span data-ttu-id="f3042-129">アプリで、ルート名を使用して HTTP リンクを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f3042-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="f3042-130">説明は後ほど行います。</span><span class="sxs-lookup"><span data-stu-id="f3042-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="f3042-131">戻り値</span><span class="sxs-lookup"><span data-stu-id="f3042-131">Return values</span></span>

<span data-ttu-id="f3042-132">`GetAll` メソッドは、`TodoItem` オブジェクトのコレクションを返します。</span><span class="sxs-lookup"><span data-stu-id="f3042-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="f3042-133">MVC は自動的にオブジェクトを [JSON](https://www.json.org/) にシリアル化して、応答メッセージの本文に JSON を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="f3042-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f3042-134">このメソッドの応答コードは 200 で、ハンドルされない例外がないものと想定します </span><span class="sxs-lookup"><span data-stu-id="f3042-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f3042-135">ハンドルされない例外は 5xx エラーに変換されます。</span><span class="sxs-lookup"><span data-stu-id="f3042-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f3042-136">これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、より一般的な [IActionResult type](xref:web-api/action-return-types#iactionresult-type) 型を返します。</span><span class="sxs-lookup"><span data-stu-id="f3042-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="f3042-137">`GetById` には、次の 2 つの異なる戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="f3042-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="f3042-138">要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="f3042-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="f3042-139">戻り値が [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) の場合、HTTP 404 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="f3042-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="f3042-140">それ以外の場合、メソッドは JSON 応答本文で 200 を返します。</span><span class="sxs-lookup"><span data-stu-id="f3042-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f3042-141">戻り値が [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) の場合、HTTP 200 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="f3042-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f3042-142">これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、[ActionResult\<T> 型](xref:web-api/action-return-types#actionresultt-type)を返します。</span><span class="sxs-lookup"><span data-stu-id="f3042-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="f3042-143">`GetById` には、次の 2 つの異なる戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="f3042-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="f3042-144">要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="f3042-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="f3042-145">戻り値が [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) の場合、HTTP 404 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="f3042-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="f3042-146">それ以外の場合、メソッドは JSON 応答本文で 200 を返します。</span><span class="sxs-lookup"><span data-stu-id="f3042-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f3042-147">戻り値が `item` の場合、HTTP 200 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="f3042-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end