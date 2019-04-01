---
title: ASP.NET Core の部分ビュー
author: ardalis
description: 部分ビューを使用して大規模なマークアップ ファイルを分割し、ASP.NET Core アプリの Web ページ間で共通するマークアップの重複を減らす方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: b7c1545007086053e879bce6781802959da77901
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327379"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="8b4f3-103">ASP.NET Core の部分ビュー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="8b4f3-104">作成者: [Steve Smith](https://ardalis.com/)、[Luke Latham](https://github.com/guardrex)、[Maher JENDOUBI](https://twitter.com/maherjend)、[Rick Anderson](https://twitter.com/RickAndMSFT)、および [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="8b4f3-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="8b4f3-105">部分ビューとは、別のマークアップ ファイルの出力表示の "*中に*"、HTML をレンダリングする [Razor](xref:mvc/views/razor) マークアップ ファイル (*.cshtml*) です。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8b4f3-106">マークアップ ファイルが*ビュー*と呼ばれている MVC アプリか、マークアップ ファイルが*ページ*と呼ばれている Razor Pages アプリのどちらかを開発している場合に、*部分ビュー*という用語が使用されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="8b4f3-107">このトピックでは総称として、MVC ビューと Razor Pages ページを "*マークアップ ファイル*" と呼びます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="8b4f3-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="8b4f3-109">部分ビューを使用する状況</span><span class="sxs-lookup"><span data-stu-id="8b4f3-109">When to use partial views</span></span>

<span data-ttu-id="8b4f3-110">部分ビューは、次の場合に効果的な方法です。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="8b4f3-111">大規模なマークアップ ファイルをより小さなコンポーネントに分割します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="8b4f3-112">複数の論理パーツで構成された大規模かつ複雑なマークアップ ファイルでは、各パーツが部分ビューに分かれた状態で操作すると便利です。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="8b4f3-113">全体のページ構造と部分ビューへの参照がマークアップのみに含まれるため、マークアップ ファイルのコードは管理性に優れています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="8b4f3-114">マークアップ ファイル間で共通するマークアップ コンテンツの重複を減らします。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="8b4f3-115">マークアップ ファイル間で同じマークアップ要素が使用されている場合、1 つの部分ビュー ファイルとなり、部分ビューによってマークアップ コンテンツの重複が除去されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="8b4f3-116">部分ビュー内でマークアップが変更された場合、その部分ビューを使用しているマークアップ ファイルの出力表示が更新されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="8b4f3-117">共通するレイアウト要素を維持するために、部分ビューを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="8b4f3-118">共通するレイアウト要素は、[_Layout.cshtml](xref:mvc/views/layout) ファイルで指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="8b4f3-119">マークアップを表示するために、複雑な表示ロジックやコード実行が必要になる場合は、部分ビューを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="8b4f3-120">部分ビューの代わりに、[ビュー コンポーネント](xref:mvc/views/view-components)を使用してください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="8b4f3-121">部分ビューを宣言する</span><span class="sxs-lookup"><span data-stu-id="8b4f3-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8b4f3-122">部分ビューは、*Views* フォルダー (MVC) または *Pages* フォルダー (Razor Pages) 内で保持される *.cshtml* マークアップ ファイルです。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="8b4f3-123">ASP.NET Core MVC では、コントローラーの <xref:Microsoft.AspNetCore.Mvc.ViewResult> が、ビューまたは部分ビューのどちらかを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="8b4f3-124">ASP.NET Core 2.2 の Razor Pages で、類似の機能が計画されています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-124">An analogous capability is planned for Razor Pages in ASP.NET Core 2.2.</span></span> <span data-ttu-id="8b4f3-125">Razor Pages では、<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> は <xref:Microsoft.AspNetCore.Mvc.PartialViewResult> を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-125">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="8b4f3-126">部分ビューの参照と表示については、「[部分ビューを参照する](#reference-a-partial-view)」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-126">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="8b4f3-127">MVC ビューやページ レンダリングとは異なり、部分ビューは *_ViewStart.cshtml* を実行しません。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-127">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="8b4f3-128">*_ViewStart.cshtml* の詳細については、<xref:mvc/views/layout> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-128">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="8b4f3-129">部分ビューのファイル名は、多くの場合アンダースコア (`_`) から始まります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-129">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="8b4f3-130">この名前付け規則は必須ではありませんが、ビューおよびページと部分ビューを視覚的に区別するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-130">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8b4f3-131">部分ビューは、*Views* フォルダー内で保持される *.cshtml* マークアップ ファイルです。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-131">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="8b4f3-132">コントローラーの <xref:Microsoft.AspNetCore.Mvc.ViewResult> は、ビューまたは部分ビューのどちらかを返すことができます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-132">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="8b4f3-133">MVC ビューのレンダリングとは異なり、部分ビューは *_ViewStart.cshtml* を実行しません。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-133">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="8b4f3-134">*_ViewStart.cshtml* の詳細については、<xref:mvc/views/layout> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-134">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="8b4f3-135">部分ビューのファイル名は、多くの場合アンダースコア (`_`) から始まります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-135">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="8b4f3-136">この名前付け規則は必須ではありませんが、ビューと部分ビューを視覚的に区別するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-136">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="8b4f3-137">部分ビューを参照する</span><span class="sxs-lookup"><span data-stu-id="8b4f3-137">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8b4f3-138">マークアップ ファイル内で、部分ビューを参照する方法はいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-138">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="8b4f3-139">アプリでは、次の非同期レンダリング手法のいずれかを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-139">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="8b4f3-140">部分タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-140">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="8b4f3-141">非同期の HTML ヘルパー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-141">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="8b4f3-142">マークアップ ファイル内で、部分ビューを参照するには、次の 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-142">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="8b4f3-143">非同期の HTML ヘルパー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-143">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="8b4f3-144">同期の HTML ヘルパー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-144">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="8b4f3-145">アプリでは、[非同期の HTML ヘルパー](#asynchronous-html-helper)を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-145">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="8b4f3-146">部分タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-146">Partial Tag Helper</span></span>

<span data-ttu-id="8b4f3-147">[部分タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)では、ASP.NET Core 2.1 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-147">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="8b4f3-148">部分タグ ヘルパーでは、非同期でコンテンツをレンダリングして、HTML のような構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-148">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="8b4f3-149">ファイル拡張子がある場合、タグ ヘルパーは、部分ビューを呼び出すマークアップ ファイルと同じフォルダー内に必ず配置されている部分ビューを参照します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-149">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="8b4f3-150">次の例では、アプリ ルートから部分ビューを参照しています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-150">The following example references a partial view from the app root.</span></span> <span data-ttu-id="8b4f3-151">チルダとスラッシュ (`~/`) またはスラッシュ (`/`) から始まるパスは、次のようにアプリ ルートを参照します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-151">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="8b4f3-152">**Razor ページ**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-152">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="8b4f3-153">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-153">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="8b4f3-154">次の例では、相対パスを使って部分ビューを参照しています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-154">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="8b4f3-155">詳細については、「<xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-155">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="8b4f3-156">非同期の HTML ヘルパー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-156">Asynchronous HTML Helper</span></span>

<span data-ttu-id="8b4f3-157">HTML ヘルパーを使用している場合、ベスト プラクティスは <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*> を使用することです。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-157">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="8b4f3-158">`PartialAsync` は、<xref:System.Threading.Tasks.Task%601> でラップされた <xref:Microsoft.AspNetCore.Html.IHtmlContent> 型を返します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-158">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task%601>.</span></span> <span data-ttu-id="8b4f3-159">待機中の呼び出しの前に `@` 文字を付与することで、メソッドが参照されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-159">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="8b4f3-160">ファイル拡張子がある場合、HTML ヘルパーは、部分ビューを呼び出すマークアップ ファイルと同じフォルダー内に必ず配置されている部分ビューを参照します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-160">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="8b4f3-161">次の例では、アプリ ルートから部分ビューを参照しています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-161">The following example references a partial view from the app root.</span></span> <span data-ttu-id="8b4f3-162">チルダとスラッシュ (`~/`) またはスラッシュ (`/`) から始まるパスは、次のようにアプリ ルートを参照します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-162">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8b4f3-163">**Razor ページ**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-163">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="8b4f3-164">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-164">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="8b4f3-165">次の例では、相対パスを使って部分ビューを参照しています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-165">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="8b4f3-166">代わりに、<xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*> を使って部分ビューをレンダリングすることもできます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-166">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="8b4f3-167">このメソッドは <xref:Microsoft.AspNetCore.Html.IHtmlContent> を返しません。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-167">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="8b4f3-168">レンダリングされた出力を直接応答にストリーミングします。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-168">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="8b4f3-169">メソッドが結果を返さないため、Razor コード ブロック内で呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-169">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="8b4f3-170">`RenderPartialAsync` は表示されるコンテンツをストリーム配信するため、一部のシナリオではより高いパフォーマンスが得られます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-170">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="8b4f3-171">パフォーマンスが重要な場合には、両方の手法を使用してページのベンチマークを実行し、より迅速に応答を生成する手法を採用します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-171">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="8b4f3-172">同期の HTML ヘルパー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-172">Synchronous HTML Helper</span></span>

<span data-ttu-id="8b4f3-173"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> と <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> はそれぞれ、`PartialAsync` と `RenderPartialAsync` の同期に相当します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-173"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="8b4f3-174">デッドロックが発生するシナリオがあるため、同期に相当する機能はお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-174">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="8b4f3-175">同期メソッドは、将来のリリースでは削除対象になります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-175">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8b4f3-176">コードを実行する必要がある場合は、部分ビューの代わりに[ビュー コンポーネント](xref:mvc/views/view-components)を使用してください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-176">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8b4f3-177">`Partial` または `RenderPartial` を呼び出すと、Visual Studio アナライザーの警告が発生します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-177">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="8b4f3-178">たとえば、`Partial` を指定すると、次の警告メッセージが生成されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-178">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="8b4f3-179">IHtmlHelper.Partial を使用すると、アプリケーションのデッドロックが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-179">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="8b4f3-180">&lt;部分&gt;タグ ヘルパーまたは IHtmlHelper.PartialAsync を使用することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-180">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="8b4f3-181">`@Html.Partial` の呼び出しを、`@await Html.PartialAsync` または[部分タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-181">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="8b4f3-182">部分タグ ヘルパーの移行の詳細については、「[HTML ヘルパーから移行する](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-182">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="8b4f3-183">部分ビューの検出</span><span class="sxs-lookup"><span data-stu-id="8b4f3-183">Partial view discovery</span></span>

<span data-ttu-id="8b4f3-184">部分ビューがファイル拡張子を指定せずに名前で参照された場合、所定の順序で次の場所が検索されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-184">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8b4f3-185">**Razor ページ**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-185">**Razor Pages**</span></span>

1. <span data-ttu-id="8b4f3-186">現在実行中のページのフォルダー</span><span class="sxs-lookup"><span data-stu-id="8b4f3-186">Currently executing page's folder</span></span>
1. <span data-ttu-id="8b4f3-187">ページのフォルダーの上にあるディレクトリ グラフ</span><span class="sxs-lookup"><span data-stu-id="8b4f3-187">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="8b4f3-188">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-188">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="8b4f3-189">部分ビューの検索には、次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-189">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="8b4f3-190">部分ビューが異なるフォルダー内にある場合は、同じファイル名の別の部分ビューが許可されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-190">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="8b4f3-191">ファイル拡張子を指定せずに部分ビューを名前で参照しており、かつ、部分ビューが呼び出し元のフォルダーと *Shared* フォルダーの両方に存在する場合、呼び出し元のフォルダーにある部分ビューが、部分ビューとして機能します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-191">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="8b4f3-192">部分ビューが呼び出し元のフォルダーに存在しない場合、部分ビューは *Shared* フォルダーから提供されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-192">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="8b4f3-193">*Shared* フォルダーの部分ビューは、"*共有の部分ビュー*" または "*既定の部分ビュー*" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-193">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="8b4f3-194">部分ビューは "*チェーン*" になることがある &mdash; 呼び出しによって循環参照が形成されない場合、部分ビューが別の部分ビューを呼び出す場合があります。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-194">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="8b4f3-195">相対パスは常に、ファイルのルートや親ではなく、現在のファイルを基準とします。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-195">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="8b4f3-196">部分ビューで定義されている [Razor](xref:mvc/views/razor) の `section` は、親のマークアップ ファイルには表示されません。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-196">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="8b4f3-197">`section` は定義されている部分ビューにのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-197">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="8b4f3-198">部分ビューからデータにアクセスする</span><span class="sxs-lookup"><span data-stu-id="8b4f3-198">Access data from partial views</span></span>

<span data-ttu-id="8b4f3-199">部分ビューがインスタンス化されると、親の `ViewData` ディクショナリの*コピー*を取得します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-199">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="8b4f3-200">親ビュー内のデータに対して行われた更新は、親ビューでは保持されません。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-200">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="8b4f3-201">部分ビューで変更された `ViewData` は、部分ビューから返されるときに失われます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-201">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="8b4f3-202">次の例に、[ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) のインスタンスを部分ビューに渡す方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-202">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="8b4f3-203">部分ビューにモデルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-203">You can pass a model into a partial view.</span></span> <span data-ttu-id="8b4f3-204">モデルは、カスタム オブジェクトでもかまいません。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-204">The model can be a custom object.</span></span> <span data-ttu-id="8b4f3-205">`PartialAsync` (呼び出し元にコンテンツのブロックを表示する) または `RenderPartialAsync` (コンテンツを出力にストリーム配信する) を使って、モデルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-205">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8b4f3-206">**Razor ページ**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-206">**Razor Pages**</span></span>

<span data-ttu-id="8b4f3-207">サンプル アプリの次のマークアップは、*Pages/ArticlesRP/ReadRP.cshtml* ページが元になっています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-207">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="8b4f3-208">ページには、2 つの部分ビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-208">The page contains two partial views.</span></span> <span data-ttu-id="8b4f3-209">2 番目の部分ビューは、モデルと `ViewData` を部分ビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-209">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="8b4f3-210">`ViewDataDictionary` のコンストラクター オーバーロードは、既存の `ViewData` ディクショナリを維持したまま、新しい `ViewData` ディクショナリを渡すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-210">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

<span data-ttu-id="8b4f3-211">*Pages/Shared/_AuthorPartialRP.cshtml* は、*ReadRP.cshtml* マークアップ ファイルで参照される最初の部分ビューです。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-211">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="8b4f3-212">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* は、*ReadRP.cshtml* マークアップ ファイルで参照される 2 番目の部分ビューです。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-212">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="8b4f3-213">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8b4f3-213">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="8b4f3-214">サンプル アプリの以下のマークアップは、*Views/Articles/Read.cshtml* ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-214">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="8b4f3-215">ビューには、2 つの部分ビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-215">The view contains two partial views.</span></span> <span data-ttu-id="8b4f3-216">2 番目の部分ビューは、モデルと `ViewData` を部分ビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-216">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="8b4f3-217">`ViewDataDictionary` のコンストラクター オーバーロードは、既存の `ViewData` ディクショナリを維持したまま、新しい `ViewData` ディクショナリを渡すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-217">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

<span data-ttu-id="8b4f3-218">*Views/Shared/_AuthorPartial.cshtml* は、*ReadRP.cshtml* マークアップ ファイルで参照される最初の部分ビューです。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-218">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="8b4f3-219">*Views/Articles/_ArticleSection.cshtml* は、*Read.cshtml* マークアップ ファイルで参照される 2 番目の部分ビューです。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-219">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="8b4f3-220">この部分は実行時に、親のマークアップファイルの出力表示にレンダリングされます。親ビュー自体は共有されている *_Layout.cshtml* 内にレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-220">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="8b4f3-221">最初の部分ビューは、記事の作成者の名前と発行日を表示します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-221">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="8b4f3-222">アブラハム リンカーン</span><span class="sxs-lookup"><span data-stu-id="8b4f3-222">Abraham Lincoln</span></span>
>
> <span data-ttu-id="8b4f3-223">この部分ビューは&lt;共有されている部分ビューのファイル パス&gt;が元になっている。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-223">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="8b4f3-224">1863 年 11 月 19 日 12 時 00 分 00 秒</span><span class="sxs-lookup"><span data-stu-id="8b4f3-224">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="8b4f3-225">2 番目の部分ビューは、次に示す記事のセクションを表示します。</span><span class="sxs-lookup"><span data-stu-id="8b4f3-225">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="8b4f3-226">セクション 1 のインデックス: 0</span><span class="sxs-lookup"><span data-stu-id="8b4f3-226">Section One Index: 0</span></span>
>
> <span data-ttu-id="8b4f3-227">87 年前...</span><span class="sxs-lookup"><span data-stu-id="8b4f3-227">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="8b4f3-228">セクション 2 のインデックス: 1</span><span class="sxs-lookup"><span data-stu-id="8b4f3-228">Section Two Index: 1</span></span>
>
> <span data-ttu-id="8b4f3-229">大きな内紛の渦中で、試した...</span><span class="sxs-lookup"><span data-stu-id="8b4f3-229">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="8b4f3-230">セクション 3 のインデックス: 2</span><span class="sxs-lookup"><span data-stu-id="8b4f3-230">Section Three Index: 2</span></span>
>
> <span data-ttu-id="8b4f3-231">しかし、広義では、我々が献身することはできない...</span><span class="sxs-lookup"><span data-stu-id="8b4f3-231">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b4f3-232">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8b4f3-232">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
