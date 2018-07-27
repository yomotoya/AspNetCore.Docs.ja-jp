# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="837bd-101">ASP.NET Core でスキャフォールディングされた Razor ページ</span><span class="sxs-lookup"><span data-stu-id="837bd-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="837bd-102">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="837bd-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="837bd-103">このチュートリアルでは、前のチュートリアルでスキャフォールディングによって作成された Razor ページについて説明します。</span><span class="sxs-lookup"><span data-stu-id="837bd-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="837bd-104">サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します。</span><span class="sxs-lookup"><span data-stu-id="837bd-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="837bd-105">[作成]、[削除]、[詳細]、および [編集] ページ</span><span class="sxs-lookup"><span data-stu-id="837bd-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="837bd-106">次のように、*Pages/Movies/Index.cshtml.cs* ページ モデルを確認します。</span><span class="sxs-lookup"><span data-stu-id="837bd-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="837bd-107">Razor ページは `PageModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="837bd-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="837bd-108">慣例により、`PageModel` 派生クラスは `<PageName>Model` と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="837bd-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="837bd-109">コンストラクターは[依存性の注入](xref:fundamentals/dependency-injection)を使用して、`MovieContext` をページに追加します。</span><span class="sxs-lookup"><span data-stu-id="837bd-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="837bd-110">スキャフォールディングされたページではすべてこのパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="837bd-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="837bd-111">Entity Framework での非同期プログラミングの詳細については、「[非同期コード](xref:data/ef-rp/intro#asynchronous-code)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="837bd-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="837bd-112">ページに対して要求が行われると、`OnGetAsync` メソッドは Razor ページにムービーのリストを返します。</span><span class="sxs-lookup"><span data-stu-id="837bd-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="837bd-113">Razor ページで `OnGetAsync` または `OnGet` が呼び出され、ページの状態が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="837bd-114">この場合、`OnGetAsync` でムービーのリストを取得し、表示します。</span><span class="sxs-lookup"><span data-stu-id="837bd-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="837bd-115">`OnGet` が `void` を返す場合、または `OnGetAsync` が `Task` を返す場合、return メソッドは使用されません。</span><span class="sxs-lookup"><span data-stu-id="837bd-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="837bd-116">戻り値の型が `IActionResult` または `Task<IActionResult>` の場合は、return ステートメントを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="837bd-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="837bd-117">たとえば、*Pages/Movies/Create.cshtml.cs* `OnPostAsync` メソッドでは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="837bd-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="837bd-118">次のように、*Pages/Movies/Index.cshtml* Razor ページを確認します。</span><span class="sxs-lookup"><span data-stu-id="837bd-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="837bd-119">Razor では、HTML から C# または Razor 固有のマークアップに移行できます。</span><span class="sxs-lookup"><span data-stu-id="837bd-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="837bd-120">`@` シンボルの後に [Razor で予約済みのキーワード](xref:mvc/views/razor#razor-reserved-keywords)が続いている場合は、Razor 固有のマークアップに移行します。それ以外の場合は、C# に移行します。</span><span class="sxs-lookup"><span data-stu-id="837bd-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="837bd-121">`@page` Razor ディレクティブはファイルを MVC アクションに分割します。これは、要求を処理できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="837bd-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="837bd-122">`@page` はページで最初の Razor ディレクティブである必要があります。</span><span class="sxs-lookup"><span data-stu-id="837bd-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="837bd-123">`@page` は、Razor 固有のマークアップへの移行の例です。</span><span class="sxs-lookup"><span data-stu-id="837bd-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="837bd-124">詳細については、「[Razor syntax](xref:mvc/views/razor#razor-syntax)」 (Razor の構文) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="837bd-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="837bd-125">次の HTML ヘルパーで使用されるラムダ式を確認します。</span><span class="sxs-lookup"><span data-stu-id="837bd-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="837bd-126">`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。</span><span class="sxs-lookup"><span data-stu-id="837bd-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="837bd-127">ラムダ式は評価されるのではなく検査されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="837bd-128">これは、`model`、`model.Movie`、または `model.Movie[0]` が `null` または空である場合、アクセス違反がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="837bd-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="837bd-129">ラムダ式が (`@Html.DisplayFor(modelItem => item.Title)` などを使用して) 評価される場合は、モデルのプロパティ値が評価されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="837bd-130">@model ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="837bd-130">The @model directive</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="837bd-131">`@model` ディレクティブは、Razor ページに渡されるモデルの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="837bd-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="837bd-132">前の例の `@model` 行は、Razor ページで `PageModel` 派生クラスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="837bd-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="837bd-133">モデルは、ページの `@Html.DisplayNameFor` および `@Html.DisplayName` [HTML ヘルパーで](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)使用されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="837bd-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData とレイアウト</span><span class="sxs-lookup"><span data-stu-id="837bd-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="837bd-135">次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="837bd-135">Consider the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="837bd-136">上の強調表示されたコードは、Razor の C# への移行例です。</span><span class="sxs-lookup"><span data-stu-id="837bd-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="837bd-137">`{` 文字と `}` 文字で C# コードのブロックを囲みます。</span><span class="sxs-lookup"><span data-stu-id="837bd-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="837bd-138">`PageModel` 基本クラスには `ViewData` 辞書プロパティがあり、これを使用して、ビューに渡すデータを追加できます。</span><span class="sxs-lookup"><span data-stu-id="837bd-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="837bd-139">キー/値のパターンを使用して、`ViewData` 辞書にオブジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="837bd-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="837bd-140">上のサンプルでは、"Title" プロパティが `ViewData` 辞書に追加されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="837bd-141">"Title" プロパティは *Pages/_Layout.cshtml* ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-141">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="837bd-142">次のマークアップは、*Pages/_Layout.cshtml* ファイルの最初の数行を示しています。</span><span class="sxs-lookup"><span data-stu-id="837bd-142">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="837bd-143">"Title" プロパティは *Pages/Shared/_Layout.cshtml* ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="837bd-144">次のマークアップは、*_Layout.cshtml* ファイルの最初の数行を示しています。</span><span class="sxs-lookup"><span data-stu-id="837bd-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="837bd-145">`@*Markup removed for brevity.*@` 行は Razor コメントです。</span><span class="sxs-lookup"><span data-stu-id="837bd-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="837bd-146">HTML コメント (`<!-- -->`) とは異なり、Razor コメントはクライアントには送信されません。</span><span class="sxs-lookup"><span data-stu-id="837bd-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="837bd-147">アプリを実行して、プロジェクトのリンク (**[ホーム]**、**[バージョン情報]**、**[連絡先]**、**[作成]**、**[編集]**、および **[削除]**) をテストします。</span><span class="sxs-lookup"><span data-stu-id="837bd-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="837bd-148">各ページで、ブラウザー タブで表示できるタイトルを設定します。ページをブックマークすると、ブックマークでタイトルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="837bd-149">現在、*Pages/Index.cshtml* と *Pages/Movies/Index.cshtml* のタイトルは同じですが、変更して別の値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="837bd-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="837bd-150">`Price` フィールドに小数点のコンマを入力できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="837bd-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="837bd-151">小数点にコンマ (",") を使い、英語 (米国) 以外の日付形式を使う英語以外のロケールの [jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する手順を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="837bd-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="837bd-152">小数点のコンマの追加方法については、[こちらの GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="837bd-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="837bd-153">`Layout` プロパティは *Pages/_ViewStart.cshtml* ファイルで設定されています。</span><span class="sxs-lookup"><span data-stu-id="837bd-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="837bd-154">上のマークアップでは、*Pages* フォルダーの下のすべての Razor ファイルの *Pages/_Layout.cshtml* にレイアウト ファイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="837bd-154">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="837bd-155">詳細については、「[Layout](xref:razor-pages/index#layout)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="837bd-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="837bd-156">レイアウトの更新</span><span class="sxs-lookup"><span data-stu-id="837bd-156">Update the layout</span></span>

<span data-ttu-id="837bd-157">より短い文字列を使用するために、*Pages/_Layout.cshtml* ファイルの `<title>` 要素を変更します。</span><span class="sxs-lookup"><span data-stu-id="837bd-157">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="837bd-158">*Pages/_Layout.cshtml* ファイルで次のアンカー要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="837bd-158">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="837bd-159">上の要素を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="837bd-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="837bd-160">上のアンカー要素は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="837bd-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="837bd-161">この場合は、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="837bd-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="837bd-162">`asp-page="/Movies/Index"` タグ ヘルパー属性と値で、`/Movies/Index` Razor ページへのリンクが作成されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="837bd-163">変更内容を保存し、**RpMovie** リンクをクリックしてアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="837bd-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="837bd-164">GitHub の [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) ファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="837bd-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="837bd-165">Create ページのモデル</span><span class="sxs-lookup"><span data-stu-id="837bd-165">The Create page model</span></span>

<span data-ttu-id="837bd-166">*Pages/Movies/Create.cshtml.cs* ページ モデルを確認します。</span><span class="sxs-lookup"><span data-stu-id="837bd-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]
::: moniker-end


<span data-ttu-id="837bd-167">`OnGet` メソッドは、ページに必要な状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="837bd-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="837bd-168">[作成] ページには初期化する状態はないため、`Page` が返されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="837bd-169">このチュートリアルで後ほど、`OnGet` メソッドの状態の初期化を確認できます。</span><span class="sxs-lookup"><span data-stu-id="837bd-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="837bd-170">`Page` メソッドでは、*Create.cshtml* ページをレンダリングする `PageResult` オブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="837bd-171">`Movie` プロパティは `[BindProperty]` 属性を使用して、[モデル バインド](xref:mvc/models/model-binding)にオプトインします。</span><span class="sxs-lookup"><span data-stu-id="837bd-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="837bd-172">[作成] フォームでフォーム値が投稿されると、ASP.NET Core ランタイムが投稿された値を `Movie` モデルにバインドします。</span><span class="sxs-lookup"><span data-stu-id="837bd-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="837bd-173">`OnPostAsync` メソッドは、ページでフォーム データが投稿されたときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="837bd-174">モデル エラーがある場合は、投稿されたフォーム データと共にフォームが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="837bd-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="837bd-175">ほとんどのモデル エラーは、フォームが投稿される前にクライアント側でキャッチできます。</span><span class="sxs-lookup"><span data-stu-id="837bd-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="837bd-176">モデル エラーの例では、日付に変換できない日付フィールドの値が投稿されています。</span><span class="sxs-lookup"><span data-stu-id="837bd-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="837bd-177">クライアント側の検証とモデルの検証については、チュートリアルの後半で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="837bd-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="837bd-178">モデル エラーがない場合、データは保存され、ブラウザーはインデックス ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="837bd-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="837bd-179">[作成] Razor ページ</span><span class="sxs-lookup"><span data-stu-id="837bd-179">The Create Razor Page</span></span>

<span data-ttu-id="837bd-180">次のように、*Pages/Movies/Create.cshtml* Razor ページ ファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="837bd-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
