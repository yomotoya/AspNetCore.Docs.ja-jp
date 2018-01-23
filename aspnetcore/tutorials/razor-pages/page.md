---
title: "ASP.NET Core でスキャフォールディングされた Razor ページ"
author: rick-anderson
description: "スキャフォールディングによって生成された Razor ページについて説明します。"
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: ad2a2b48beb31dddcfd78a8aab79ac58ccda28f3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="ae328-103">ASP.NET Core でスキャフォールディングされた Razor ページ</span><span class="sxs-lookup"><span data-stu-id="ae328-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ae328-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ae328-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ae328-105">このチュートリアルでは、前のチュートリアルの[モデルの追加](xref:tutorials/razor-pages/model#scaffold-the-movie-model)に関するトピックで、スキャフォールディングによって作成された Razor ページについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ae328-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="ae328-106">サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します。</span><span class="sxs-lookup"><span data-stu-id="ae328-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="ae328-107">[作成]、[削除]、[詳細]、および [編集] ページ</span><span class="sxs-lookup"><span data-stu-id="ae328-107">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="ae328-108">次のように、*Pages/Movies/Index.cshtml.cs* ページ モデルを確認します。[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="ae328-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="ae328-109">Razor ページは `PageModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="ae328-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="ae328-110">慣例により、`PageModel` 派生クラスは `<PageName>Model` と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="ae328-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="ae328-111">コンストラクターは[依存性の注入](xref:fundamentals/dependency-injection)を使用して、`MovieContext` をページに追加します。</span><span class="sxs-lookup"><span data-stu-id="ae328-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="ae328-112">スキャフォールディングされたページではすべてこのパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="ae328-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="ae328-113">Entity Framework での非同期プログラミングの詳細については、「[非同期コード](xref:data/ef-rp/intro#asynchronous-code)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ae328-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="ae328-114">ページに対して要求が行われると、`OnGetAsync` メソッドは Razor ページにムービーのリストを返します。</span><span class="sxs-lookup"><span data-stu-id="ae328-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="ae328-115">Razor ページで `OnGetAsync` または `OnGet` が呼び出され、ページの状態が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="ae328-116">この場合、`OnGetAsync` でムービーのリストを取得し、表示します。</span><span class="sxs-lookup"><span data-stu-id="ae328-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="ae328-117">`OnGet` が `void` を返す場合、または `OnGetAsync` が `Task` を返す場合、return メソッドは使用されません。</span><span class="sxs-lookup"><span data-stu-id="ae328-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="ae328-118">戻り値の型が `IActionResult` または `Task<IActionResult>` の場合は、return ステートメントを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ae328-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="ae328-119">たとえば、*Pages/Movies/Create.cshtml.cs* `OnPostAsync` メソッドでは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ae328-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
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
<span data-ttu-id="ae328-120">次のように、*Pages/Movies/Index.cshtml* Razor ページを確認します。</span><span class="sxs-lookup"><span data-stu-id="ae328-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="ae328-121">Razor では、HTML から C# または Razor 固有のマークアップに移行できます。</span><span class="sxs-lookup"><span data-stu-id="ae328-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="ae328-122">`@` シンボルの後に [Razor で予約済みのキーワード](xref:mvc/views/razor#razor-reserved-keywords)が続いている場合は、Razor 固有のマークアップに移行します。それ以外の場合は、C# に移行します。</span><span class="sxs-lookup"><span data-stu-id="ae328-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="ae328-123">`@page` Razor ディレクティブはファイルを MVC アクションに分割します。これは、要求を処理できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="ae328-123">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="ae328-124">`@page` はページで最初の Razor ディレクティブである必要があります。</span><span class="sxs-lookup"><span data-stu-id="ae328-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="ae328-125">`@page` は、Razor 固有のマークアップへの移行の例です。</span><span class="sxs-lookup"><span data-stu-id="ae328-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="ae328-126">詳細については、「[Razor syntax](xref:mvc/views/razor#razor-syntax)」 (Razor の構文) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ae328-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="ae328-127">次の HTML ヘルパーで使用されるラムダ式を確認します。</span><span class="sxs-lookup"><span data-stu-id="ae328-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="ae328-128">`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。</span><span class="sxs-lookup"><span data-stu-id="ae328-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="ae328-129">ラムダ式は評価されるのではなく検査されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="ae328-130">これは、`model`、`model.Movie`、または `model.Movie[0]` が `null` または空である場合、アクセス違反がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="ae328-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="ae328-131">ラムダ式が (`@Html.DisplayFor(modelItem => item.Title)` などを使用して) 評価される場合は、モデルのプロパティ値が評価されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="ae328-132">@model ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="ae328-132">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="ae328-133">`@model` ディレクティブは、Razor ページに渡されるモデルの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="ae328-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="ae328-134">前の例の `@model` 行は、Razor ページで `PageModel` 派生クラスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ae328-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="ae328-135">モデルは、ページの `@Html.DisplayNameFor` および `@Html.DisplayName` [HTML ヘルパーで](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)使用されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="ae328-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData とレイアウト</span><span class="sxs-lookup"><span data-stu-id="ae328-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="ae328-137">次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="ae328-137">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="ae328-138">上の強調表示されたコードは、Razor の C# への移行例です。</span><span class="sxs-lookup"><span data-stu-id="ae328-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="ae328-139">`{` 文字と `}` 文字で C# コードのブロックを囲みます。</span><span class="sxs-lookup"><span data-stu-id="ae328-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="ae328-140">`PageModel` 基本クラスには `ViewData` 辞書プロパティがあり、これを使用して、ビューに渡すデータを追加できます。</span><span class="sxs-lookup"><span data-stu-id="ae328-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="ae328-141">キー/値のパターンを使用して、`ViewData` 辞書にオブジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="ae328-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="ae328-142">上のサンプルでは、"Title" プロパティが `ViewData` 辞書に追加されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="ae328-143">"Title" プロパティは *Pages/_Layout.cshtml* ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-143">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="ae328-144">次のマークアップは、*Pages/_Layout.cshtml* ファイルの最初の数行を示しています。</span><span class="sxs-lookup"><span data-stu-id="ae328-144">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="ae328-145">`@*Markup removed for brevity.*@` 行は Razor コメントです。</span><span class="sxs-lookup"><span data-stu-id="ae328-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="ae328-146">HTML コメント (`<!-- -->`) とは異なり、Razor コメントはクライアントには送信されません。</span><span class="sxs-lookup"><span data-stu-id="ae328-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="ae328-147">アプリを実行して、プロジェクトのリンク (**[ホーム]**、**[バージョン情報]**、**[連絡先]**、**[作成]**、**[編集]**、および **[削除]**) をテストします。</span><span class="sxs-lookup"><span data-stu-id="ae328-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="ae328-148">各ページで、ブラウザー タブで表示できるタイトルを設定します。ページをブックマークすると、ブックマークでタイトルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="ae328-149">現在、*Pages/Index.cshtml* と *Pages/Movies/Index.cshtml* のタイトルは同じですが、変更して別の値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="ae328-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="ae328-150">`Layout` プロパティは *Pages/_ViewStart.cshtml* ファイルで設定されています。</span><span class="sxs-lookup"><span data-stu-id="ae328-150">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="ae328-151">上のマークアップでは、*Pages* フォルダーの下のすべての Razor ファイルの *Pages/_Layout.cshtml* にレイアウト ファイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="ae328-151">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="ae328-152">詳細については、「[Layout](xref:mvc/razor-pages/index#layout)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ae328-152">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="ae328-153">レイアウトの更新</span><span class="sxs-lookup"><span data-stu-id="ae328-153">Update the layout</span></span>

<span data-ttu-id="ae328-154">より短い文字列を使用するために、*Pages/_Layout.cshtml* ファイルの `<title>` 要素を変更します。</span><span class="sxs-lookup"><span data-stu-id="ae328-154">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="ae328-155">*Pages/_Layout.cshtml* ファイルで次のアンカー要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="ae328-155">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="ae328-156">上の要素を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ae328-156">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="ae328-157">上のアンカー要素は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="ae328-157">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="ae328-158">この場合は、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="ae328-158">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="ae328-159">`asp-page="/Movies/Index"` タグ ヘルパー属性と値で、`/Movies/Index` Razor ページへのリンクが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-159">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="ae328-160">変更内容を保存し、**RpMovie** リンクをクリックしてアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="ae328-160">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="ae328-161">GitHub の [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) ファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ae328-161">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="ae328-162">[作成] 分離コード ページ</span><span class="sxs-lookup"><span data-stu-id="ae328-162">The Create code-behind page</span></span>

<span data-ttu-id="ae328-163">次のように、*Pages/Movies/Create.cshtml.cs* という分離コード ファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="ae328-163">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="ae328-164">`OnGet` メソッドは、ページに必要な状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="ae328-164">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="ae328-165">[作成] ページには初期化する状態はありません。</span><span class="sxs-lookup"><span data-stu-id="ae328-165">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="ae328-166">`Page` メソッドでは、*Create.cshtml* ページをレンダリングする `PageResult` オブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-166">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="ae328-167">`Movie` プロパティは `[BindProperty]` 属性を使用して、[モデル バインド](xref:mvc/models/model-binding)にオプトインします。</span><span class="sxs-lookup"><span data-stu-id="ae328-167">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="ae328-168">[作成] フォームでフォーム値が投稿されると、ASP.NET Core ランタイムが投稿された値を `Movie` モデルにバインドします。</span><span class="sxs-lookup"><span data-stu-id="ae328-168">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="ae328-169">`OnPostAsync` メソッドは、ページでフォーム データが投稿されたときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-169">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="ae328-170">モデル エラーがある場合は、投稿されたフォーム データと共にフォームが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-170">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="ae328-171">ほとんどのモデル エラーは、フォームが投稿される前にクライアント側でキャッチできます。</span><span class="sxs-lookup"><span data-stu-id="ae328-171">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="ae328-172">モデル エラーの例では、日付に変換できない日付フィールドの値が投稿されています。</span><span class="sxs-lookup"><span data-stu-id="ae328-172">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="ae328-173">クライアント側の検証とモデルの検証については、チュートリアルの後半で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="ae328-173">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="ae328-174">モデル エラーがない場合、データは保存され、ブラウザーはインデックス ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="ae328-174">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="ae328-175">[作成] Razor ページ</span><span class="sxs-lookup"><span data-stu-id="ae328-175">The Create Razor Page</span></span>

<span data-ttu-id="ae328-176">次のように、*Pages/Movies/Create.cshtml* Razor ページ ファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="ae328-176">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="ae328-177">Visual Studio に、タグ ヘルパーで使用される独特なフォントで `<form method="post">` タグが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-177">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="ae328-178">`<form method="post">` 要素は[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="ae328-178">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="ae328-179">フォーム タグ ヘルパーには自動的に[偽造防止トークン](xref:security/anti-request-forgery)が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ae328-179">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![Create.cshtml ページの VS17 ビュー](page/_static/th.png)

<span data-ttu-id="ae328-181">スキャフォールディング エンジンは、次のような、(ID を除く) モデルの各フィールドの Razor マークアップを作成します。</span><span class="sxs-lookup"><span data-stu-id="ae328-181">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="ae328-182">[検証タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` と ` <span asp-validation-for`) には検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ae328-182">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="ae328-183">検証については、後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="ae328-183">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="ae328-184">[ラベル タグ ヘルパー](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) は、`Title` プロパティのラベル キャプションと `for` 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="ae328-184">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="ae328-185">[入力タグ ヘルパー](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) は [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 属性を使用し、クライアント側で jQuery 検証に必要な HTML 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="ae328-185">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="ae328-186">次のチュートリアルでは、SQL Server LocalDB とデータベースのシード処理について説明します。</span><span class="sxs-lookup"><span data-stu-id="ae328-186">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ae328-187">[前: モデルの追加](xref:tutorials/razor-pages/model)
[次: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="ae328-187">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
