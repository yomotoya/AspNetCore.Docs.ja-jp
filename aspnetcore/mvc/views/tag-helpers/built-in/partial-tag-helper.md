---
title: ASP.NET Core の部分タグ ヘルパー
author: scottaddie
description: ASP.NET Core 部分タグ ヘルパーと、その各属性が部分ビューのレンダリングにおいて果たす役割について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: fea84621f185c4113147cf0dfd173704bc7b6d81
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274415"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="d3094-103">ASP.NET Core の部分タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="d3094-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="d3094-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="d3094-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="d3094-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d3094-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="d3094-106">概要</span><span class="sxs-lookup"><span data-stu-id="d3094-106">Overview</span></span>

<span data-ttu-id="d3094-107">部分タグ ヘルパーは、Razor Pages と MVC アプリで[部分ビュー](xref:mvc/views/partial)をレンダリングするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="d3094-108">考慮事項:</span><span class="sxs-lookup"><span data-stu-id="d3094-108">Consider that it:</span></span>

* <span data-ttu-id="d3094-109">ASP.NET Core 2.1 以降を必要とします。</span><span class="sxs-lookup"><span data-stu-id="d3094-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="d3094-110">[HTML ヘルパー構文](xref:mvc/views/partial#referencing-a-partial-view)の代替です。</span><span class="sxs-lookup"><span data-stu-id="d3094-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="d3094-111">部分ビューを非同期でレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="d3094-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="d3094-112">部分ビューをレンダリングするための HTML ヘルパー オプション:</span><span class="sxs-lookup"><span data-stu-id="d3094-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="d3094-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="d3094-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="d3094-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="d3094-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="d3094-115">*Product* モデルがこのドキュメント全体でサンプルとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="d3094-116">部分タグ ヘルパー属性のインベントリが続きます。</span><span class="sxs-lookup"><span data-stu-id="d3094-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="d3094-117">name</span><span class="sxs-lookup"><span data-stu-id="d3094-117">name</span></span>

<span data-ttu-id="d3094-118">`name` 属性は必須です。</span><span class="sxs-lookup"><span data-stu-id="d3094-118">The `name` attribute is required.</span></span> <span data-ttu-id="d3094-119">レンダリングする部分ビューの名前またはパスを示します。</span><span class="sxs-lookup"><span data-stu-id="d3094-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="d3094-120">部分ビュー名が指定されると、[ビューの検出](xref:mvc/views/overview#view-discovery)プロセスが開始します。</span><span class="sxs-lookup"><span data-stu-id="d3094-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="d3094-121">明示的なパスが指定されているとき、このプロセスは回避されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="d3094-122">次のマークアップでは明示的なパスが使用されており、*_ProductPartial.cshtml* が *Shared* フォルダーから読み込まれることを示しています。</span><span class="sxs-lookup"><span data-stu-id="d3094-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="d3094-123">[for](#for) 属性を使用し、バインディングのために部分ビューにモデルが渡されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="d3094-124">for</span><span class="sxs-lookup"><span data-stu-id="d3094-124">for</span></span>

<span data-ttu-id="d3094-125">`for` 属性によって、現在のモデルに対して評価する [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="d3094-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="d3094-126">`ModelExpression` によって `@Model.` 構文が推論されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="d3094-127">たとえば、`for="Product"` の代わりに `for="@Model.Product"` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d3094-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="d3094-128">この既定の推論動作は、`@` シンボルを使用してインライン式を定義することでオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="d3094-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="d3094-129">`for` 属性は [model](#model) 属性と共に使用できません。</span><span class="sxs-lookup"><span data-stu-id="d3094-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="d3094-130">次のマークアップでは、*_ProductPartial.cshtml* が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="d3094-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="d3094-131">部分ビューは、関連ページ モデルの `Product` プロパティにバインドされています。</span><span class="sxs-lookup"><span data-stu-id="d3094-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="d3094-132">モデル</span><span class="sxs-lookup"><span data-stu-id="d3094-132">model</span></span>

<span data-ttu-id="d3094-133">`model` 属性によって、部分ビューに渡すモデル インスタンスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="d3094-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="d3094-134">`model` 属性は [for](#for) 属性と共に使用できません。</span><span class="sxs-lookup"><span data-stu-id="d3094-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="d3094-135">次のマークアップでは、新しい `Product` オブジェクトがインスタンス化され、バインディングのために `model` 属性に渡されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="d3094-136">view-data</span><span class="sxs-lookup"><span data-stu-id="d3094-136">view-data</span></span>

<span data-ttu-id="d3094-137">`view-data` 属性によって、部分ビューに渡す [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="d3094-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="d3094-138">次のマークアップでは、ViewData コレクション全体が部分ビューで使えるようになります。</span><span class="sxs-lookup"><span data-stu-id="d3094-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="d3094-139">前述のコードでは、`IsNumberReadOnly` キーの値が `true` に設定されており、ViewData コレクションに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="d3094-140">その結果、次の部分ビュー内で `ViewData["IsNumberReadOnly"]` を利用できます。</span><span class="sxs-lookup"><span data-stu-id="d3094-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="d3094-141">この例では、`ViewData["IsNumberReadOnly"]` の値によって、*Number* フィールドを読み取り専用として表示するかどうかが決定されます。</span><span class="sxs-lookup"><span data-stu-id="d3094-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3094-142">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="d3094-142">Additional resources</span></span>

* [<span data-ttu-id="d3094-143">部分ビュー</span><span class="sxs-lookup"><span data-stu-id="d3094-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="d3094-144">弱い型指定のデータ (ViewData、ViewData 属性、ViewBag)</span><span class="sxs-lookup"><span data-stu-id="d3094-144">Weakly typed data (ViewData, ViewData attribute, and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
