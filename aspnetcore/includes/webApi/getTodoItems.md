<span data-ttu-id="5bf35-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="5bf35-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="5bf35-102">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-102">The preceding code:</span></span>

* <span data-ttu-id="5bf35-103">空のコントローラー クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-103">Defines an empty controller class.</span></span> <span data-ttu-id="5bf35-104">次のセクションでは、API を実装するメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="5bf35-105">コンストラクターでは、[依存性の注入](xref:fundamentals/dependency-injection)を使ってデータベース コンテキスト (`TodoContext `) がコントローラーに挿入されています。</span><span class="sxs-lookup"><span data-stu-id="5bf35-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="5bf35-106">データベース コンテキストは、コントローラーの各 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) メソッドで使用されます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="5bf35-107">アイテムが存在しない場合は、コンストラクターがメモリ内データベースにアイテムを追加します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="5bf35-108">To Do アイテムの取得</span><span class="sxs-lookup"><span data-stu-id="5bf35-108">Getting to-do items</span></span>

<span data-ttu-id="5bf35-109">To Do アイテムを取得するには、`TodoController` クラスに次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="5bf35-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="5bf35-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="5bf35-111">これらのメソッドでは、以下の 2 つの GET メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="5bf35-112">`GetAll` メソッドの HTTP 応答例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="5bf35-113">[Postman](https://www.getpostman.com/) または [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) を使用して HTTP 応答を表示する方法については、後で説明します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="5bf35-114">ルーティングと URL パス</span><span class="sxs-lookup"><span data-stu-id="5bf35-114">Routing and URL paths</span></span>

<span data-ttu-id="5bf35-115">`[HttpGet]` 属性では HTTP GET メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="5bf35-116">各メソッドの URL パスは次のように構成されます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="5bf35-117">コントローラーのルート属性でテンプレート文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="5bf35-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="5bf35-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="5bf35-119">"[Controller]" をコントローラーの名前 ("Controller" サフィックスを除くコントローラー名) に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="5bf35-120">このサンプルでは、コントローラー クラス名は **Todo**Controller で、ルート名は "todo" です。</span><span class="sxs-lookup"><span data-stu-id="5bf35-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="5bf35-121">ASP.NET Core の[ルーティング](xref:mvc/controllers/routing)では大文字と小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="5bf35-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="5bf35-122">`[HttpGet]` 属性にルート テンプレート (`[HttpGet("/products")]` など) がある場合は、それをパスに追加します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="5bf35-123">このサンプルではテンプレートを使用しません。</span><span class="sxs-lookup"><span data-stu-id="5bf35-123">This sample doesn't use a template.</span></span> <span data-ttu-id="5bf35-124">詳細については、「[Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)」 (Http[Verb] 属性を使用する属性のルーティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5bf35-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="5bf35-125">`GetById` メソッドの場合:</span><span class="sxs-lookup"><span data-stu-id="5bf35-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="5bf35-126">`"{id}"` は、`todo` アイテムの ID に対するプレースホルダー変数です。</span><span class="sxs-lookup"><span data-stu-id="5bf35-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="5bf35-127">`GetById` が呼び出されると、メソッドの `id` パラメーターに URL の "{id}" の値が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="5bf35-128">`Name = "GetTodo"` は名前付きルートを作成し、HTTP 応答でこのルートにリンクできるようにします。</span><span class="sxs-lookup"><span data-stu-id="5bf35-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="5bf35-129">これについては、後で例を使用して説明します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-129">I'll explain it with an example later.</span></span> <span data-ttu-id="5bf35-130">詳細については、「[コントローラー アクションへのルーティング](xref:mvc/controllers/routing)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5bf35-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="5bf35-131">戻り値</span><span class="sxs-lookup"><span data-stu-id="5bf35-131">Return values</span></span>

<span data-ttu-id="5bf35-132">`GetAll` メソッドは `IEnumerable` を返します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="5bf35-133">MVC は自動的にオブジェクトを [JSON](http://www.json.org/) にシリアル化して、応答メッセージの本文に JSON を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="5bf35-134">このメソッドの応答コードは 200 で、ハンドルされない例外がないものと想定します </span><span class="sxs-lookup"><span data-stu-id="5bf35-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="5bf35-135">(ハンドルされない例外は 5xx エラーに変換されます)。</span><span class="sxs-lookup"><span data-stu-id="5bf35-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="5bf35-136">これに対し、`GetById` メソッドは、広範囲の戻り値の型を表す、より一般的な `IActionResult` 型を返します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="5bf35-137">`GetById` には、次の 2 つの異なる戻り値の型があります。</span><span class="sxs-lookup"><span data-stu-id="5bf35-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="5bf35-138">要求された ID と一致するアイテムがない場合、メソッドは 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="5bf35-139">これは、`NotFound` を返すことによって行われます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="5bf35-140">それ以外の場合、メソッドは JSON 応答本文で 200 を返します。</span><span class="sxs-lookup"><span data-stu-id="5bf35-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="5bf35-141">これは、`ObjectResult` を返すことによって行われます。</span><span class="sxs-lookup"><span data-stu-id="5bf35-141">This is done by returning an `ObjectResult`</span></span>
