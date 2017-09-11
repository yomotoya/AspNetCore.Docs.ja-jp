---
title: "ASP.NET Core でスキャフォールディングされた Razor ページ"
author: rick-anderson
description: "スキャフォールディングによって生成された Razor ページについて説明します。"
keywords: "ASP.NET Core,Razor ページ,Razor,MVC"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 02cbc7c7caf5128167dd3ecfdc0e2340f4876df5
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="42de0-104">ASP.NET Core でスキャフォールディングされた Razor ページ</span><span class="sxs-lookup"><span data-stu-id="42de0-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="42de0-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42de0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42de0-106">このチュートリアルでは、[前のチュートリアル](xref:tutorials/razor-pages/page)でスキャフォールディングによって作成された Razor ページについて説明します。</span><span class="sxs-lookup"><span data-stu-id="42de0-106">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/page).</span></span> 

<span data-ttu-id="42de0-107">サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します。</span><span class="sxs-lookup"><span data-stu-id="42de0-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="42de0-108">[作成]、[削除]、[詳細]、および [編集] ページ</span><span class="sxs-lookup"><span data-stu-id="42de0-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="42de0-109">*Pages/Movies/Index.cshtml.cs* という分離コード ファイルを確認します。[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="42de0-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml.cs)]</span></span>

<span data-ttu-id="42de0-110">Razor ページは `PageModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="42de0-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="42de0-111">慣例により、`PageModel` 派生クラスは `<PageName>Model` と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="42de0-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="42de0-112">コンストラクターは[依存性の注入](xref:fundamentals/dependency-injection)を使用して、`MovieContext` をページに追加します。</span><span class="sxs-lookup"><span data-stu-id="42de0-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="42de0-113">スキャフォールディングされたページではすべてこのパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="42de0-113">All the scaffolded pages follow this pattern.</span></span>

<span data-ttu-id="42de0-114">ページに対して要求が行われると、`OnGetAsync` メソッドは Razor ページにムービーのリストを返します。</span><span class="sxs-lookup"><span data-stu-id="42de0-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="42de0-115">Razor ページで `OnGetAsync` または `OnGet` が呼び出され、ページの状態が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="42de0-116">この場合、`OnGetAsync` で表示するムービーのリストを取得します。</span><span class="sxs-lookup"><span data-stu-id="42de0-116">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="42de0-117">次のように、*Pages/Movies/Index.cshtml* Razor ページを確認します。</span><span class="sxs-lookup"><span data-stu-id="42de0-117">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

<span data-ttu-id="42de0-118">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="42de0-118">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml)]</span></span>

<span data-ttu-id="42de0-119">Razor では、HTML から C# または Razor 固有のマークアップに移行できます。</span><span class="sxs-lookup"><span data-stu-id="42de0-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="42de0-120">`@` シンボルの後に [Razor で予約済みのキーワード](xref:mvc/views/razor#razor-reserved-keywords)が続いている場合は、Razor 固有のマークアップに移行します。それ以外の場合は、C# に移行します。</span><span class="sxs-lookup"><span data-stu-id="42de0-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="42de0-121">`@page` Razor ディレクティブはファイルを MVC アクションに分割します。これは、要求を処理できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="42de0-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="42de0-122">`@page` はページで最初の Razor ディレクティブである必要があります。</span><span class="sxs-lookup"><span data-stu-id="42de0-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="42de0-123">`@page` は、Razor 固有のマークアップへの移行の例です。</span><span class="sxs-lookup"><span data-stu-id="42de0-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="42de0-124">詳細については、「[Razor syntax](xref:mvc/views/razor#razor-syntax)」 (Razor の構文) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="42de0-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="42de0-125">次の HTML ヘルパーで使用されるラムダ式を確認します。</span><span class="sxs-lookup"><span data-stu-id="42de0-125">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movie[0].Title))`

<span data-ttu-id="42de0-126">`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。</span><span class="sxs-lookup"><span data-stu-id="42de0-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="42de0-127">ラムダ式は評価されるのではなく検査されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="42de0-128">これは、`model`、`model.Movies`、または `model.Movies[0]` が `null` または空である場合、アクセス違反がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="42de0-128">That means there is no access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="42de0-129">ラムダ式が (`@Html.DisplayFor(modelItem => item.Title)` などを使用して) 評価される場合は、モデルのプロパティ値が評価されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="42de0-130">@model ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="42de0-130">The @model directive</span></span>

<span data-ttu-id="42de0-131">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-2&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="42de0-131">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-2&highlight=2)]</span></span>

<span data-ttu-id="42de0-132">`@model` ディレクティブは、Razor ページに渡されるモデルの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="42de0-132">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="42de0-133">前の例の `@model` 行は、Razor ページで `PageModel` 派生クラスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="42de0-133">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="42de0-134">モデルは、ページの `@Html.DisplayNameFor` および `@Html.DisplayName` [HTML ヘルパーで](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)使用されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-134">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="42de0-135"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData とレイアウト</span><span class="sxs-lookup"><span data-stu-id="42de0-135"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="42de0-136">次のコードがあるとします。</span><span class="sxs-lookup"><span data-stu-id="42de0-136">Consider the following code:</span></span>

<span data-ttu-id="42de0-137">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-6&highlight=4-)]</span><span class="sxs-lookup"><span data-stu-id="42de0-137">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Index.cshtml?range=1-6&highlight=4-)]</span></span>

<span data-ttu-id="42de0-138">上の強調表示されたコードは、Razor の C# への移行例です。</span><span class="sxs-lookup"><span data-stu-id="42de0-138">The preceding highligted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="42de0-139">`{` 文字と `}` 文字で C# コードのブロックを囲みます。</span><span class="sxs-lookup"><span data-stu-id="42de0-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="42de0-140">`Controller` 基本クラスには `ViewData` 辞書プロパティがあり、これを使用して、ビューに渡すデータを追加できます。</span><span class="sxs-lookup"><span data-stu-id="42de0-140">The `Controller` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="42de0-141">キー/値のパターンを使用して、`ViewData` 辞書にオブジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="42de0-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="42de0-142">上のサンプルでは、"Title" プロパティが `ViewData` 辞書に追加されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="42de0-143">"Title" プロパティは *Pages/_Layout.cshtml* ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-143">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="42de0-144">次のマークアップは、*Pages/_Layout.cshtml* ファイルの最初の数行を示しています。</span><span class="sxs-lookup"><span data-stu-id="42de0-144">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

<span data-ttu-id="42de0-145">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]</span><span class="sxs-lookup"><span data-stu-id="42de0-145">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]</span></span>

<span data-ttu-id="42de0-146">`@*Markup removed for brevity.*@` 行は Razor コメントです。</span><span class="sxs-lookup"><span data-stu-id="42de0-146">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="42de0-147">HTML コメント (`<!-- -->`) とは異なり、Razor コメントはクライアントには送信されません。</span><span class="sxs-lookup"><span data-stu-id="42de0-147">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="42de0-148">アプリを実行して、プロジェクトのリンク (**[ホーム]**、**[バージョン情報]**、**[連絡先]**、**[作成]**、**[編集]**、および **[削除]**) をテストします。</span><span class="sxs-lookup"><span data-stu-id="42de0-148">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="42de0-149">各ページで、ブラウザー タブで表示できるタイトルを設定します。ページをブックマークすると、ブックマークでタイトルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-149">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="42de0-150">現在、*Pages/Index.cshtml* と *Pages/Movies/Index.cshtml* のタイトルは同じですが、変更して別の値を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="42de0-150">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="42de0-151">`Layout` プロパティは *Pages/_ViewStart.cshtml* ファイルで設定されています。</span><span class="sxs-lookup"><span data-stu-id="42de0-151">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

<span data-ttu-id="42de0-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="42de0-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="42de0-153">上のマークアップでは、*Pages* フォルダーの下のすべての Razor ファイルの *Pages/_Layout.cshtml* にレイアウト ファイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="42de0-153">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="42de0-154">詳細については、「[Layout](xref:mvc/razor-pages/index#layout)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="42de0-154">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="42de0-155">レイアウトの更新</span><span class="sxs-lookup"><span data-stu-id="42de0-155">Update the layout</span></span>

<span data-ttu-id="42de0-156">より短い文字列を使用するために、*Pages/_Layout.cshtml* ファイルの `<title>` 要素を変更します。</span><span class="sxs-lookup"><span data-stu-id="42de0-156">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

<span data-ttu-id="42de0-157">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6-)]</span><span class="sxs-lookup"><span data-stu-id="42de0-157">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6-)]</span></span>

<span data-ttu-id="42de0-158">*Pages/_Layout.cshtml* ファイルで次のアンカー要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="42de0-158">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="42de0-159">上の要素を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="42de0-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="42de0-160">上のアンカー要素は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="42de0-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="42de0-161">この場合は、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)です。</span><span class="sxs-lookup"><span data-stu-id="42de0-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper).</span></span> <span data-ttu-id="42de0-162">`asp-page="/Movies/Index"` タグ ヘルパー属性と値で、`/Movies/Index` Razor ページへのリンクが作成されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="42de0-163">変更内容を保存し、**RpMovie** リンクをクリックしてアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="42de0-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="42de0-164">GitHub の [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) ファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="42de0-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="42de0-165">[作成] 分離コード ページ</span><span class="sxs-lookup"><span data-stu-id="42de0-165">The Create code-behind page</span></span>

<span data-ttu-id="42de0-166">次のように、*Pages/Movies/Create.cshtml.cs* という分離コード ファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="42de0-166">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

<span data-ttu-id="42de0-167">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetALL)]</span><span class="sxs-lookup"><span data-stu-id="42de0-167">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetALL)]</span></span>

<span data-ttu-id="42de0-168">`OnGet` メソッドは、ページに必要な状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="42de0-168">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="42de0-169">[作成] ページには初期化する状態はありません。</span><span class="sxs-lookup"><span data-stu-id="42de0-169">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="42de0-170">`Page` メソッドでは、*Create.cshtml* ページをレンダリングする `PageResult` オブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="42de0-171">`Movie` プロパティは `[BindProperty]` 属性を使用して、[モデル バインド](xref:mvc/models/model-binding)にオプトインします。</span><span class="sxs-lookup"><span data-stu-id="42de0-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="42de0-172">[作成] フォームでフォーム値が投稿されると、ASP.NET Core ランタイムが投稿された値を `Movie` モデルにバインドします。</span><span class="sxs-lookup"><span data-stu-id="42de0-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="42de0-173">`OnPostAsync` メソッドは、ページでフォーム データが投稿されたときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

<span data-ttu-id="42de0-174">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetPost)]</span><span class="sxs-lookup"><span data-stu-id="42de0-174">[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml.cs?name=snippetPost)]</span></span>

<span data-ttu-id="42de0-175">モデル エラーがある場合は、投稿されたフォーム データと共にフォームが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-175">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="42de0-176">ほとんどのモデル エラーは、フォームが投稿される前にクライアント側でキャッチできます。</span><span class="sxs-lookup"><span data-stu-id="42de0-176">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="42de0-177">モデル エラーの例では、日付に変換できない日付フィールドの値が投稿されています。</span><span class="sxs-lookup"><span data-stu-id="42de0-177">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="42de0-178">クライアント側の検証とモデルの検証については、チュートリアルの後半で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="42de0-178">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="42de0-179">モデル エラーがない場合、データは保存され、ブラウザーはインデックス ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="42de0-179">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="42de0-180">[作成] Razor ページ</span><span class="sxs-lookup"><span data-stu-id="42de0-180">The Create Razor Page</span></span>

<span data-ttu-id="42de0-181">次のように、*Pages/Movies/Create.cshtml* Razor ページ ファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="42de0-181">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

<span data-ttu-id="42de0-182">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="42de0-182">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml)]</span></span>

<span data-ttu-id="42de0-183">Visual Studio に、タグ ヘルパーで使用される独特なフォントで `<form method="post">` タグが表示されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-183">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="42de0-184">`<form method="post">` 要素は[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="42de0-184">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="42de0-185">フォーム タグ ヘルパーには自動的に[偽造防止トークン](xref:security/anti-request-forgery)が含まれます。</span><span class="sxs-lookup"><span data-stu-id="42de0-185">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![Create.cshtml ページの VS17 ビュー](page/_static/th.png)

<span data-ttu-id="42de0-187">スキャフォールディング エンジンは、次のような、(ID を除く) モデルの各フィールドの Razor マークアップを作成します。</span><span class="sxs-lookup"><span data-stu-id="42de0-187">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

<span data-ttu-id="42de0-188">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml?range=15-20)]</span><span class="sxs-lookup"><span data-stu-id="42de0-188">[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movie/Create.cshtml?range=15-20)]</span></span>

<span data-ttu-id="42de0-189">[検証タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` と ` <span asp-validation-for`) には検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="42de0-189">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="42de0-190">検証については、後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="42de0-190">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="42de0-191">[ラベル タグ ヘルパー](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) は、`Title` プロパティのラベル キャプションと `for` 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="42de0-191">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="42de0-192">[入力タグ ヘルパー](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) は [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) 属性を使用し、クライアント側で jQuery 検証に必要な HTML 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="42de0-192">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="42de0-193">次のチュートリアルでは、SQL Server LocalDB とデータベースのシード処理について説明します。</span><span class="sxs-lookup"><span data-stu-id="42de0-193">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="42de0-194">[前: モデルの追加](xref:tutorials/razor-pages/modelz)
[次: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="42de0-194">[Previous: Adding a model](xref:tutorials/razor-pages/modelz)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
