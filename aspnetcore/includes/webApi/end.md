## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="3d281-101">その他の CRUD 操作の実装</span><span class="sxs-lookup"><span data-stu-id="3d281-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="3d281-102">次のセクションでは、`Create`、`Update`、`Delete` の各メソッドをコントローラーに追加します。</span><span class="sxs-lookup"><span data-stu-id="3d281-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="3d281-103">作成</span><span class="sxs-lookup"><span data-stu-id="3d281-103">Create</span></span>

<span data-ttu-id="3d281-104">次の `Create` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="3d281-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d281-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="3d281-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="3d281-106">上記のコードは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されるように、HTTP POST メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3d281-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="3d281-107">MVC は、[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="3d281-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d281-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="3d281-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="3d281-109">上記のコードは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されるように、HTTP POST メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3d281-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="3d281-110">MVC は、HTTP 要求の本文から to-do 項目の値を取得します。</span><span class="sxs-lookup"><span data-stu-id="3d281-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="3d281-111">`CreatedAtRoute` メソッド:</span><span class="sxs-lookup"><span data-stu-id="3d281-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="3d281-112">201 応答を返します。</span><span class="sxs-lookup"><span data-stu-id="3d281-112">Returns a 201 response.</span></span> <span data-ttu-id="3d281-113">HTTP 201 は、サーバーに新しいリソースを作成する HTTP POST メソッドに対する標準の応答です。</span><span class="sxs-lookup"><span data-stu-id="3d281-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="3d281-114">応答に場所ヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3d281-114">Adds a Location header to the response.</span></span> <span data-ttu-id="3d281-115">場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="3d281-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="3d281-116">「[10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3d281-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="3d281-117">"GetTodo" という名前のルートを使用して、URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="3d281-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="3d281-118">"GetTodo" という名前のルートは `GetById` で定義します。</span><span class="sxs-lookup"><span data-stu-id="3d281-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d281-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="3d281-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d281-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="3d281-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="3d281-121">Postman を使用した作成要求の送信</span><span class="sxs-lookup"><span data-stu-id="3d281-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="3d281-122">アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="3d281-122">Start the app.</span></span>
* <span data-ttu-id="3d281-123">Postman を開きます。</span><span class="sxs-lookup"><span data-stu-id="3d281-123">Open Postman.</span></span>

![Postman コンソール](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="3d281-125">localhost URL のポート番号を更新します。</span><span class="sxs-lookup"><span data-stu-id="3d281-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="3d281-126">HTTP メソッドを *POST* に設定します。</span><span class="sxs-lookup"><span data-stu-id="3d281-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="3d281-127">**[Body]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d281-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="3d281-128">**[raw]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="3d281-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="3d281-129">型を *[JSON (application/json)]* に設定します。</span><span class="sxs-lookup"><span data-stu-id="3d281-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="3d281-130">次の JSON のような、to-do 項目を含む要求本文を入力します。</span><span class="sxs-lookup"><span data-stu-id="3d281-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="3d281-131">**[Send]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d281-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="3d281-132">**[Send]** をクリックしても応答が表示されない場合、**[SSL certification verification]** オプションを無効にします。</span><span class="sxs-lookup"><span data-stu-id="3d281-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="3d281-133">これは、**[File]**、**[Settings]** の下にあります。</span><span class="sxs-lookup"><span data-stu-id="3d281-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="3d281-134">設定を無効にした後にもう一度 **[Send]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d281-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="3d281-135">次のように、**[Response]** ウィンドウの **[Headers]** タブをクリックして、**[Location]** ヘッダー値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="3d281-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman コンソールの [Headers] タブ](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="3d281-137">場所ヘッダー URI を使用して新しいアイテムにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3d281-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="3d281-138">更新</span><span class="sxs-lookup"><span data-stu-id="3d281-138">Update</span></span>

<span data-ttu-id="3d281-139">次の `Update` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="3d281-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d281-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="3d281-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d281-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="3d281-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="3d281-142">`Update` は `Create` と似ていますが、HTTP PUT を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d281-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="3d281-143">応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="3d281-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="3d281-144">HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。</span><span class="sxs-lookup"><span data-stu-id="3d281-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="3d281-145">部分的な更新をサポートするには、HTTP PATCH を使用します。</span><span class="sxs-lookup"><span data-stu-id="3d281-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="3d281-146">Postman を使用し、to-do 項目の名前を "walk cat" に更新します。</span><span class="sxs-lookup"><span data-stu-id="3d281-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="3d281-148">削除</span><span class="sxs-lookup"><span data-stu-id="3d281-148">Delete</span></span>

<span data-ttu-id="3d281-149">次の `Delete` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="3d281-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="3d281-150">`Delete` の応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="3d281-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="3d281-151">Postman を使用し、to-do 項目を削除します。</span><span class="sxs-lookup"><span data-stu-id="3d281-151">Use Postman to delete the to-do item:</span></span>

![204 (No Content) の応答を示す Postman コンソール](../../tutorials/first-web-api/_static/pmd.png)
