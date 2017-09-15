<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="73e9c-101">上記の `Index` メソッド:</span><span class="sxs-lookup"><span data-stu-id="73e9c-101">The previous `Index` method:</span></span>

<span data-ttu-id="73e9c-102">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]</span><span class="sxs-lookup"><span data-stu-id="73e9c-102">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]</span></span>

<span data-ttu-id="73e9c-103">`id` パラメーターで更新された `Index` メソッド:</span><span class="sxs-lookup"><span data-stu-id="73e9c-103">The updated `Index` method with `id` parameter:</span></span>

<span data-ttu-id="73e9c-104">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]</span><span class="sxs-lookup"><span data-stu-id="73e9c-104">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]</span></span>

<span data-ttu-id="73e9c-105">これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-105">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![ghost という単語が URL に追加された索引ビュー。Ghostbusters と Ghostbusters 2 という 2 本のムービーからなるムービーリストが返されています。](../../tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="73e9c-107">ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-107">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="73e9c-108">そのため、ここでは UI を追加して、ムービーをフィルターできるようにします。</span><span class="sxs-lookup"><span data-stu-id="73e9c-108">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="73e9c-109">ルート バインドされた `ID` パラメーターを渡す方法をテストするために `Index` メソッドの署名を変更した場合は、`searchString` という名前のパラメーターを受け取るように署名を元に戻します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-109">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

<span data-ttu-id="73e9c-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]</span><span class="sxs-lookup"><span data-stu-id="73e9c-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]</span></span>

<span data-ttu-id="73e9c-111">*Views/Movies/Index.cshtml* ファイルを開き、以下の強調表示されている `<form>` マークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-111">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

<span data-ttu-id="73e9c-112">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]</span><span class="sxs-lookup"><span data-stu-id="73e9c-112">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]</span></span>

<span data-ttu-id="73e9c-113">HTML `<form>` タグでは[フォーム タグ ヘルパー](../../mvc/views/working-with-forms.md)が使用されるため、フォームを送信するときに、フィルター文字列がムービー コントローラーの `Index` アクションに投稿されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-113">The HTML `<form>` tag uses the [Form Tag Helper](../../mvc/views/working-with-forms.md), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="73e9c-114">変更内容を保存してから、フィルターをテストします。</span><span class="sxs-lookup"><span data-stu-id="73e9c-114">Save your changes and then test the filter.</span></span>

![タイトル フィルター テキストボックスに ghost という単語が入力されたインデックス ビュー](../../tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="73e9c-116">予想どおり、`Index` メソッドの `[HttpPost]` オーバーロードはありません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-116">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="73e9c-117">メソッドではデータをフィルターするだけで、アプリの状態を変更しないため、オーバーロードは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-117">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="73e9c-118">以下の `[HttpPost] Index` メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-118">You could add the following `[HttpPost] Index` method.</span></span>

<span data-ttu-id="73e9c-119">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]</span><span class="sxs-lookup"><span data-stu-id="73e9c-119">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]</span></span>

<span data-ttu-id="73e9c-120">`notUsed` パラメーターは、`Index` メソッドのオーバーロードを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-120">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="73e9c-121">これについては、チュートリアルの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-121">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="73e9c-122">このメソッドを追加すると、アクション呼び出し元が `[HttpPost] Index` メソッドと一致し、`[HttpPost] Index` メソッドが以下のイメージのように実行されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-122">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![From HttpPost Index: filter on ghost というアプリケーション応答を示すブラウザー ウィンドウ](../../tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="73e9c-124">ただし、この `[HttpPost]` バージョンの `Index` メソッドを追加しても、実装方法は制限されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-124">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="73e9c-125">たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="73e9c-125">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="73e9c-126">HTTP POST 要求の URL は、GET 要求の URL (localhost:xxxxx/Movies/Index) と同じであり、URL には検索情報がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="73e9c-126">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="73e9c-127">検索文字列情報は、[フォーム フィールド値](https://developer.mozilla.org/docs/Web/Guide/HTML/Forms/Sending_and_retrieving_form_data)としてサーバーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-127">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Web/Guide/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="73e9c-128">ブラウザーの開発者ツールまたは優れた [Fiddler ツール](http://www.telerik.com/fiddler)を使用して、これを確認できます。</span><span class="sxs-lookup"><span data-stu-id="73e9c-128">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="73e9c-129">次のイメージは、Chrome ブラウザーの開発者ツールを示しています。</span><span class="sxs-lookup"><span data-stu-id="73e9c-129">The image below shows the Chrome browser Developer tools:</span></span>

![searchString 値が ghost の要求本文を示す、Microsoft Edge の開発者ツールの [ネットワーク] タブ](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="73e9c-131">要求本文に検索パラメーターと [XSRF](../../security/anti-request-forgery.md) トークンが表示されています。</span><span class="sxs-lookup"><span data-stu-id="73e9c-131">You can see the search parameter and [XSRF](../../security/anti-request-forgery.md) token in the request body.</span></span> <span data-ttu-id="73e9c-132">前のチュートリアルで説明したように、[フォーム タグ ヘルパー](../../mvc/views/working-with-forms.md)では [XSRF](../../security/anti-request-forgery.md) 偽造防止トークンが生成されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="73e9c-132">Note, as mentioned in the previous tutorial, the [Form Tag Helper](../../mvc/views/working-with-forms.md) generates an [XSRF](../../security/anti-request-forgery.md) anti-forgery token.</span></span> <span data-ttu-id="73e9c-133">ここではデータを変更しないため、コントローラー メソッドでトークンを検証する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-133">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="73e9c-134">検索パラメーターが URL ではなく、要求本文にあるため、その検索情報をキャプチャして、ブックマークしたり、他のユーザーと共有したりすることはできません。</span><span class="sxs-lookup"><span data-stu-id="73e9c-134">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="73e9c-135">これを解決するために、`HTTP GET` 要求を指定します。</span><span class="sxs-lookup"><span data-stu-id="73e9c-135">We'll fix this by specifying the request should be `HTTP GET`.</span></span>