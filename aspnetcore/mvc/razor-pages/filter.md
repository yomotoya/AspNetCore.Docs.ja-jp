---
title: ASP.NET Core の Razor ページのフィルター メソッド
author: Rick-Anderson
description: ASP.NET Core の Razor ページのフィルター メソッドを作成する方法を説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/5/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/filter
ms.openlocfilehash: b04253b9240cb88c4f0d3824a4b9fda947d6da08
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
ms.locfileid: "31585193"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="f4709-103">ASP.NET Core の Razor ページのフィルター メソッド</span><span class="sxs-lookup"><span data-stu-id="f4709-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f4709-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f4709-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f4709-105">Razor ページのフィルターである [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) および [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) を使用すると、Razor ページ ハンドラーの実行の前後に Razor ページでコードを実行できます。</span><span class="sxs-lookup"><span data-stu-id="f4709-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="f4709-106">Razor ページ フィルターは、個々のページ ハンドラー メソッドに適用できないことを除き、[ASP.NET Core MVC アクション フィルター](xref:mvc/controllers/filters#action-filters)と類似しています。</span><span class="sxs-lookup"><span data-stu-id="f4709-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span> 

<span data-ttu-id="f4709-107">Razor ページ フィルターとは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f4709-107">Razor Page filters:</span></span>

* <span data-ttu-id="f4709-108">モデルのバインドが行われる前の、ハンドラー メソッドが選択された後にコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="f4709-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="f4709-109">モデルのバインドの完了後の、ハンドラー メソッドの実行前にコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="f4709-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="f4709-110">ハンドラー メソッドの実行後にコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="f4709-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="f4709-111">ページまたはグローバルに実装できます。</span><span class="sxs-lookup"><span data-stu-id="f4709-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="f4709-112">特定のページ ハンドラー メソッドには適用できません。</span><span class="sxs-lookup"><span data-stu-id="f4709-112">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="f4709-113">コードは、ページ コンストラクターまたはミドルウェアを使用してハンドラー メソッドの実行前に実行できますが、[HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext) にアクセスできるのは Razor ページ フィルターのみです。</span><span class="sxs-lookup"><span data-stu-id="f4709-113">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="f4709-114">フィルターには、`HttpContext` へのアクセスを提供する [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) 派生のパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="f4709-114">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="f4709-115">たとえば、「[フィルター属性を実装する](#ifa)」のサンプルでは、応答にヘッダーが追加されます。これは、コンストラクターやミドルウェアでは実行できません。</span><span class="sxs-lookup"><span data-stu-id="f4709-115">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="f4709-116">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f4709-116">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f4709-117">Razor ページ フィルターには、グローバルまたはページ レベルで適用できる次のメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="f4709-117">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="f4709-118">同期メソッド:</span><span class="sxs-lookup"><span data-stu-id="f4709-118">Synchronous methods:</span></span>

    * <span data-ttu-id="f4709-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : モデルのバインドが行われる前の、ハンドラー メソッドが選択された後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4709-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="f4709-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : モデルのバインドの完了後の、ハンドラー メソッドの実行前に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4709-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
    * <span data-ttu-id="f4709-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : アクション結果の前の、ハンドラー メソッドの実行前に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4709-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="f4709-122">非同期メソッド:</span><span class="sxs-lookup"><span data-stu-id="f4709-122">Asynchronous methods:</span></span>

    * <span data-ttu-id="f4709-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : モデルのバインドが行われる前の、ハンドラー メソッドが選択された後に非同期で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4709-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="f4709-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : モデルのバインドの完了後の、ハンドラー メソッドが呼び出される前に非同期で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4709-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="f4709-125">フィルター インターフェイスの同期と非同期バージョンの両方ではなく、**いずれか**を実装します。</span><span class="sxs-lookup"><span data-stu-id="f4709-125">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="f4709-126">フレームワークは、最初にフィルターが非同期インターフェイスを実装しているかどうかをチェックして、している場合はそれを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f4709-126">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="f4709-127">していない場合は、同期インターフェイスのメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f4709-127">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="f4709-128">両方のインターフェイスを実装した場合、同期メソッドのみが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f4709-128">If both interfaces are implemented, only the async methods are be called.</span></span> <span data-ttu-id="f4709-129">ページの上書きでもこの規則は同じです。上書きの同期バージョンまたは非同期バージョンを実装でき、両方はできません。</span><span class="sxs-lookup"><span data-stu-id="f4709-129">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="f4709-130">Razor ページにフィルターをグローバルに実装する</span><span class="sxs-lookup"><span data-stu-id="f4709-130">Implement Razor Page filters globally</span></span>

<span data-ttu-id="f4709-131">`IAsyncPageFilter` は、次のコードによって実装されます。</span><span class="sxs-lookup"><span data-stu-id="f4709-131">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="f4709-132">前述のコードでは、[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) は不要です。</span><span class="sxs-lookup"><span data-stu-id="f4709-132">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="f4709-133">これは、アプリケーションのトレース情報を提供するためにサンプルで使用されています。</span><span class="sxs-lookup"><span data-stu-id="f4709-133">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="f4709-134">次のコードは、`Startup` クラスで `SampleAsyncPageFilter` を有効にします。</span><span class="sxs-lookup"><span data-stu-id="f4709-134">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="f4709-135">次は、完全な `Startup` クラスのコードです。</span><span class="sxs-lookup"><span data-stu-id="f4709-135">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="f4709-136">次のコードは、`AddFolderApplicationModelConvention` を呼び出し、*/subFolder* のページにのみ `SampleAsyncPageFilter` を適用します。</span><span class="sxs-lookup"><span data-stu-id="f4709-136">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="f4709-137">次のコードは、同期 `IPageFilter` を実装します。</span><span class="sxs-lookup"><span data-stu-id="f4709-137">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="f4709-138">次のコードは、`SamplePageFilter` を有効にします。</span><span class="sxs-lookup"><span data-stu-id="f4709-138">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"
## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="f4709-139">フィルター メソッドを上書きして Razor ページにフィルターを実装する</span><span class="sxs-lookup"><span data-stu-id="f4709-139">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="f4709-140">次のコードは、同期 Razor ページ フィルターを上書きします。</span><span class="sxs-lookup"><span data-stu-id="f4709-140">The following code overrides the synchronous Razor Page filters:</span></span>

<span data-ttu-id="f4709-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="f4709-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span></span>

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a><span data-ttu-id="f4709-142">フィルター属性を実装する</span><span class="sxs-lookup"><span data-stu-id="f4709-142">Implement a filter attribute</span></span>

<span data-ttu-id="f4709-143">組み込みの属性ベースのフィルターである [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) フィルターはサブクラス化することができます。</span><span class="sxs-lookup"><span data-stu-id="f4709-143">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="f4709-144">次のフィルターは、応答にヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="f4709-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="f4709-145">次のコードは、`AddHeader` 属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="f4709-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="f4709-146">順序を上書きする手順については、「[既定の順序の上書き](xref:mvc/controllers/filters#overriding-the-default-order)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f4709-146">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="f4709-147">フィルターからフィルター パイプラインをショート サーキットする手順については、「[キャンセルとショート サーキット](xref:mvc/controllers/filters#cancellation-and-short-circuiting)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f4709-147">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a><span data-ttu-id="f4709-148">Authorize フィルター属性</span><span class="sxs-lookup"><span data-stu-id="f4709-148">Authorize filter attribute</span></span>

<span data-ttu-id="f4709-149">`PageModel` に [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) 属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="f4709-149">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
