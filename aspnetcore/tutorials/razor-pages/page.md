---
title: ASP.NET Core でスキャフォールディングされた Razor ページ
author: rick-anderson
description: スキャフォールディングによって生成された Razor ページについて説明します。
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: ad87e3da72c3dd6adf8cf55d16da58fa47ed5542
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410350"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="383b2-103">ASP.NET Core でスキャフォールディングされた Razor ページ</span><span class="sxs-lookup"><span data-stu-id="383b2-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="383b2-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="383b2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="383b2-105">このチュートリアルでは、前のチュートリアルでスキャフォールディングによって作成された Razor ページについて説明します。</span><span class="sxs-lookup"><span data-stu-id="383b2-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="383b2-106">サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22)します。</span><span class="sxs-lookup"><span data-stu-id="383b2-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="383b2-107">[作成]、[削除]、[詳細]、および [編集] ページ</span><span class="sxs-lookup"><span data-stu-id="383b2-107">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="383b2-108">次のように、*Pages/Movies/Index.cshtml.cs* ページ モデルを確認します。</span><span class="sxs-lookup"><span data-stu-id="383b2-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="383b2-109">Razor ページは `PageModel` から派生します。</span><span class="sxs-lookup"><span data-stu-id="383b2-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="383b2-110">慣例により、`PageModel` 派生クラスは `<PageName>Model` と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="383b2-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="383b2-111">コンストラクターは[依存性の注入](xref:fundamentals/dependency-injection)を使用して、`RazorPagesMovieContext` をページに追加します。</span><span class="sxs-lookup"><span data-stu-id="383b2-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="383b2-112">スキャフォールディングされたページではすべてこのパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="383b2-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="383b2-113">Entity Framework での非同期プログラミングの詳細については、「[非同期コード](xref:data/ef-rp/intro#asynchronous-code)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="383b2-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="383b2-114">ページに対して要求が行われると、`OnGetAsync` メソッドは Razor ページにムービーのリストを返します。</span><span class="sxs-lookup"><span data-stu-id="383b2-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="383b2-115">Razor ページで `OnGetAsync` または `OnGet` が呼び出され、ページの状態が初期化されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="383b2-116">この場合、`OnGetAsync` でムービーのリストを取得し、表示します。</span><span class="sxs-lookup"><span data-stu-id="383b2-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="383b2-117">`OnGet` が `void` を返す場合、または `OnGetAsync` が `Task` を返す場合、return メソッドは使用されません。</span><span class="sxs-lookup"><span data-stu-id="383b2-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="383b2-118">戻り値の型が `IActionResult` または `Task<IActionResult>` の場合は、return ステートメントを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="383b2-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="383b2-119">たとえば、*Pages/Movies/Create.cshtml.cs* `OnPostAsync` メソッドでは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="383b2-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="383b2-120">次のように、*Pages/Movies/Index.cshtml* Razor ページを確認します。</span><span class="sxs-lookup"><span data-stu-id="383b2-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="383b2-121">Razor では、HTML から C# または Razor 固有のマークアップに移行できます。</span><span class="sxs-lookup"><span data-stu-id="383b2-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="383b2-122">`@` シンボルの後に [Razor で予約済みのキーワード](xref:mvc/views/razor#razor-reserved-keywords)が続いている場合は、Razor 固有のマークアップに移行します。それ以外の場合は、C# に移行します。</span><span class="sxs-lookup"><span data-stu-id="383b2-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="383b2-123">`@page` Razor ディレクティブはファイルを MVC アクションに分割します。これは、要求を処理できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="383b2-123">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="383b2-124">`@page` はページで最初の Razor ディレクティブである必要があります。</span><span class="sxs-lookup"><span data-stu-id="383b2-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="383b2-125">`@page` は、Razor 固有のマークアップへの移行の例です。</span><span class="sxs-lookup"><span data-stu-id="383b2-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="383b2-126">詳細については、「[Razor syntax](xref:mvc/views/razor#razor-syntax)」 (Razor の構文) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="383b2-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="383b2-127">次の HTML ヘルパーで使用されるラムダ式を確認します。</span><span class="sxs-lookup"><span data-stu-id="383b2-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="383b2-128">`DisplayNameFor` HTML ヘルパーは、ラムダ式で参照される `Title` プロパティを検査し、表示名を判別します。</span><span class="sxs-lookup"><span data-stu-id="383b2-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="383b2-129">ラムダ式は評価されるのではなく検査されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="383b2-130">これは、`model`、`model.Movie`、または `model.Movie[0]` が `null` または空である場合、アクセス違反がないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="383b2-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="383b2-131">ラムダ式が (`@Html.DisplayFor(modelItem => item.Title)` などを使用して) 評価される場合は、モデルのプロパティ値が評価されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="383b2-132">
  @model ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="383b2-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="383b2-133">`@model` ディレクティブは、Razor ページに渡されるモデルの型を指定します。</span><span class="sxs-lookup"><span data-stu-id="383b2-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="383b2-134">前の例の `@model` 行は、Razor ページで `PageModel` 派生クラスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="383b2-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="383b2-135">モデルは、ページの `@Html.DisplayNameFor` および `@Html.DisplayFor` [HTML ヘルパーで](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)使用されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="383b2-136">レイアウト ページ</span><span class="sxs-lookup"><span data-stu-id="383b2-136">The layout page</span></span>

<span data-ttu-id="383b2-137">メニューのリンク (**[RazorPagesMovie]**、**[ホーム]**、**[プライバシー]**) を選択します。</span><span class="sxs-lookup"><span data-stu-id="383b2-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="383b2-138">各ページには同じメニューのレイアウトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="383b2-139">メニューのレイアウトは、*Pages/Shared/_Layout.cshtml* ファイルに実装されています。</span><span class="sxs-lookup"><span data-stu-id="383b2-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="383b2-140">*Pages/Shared/_Layout.cshtml* ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="383b2-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="383b2-141">[[レイアウト]](xref:mvc/views/layout) テンプレートでは、1 か所でサイトの HTML コンテナー レイアウトを指定し、それをサイト内の複数のページに適用できます。</span><span class="sxs-lookup"><span data-stu-id="383b2-141">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="383b2-142">`@RenderBody()` という行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="383b2-142">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="383b2-143">`RenderBody` は、作成したページ固有のビューがすべて表示されるプレースホルダーで、レイアウト ページに*ラップ*されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-143">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="383b2-144">たとえば、**[プライバシー]** リンクを選択した場合、`RenderBody` メソッド内で **Pages/Privacy.cshtml** ビューがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="383b2-144">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>
### <a name="viewdata-and-layout"></a><span data-ttu-id="383b2-145">ViewData とレイアウト</span><span class="sxs-lookup"><span data-stu-id="383b2-145">ViewData and layout</span></span>

<span data-ttu-id="383b2-146">*Pages/Movies/Index.cshtml* ファイルの次のコードを考えてみます。</span><span class="sxs-lookup"><span data-stu-id="383b2-146">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="383b2-147">上の強調表示されたコードは、Razor の C# への移行例です。</span><span class="sxs-lookup"><span data-stu-id="383b2-147">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="383b2-148">`{` 文字と `}` 文字で C# コードのブロックを囲みます。</span><span class="sxs-lookup"><span data-stu-id="383b2-148">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="383b2-149">`PageModel` 基本クラスには `ViewData` 辞書プロパティがあり、これを使用して、ビューに渡すデータを追加できます。</span><span class="sxs-lookup"><span data-stu-id="383b2-149">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="383b2-150">キー/値のパターンを使用して、`ViewData` 辞書にオブジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="383b2-150">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="383b2-151">上のサンプルでは、"Title" プロパティが `ViewData` 辞書に追加されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-151">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

<span data-ttu-id="383b2-152">"Title" プロパティは *Pages/Shared/_Layout.cshtml* ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-152">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="383b2-153">次のマークアップは、*_Layout.cshtml* ファイルの最初の数行を示しています。</span><span class="sxs-lookup"><span data-stu-id="383b2-153">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="383b2-154">`@*Markup removed for brevity.*@` の行は Razor コメントで、レイアウト ファイルには表示されません。</span><span class="sxs-lookup"><span data-stu-id="383b2-154">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="383b2-155">HTML コメント (`<!-- -->`) とは異なり、Razor コメントはクライアントには送信されません。</span><span class="sxs-lookup"><span data-stu-id="383b2-155">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="383b2-156">レイアウトの更新</span><span class="sxs-lookup"><span data-stu-id="383b2-156">Update the layout</span></span>

<span data-ttu-id="383b2-157">**RazorPagesMovie** ではなく **Movie** が表示されるように、*Pages/Shared/_Layout.cshtml* ファイルの `<title>` 要素を変更します。</span><span class="sxs-lookup"><span data-stu-id="383b2-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


<span data-ttu-id="383b2-158">*Pages/Shared/_Layout.cshtml* ファイルで次のアンカー要素を見つけます。</span><span class="sxs-lookup"><span data-stu-id="383b2-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="383b2-159">上の要素を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="383b2-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="383b2-160">上のアンカー要素は[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="383b2-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="383b2-161">この場合は、[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="383b2-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="383b2-162">`asp-page="/Movies/Index"` タグ ヘルパー属性と値で、`/Movies/Index` Razor ページへのリンクが作成されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="383b2-163">`asp-area` 属性の値が空なので、リンクではこの区分が使用されていません。</span><span class="sxs-lookup"><span data-stu-id="383b2-163">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="383b2-164">詳細については、[区分](xref:mvc/controllers/areas)に関する記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="383b2-164">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="383b2-165">変更内容を保存し、**RpMovie** リンクをクリックしてアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="383b2-165">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="383b2-166">問題がある場合は、GitHub の [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) ファイルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="383b2-166">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="383b2-167">その他のリンク (**[Home]**、**[RpMovie]**、**[Create]**、**[Edit]**、および **[Delete]**) をテストします。</span><span class="sxs-lookup"><span data-stu-id="383b2-167">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="383b2-168">各ページで、ブラウザー タブで表示できるタイトルを設定します。ページをブックマークすると、ブックマークでタイトルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-168">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="383b2-169">`Price` フィールドに小数点のコンマを入力できない場合があります。</span><span class="sxs-lookup"><span data-stu-id="383b2-169">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="383b2-170">小数点にコンマ (",") を使い、英語 (米国) 以外の日付形式を使う英語以外のロケールの [jQuery 検証](https://jqueryvalidation.org/)をサポートするには、アプリをグローバル化する手順を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="383b2-170">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="383b2-171">小数点のコンマの追加方法については、[こちらの GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="383b2-171">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="383b2-172">`Layout` プロパティは *Pages/_ViewStart.cshtml* ファイルで設定されています。</span><span class="sxs-lookup"><span data-stu-id="383b2-172">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="383b2-173">上のマークアップでは、*Pages* フォルダーの下のすべての Razor ファイルに対して、レイアウト ファイルを *Pages/Shared/_Layout.cshtml* に設定します。</span><span class="sxs-lookup"><span data-stu-id="383b2-173">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="383b2-174">詳細については、「[Layout](xref:razor-pages/index#layout)」 (レイアウト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="383b2-174">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="383b2-175">Create ページのモデル</span><span class="sxs-lookup"><span data-stu-id="383b2-175">The Create page model</span></span>

<span data-ttu-id="383b2-176">*Pages/Movies/Create.cshtml.cs* ページ モデルを確認します。</span><span class="sxs-lookup"><span data-stu-id="383b2-176">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="383b2-177">`OnGet` メソッドは、ページに必要な状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="383b2-177">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="383b2-178">[作成] ページには初期化する状態はないため、`Page` が返されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-178">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="383b2-179">このチュートリアルで後ほど、`OnGet` メソッドの状態の初期化を確認できます。</span><span class="sxs-lookup"><span data-stu-id="383b2-179">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="383b2-180">`Page` メソッドでは、*Create.cshtml* ページをレンダリングする `PageResult` オブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-180">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="383b2-181">`Movie` プロパティは `[BindProperty]` 属性を使用して、[モデル バインド](xref:mvc/models/model-binding)にオプトインします。</span><span class="sxs-lookup"><span data-stu-id="383b2-181">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="383b2-182">[作成] フォームでフォーム値が投稿されると、ASP.NET Core ランタイムが投稿された値を `Movie` モデルにバインドします。</span><span class="sxs-lookup"><span data-stu-id="383b2-182">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="383b2-183">`OnPostAsync` メソッドは、ページでフォーム データが投稿されたときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-183">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="383b2-184">モデル エラーがある場合は、投稿されたフォーム データと共にフォームが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-184">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="383b2-185">ほとんどのモデル エラーは、フォームが投稿される前にクライアント側でキャッチできます。</span><span class="sxs-lookup"><span data-stu-id="383b2-185">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="383b2-186">モデル エラーの例では、日付に変換できない日付フィールドの値が投稿されています。</span><span class="sxs-lookup"><span data-stu-id="383b2-186">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="383b2-187">クライアント側の検証とモデルの検証については、チュートリアルの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="383b2-187">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="383b2-188">モデル エラーがない場合、データは保存され、ブラウザーはインデックス ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="383b2-188">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="383b2-189">[作成] Razor ページ</span><span class="sxs-lookup"><span data-stu-id="383b2-189">The Create Razor Page</span></span>

<span data-ttu-id="383b2-190">次のように、*Pages/Movies/Create.cshtml* Razor ページ ファイルを確認します。</span><span class="sxs-lookup"><span data-stu-id="383b2-190">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="383b2-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="383b2-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="383b2-192">Visual Studio に、タグ ヘルパーで使用される独特な太字のフォントで `<form method="post">` タグが表示されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-192">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

<span data-ttu-id="383b2-193">![Create.cshtml ページの VS17 ビュー](page/_static/th.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="383b2-193">![VS17 view of Create.cshtml page](page/_static/th.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="383b2-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="383b2-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="383b2-195">`<form method="post">` などのタグ ヘルパーについては、「[ASP.NET Core のタグ ヘルパー](xref:mvc/views/tag-helpers/intro)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="383b2-195">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="383b2-196">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="383b2-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="383b2-197">Visual Studio for Mac に、タグ ヘルパーで使用される独特な太字のフォントで `<form method="post">` タグが表示されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-197">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="383b2-198">`<form method="post">` 要素は[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="383b2-198">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="383b2-199">フォーム タグ ヘルパーには自動的に[偽造防止トークン](xref:security/anti-request-forgery)が含まれます。</span><span class="sxs-lookup"><span data-stu-id="383b2-199">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="383b2-200">スキャフォールディング エンジンは、次のような、(ID を除く) モデルの各フィールドの Razor マークアップを作成します。</span><span class="sxs-lookup"><span data-stu-id="383b2-200">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="383b2-201">[検証タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` と ` <span asp-validation-for`) には検証エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="383b2-201">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="383b2-202">検証については、後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="383b2-202">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="383b2-203">[ラベル タグ ヘルパー](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) は、`Title` プロパティのラベル キャプションと `for` 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="383b2-203">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="383b2-204">[入力タグ ヘルパー](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) は [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 属性を使用し、クライアント側で jQuery 検証に必要な HTML 属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="383b2-204">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="383b2-205">[前へ:モデルの追加](xref:tutorials/razor-pages/model)
> [次:データベース](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="383b2-205">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>
