## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="c6a93-101">その他の CRUD 操作の実装</span><span class="sxs-lookup"><span data-stu-id="c6a93-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="c6a93-102">次のセクションでは、`Create`、`Update`、`Delete` の各メソッドをコントローラーに追加します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="c6a93-103">作成</span><span class="sxs-lookup"><span data-stu-id="c6a93-103">Create</span></span>

<span data-ttu-id="c6a93-104">次の `Create` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="c6a93-105">上記のコードは HTTP POST メソッドです。[`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="c6a93-106">MVC は、[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="c6a93-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="c6a93-107">`CreatedAtRoute` メソッド:</span><span class="sxs-lookup"><span data-stu-id="c6a93-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="c6a93-108">201 応答を返します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-108">Returns a 201 response.</span></span> <span data-ttu-id="c6a93-109">HTTP 201 は、サーバーに新しいリソースを作成する HTTP POST メソッドに対する標準の応答です。</span><span class="sxs-lookup"><span data-stu-id="c6a93-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="c6a93-110">応答に場所ヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-110">Adds a Location header to the response.</span></span> <span data-ttu-id="c6a93-111">場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="c6a93-112">「[10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c6a93-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="c6a93-113">"GetTodo" という名前のルートを使用して、URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="c6a93-114">"GetTodo" という名前のルートは `GetById` で定義します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="c6a93-115">Postman を使用した作成要求の送信</span><span class="sxs-lookup"><span data-stu-id="c6a93-115">Use Postman to send a Create request</span></span>

![Postman コンソール](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="c6a93-117">HTTP メソッド名を `POST` に設定します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="c6a93-118">**[Body]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="c6a93-119">**[raw]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="c6a93-120">型を JSON に設定します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-120">Set the type to JSON</span></span>
* <span data-ttu-id="c6a93-121">キー/値エディターで次のような To do 項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="c6a93-122">**[Send]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-122">Select **Send**</span></span>
* <span data-ttu-id="c6a93-123">次のように、下のウィンドウの [Headers] タブを選択して、**[Location]** ヘッダーをコピーします。</span><span class="sxs-lookup"><span data-stu-id="c6a93-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman コンソールの [Headers] タブ](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="c6a93-125">場所ヘッダー URI を使用して新しいアイテムにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="c6a93-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="c6a93-126">更新</span><span class="sxs-lookup"><span data-stu-id="c6a93-126">Update</span></span>

<span data-ttu-id="c6a93-127">次の `Update` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="c6a93-128">`Update` は `Create` と似ていますが、HTTP PUT を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="c6a93-129">応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="c6a93-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="c6a93-130">HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。</span><span class="sxs-lookup"><span data-stu-id="c6a93-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="c6a93-131">部分的な更新をサポートするには、HTTP PATCH を使用します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-131">To support partial updates, use HTTP PATCH.</span></span>

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="c6a93-133">削除</span><span class="sxs-lookup"><span data-stu-id="c6a93-133">Delete</span></span>

<span data-ttu-id="c6a93-134">次の `Delete` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="c6a93-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="c6a93-135">`Delete` の応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="c6a93-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="c6a93-136">`Delete` をテストします。</span><span class="sxs-lookup"><span data-stu-id="c6a93-136">Test `Delete`:</span></span> 

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmd.png)
