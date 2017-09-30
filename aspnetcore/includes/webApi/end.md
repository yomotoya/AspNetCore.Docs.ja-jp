## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="018a6-101">その他の CRUD 操作の実装</span><span class="sxs-lookup"><span data-stu-id="018a6-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="018a6-102">コントローラーに、`Create`、`Update`、および `Delete` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="018a6-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="018a6-103">これらはテーマのバリエーションなので、単にコードを示し、主な違いを強調します。</span><span class="sxs-lookup"><span data-stu-id="018a6-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="018a6-104">プロジェクトは、コードの追加または変更後にビルドします。</span><span class="sxs-lookup"><span data-stu-id="018a6-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="018a6-105">作成</span><span class="sxs-lookup"><span data-stu-id="018a6-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="018a6-106">これは、HTTP POST メソッドです。[`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="018a6-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="018a6-107">MVC は、[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="018a6-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="018a6-108">`CreatedAtRoute` メソッドは、サーバーに新しいリソースを作成する HTTP POST メソッドの標準の応答である 201 の応答を返します。</span><span class="sxs-lookup"><span data-stu-id="018a6-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="018a6-109">`CreatedAtRoute` では、応答に場所ヘッダーも追加されます。</span><span class="sxs-lookup"><span data-stu-id="018a6-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="018a6-110">場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="018a6-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="018a6-111">「[10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="018a6-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="018a6-112">Postman を使用した作成要求の送信</span><span class="sxs-lookup"><span data-stu-id="018a6-112">Use Postman to send a Create request</span></span>

![Postman コンソール](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="018a6-114">HTTP メソッド名を `POST` に設定します。</span><span class="sxs-lookup"><span data-stu-id="018a6-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="018a6-115">**[Body]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="018a6-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="018a6-116">**[raw]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="018a6-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="018a6-117">型を JSON に設定します。</span><span class="sxs-lookup"><span data-stu-id="018a6-117">Set the type to JSON</span></span>
* <span data-ttu-id="018a6-118">キー/値エディターで次のような To do 項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="018a6-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="018a6-119">**[Send]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="018a6-119">Select **Send**</span></span>

* <span data-ttu-id="018a6-120">次のように、下のウィンドウの [Headers] タブを選択して、**[Location]** ヘッダーをコピーします。</span><span class="sxs-lookup"><span data-stu-id="018a6-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman コンソールの [Headers] タブ](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="018a6-122">[Location] ヘッダーの URI を使用して、作成したリソースにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="018a6-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="018a6-123">`GetById` メソッドによって、`"GetTodo"` という名前のルートが作成されたことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="018a6-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="018a6-124">更新</span><span class="sxs-lookup"><span data-stu-id="018a6-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="018a6-125">`Update` は `Create` と似ていますが、HTTP PUT を使用します。</span><span class="sxs-lookup"><span data-stu-id="018a6-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="018a6-126">応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="018a6-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="018a6-127">HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。</span><span class="sxs-lookup"><span data-stu-id="018a6-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="018a6-128">部分的な更新をサポートするには、HTTP PATCH を使用します。</span><span class="sxs-lookup"><span data-stu-id="018a6-128">To support partial updates, use HTTP PATCH.</span></span>

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="018a6-130">削除</span><span class="sxs-lookup"><span data-stu-id="018a6-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="018a6-131">応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="018a6-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmd.png)
