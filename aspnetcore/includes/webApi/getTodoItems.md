::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="73e9c-101">上記のコードでは、メソッドを使用せず、API コントローラー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="73e9c-102">次のセクションでは、API を実装するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="73e9c-103">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-103">The preceding code:</span></span>

* <span data-ttu-id="73e9c-104">メソッドを使用せず、API コントローラー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-104">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="73e9c-105">`TodoItems` が空の場合は、新しい Todo アイテムを作成します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-105">Creates a new Todo item when `TodoItems` is empty.</span></span> <span data-ttu-id="73e9c-106">`TodoItems` が空の場合はコンストラクターが新しいアイテムを作成するため、すべての Todo アイテムを削除することはできません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-106">You won't be able to delete all the Todo items because the constructor creates a new one if `TodoItems` is empty.</span></span>

<span data-ttu-id="73e9c-107">次のセクションでは、API を実装するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-107">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="73e9c-108">いくつかの便利な機能を有効にするには、`[ApiController]` 属性でクラスに注釈を付けます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-108">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="73e9c-109">属性によって有効にする機能の詳細については、「[ApiControllerAttribute でクラスに注釈を付ける](xref:web-api/index#annotate-class-with-apicontrollerattribute)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73e9c-109">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="73e9c-110">コントローラーのコンストラクターでは、[依存性の挿入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`TodoContext`) がコントローラーに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="73e9c-110">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="73e9c-111">データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-111">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="73e9c-112">アイテムが存在しない場合は、コンストラクターがメモリ内データベースにアイテムを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-112">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="73e9c-113">To Do アイテムの取得</span><span class="sxs-lookup"><span data-stu-id="73e9c-113">Get to-do items</span></span>

<span data-ttu-id="73e9c-114">To Do アイテムを取得するには、`TodoController` クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-114">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="73e9c-115">これらのメソッドでは、以下の 2 つの GET メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-115">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="73e9c-116">`GetAll` メソッドの HTTP 応答例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-116">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="73e9c-117">[Postman](https://www.getpostman.com/) または [curl](https://curl.haxx.se/docs/manpage.html) を使用して HTTP 応答を表示する方法については、後で説明します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-117">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="73e9c-118">ルーティングと URL パス</span><span class="sxs-lookup"><span data-stu-id="73e9c-118">Routing and URL paths</span></span>

<span data-ttu-id="73e9c-119">`[HttpGet]` 属性は、HTTP GET 要求に応答するメソッドを表します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-119">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="73e9c-120">各メソッドの URL パスは次のように構成されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-120">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="73e9c-121">コントローラーの `Route` 属性でテンプレート文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-121">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="73e9c-122">`[controller]` をコントローラーの名前 ("Controller" サフィックスを除くコントローラー クラス名) に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-122">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="73e9c-123">このサンプルでは、コントローラー クラス名は **Todo**Controller で、ルート名は "todo" です。</span><span class="sxs-lookup"><span data-stu-id="73e9c-123">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="73e9c-124">ASP.NET Core の[ルーティング](xref:mvc/controllers/routing)では、大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-124">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="73e9c-125">`[HttpGet]` 属性にルート テンプレート (`[HttpGet("/products")]` など) がある場合は、それをパスに追加します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-125">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="73e9c-126">このサンプルではテンプレートを使用しません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-126">This sample doesn't use a template.</span></span> <span data-ttu-id="73e9c-127">詳細については、「[Http[Verb] 属性を使用する属性ルーティング](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="73e9c-127">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="73e9c-128">次の `GetById` メソッドで、`"{id}"` は To Do アイテムの一意識別子に使用するプレースホルダーの変数です。</span><span class="sxs-lookup"><span data-stu-id="73e9c-128">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="73e9c-129">`GetById` が呼び出されると、メソッドの `id` パラメーターに URL の `"{id}"` の値が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-129">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="73e9c-130">`Name = "GetTodo"` により名前付きのルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-130">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="73e9c-131">名前付きのルートの役割は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="73e9c-131">Named routes:</span></span>

* <span data-ttu-id="73e9c-132">アプリで、ルート名を使用して HTTP リンクを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="73e9c-132">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="73e9c-133">説明は後ほど行います。</span><span class="sxs-lookup"><span data-stu-id="73e9c-133">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="73e9c-134">戻り値</span><span class="sxs-lookup"><span data-stu-id="73e9c-134">Return values</span></span>

<span data-ttu-id="73e9c-135">`GetAll` メソッドは、`TodoItem` オブジェクトのコレクションを返します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-135">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="73e9c-136">MVC は自動的にオブジェクトを [JSON](https://www.json.org/) にシリアル化して、応答メッセージの本文に JSON を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-136">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="73e9c-137">このメソッドの応答コードは 200 で、ハンドルされない例外がないものと想定します </span><span class="sxs-lookup"><span data-stu-id="73e9c-137">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="73e9c-138">ハンドルされない例外は 5xx エラーに変換されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-138">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="73e9c-139">これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、より一般的な [IActionResult type](xref:web-api/action-return-types#iactionresult-type) 型を返します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-139">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="73e9c-140">`GetById` には、次の 2 つの異なる戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="73e9c-140">`GetById` has two different return types:</span></span>

* <span data-ttu-id="73e9c-141">要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-141">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="73e9c-142">戻り値が [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) の場合、HTTP 404 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-142">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="73e9c-143">それ以外の場合、メソッドは JSON 応答本文で 200 を返します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-143">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="73e9c-144">戻り値が [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) の場合、HTTP 200 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-144">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="73e9c-145">これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、[ActionResult\<T> 型](xref:web-api/action-return-types#actionresultt-type)を返します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-145">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="73e9c-146">`GetById` には、次の 2 つの異なる戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="73e9c-146">`GetById` has two different return types:</span></span>

* <span data-ttu-id="73e9c-147">要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-147">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="73e9c-148">戻り値が [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) の場合、HTTP 404 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-148">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="73e9c-149">それ以外の場合、メソッドは JSON 応答本文で 200 を返します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-149">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="73e9c-150">戻り値が `item` の場合、HTTP 200 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-150">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
