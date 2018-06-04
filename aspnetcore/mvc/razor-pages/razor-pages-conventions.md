---
title: ASP.NET Core での Razor ページのルートとアプリの規則
author: guardrex
description: ルートとアプリ モデル プロバイダーの規則が、ページのルーティング、検出、および処理の制御にどのように役立つかについて確認します。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-conventions
ms.openlocfilehash: eba3422fbf46ac181a783b7f8cc605c2a549b4b7
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729744"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="efd7c-103">ASP.NET Core での Razor ページのルートとアプリの規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="efd7c-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="efd7c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="efd7c-105">[ページ ルートとアプリ モデル プロバイダーの規則](xref:mvc/controllers/application-model#conventions)を使用して、Razor ページ アプリでページのルーティング、検出、および処理を制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span> <span data-ttu-id="efd7c-106">個別のページにカスタムのページ ルートを構成する必要がある場合、このトピックで後述される「[AddPageRoute convention](#configure-a-page-route)」で、ページへのルーティングを構成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="efd7c-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="efd7c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range="= aspnetcore-2.0"
| <span data-ttu-id="efd7c-108">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd7c-108">Scenario</span></span> | <span data-ttu-id="efd7c-109">このサンプルでは、次のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-109">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="efd7c-110">モデルの規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-110">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="efd7c-111">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="efd7c-111">Conventions.Add</span></span><ul><li><span data-ttu-id="efd7c-112">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-112">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="efd7c-113">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-113">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="efd7c-114">ルート テンプレートとヘッダーをアプリのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-114">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="efd7c-115">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-115">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="efd7c-116">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-116">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="efd7c-117">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-117">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="efd7c-118">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="efd7c-118">AddPageRoute</span></span></li></ul> | <span data-ttu-id="efd7c-119">ルート テンプレートをフォルダー内のページおよび単一ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-119">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="efd7c-120">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-120">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="efd7c-121">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-121">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="efd7c-122">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-122">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="efd7c-123">ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</span><span class="sxs-lookup"><span data-stu-id="efd7c-123">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="efd7c-124">ヘッダーをフォルダー内のページに追加し、ヘッダーを単一ページに追加し、ヘッダーをアプリのページに追加するように[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を構成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-124">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="efd7c-125">既定のページ アプリ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="efd7c-125">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="efd7c-126">既定のページ モデル プロバイダーを置き換えて、ハンドラー名の規則を変更します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-126">Replace the default page model provider to change the conventions for handler names.</span></span> |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| <span data-ttu-id="efd7c-127">シナリオ</span><span class="sxs-lookup"><span data-stu-id="efd7c-127">Scenario</span></span> | <span data-ttu-id="efd7c-128">このサンプルでは、次のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-128">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="efd7c-129">モデルの規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-129">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="efd7c-130">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="efd7c-130">Conventions.Add</span></span><ul><li><span data-ttu-id="efd7c-131">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-131">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="efd7c-132">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-132">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="efd7c-133">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-133">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="efd7c-134">ルート テンプレートとヘッダーをアプリのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-134">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="efd7c-135">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-135">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="efd7c-136">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-136">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="efd7c-137">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-137">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="efd7c-138">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="efd7c-138">AddPageRoute</span></span></li></ul> | <span data-ttu-id="efd7c-139">ルート テンプレートをフォルダー内のページおよび単一ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-139">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="efd7c-140">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-140">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="efd7c-141">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-141">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="efd7c-142">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="efd7c-142">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="efd7c-143">ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</span><span class="sxs-lookup"><span data-stu-id="efd7c-143">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="efd7c-144">ヘッダーをフォルダー内のページに追加し、ヘッダーを単一ページに追加し、ヘッダーをアプリのページに追加するように[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を構成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-144">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="efd7c-145">既定のページ アプリ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="efd7c-145">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="efd7c-146">既定のページ モデル プロバイダーを置き換えて、ハンドラー名の規則を変更します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-146">Replace the default page model provider to change the conventions for handler names.</span></span> |
::: moniker-end

<span data-ttu-id="efd7c-147">Razor ページの規則は、`Startup` クラス内のサービス コレクションの [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) に [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) 拡張メソッドを使用することで、追加および構成されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-147">Razor Pages conventions are added and configured using the [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) extension method to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) on the service collection in the `Startup` class.</span></span> <span data-ttu-id="efd7c-148">次の規則の例は、このトピックで後述されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-148">The following convention examples are explained later in this topic:</span></span>

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

## <a name="model-conventions"></a><span data-ttu-id="efd7c-149">モデルの規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-149">Model conventions</span></span>

<span data-ttu-id="efd7c-150">[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) の委任を追加して、Razor ページに適用する[モデル規則](xref:mvc/controllers/application-model#conventions)を追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-150">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

<span data-ttu-id="efd7c-151">**すべてのページにルート モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="efd7c-151">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="efd7c-152">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成し、ページ ルート モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-152">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page route model construction.</span></span>

<span data-ttu-id="efd7c-153">サンプル アプリでは、`{globalTemplate?}` ルート テンプレートをアプリ内のすべてのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-153">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="efd7c-154">`AttributeRouteModel` の `Order` プロパティに `-1` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-154">The `Order` property for the `AttributeRouteModel` is set to `-1`.</span></span> <span data-ttu-id="efd7c-155">この設定により、単一のルート値が指定されたときに、このテンプレートが優先的に最初のルート データ値の位置に指定され、自動的に生成された Razor ページ ルートよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-155">This ensures that this template is given priority for the first route data value position when a single route value is provided and also that it would have priority over automatically generated Razor Pages routes.</span></span> <span data-ttu-id="efd7c-156">たとえば、このサンプルでは、このトピックの後で `{aboutTemplate?}` ルート テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-156">For example, the sample adds an `{aboutTemplate?}` route template later in the topic.</span></span> <span data-ttu-id="efd7c-157">`{aboutTemplate?}` テンプレートの `Order` には `1` が指定されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-157">The `{aboutTemplate?}` template is given an `Order` of `1`.</span></span> <span data-ttu-id="efd7c-158">[About] ページが `/About/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = -1`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["aboutTemplate"]` (`Order = 1`) には読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="efd7c-158">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = -1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="efd7c-159">Razor ページ オプション ([規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)の追加など) は、MVC が `Startup.ConfigureServices` のサービス コレクションに追加されたときに追加されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-159">Razor Pages options, such as adding [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="efd7c-160">例については、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd7c-160">For an example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/).</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="efd7c-161">`localhost:5000/About/GlobalRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-161">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![[About] ページは、GlobalRouteValue のルート セグメントで要求されます。](razor-pages-conventions/_static/about-page-global-template.png)

<span data-ttu-id="efd7c-164">**すべてのページにアプリ モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="efd7c-164">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="efd7c-165">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成し、ページ アプリ モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-165">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page app model construction.</span></span>

<span data-ttu-id="efd7c-166">この規則やこのトピックで後述されるその他の規則のデモを実行するには、サンプル アプリに `AddHeaderAttribute` クラスを含めます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-166">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="efd7c-167">クラス コンストラクターは、`name` 文字列と `values` 文字列配列を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-167">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="efd7c-168">これらの値は、応答ヘッダーを設定するために、その `OnResultExecuting` メソッド内で使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-168">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="efd7c-169">完全クラスは、このトピックで後述される「[ページ モデル アクション規則](#page-model-action-conventions)」セクションで示されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-169">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="efd7c-170">サンプル アプリでは、`AddHeaderAttribute` クラスを使用して、ヘッダー (`GlobalHeader`) をアプリ内のすべてのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-170">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="efd7c-171">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="efd7c-171">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="efd7c-172">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-172">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、GlobalHeader が追加されたことを示しています。](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="efd7c-174">**すべてのページにハンドラー モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="efd7c-174">**Add a handler model convention to all pages**</span></span>

<span data-ttu-id="efd7c-175">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) を作成し、ページ ハンドラー モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-175">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during page handler model construction.</span></span>

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

<span data-ttu-id="efd7c-176">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="efd7c-176">`Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```
::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="efd7c-177">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-177">Page route action conventions</span></span>

<span data-ttu-id="efd7c-178">[IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) から派生する既定のルート モデル プロバイダーは、ページ ルートを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-178">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="efd7c-179">**フォルダー ルート モデル規則**</span><span class="sxs-lookup"><span data-stu-id="efd7c-179">**Folder route model convention**</span></span>

<span data-ttu-id="efd7c-180">[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) を使用して、指定したフォルダーのページにある [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-180">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="efd7c-181">サンプル アプリでは `AddFolderRouteModelConvention` を使用して、`{otherPagesTemplate?}` ルート テンプレートを *OtherPages* フォルダーのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-181">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="efd7c-182">`AttributeRouteModel` の `Order` プロパティに `1` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-182">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="efd7c-183">この設定により、単一のルート値が指定されたときに、(このトピックの前半で設定した) `{globalTemplate?}` のテンプレートが優先的に最初のルート データ値の位置に指定されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-183">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="efd7c-184">Page1 ページが `/OtherPages/Page1/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = -1`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) には読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="efd7c-184">If the Page1 page is requested at `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = -1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="efd7c-185">`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` でサンプルの Page1 ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-185">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![OtherPages フォルダーの Page1 は、GlobalRouteValue と OtherPagesRouteValue のルート セグメントで要求されます。](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="efd7c-188">**ページ ルート モデル規則**</span><span class="sxs-lookup"><span data-stu-id="efd7c-188">**Page route model convention**</span></span>

<span data-ttu-id="efd7c-189">[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) を使用して、指定した名前でページの [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-189">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="efd7c-190">サンプル アプリでは `AddPageRouteModelConvention` を使用して、`{aboutTemplate?}` ルート テンプレートを [About] ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-190">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> <span data-ttu-id="efd7c-191">`AttributeRouteModel` の `Order` プロパティに `1` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-191">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="efd7c-192">この設定により、単一のルート値が指定されたときに、(このトピックの前半で設定した) `{globalTemplate?}` のテンプレートが優先的に最初のルート データ値の位置に指定されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-192">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="efd7c-193">[About] ページが `/About/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = -1`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["aboutTemplate"]` (`Order = 1`) には読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="efd7c-193">If the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = -1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="efd7c-194">`localhost:5000/About/GlobalRouteValue/AboutRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-194">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![[About] ページは、GlobalRouteValue と AboutRouteValue のルート セグメントで要求されます。](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="efd7c-197">ページ ルートの構成</span><span class="sxs-lookup"><span data-stu-id="efd7c-197">Configure a page route</span></span>

<span data-ttu-id="efd7c-198">[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) を使用して、特定のページ パスでページへのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-198">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="efd7c-199">そのページに対して生成されたリンクでは、指定したルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-199">Generated links to the page use your specified route.</span></span> <span data-ttu-id="efd7c-200">`AddPageRoute` では、`AddPageRouteModelConvention` を使用してルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-200">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="efd7c-201">サンプル アプリでは、*Contact.cshtml* の `/TheContactPage` へのルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-201">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="efd7c-202">[Contact] ページには、既定のルート経由の `/Contact` でアクセスすることもできます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-202">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="efd7c-203">サンプル アプリの [Contact] ページに対するカスタム ルートでは、省略可能な `text` ルート セグメント (`{text?}`) を許可します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-203">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="efd7c-204">また、訪問者が `/Contact` ルートでページにアクセスする場合、ページの `@page` ディレクティブにはこの省略可能なセグメントも含まれます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-204">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="efd7c-205">レンダリングされたページの **Contact** リンク用に生成された URL には、更新されたルートが反映されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="efd7c-205">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![ナビゲーション バーのサンプル アプリの [Contact] リンク](razor-pages-conventions/_static/contact-link.png)

![レンダリングされた HTML の [Contact] リンクを調べると、href に '/TheContactPage' が設定されています](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="efd7c-208">通常のルート (`/Contact`) またはカスタム ルート (`/TheContactPage`) のいずれかで、[Contact] ページにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="efd7c-208">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="efd7c-209">追加の `text` ルート セグメントを指定した場合、ページには指定した HTML エンコードのセグメントが示されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-209">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![URL の 'TextValue' の省略可能な 'text' ルート セグメントを適用する Microsoft Edge ブラウザーの例。](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="efd7c-212">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-212">Page model action conventions</span></span>

<span data-ttu-id="efd7c-213">[IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) を実装する既定のページ モデル プロバイダーは、ページ モデルを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-213">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="efd7c-214">これらの規則は、ページ検出をビルドおよび変更したり、シナリオを処理したりするときに便利です。</span><span class="sxs-lookup"><span data-stu-id="efd7c-214">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="efd7c-215">このセクションの例の場合、サンプル アプリでは、応答ヘッダーを適用する [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute) である、`AddHeaderAttribute` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-215">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="efd7c-216">規則を使用して、このサンプルでは、フォルダー内のすべてのページおよび単一ページに属性を適用する方法のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-216">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="efd7c-217">**フォルダー アプリ モデル規則**</span><span class="sxs-lookup"><span data-stu-id="efd7c-217">**Folder app model convention**</span></span>

<span data-ttu-id="efd7c-218">[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) を使用して、指定したフォルダーにあるすべてのページの [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) インスタンスのアクションを呼び出す、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-218">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="efd7c-219">このサンプルでは、ヘッダー (`OtherPagesHeader`) をアプリの *OtherPages* フォルダー内にあるページに追加して、`AddFolderApplicationModelConvention` を使用するデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-219">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="efd7c-220">`localhost:5000/OtherPages/Page1` でサンプルの Page1 ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-220">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 ページの応答ヘッダーは、OtherPagesHeader が追加されていることを示しています。](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="efd7c-222">**ページ アプリ モデル規則**</span><span class="sxs-lookup"><span data-stu-id="efd7c-222">**Page app model convention**</span></span>

<span data-ttu-id="efd7c-223">[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) を使用して、指定した名前でページの [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) のアクションを呼び出す、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-223">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the speciifed name.</span></span>

<span data-ttu-id="efd7c-224">サンプルでは、ヘッダー (`AboutHeader`) を [About] ページに追加して、`AddPageApplicationModelConvention` を使用するデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-224">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="efd7c-225">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-225">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、AboutHeader が追加されたことを示しています。](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="efd7c-227">**フィルターの構成**</span><span class="sxs-lookup"><span data-stu-id="efd7c-227">**Configure a filter**</span></span>

<span data-ttu-id="efd7c-228">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) では、指定したフィルターを適用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-228">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="efd7c-229">フィルター クラスを実装できますが、サンプル アプリでは、ラムダ式でフィルターを実装する方法を示しています。これは、フィルターを返すファクトリとしてバックグラウンドで実装されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-229">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="efd7c-230">ページ アプリ モデルは、*OtherPages* フォルダーの Page2 ページにつながるセグメントの相対パスを確認するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-230">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="efd7c-231">条件を満たすと、ヘッダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-231">If the condition passes, a header is added.</span></span> <span data-ttu-id="efd7c-232">満たさない場合は、`EmptyFilter` が適用されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-232">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="efd7c-233">`EmptyFilter` は[アクション フィルター](xref:mvc/controllers/filters#action-filters)です。</span><span class="sxs-lookup"><span data-stu-id="efd7c-233">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="efd7c-234">アクション フィルターは Razor ページによって無視されるため、パスに `OtherPages/Page2` が含まれない場合は、意図されたように `EmptyFilter` は操作されません。</span><span class="sxs-lookup"><span data-stu-id="efd7c-234">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="efd7c-235">`localhost:5000/OtherPages/Page2` でサンプルの Page2 ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-235">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header は、Page2 への応答に追加されます。](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="efd7c-237">**フィルター ファクトリの構成**</span><span class="sxs-lookup"><span data-stu-id="efd7c-237">**Configure a filter factory**</span></span>

<span data-ttu-id="efd7c-238">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) では、[フィルター](xref:mvc/controllers/filters)をすべての Razor ページに適用するように、指定したファクトリを構成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-238">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="efd7c-239">サンプル アプリでは、アプリのページに対する 2 つの値と共にヘッダー (`FilterFactoryHeader`) を追加して、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を使用する例を提供します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-239">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="efd7c-240">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="efd7c-240">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="efd7c-241">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-241">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、FilterFactoryHeader ヘッダーが 2 つ追加されたことを示しています。](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="efd7c-243">既定のページ アプリ モデル プロバイダーを置き換える</span><span class="sxs-lookup"><span data-stu-id="efd7c-243">Replace the default page app model provider</span></span>

<span data-ttu-id="efd7c-244">Razor ページでは、`IPageApplicationModelProvider` インターフェイスを使用して、[DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider) を作成します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-244">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="efd7c-245">既定のモデル プロバイダーから継承し、ハンドラーの検出と処理に独自の実装ロジックを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-245">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="efd7c-246">既定の実装 ([参照ソース](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) では、以下に示すように、*名前なし*と*名前付き*の名前付けハンドラーの規則を確立します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-246">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="efd7c-247">**既定の名前なしハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="efd7c-247">**Default unnamed handler methods**</span></span>

<span data-ttu-id="efd7c-248">HTTP 動詞のハンドラー メソッド ("名前なし" ハンドラー メソッド) は、`On<HTTP verb>[Async]` の規則に従います (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます)。</span><span class="sxs-lookup"><span data-stu-id="efd7c-248">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="efd7c-249">名前なしハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="efd7c-249">Unnamed handler method</span></span>     | <span data-ttu-id="efd7c-250">操作</span><span class="sxs-lookup"><span data-stu-id="efd7c-250">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="efd7c-251">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-251">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="efd7c-252">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-252">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="efd7c-253">DELETE 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-253">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="efd7c-254">PUT 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-254">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="efd7c-255">PATCH 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-255">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="efd7c-256">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-256">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="efd7c-257">**既定の名前付きハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="efd7c-257">**Default named handler methods**</span></span>

<span data-ttu-id="efd7c-258">開発者 ("名前付き" ハンドラー メソッド) によって指定されたハンドラー メソッドは、同様の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="efd7c-258">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="efd7c-259">ハンドラー名は HTTP 動詞の後、または HTTP 動詞と `Async`: `On<HTTP verb><handler name>[Async]` (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます) の間に表示されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-259">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="efd7c-260">たとえば、メッセージを処理するメソッドは、以下の表に示されている名前指定を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="efd7c-260">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="efd7c-261">名前付きハンドラー メソッドの例</span><span class="sxs-lookup"><span data-stu-id="efd7c-261">Example named handler method</span></span>             | <span data-ttu-id="efd7c-262">操作の例</span><span class="sxs-lookup"><span data-stu-id="efd7c-262">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="efd7c-263">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-263">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="efd7c-264">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-264">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="efd7c-265">メッセージを削除します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-265">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="efd7c-266">メッセージを配置します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-266">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="efd7c-267">メッセージを修正します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-267">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="efd7c-268">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-268">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="efd7c-269">**ハンドラー メソッド名をカスタマイズする**</span><span class="sxs-lookup"><span data-stu-id="efd7c-269">**Customize handler method names**</span></span>

<span data-ttu-id="efd7c-270">名前なしハンドラー メソッドと名前付きハンドラー メソッドに名前を付ける方法を変更するとします。</span><span class="sxs-lookup"><span data-stu-id="efd7c-270">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="efd7c-271">別の名前付けスキームは、メソッド名が "On" で始まることを避けて、最初の単語セグメントを使用して HTTP 動詞を決定するためのものです。</span><span class="sxs-lookup"><span data-stu-id="efd7c-271">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="efd7c-272">DELETE、PUT、PATCH の動詞を POST に変換するなど、その他の変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-272">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="efd7c-273">このようなスキームは、次の表に示すようなメソッド名を指定します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-273">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="efd7c-274">ハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="efd7c-274">Handler method</span></span>                       | <span data-ttu-id="efd7c-275">操作</span><span class="sxs-lookup"><span data-stu-id="efd7c-275">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="efd7c-276">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-276">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="efd7c-277">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-277">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="efd7c-278">DELETE 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-278">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="efd7c-279">PUT 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-279">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="efd7c-280">PATCH 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="efd7c-280">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="efd7c-281">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-281">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="efd7c-282">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-282">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="efd7c-283">削除するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-283">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="efd7c-284">配置するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-284">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="efd7c-285">修正するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-285">POST a message to patch.</span></span>       |

<span data-ttu-id="efd7c-286">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-286">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="efd7c-287">このスキームを確立するには、`DefaultPageApplicationModelProvider` クラスから継承して [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) メソッドを上書きし、カスタム ロジックを適用して [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) ハンドラー名を解決します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-287">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="efd7c-288">サンプル アプリは、その `CustomPageApplicationModelProvider` クラスでこれがどのように行われるかを示します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-288">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="efd7c-289">クラスの主な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="efd7c-289">Highlights of the class include:</span></span>

* <span data-ttu-id="efd7c-290">クラスは、`DefaultPageApplicationModelProvider` を継承します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-290">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="efd7c-291">`PageHandlerModel` を作成するときに、`TryParseHandlerMethod` は HTTP 動詞 (`httpMethod`) と名前付きハンドラー名 (`handlerName`) を決定するハンドラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-291">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="efd7c-292">存在する場合、`Async` の後置形式は無視されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-292">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="efd7c-293">メソッド名から HTTP 動詞を解析するために、文字種が使用されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-293">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="efd7c-294">メソッド名 (`Async` なし) が HTTP 動詞名と同じ場合、名前付きハンドラーはありません。</span><span class="sxs-lookup"><span data-stu-id="efd7c-294">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="efd7c-295">`handlerName` に `null` が設定され、メソッド名は `Get`、`Post`、`Delete`、`Put`、または `Patch` になります。</span><span class="sxs-lookup"><span data-stu-id="efd7c-295">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="efd7c-296">メソッド名 (`Async` なし) が HTTP 動詞名より長い場合、名前付きハンドラーは存在します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-296">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="efd7c-297">`handlerName` に `<method name (less 'Async', if present)>` が設定されています。</span><span class="sxs-lookup"><span data-stu-id="efd7c-297">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="efd7c-298">たとえば、"GetMessage" と "GetMessageAsync" は両方、"GetMessage" のハンドラー名を使用します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-298">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="efd7c-299">DELETE、PUT、および PATCH HTTP 動詞は、POST に変換されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-299">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="efd7c-300">`Startup` クラスに `CustomPageApplicationModelProvider` を登録します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-300">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="efd7c-301">*Index.cshtml.cs* のページ モデルは、通常のハンドラー メソッドの名前付け規則が、アプリのページに対してどのように変更されるかを示します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-301">The page model in *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="efd7c-302">Razor ページで使用される通常の "On" プレフィックスの名前付けは削除されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-302">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="efd7c-303">ページの状態を初期化するメソッドは、`Get` という名前になりました。</span><span class="sxs-lookup"><span data-stu-id="efd7c-303">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="efd7c-304">いずれかのページでいずれかのモデルを開くと、アプリ全体で使用されるこの規則を表示できます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-304">You can see this convention used throughout the app if you open any page model for any of the pages.</span></span>

<span data-ttu-id="efd7c-305">その他の各メソッドは、その処理について説明する HTTP 動詞で始まります。</span><span class="sxs-lookup"><span data-stu-id="efd7c-305">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="efd7c-306">通常、`Delete` で開始する 2 つのメソッドは、DELETE HTTP 動詞として処理されますが、`TryParseHandlerMethod` のロジックは、明示的に両方のハンドラーの動詞を POST に設定します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-306">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="efd7c-307">`Async` は、`DeleteAllMessages` と `DeleteMessageAsync` の間で省略可能であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="efd7c-307">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="efd7c-308">これらは両方、非同期メソッドですが、`Async` の後置形式を使用するかどうかを選択できます。使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="efd7c-308">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="efd7c-309">ここでは、`DeleteAllMessages` はデモンストレーション目的で使用されますが、メソッド `DeleteAllMessagesAsync` のように名前を付けることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="efd7c-309">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="efd7c-310">これはサンプルの実装の処理に影響しませんが、`Async` の後置形式を使用して、非同期メソッドというファクトを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-310">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="efd7c-311">*Index.cshtml* で指定されたハンドラー名は、`DeleteAllMessages` と `DeleteMessageAsync` ハンドラー メソッドに一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="efd7c-311">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="efd7c-312">ハンドラー メソッド名 `DeleteMessageAsync` の `Async` は、メソッドへの POST 要求に一致するハンドラーの `TryParseHandlerMethod` によって除外されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-312">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="efd7c-313">`DeleteMessage` の `asp-page-handler` の名前は、ハンドラー メソッド `DeleteMessageAsync` に一致します。</span><span class="sxs-lookup"><span data-stu-id="efd7c-313">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="efd7c-314">MVC フィルターとページ フィルター (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="efd7c-314">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="efd7c-315">Razor ページはハンドラー メソッドを使用するため、MVC [アクション フィルター](xref:mvc/controllers/filters#action-filters)は Razor ページによって無視されます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-315">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="efd7c-316">その他の型の MVC フィルターは、[承認](xref:mvc/controllers/filters#authorization-filters)、[例外](xref:mvc/controllers/filters#exception-filters)、[リソース](xref:mvc/controllers/filters#resource-filters)、および[結果](xref:mvc/controllers/filters#result-filters)を使用するために利用できます。</span><span class="sxs-lookup"><span data-stu-id="efd7c-316">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="efd7c-317">詳細については、「[フィルター](xref:mvc/controllers/filters)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd7c-317">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="efd7c-318">ページ フィルター ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) は、Razor ページに適用されるフィルターです。</span><span class="sxs-lookup"><span data-stu-id="efd7c-318">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="efd7c-319">詳細については、[Razor ページのフィルター メソッド](xref:mvc/razor-pages/filter)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="efd7c-319">For more information, see [Filter methods for Razor Pages](xref:mvc/razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="efd7c-320">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="efd7c-320">Additional resources</span></span>

* [<span data-ttu-id="efd7c-321">Razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="efd7c-321">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
