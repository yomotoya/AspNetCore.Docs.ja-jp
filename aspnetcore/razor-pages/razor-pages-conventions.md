---
title: ASP.NET Core での Razor ページのルートとアプリの規則
author: guardrex
description: ルートとアプリ モデル プロバイダーの規則が、ページのルーティング、検出、および処理の制御にどのように役立つかについて確認します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/17/2018
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: ea4f785dc8a64b430e312fd122a4d3184b61949e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011863"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="d465c-103">ASP.NET Core での Razor ページのルートとアプリの規則</span><span class="sxs-lookup"><span data-stu-id="d465c-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="d465c-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d465c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d465c-105">[ページ ルートとアプリ モデル プロバイダーの規則](xref:mvc/controllers/application-model#conventions)を使用して、Razor ページ アプリでページのルーティング、検出、および処理を制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d465c-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="d465c-106">個別のページにカスタムのページ ルートを構成する必要がある場合、このトピックで後述される「[AddPageRoute convention](#configure-a-page-route)」で、ページへのルーティングを構成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="d465c-107">ページ ルートを指定、ルート セグメントを追加またはルートにパラメーターを追加使用ページの`@page`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="d465c-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="d465c-108">詳細については、次を参照してください。[カスタム ルート](xref:razor-pages/index#custom-routes)します。</span><span class="sxs-lookup"><span data-stu-id="d465c-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="d465c-109">ルート セグメントまたはパラメーター名として使用できません。 予約語があります。</span><span class="sxs-lookup"><span data-stu-id="d465c-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="d465c-110">詳細については、次を参照してください。[ルーティング: 予約された名前のルーティング](xref:fundamentals/routing#reserved-routing-names)します。</span><span class="sxs-lookup"><span data-stu-id="d465c-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="d465c-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d465c-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range="= aspnetcore-2.0"

| <span data-ttu-id="d465c-112">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d465c-112">Scenario</span></span> | <span data-ttu-id="d465c-113">このサンプルでは、次のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="d465c-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="d465c-114">モデルの規則</span><span class="sxs-lookup"><span data-stu-id="d465c-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="d465c-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="d465c-115">Conventions.Add</span></span><ul><li><span data-ttu-id="d465c-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="d465c-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-117">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="d465c-118">ルート テンプレートとヘッダーをアプリのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-118">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="d465c-119">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="d465c-119">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="d465c-120">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-120">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="d465c-121">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-121">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="d465c-122">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="d465c-122">AddPageRoute</span></span></li></ul> | <span data-ttu-id="d465c-123">ルート テンプレートをフォルダー内のページおよび単一ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-123">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="d465c-124">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="d465c-124">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="d465c-125">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-125">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="d465c-126">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-126">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d465c-127">ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</span><span class="sxs-lookup"><span data-stu-id="d465c-127">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="d465c-128">ヘッダーをフォルダー内のページに追加し、ヘッダーを単一ページに追加し、ヘッダーをアプリのページに追加するように[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を構成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-128">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="d465c-129">既定のページ アプリ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="d465c-129">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="d465c-130">既定のページ モデル プロバイダーを置き換えて、ハンドラー名の規則を変更します。</span><span class="sxs-lookup"><span data-stu-id="d465c-130">Replace the default page model provider to change the conventions for handler names.</span></span> |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="d465c-131">シナリオ</span><span class="sxs-lookup"><span data-stu-id="d465c-131">Scenario</span></span> | <span data-ttu-id="d465c-132">このサンプルでは、次のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="d465c-132">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="d465c-133">モデルの規則</span><span class="sxs-lookup"><span data-stu-id="d465c-133">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="d465c-134">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="d465c-134">Conventions.Add</span></span><ul><li><span data-ttu-id="d465c-135">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-135">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="d465c-136">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-136">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d465c-137">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-137">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="d465c-138">ルート テンプレートとヘッダーをアプリのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-138">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="d465c-139">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="d465c-139">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="d465c-140">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-140">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="d465c-141">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-141">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="d465c-142">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="d465c-142">AddPageRoute</span></span></li></ul> | <span data-ttu-id="d465c-143">ルート テンプレートをフォルダー内のページおよび単一ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-143">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="d465c-144">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="d465c-144">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="d465c-145">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-145">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="d465c-146">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="d465c-146">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="d465c-147">ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</span><span class="sxs-lookup"><span data-stu-id="d465c-147">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="d465c-148">ヘッダーをフォルダー内のページに追加し、ヘッダーを単一ページに追加し、ヘッダーをアプリのページに追加するように[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を構成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-148">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="d465c-149">既定のページ アプリ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="d465c-149">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="d465c-150">既定のページ モデル プロバイダーを置き換えて、ハンドラー名の規則を変更します。</span><span class="sxs-lookup"><span data-stu-id="d465c-150">Replace the default page model provider to change the conventions for handler names.</span></span> |

::: moniker-end

<span data-ttu-id="d465c-151">Razor ページの規則は、`Startup` クラス内のサービス コレクションの [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) に [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) 拡張メソッドを使用することで、追加および構成されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-151">Razor Pages conventions are added and configured using the [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) extension method to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) on the service collection in the `Startup` class.</span></span> <span data-ttu-id="d465c-152">次の規則の例は、このトピックで後述されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-152">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="d465c-153">ルートの順序</span><span class="sxs-lookup"><span data-stu-id="d465c-153">Route order</span></span>

<span data-ttu-id="d465c-154">ルートの指定、 <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> (ルートの一致) を処理するためです。</span><span class="sxs-lookup"><span data-stu-id="d465c-154">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="d465c-155">順番</span><span class="sxs-lookup"><span data-stu-id="d465c-155">Order</span></span>            | <span data-ttu-id="d465c-156">動作</span><span class="sxs-lookup"><span data-stu-id="d465c-156">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="d465c-157">-1</span><span class="sxs-lookup"><span data-stu-id="d465c-157">-1</span></span>               | <span data-ttu-id="d465c-158">ルートは、他のルートが処理される前に処理されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-158">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="d465c-159">0</span><span class="sxs-lookup"><span data-stu-id="d465c-159">0</span></span>                | <span data-ttu-id="d465c-160">順序が指定されていない (既定値)。</span><span class="sxs-lookup"><span data-stu-id="d465c-160">Order isn't specified (default value).</span></span> <span data-ttu-id="d465c-161">割り当てない`Order`(`Order = null`) 既定値は、ルート`Order`を処理するための 0 (ゼロ)。</span><span class="sxs-lookup"><span data-stu-id="d465c-161">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="d465c-162">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="d465c-162">1, 2, &hellip; n</span></span> | <span data-ttu-id="d465c-163">ルートの処理順序を指定します。</span><span class="sxs-lookup"><span data-stu-id="d465c-163">Specifies the route processing order.</span></span> |

<span data-ttu-id="d465c-164">ルートの処理規則を確立するには。</span><span class="sxs-lookup"><span data-stu-id="d465c-164">Route processing is established by convention:</span></span>

* <span data-ttu-id="d465c-165">ルートが順番に処理される (-1、0、1、2、 &hellip; n)。</span><span class="sxs-lookup"><span data-stu-id="d465c-165">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="d465c-166">ルートがある同じ`Order`最も低い特定のルートの順に特定のルートが一致します。</span><span class="sxs-lookup"><span data-stu-id="d465c-166">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="d465c-167">ときに、同じルート`Order`と同じ数のパラメーターが要求 URL と一致、ルートに追加された順序で処理、<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>します。</span><span class="sxs-lookup"><span data-stu-id="d465c-167">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="d465c-168">可能であれば、確立されたルートの処理順序に応じてしないでください。</span><span class="sxs-lookup"><span data-stu-id="d465c-168">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="d465c-169">一般に、ルーティングが URL に一致する適切なルートを選択します。</span><span class="sxs-lookup"><span data-stu-id="d465c-169">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="d465c-170">ルートを設定する必要がありますと`Order`をルーティングするプロパティの要求を正しく、アプリのルーティング スキームは、おそらくクライアントに混乱を維持するために脆弱です。</span><span class="sxs-lookup"><span data-stu-id="d465c-170">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="d465c-171">アプリのルーティング スキームを簡略化しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="d465c-171">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="d465c-172">サンプル アプリでは、明示的なルートを 1 つのアプリを使用してルーティングのシナリオをいくつかを示すために注文の処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="d465c-172">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="d465c-173">ただし、設定のルートのプラクティスを回避しようとする必要があります`Order`運用アプリでします。</span><span class="sxs-lookup"><span data-stu-id="d465c-173">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="d465c-174">ルーティングの razor ページと MVC コント ローラーのルーティングの共有を実装します。</span><span class="sxs-lookup"><span data-stu-id="d465c-174">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="d465c-175">MVC のトピックでルートの順序については、「[コント ローラー アクションへのルーティング: 属性ルートの順序付け](xref:mvc/controllers/routing#ordering-attribute-routes)します。</span><span class="sxs-lookup"><span data-stu-id="d465c-175">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="d465c-176">モデルの規則</span><span class="sxs-lookup"><span data-stu-id="d465c-176">Model conventions</span></span>

<span data-ttu-id="d465c-177">[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) の委任を追加して、Razor ページに適用する[モデル規則](xref:mvc/controllers/application-model#conventions)を追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-177">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

<span data-ttu-id="d465c-178">**すべてのページにルート モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="d465c-178">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="d465c-179">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成し、ページ ルート モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-179">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page route model construction.</span></span>

<span data-ttu-id="d465c-180">サンプル アプリでは、`{globalTemplate?}` ルート テンプレートをアプリ内のすべてのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-180">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="d465c-181"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> の <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> プロパティに `1` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-181">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="d465c-182">これにより、次のルートが一致のサンプル アプリで動作します。</span><span class="sxs-lookup"><span data-stu-id="d465c-182">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="d465c-183">ルート テンプレート`TheContactPage/{text?}`トピックの後半で追加されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-183">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="d465c-184">Contact ページのルートの既定の順序を持つ`null`(`Order = 0`) する前に、一致するよう、`{globalTemplate?}`ルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="d465c-184">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="d465c-185">`{aboutTemplate?}`トピックの後半でルート テンプレートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-185">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d465c-186">`{aboutTemplate?}` テンプレートの `Order` には `2` が指定されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-186">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d465c-187">[About] ページが `/About/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = 1`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["aboutTemplate"]` (`Order = 2`) には読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="d465c-187">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="d465c-188">`{otherPagesTemplate?}`トピックの後半でルート テンプレートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-188">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="d465c-189">`{otherPagesTemplate?}` テンプレートの `Order` には `2` が指定されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-189">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="d465c-190">いずれかのページ、*ページ/OtherPages*フォルダーはルート パラメーターで要求されます (たとえば、 `/OtherPages/Page1/RouteDataValue`)、"RouteDataValue"に読み込まれる`RouteData.Values["globalTemplate"]`(`Order = 1`) および not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`)設定により、`Order`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d465c-190">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d465c-191">可能であれば、任意の場所を設定しないでください、 `Order`、その結果は`Order = 0`します。</span><span class="sxs-lookup"><span data-stu-id="d465c-191">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d465c-192">適切なルートを選択するルーティングに依存します。</span><span class="sxs-lookup"><span data-stu-id="d465c-192">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d465c-193">Razor ページ オプション ([規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)の追加など) は、MVC が `Startup.ConfigureServices` のサービス コレクションに追加されたときに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-193">Razor Pages options, such as adding [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d465c-194">例については、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d465c-194">For an example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/).</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="d465c-195">`localhost:5000/About/GlobalRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="d465c-195">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![[About] ページは、GlobalRouteValue のルート セグメントで要求されます。](razor-pages-conventions/_static/about-page-global-template.png)

<span data-ttu-id="d465c-198">**すべてのページにアプリ モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="d465c-198">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="d465c-199">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成し、ページ アプリ モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-199">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page app model construction.</span></span>

<span data-ttu-id="d465c-200">この規則やこのトピックで後述されるその他の規則のデモを実行するには、サンプル アプリに `AddHeaderAttribute` クラスを含めます。</span><span class="sxs-lookup"><span data-stu-id="d465c-200">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="d465c-201">クラス コンストラクターは、`name` 文字列と `values` 文字列配列を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="d465c-201">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="d465c-202">これらの値は、応答ヘッダーを設定するために、その `OnResultExecuting` メソッド内で使用されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-202">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="d465c-203">完全クラスは、このトピックで後述される「[ページ モデル アクション規則](#page-model-action-conventions)」セクションで示されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-203">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="d465c-204">サンプル アプリでは、`AddHeaderAttribute` クラスを使用して、ヘッダー (`GlobalHeader`) をアプリ内のすべてのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-204">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="d465c-205">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d465c-205">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="d465c-206">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="d465c-206">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、GlobalHeader が追加されたことを示しています。](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d465c-208">**すべてのページにハンドラー モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="d465c-208">**Add a handler model convention to all pages**</span></span>

<span data-ttu-id="d465c-209">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) を作成し、ページ ハンドラー モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-209">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page handler model construction.</span></span>

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

<span data-ttu-id="d465c-210">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d465c-210">`Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="d465c-211">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="d465c-211">Page route action conventions</span></span>

<span data-ttu-id="d465c-212">[IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) から派生する既定のルート モデル プロバイダーは、ページ ルートを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d465c-212">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="d465c-213">**フォルダー ルート モデル規則**</span><span class="sxs-lookup"><span data-stu-id="d465c-213">**Folder route model convention**</span></span>

<span data-ttu-id="d465c-214">[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) を使用して、指定したフォルダーのページにある [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-214">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="d465c-215">サンプル アプリでは `AddFolderRouteModelConvention` を使用して、`{otherPagesTemplate?}` ルート テンプレートを *OtherPages* フォルダーのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-215">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="d465c-216"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> の <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> プロパティに `2` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-216">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d465c-217">これにより、テンプレートを`{globalTemplate?}`(にこのトピックの設定`1`) 単一のルート値を指定した場合の最初のルート データ値の位置が優先されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-217">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d465c-218">場合内のページ、*ページ/OtherPages*フォルダーはルート パラメーターの値で要求されます (たとえば、 `/OtherPages/Page1/RouteDataValue`)、"RouteDataValue"は読み込ま`RouteData.Values["globalTemplate"]`(`Order = 1`) および not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`)設定により、`Order`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d465c-218">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d465c-219">可能であれば、任意の場所を設定しないでください、 `Order`、その結果は`Order = 0`します。</span><span class="sxs-lookup"><span data-stu-id="d465c-219">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d465c-220">適切なルートを選択するルーティングに依存します。</span><span class="sxs-lookup"><span data-stu-id="d465c-220">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d465c-221">`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` でサンプルの Page1 ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="d465c-221">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![OtherPages フォルダーの Page1 は、GlobalRouteValue と OtherPagesRouteValue のルート セグメントで要求されます。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="d465c-224">**ページ ルート モデル規則**</span><span class="sxs-lookup"><span data-stu-id="d465c-224">**Page route model convention**</span></span>

<span data-ttu-id="d465c-225">[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) を使用して、指定した名前でページの [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-225">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="d465c-226">サンプル アプリでは `AddPageRouteModelConvention` を使用して、`{aboutTemplate?}` ルート テンプレートを [About] ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-226">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="d465c-227"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> の <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> プロパティに `2` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-227">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="d465c-228">これにより、テンプレートを`{globalTemplate?}`(にこのトピックの設定`1`) 単一のルート値を指定した場合の最初のルート データ値の位置が優先されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-228">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="d465c-229">[About] ページがあるルート パラメーター値で要求されたかどうか`/About/RouteDataValue`、"RouteDataValue"は読み込ま`RouteData.Values["globalTemplate"]`(`Order = 1`) および not `RouteData.Values["aboutTemplate"]` (`Order = 2`) 設定により、`Order`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d465c-229">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="d465c-230">可能であれば、任意の場所を設定しないでください、 `Order`、その結果は`Order = 0`します。</span><span class="sxs-lookup"><span data-stu-id="d465c-230">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="d465c-231">適切なルートを選択するルーティングに依存します。</span><span class="sxs-lookup"><span data-stu-id="d465c-231">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="d465c-232">`localhost:5000/About/GlobalRouteValue/AboutRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="d465c-232">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![[About] ページは、GlobalRouteValue と AboutRouteValue のルート セグメントで要求されます。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="d465c-235">ページ ルートの構成</span><span class="sxs-lookup"><span data-stu-id="d465c-235">Configure a page route</span></span>

<span data-ttu-id="d465c-236">[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) を使用して、特定のページ パスでページへのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-236">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="d465c-237">そのページに対して生成されたリンクでは、指定したルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="d465c-237">Generated links to the page use your specified route.</span></span> <span data-ttu-id="d465c-238">`AddPageRoute` では、`AddPageRouteModelConvention` を使用してルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="d465c-238">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="d465c-239">サンプル アプリでは、*Contact.cshtml* の `/TheContactPage` へのルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-239">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="d465c-240">[Contact] ページには、既定のルート経由の `/Contact` でアクセスすることもできます。</span><span class="sxs-lookup"><span data-stu-id="d465c-240">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="d465c-241">サンプル アプリの [Contact] ページに対するカスタム ルートでは、省略可能な `text` ルート セグメント (`{text?}`) を許可します。</span><span class="sxs-lookup"><span data-stu-id="d465c-241">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="d465c-242">また、訪問者が `/Contact` ルートでページにアクセスする場合、ページの `@page` ディレクティブにはこの省略可能なセグメントも含まれます。</span><span class="sxs-lookup"><span data-stu-id="d465c-242">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="d465c-243">レンダリングされたページの **Contact** リンク用に生成された URL には、更新されたルートが反映されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d465c-243">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![ナビゲーション バーのサンプル アプリの [Contact] リンク](razor-pages-conventions/_static/contact-link.png)

![レンダリングされた HTML の [Contact] リンクを調べると、href に '/TheContactPage' が設定されています](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="d465c-246">通常のルート (`/Contact`) またはカスタム ルート (`/TheContactPage`) のいずれかで、[Contact] ページにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="d465c-246">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="d465c-247">追加の `text` ルート セグメントを指定した場合、ページには指定した HTML エンコードのセグメントが示されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-247">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![URL の 'TextValue' の省略可能な 'text' ルート セグメントを適用する Microsoft Edge ブラウザーの例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="d465c-250">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="d465c-250">Page model action conventions</span></span>

<span data-ttu-id="d465c-251">[IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) を実装する既定のページ モデル プロバイダーは、ページ モデルを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d465c-251">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="d465c-252">これらの規則は、ページ検出をビルドおよび変更したり、シナリオを処理したりするときに便利です。</span><span class="sxs-lookup"><span data-stu-id="d465c-252">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="d465c-253">このセクションの例の場合、サンプル アプリでは、応答ヘッダーを適用する [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute) である、`AddHeaderAttribute` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d465c-253">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="d465c-254">規則を使用して、このサンプルでは、フォルダー内のすべてのページおよび単一ページに属性を適用する方法のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="d465c-254">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="d465c-255">**フォルダー アプリ モデル規則**</span><span class="sxs-lookup"><span data-stu-id="d465c-255">**Folder app model convention**</span></span>

<span data-ttu-id="d465c-256">[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) を使用して、指定したフォルダーにあるすべてのページの [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) インスタンスのアクションを呼び出す、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="d465c-256">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="d465c-257">このサンプルでは、ヘッダー (`OtherPagesHeader`) をアプリの *OtherPages* フォルダー内にあるページに追加して、`AddFolderApplicationModelConvention` を使用するデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="d465c-257">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="d465c-258">`localhost:5000/OtherPages/Page1` でサンプルの Page1 ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="d465c-258">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 ページの応答ヘッダーは、OtherPagesHeader が追加されていることを示しています。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="d465c-260">**ページ アプリ モデル規則**</span><span class="sxs-lookup"><span data-stu-id="d465c-260">**Page app model convention**</span></span>

<span data-ttu-id="d465c-261">使用[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)上のアクションを呼び出す、 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)ページの指定した名前。</span><span class="sxs-lookup"><span data-stu-id="d465c-261">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the specified name.</span></span>

<span data-ttu-id="d465c-262">サンプルでは、ヘッダー (`AboutHeader`) を [About] ページに追加して、`AddPageApplicationModelConvention` を使用するデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="d465c-262">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="d465c-263">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="d465c-263">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、AboutHeader が追加されたことを示しています。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="d465c-265">**フィルターの構成**</span><span class="sxs-lookup"><span data-stu-id="d465c-265">**Configure a filter**</span></span>

<span data-ttu-id="d465c-266">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) では、指定したフィルターを適用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-266">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="d465c-267">フィルター クラスを実装できますが、サンプル アプリでは、ラムダ式でフィルターを実装する方法を示しています。これは、フィルターを返すファクトリとしてバックグラウンドで実装されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-267">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="d465c-268">ページ アプリ モデルは、*OtherPages* フォルダーの Page2 ページにつながるセグメントの相対パスを確認するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-268">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="d465c-269">条件を満たすと、ヘッダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-269">If the condition passes, a header is added.</span></span> <span data-ttu-id="d465c-270">満たさない場合は、`EmptyFilter` が適用されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-270">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="d465c-271">`EmptyFilter` は[アクション フィルター](xref:mvc/controllers/filters#action-filters)です。</span><span class="sxs-lookup"><span data-stu-id="d465c-271">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="d465c-272">アクション フィルターは Razor ページによって無視されるため、パスに `OtherPages/Page2` が含まれない場合は、意図されたように `EmptyFilter` は操作されません。</span><span class="sxs-lookup"><span data-stu-id="d465c-272">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="d465c-273">`localhost:5000/OtherPages/Page2` でサンプルの Page2 ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="d465c-273">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header は、Page2 への応答に追加されます。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="d465c-275">**フィルター ファクトリの構成**</span><span class="sxs-lookup"><span data-stu-id="d465c-275">**Configure a filter factory**</span></span>

<span data-ttu-id="d465c-276">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) では、[フィルター](xref:mvc/controllers/filters)をすべての Razor ページに適用するように、指定したファクトリを構成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-276">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="d465c-277">サンプル アプリでは、アプリのページに対する 2 つの値と共にヘッダー (`FilterFactoryHeader`) を追加して、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を使用する例を提供します。</span><span class="sxs-lookup"><span data-stu-id="d465c-277">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="d465c-278">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="d465c-278">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="d465c-279">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="d465c-279">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、FilterFactoryHeader ヘッダーが 2 つ追加されたことを示しています。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="d465c-281">既定のページ アプリ モデル プロバイダーを置き換える</span><span class="sxs-lookup"><span data-stu-id="d465c-281">Replace the default page app model provider</span></span>

<span data-ttu-id="d465c-282">Razor ページでは、`IPageApplicationModelProvider` インターフェイスを使用して、[DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider) を作成します。</span><span class="sxs-lookup"><span data-stu-id="d465c-282">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="d465c-283">既定のモデル プロバイダーから継承し、ハンドラーの検出と処理に独自の実装ロジックを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="d465c-283">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="d465c-284">既定の実装 ([参照ソース](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) では、以下に示すように、*名前なし*と*名前付き*の名前付けハンドラーの規則を確立します。</span><span class="sxs-lookup"><span data-stu-id="d465c-284">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="d465c-285">**既定の名前なしハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="d465c-285">**Default unnamed handler methods**</span></span>

<span data-ttu-id="d465c-286">HTTP 動詞のハンドラー メソッド ("名前なし" ハンドラー メソッド) は、`On<HTTP verb>[Async]` の規則に従います (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます)。</span><span class="sxs-lookup"><span data-stu-id="d465c-286">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="d465c-287">名前なしハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="d465c-287">Unnamed handler method</span></span>     | <span data-ttu-id="d465c-288">操作</span><span class="sxs-lookup"><span data-stu-id="d465c-288">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="d465c-289">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="d465c-289">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="d465c-290">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="d465c-290">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="d465c-291">DELETE 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-291">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="d465c-292">PUT 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-292">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="d465c-293">PATCH 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-293">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="d465c-294">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-294">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="d465c-295">**既定の名前付きハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="d465c-295">**Default named handler methods**</span></span>

<span data-ttu-id="d465c-296">開発者 ("名前付き" ハンドラー メソッド) によって指定されたハンドラー メソッドは、同様の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="d465c-296">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="d465c-297">ハンドラー名は HTTP 動詞の後、または HTTP 動詞と `Async`: `On<HTTP verb><handler name>[Async]` (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます) の間に表示されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-297">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="d465c-298">たとえば、メッセージを処理するメソッドは、以下の表に示されている名前指定を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d465c-298">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="d465c-299">名前付きハンドラー メソッドの例</span><span class="sxs-lookup"><span data-stu-id="d465c-299">Example named handler method</span></span>             | <span data-ttu-id="d465c-300">操作の例</span><span class="sxs-lookup"><span data-stu-id="d465c-300">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="d465c-301">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="d465c-301">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="d465c-302">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="d465c-302">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="d465c-303">メッセージを削除します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-303">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="d465c-304">メッセージを配置します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-304">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="d465c-305">メッセージを修正します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-305">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="d465c-306">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-306">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="d465c-307">**ハンドラー メソッド名をカスタマイズする**</span><span class="sxs-lookup"><span data-stu-id="d465c-307">**Customize handler method names**</span></span>

<span data-ttu-id="d465c-308">名前なしハンドラー メソッドと名前付きハンドラー メソッドに名前を付ける方法を変更するとします。</span><span class="sxs-lookup"><span data-stu-id="d465c-308">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="d465c-309">別の名前付けスキームは、メソッド名が "On" で始まることを避けて、最初の単語セグメントを使用して HTTP 動詞を決定するためのものです。</span><span class="sxs-lookup"><span data-stu-id="d465c-309">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="d465c-310">DELETE、PUT、PATCH の動詞を POST に変換するなど、その他の変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="d465c-310">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="d465c-311">このようなスキームは、次の表に示すようなメソッド名を指定します。</span><span class="sxs-lookup"><span data-stu-id="d465c-311">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="d465c-312">ハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="d465c-312">Handler method</span></span>                       | <span data-ttu-id="d465c-313">操作</span><span class="sxs-lookup"><span data-stu-id="d465c-313">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="d465c-314">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="d465c-314">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="d465c-315">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="d465c-315">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="d465c-316">DELETE 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-316">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="d465c-317">PUT 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-317">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="d465c-318">PATCH 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="d465c-318">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="d465c-319">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="d465c-319">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="d465c-320">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="d465c-320">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="d465c-321">削除するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="d465c-321">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="d465c-322">配置するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="d465c-322">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="d465c-323">修正するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="d465c-323">POST a message to patch.</span></span>       |

<span data-ttu-id="d465c-324">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-324">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="d465c-325">このスキームを確立するには、`DefaultPageApplicationModelProvider` クラスから継承して [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) メソッドをオーバーライドし、カスタム ロジックを適用して [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) ハンドラー名を解決します。</span><span class="sxs-lookup"><span data-stu-id="d465c-325">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="d465c-326">サンプル アプリは、その `CustomPageApplicationModelProvider` クラスでこれがどのように行われるかを示します。</span><span class="sxs-lookup"><span data-stu-id="d465c-326">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="d465c-327">クラスの主な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d465c-327">Highlights of the class include:</span></span>

* <span data-ttu-id="d465c-328">クラスは、`DefaultPageApplicationModelProvider` を継承します。</span><span class="sxs-lookup"><span data-stu-id="d465c-328">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="d465c-329">`PageHandlerModel` を作成するときに、`TryParseHandlerMethod` は HTTP 動詞 (`httpMethod`) と名前付きハンドラー名 (`handlerName`) を決定するハンドラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="d465c-329">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="d465c-330">存在する場合、`Async` の後置形式は無視されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-330">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="d465c-331">メソッド名から HTTP 動詞を解析するために、文字種が使用されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-331">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="d465c-332">メソッド名 (`Async` なし) が HTTP 動詞名と同じ場合、名前付きハンドラーはありません。</span><span class="sxs-lookup"><span data-stu-id="d465c-332">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="d465c-333">`handlerName` に `null` が設定され、メソッド名は `Get`、`Post`、`Delete`、`Put`、または `Patch` になります。</span><span class="sxs-lookup"><span data-stu-id="d465c-333">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="d465c-334">メソッド名 (`Async` なし) が HTTP 動詞名より長い場合、名前付きハンドラーは存在します。</span><span class="sxs-lookup"><span data-stu-id="d465c-334">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="d465c-335">`handlerName` に `<method name (less 'Async', if present)>` が設定されています。</span><span class="sxs-lookup"><span data-stu-id="d465c-335">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="d465c-336">たとえば、"GetMessage" と "GetMessageAsync" は両方、"GetMessage" のハンドラー名を使用します。</span><span class="sxs-lookup"><span data-stu-id="d465c-336">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="d465c-337">DELETE、PUT、および PATCH HTTP 動詞は、POST に変換されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-337">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="d465c-338">`Startup` クラスに `CustomPageApplicationModelProvider` を登録します。</span><span class="sxs-lookup"><span data-stu-id="d465c-338">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="d465c-339">*Index.cshtml.cs* のページ モデルは、通常のハンドラー メソッドの名前付け規則が、アプリのページに対してどのように変更されるかを示します。</span><span class="sxs-lookup"><span data-stu-id="d465c-339">The page model in *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="d465c-340">Razor ページで使用される通常の "On" プレフィックスの名前付けは削除されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-340">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="d465c-341">ページの状態を初期化するメソッドは、`Get` という名前になりました。</span><span class="sxs-lookup"><span data-stu-id="d465c-341">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="d465c-342">いずれかのページでいずれかのモデルを開くと、アプリ全体で使用されるこの規則を表示できます。</span><span class="sxs-lookup"><span data-stu-id="d465c-342">You can see this convention used throughout the app if you open any page model for any of the pages.</span></span>

<span data-ttu-id="d465c-343">その他の各メソッドは、その処理について説明する HTTP 動詞で始まります。</span><span class="sxs-lookup"><span data-stu-id="d465c-343">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="d465c-344">通常、`Delete` で開始する 2 つのメソッドは、DELETE HTTP 動詞として処理されますが、`TryParseHandlerMethod` のロジックは、明示的に両方のハンドラーの動詞を POST に設定します。</span><span class="sxs-lookup"><span data-stu-id="d465c-344">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="d465c-345">`Async` は、`DeleteAllMessages` と `DeleteMessageAsync` の間で省略可能であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d465c-345">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="d465c-346">これらは両方、非同期メソッドですが、`Async` の後置形式を使用するかどうかを選択できます。使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d465c-346">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="d465c-347">ここでは、`DeleteAllMessages` はデモンストレーション目的で使用されますが、メソッド `DeleteAllMessagesAsync` のように名前を付けることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d465c-347">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="d465c-348">これはサンプルの実装の処理に影響しませんが、`Async` の後置形式を使用して、非同期メソッドというファクトを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d465c-348">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="d465c-349">*Index.cshtml* で指定されたハンドラー名は、`DeleteAllMessages` と `DeleteMessageAsync` ハンドラー メソッドに一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d465c-349">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="d465c-350">ハンドラー メソッド名 `DeleteMessageAsync` の `Async` は、メソッドへの POST 要求に一致するハンドラーの `TryParseHandlerMethod` によって除外されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-350">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="d465c-351">`DeleteMessage` の `asp-page-handler` の名前は、ハンドラー メソッド `DeleteMessageAsync` に一致します。</span><span class="sxs-lookup"><span data-stu-id="d465c-351">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="d465c-352">MVC フィルターとページ フィルター (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="d465c-352">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="d465c-353">Razor ページはハンドラー メソッドを使用するため、MVC [アクション フィルター](xref:mvc/controllers/filters#action-filters)は Razor ページによって無視されます。</span><span class="sxs-lookup"><span data-stu-id="d465c-353">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="d465c-354">その他の型の MVC フィルターは、[承認](xref:mvc/controllers/filters#authorization-filters)、[例外](xref:mvc/controllers/filters#exception-filters)、[リソース](xref:mvc/controllers/filters#resource-filters)、および[結果](xref:mvc/controllers/filters#result-filters)を使用するために利用できます。</span><span class="sxs-lookup"><span data-stu-id="d465c-354">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="d465c-355">詳細については、「[フィルター](xref:mvc/controllers/filters)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d465c-355">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="d465c-356">ページ フィルター ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) は、Razor ページに適用されるフィルターです。</span><span class="sxs-lookup"><span data-stu-id="d465c-356">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="d465c-357">詳細については、[Razor ページのフィルター メソッド](xref:razor-pages/filter)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d465c-357">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d465c-358">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="d465c-358">Additional resources</span></span>

* [<span data-ttu-id="d465c-359">Razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="d465c-359">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
