<span data-ttu-id="05769-101">*Views/HelloWorld/Index.cshtml* Razor ビュー ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="05769-101">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index.cshtml)]

<span data-ttu-id="05769-102">`http://localhost:xxxx/HelloWorld` に移動します。</span><span class="sxs-lookup"><span data-stu-id="05769-102">Navigate to `http://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="05769-103">`HelloWorldController` の `Index` メソッドでは多くのことは行いませんでした。つまり、ステートメント `return View();` を実行し、メソッドでビュー テンプレート ファイルを使用して、ブラウザーへの応答をレンダリングするよう指定しただけです。</span><span class="sxs-lookup"><span data-stu-id="05769-103">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="05769-104">ビュー テンプレート ファイルの名前を明示的に指定しなかったため、MVC では既定で */Views/HelloWorld* フォルダー内の *Index.cshtml* ビュー ファイルが使用されました。</span><span class="sxs-lookup"><span data-stu-id="05769-104">Because you didn't explicitly specify the name of the view template file, MVC defaulted to using the *Index.cshtml* view file in the */Views/HelloWorld* folder.</span></span> <span data-ttu-id="05769-105">次のイメージは、ビューにハード コーディングされた "Hello from our View Template!" </span><span class="sxs-lookup"><span data-stu-id="05769-105">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="05769-106">という文字列を示しています。</span><span class="sxs-lookup"><span data-stu-id="05769-106">hard-coded in the view.</span></span>

![ブラウザー ウィンドウ](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

<span data-ttu-id="05769-108">ブラウザー ウィンドウが小さい場合 (モバイル デバイスなどの場合) は、右上にある [Bootstrap のナビゲーション ボタン](http://getbootstrap.com/components/#navbar)を切り替えて (タップして)、**[ホーム]**、**[バージョン情報]**、および **[連絡先]** リンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="05769-108">If your browser window is small (for example on a mobile device), you might need to toggle (tap) the [Bootstrap navigation button](http://getbootstrap.com/components/#navbar) in the upper right to see the **Home**, **About**, and **Contact** links.</span></span>

![Bootstrap のナビゲーション ボタンが強調表示されているブラウザー ウィンドウ](~/tutorials/first-mvc-app/adding-view/_static/1.png)

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="05769-110">ビューとレイアウト ページの変更</span><span class="sxs-lookup"><span data-stu-id="05769-110">Changing views and layout pages</span></span>

<span data-ttu-id="05769-111">メニューのリンク (**[MvcMovie]**、**[ホーム]**、**[バージョン情報]**) をタップします。</span><span class="sxs-lookup"><span data-stu-id="05769-111">Tap the menu links (**MvcMovie**, **Home**, **About**).</span></span> <span data-ttu-id="05769-112">各ページには同じメニューのレイアウトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="05769-112">Each page shows the same menu layout.</span></span> <span data-ttu-id="05769-113">メニューのレイアウトは、*Views/Shared/_Layout.cshtml* ファイルに実装されています。</span><span class="sxs-lookup"><span data-stu-id="05769-113">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="05769-114">*Views/Shared/_Layout.cshtml* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="05769-114">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="05769-115">[[レイアウト]](xref:mvc/views/layout) テンプレートでは、1 か所でサイトの HTML コンテナー レイアウトを指定し、それをサイト内の複数のページに適用できます。</span><span class="sxs-lookup"><span data-stu-id="05769-115">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="05769-116">`@RenderBody()` という行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="05769-116">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="05769-117">`RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに*ラップ*されます。</span><span class="sxs-lookup"><span data-stu-id="05769-117">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="05769-118">たとえば、**[バージョン情報]** リンクを選択した場合、`RenderBody` メソッド内で **Views/Home/About.cshtml** ビューがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="05769-118">For example, if you select the **About** link, the **Views/Home/About.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-and-menu-link-in-the-layout-file"></a><span data-ttu-id="05769-119">レイアウト ファイルでのタイトルおよびメニュー リンクの変更</span><span class="sxs-lookup"><span data-stu-id="05769-119">Change the title and menu link in the layout file</span></span>

<span data-ttu-id="05769-120">タイトル要素で、`MvcMovie` を `Movie App` に変更します。</span><span class="sxs-lookup"><span data-stu-id="05769-120">In the title element, change `MvcMovie` to `Movie App`.</span></span> <span data-ttu-id="05769-121">レイアウト テンプレートのアンカー テキストを `MvcMovie` から `Movie App` に変更し、コントローラーを `Home` から `Movies` に変更します。下の強調表示されている箇所をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="05769-121">Change the anchor text in the layout template from `MvcMovie` to `Movie App` and the controller from `Home` to `Movies` as highlighted below:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="05769-122">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]</span><span class="sxs-lookup"><span data-stu-id="05769-122">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="05769-123">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=6,29)]</span><span class="sxs-lookup"><span data-stu-id="05769-123">[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout21.cshtml?highlight=6,29)]</span></span>
::: moniker-end

>[!WARNING]
> <span data-ttu-id="05769-124">`Movies` コントローラーはまだ実装されていないため、そのリンクをクリックすると、404 (見つかりません) エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="05769-124">We haven't implemented the `Movies` controller yet, so if you click on that link, you'll get a 404 (Not found) error.</span></span>

<span data-ttu-id="05769-125">変更内容を保存して、**[バージョン情報]** リンクをタップします。</span><span class="sxs-lookup"><span data-stu-id="05769-125">Save your changes and tap the **About** link.</span></span> <span data-ttu-id="05769-126">ここでブラウザー タブのタイトルが、**About - Mvc Movie** ではなく、**About - Movie App** になっていることに注目してください:</span><span class="sxs-lookup"><span data-stu-id="05769-126">Notice how the title on the browser tab now displays **About - Movie App** instead of **About - Mvc Movie**:</span></span> 

![[バージョン情報]タブ](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

<span data-ttu-id="05769-128">**[連絡先]** リンクをタップし、タイトルとアンカー テキストにも **[Movie App]** と表示されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="05769-128">Tap the **Contact** link and notice that the title and anchor text also display **Movie App**.</span></span> <span data-ttu-id="05769-129">レイアウト テンプレートで一度変更しただけで、サイト上のすべてのページに新しいリンク テキストと新しいタイトルが反映できました。</span><span class="sxs-lookup"><span data-stu-id="05769-129">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="05769-130">*Views/_ViewStart.cshtml* ファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="05769-130">Examine the *Views/_ViewStart.cshtml* file:</span></span>


```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="05769-131">*Views/_ViewStart.cshtml* ファイルは *Views/Shared/_Layout.cshtml* ファイルに取り込まれ、各ビューに適用されます。</span><span class="sxs-lookup"><span data-stu-id="05769-131">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="05769-132">`Layout` プロパティを使用すれば、別のレイアウト ビューを設定することができます。また、`null` に設定して、レイアウト ファイルが使用されないようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="05769-132">You can use the `Layout` property to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="05769-133">`Index` ビューのタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="05769-133">Change the title of the `Index` view.</span></span>

<span data-ttu-id="05769-134">*Views/HelloWorld/Index.cshtml* を開きます。</span><span class="sxs-lookup"><span data-stu-id="05769-134">Open *Views/HelloWorld/Index.cshtml*.</span></span> <span data-ttu-id="05769-135">次の 2 か所が変更されています。</span><span class="sxs-lookup"><span data-stu-id="05769-135">There are two places to make a change:</span></span>

   * <span data-ttu-id="05769-136">ブラウザーのタイトルに表示されるテキスト。</span><span class="sxs-lookup"><span data-stu-id="05769-136">The text that appears in the title of the browser.</span></span>
   * <span data-ttu-id="05769-137">セカンダリ ヘッダー (`<h2>` 要素)。</span><span class="sxs-lookup"><span data-stu-id="05769-137">The secondary header (`<h2>` element).</span></span>

<span data-ttu-id="05769-138">これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="05769-138">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>


```HTML
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

<span data-ttu-id="05769-139">上のコードの `ViewData["Title"] = "Movie List";` では、`Title` ディクショナリの `ViewData` プロパティを "Movie List" に設定します。</span><span class="sxs-lookup"><span data-stu-id="05769-139">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="05769-140">`Title` プロパティは、次のように、レイアウト ページの `<title>` HTML 要素で使用されます。</span><span class="sxs-lookup"><span data-stu-id="05769-140">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>


```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="05769-141">変更内容を保存して、`http://localhost:xxxx/HelloWorld` に移動します。</span><span class="sxs-lookup"><span data-stu-id="05769-141">Save your change and navigate to `http://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="05769-142">ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください </span><span class="sxs-lookup"><span data-stu-id="05769-142">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="05769-143">(ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="05769-143">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="05769-144">ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルは、*Index.cshtml* ビュー テンプレートで設定した `ViewData["Title"]` で作成されます。レイアウト ファイルには "- Movie App" が追加されます。</span><span class="sxs-lookup"><span data-stu-id="05769-144">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="05769-145">*Index.cshtml* ビュー テンプレートのコンテンツがどのように *Views/Shared/_Layout.cshtml* ビュー テンプレートにマージされ、1 つの HTML 応答がブラウザーに送信されたかにも注目してください。</span><span class="sxs-lookup"><span data-stu-id="05769-145">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="05769-146">レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。</span><span class="sxs-lookup"><span data-stu-id="05769-146">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="05769-147">詳細については、「[Layout](xref:mvc/views/layout)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="05769-147">To learn more see [Layout](xref:mvc/views/layout).</span></span>

![ムービー リスト ビュー](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="05769-149">ここでは、"データ" のごく一部 (この場合は "Hello from our View Template!" というメッセージ) を</span><span class="sxs-lookup"><span data-stu-id="05769-149">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="05769-150">ハード コーディングしました。</span><span class="sxs-lookup"><span data-stu-id="05769-150">message) is hard-coded, though.</span></span> <span data-ttu-id="05769-151">MVC アプリケーションには "V" (ビュー) があり、"C" (コントローラー) もありますが、"M" (モデル) はまだありません。</span><span class="sxs-lookup"><span data-stu-id="05769-151">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="05769-152">コントローラーからビューへのデータの受け渡し</span><span class="sxs-lookup"><span data-stu-id="05769-152">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="05769-153">コントローラー アクションは、受信 URL 要求への応答として呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="05769-153">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="05769-154">コントローラー クラスでは、受信ブラウザー要求を処理するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="05769-154">A controller class is where you write the code that handles the incoming browser requests.</span></span> <span data-ttu-id="05769-155">コントローラーはデータ ソースからデータを取得し、ブラウザーに返す応答の種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="05769-155">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="05769-156">ビュー テンプレートを使用すれば、コントローラーからブラウザーへの HTML 応答を生成して書式を設定できます。</span><span class="sxs-lookup"><span data-stu-id="05769-156">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="05769-157">コントローラーは、ビュー テンプレートで応答をレンダリングするために必要なデータを提供します。</span><span class="sxs-lookup"><span data-stu-id="05769-157">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="05769-158">ベスト プラクティス: ビュー テンプレートでビジネス ロジックを実行したり、データベースと直接対話したり**しない**でください。</span><span class="sxs-lookup"><span data-stu-id="05769-158">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="05769-159">ビュー テンプレートでは、コントローラーによって提供されるデータのみを処理するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="05769-159">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="05769-160">この "関心の分離" を維持すれば、コードをクリーンでテスト可能な保守しやすい状態に保つことができます。</span><span class="sxs-lookup"><span data-stu-id="05769-160">Maintaining this "separation of concerns" helps keep your code clean, testable, and maintainable.</span></span>

<span data-ttu-id="05769-161">現時点では、`HelloWorldController` クラスの `Welcome` メソッドは `name` と `ID` パラメーターを受け取ってから、ブラウザーに直接値を出力します。</span><span class="sxs-lookup"><span data-stu-id="05769-161">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="05769-162">コントローラーにこの応答を文字列としてレンダリングさせるのではなく、代わりにビュー テンプレートを使用するようにコントローラーを変更します。</span><span class="sxs-lookup"><span data-stu-id="05769-162">Rather than have the controller render this response as a string, change the controller to use a view template instead.</span></span> <span data-ttu-id="05769-163">このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="05769-163">The view template generates a dynamic response, which means that appropriate bits of data must be passed from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="05769-164">そのためには、コントローラーで動的データ (パラメーター) を設定します。これは、ビュー テンプレートでアクセスできるようにするために `ViewData` ディレクトリに必要なデータです。</span><span class="sxs-lookup"><span data-stu-id="05769-164">Do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="05769-165">*HelloWorldController.cs* ファイルに戻り、`Welcome` メソッドを変更して `Message` および `NumTimes` 値を `ViewData` ディクショナリに追加します。</span><span class="sxs-lookup"><span data-stu-id="05769-165">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="05769-166">`ViewData` ディクショナリは動的オブジェクトです。つまり、必要なものを何でも設定できます。`ViewData` オブジェクトは、その内部に何かを設定するまでプロパティは定義されません。</span><span class="sxs-lookup"><span data-stu-id="05769-166">The `ViewData` dictionary is a dynamic object, which means you can put whatever you want in to it; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="05769-167">[MVC のモデル バインド システム](xref:mvc/models/model-binding)は、名前付きパラメーター (`name` と `numTimes`) を、アドレス バーのクエリ文字列からメソッドのパラメーターに自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="05769-167">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="05769-168">完全な *HelloWorldController.cs* ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="05769-168">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="05769-169">`ViewData` ディクショナリ オブジェクトには、ビューに渡されるデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="05769-169">The `ViewData` dictionary object contains data that will be passed to the view.</span></span> 

<span data-ttu-id="05769-170">*Views/HelloWorld/Welcome.cshtml* という名前の [ようこそ] ビュー テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="05769-170">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="05769-171">"Hello" `NumTimes` を表示する *Welcome.cshtml* ビュー テンプレートでループを作成します。</span><span class="sxs-lookup"><span data-stu-id="05769-171">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="05769-172">*Views/HelloWorld/Welcome.cshtml* の内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="05769-172">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="05769-173">変更内容を保存し、次の URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="05769-173">Save your changes and browse to the following URL:</span></span>

`http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="05769-174">データは URL から取得され、[MVC モデル バインダー](xref:mvc/models/model-binding)を使用してコントローラーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="05769-174">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="05769-175">コントローラーはデータを `ViewData` ディクショナリにパッケージ化し、そのオブジェクトをビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="05769-175">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="05769-176">その後、ビューでブラウザーに HTML としてデータがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="05769-176">The view then renders the data as HTML to the browser.</span></span>

![[ようこそ] ラベルと、Hello Rick という語句が 4 つ示された [バージョン情報] ビュー](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

<span data-ttu-id="05769-178">上のサンプルでは、`ViewData` ディクショナリを使用して、コントローラーからビューにデータを渡しました。</span><span class="sxs-lookup"><span data-stu-id="05769-178">In the sample above, we used the `ViewData` dictionary to pass data from the controller to a view.</span></span> <span data-ttu-id="05769-179">チュートリアルの後半で、ビュー モデルを使用して、コントローラーからビューにデータを渡します。</span><span class="sxs-lookup"><span data-stu-id="05769-179">Later in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="05769-180">一般には、`ViewData` ディクショナリを使用する方法より、ビュー モデルを使用してデータを渡す方法が推奨されます。</span><span class="sxs-lookup"><span data-stu-id="05769-180">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="05769-181">詳細については、「[ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc)」 (MVC の ViewModel、ViewData、ViewBag、TempData、Session の比較) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="05769-181">See [ViewModel vs ViewData vs ViewBag vs TempData vs Session in MVC](http://www.mytecbits.com/microsoft/dot-net/viewmodel-viewdata-viewbag-tempdata-mvc) for more information.</span></span>

<span data-ttu-id="05769-182">"M" (モデル) については学習しましたが、データベースについてはまだです。</span><span class="sxs-lookup"><span data-stu-id="05769-182">Well, that was a kind of an "M" for model, but not the database kind.</span></span> <span data-ttu-id="05769-183">学習したことを確認し、ムービーのデータベースを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="05769-183">Let's take what we've learned and create a database of movies.</span></span>
