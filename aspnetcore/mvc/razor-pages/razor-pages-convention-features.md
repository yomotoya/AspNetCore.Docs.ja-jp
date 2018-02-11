---
title: "ASP.NET Core での Razor ページのルートとアプリの規則機能"
author: guardrex
description: "ルートとアプリ モデル プロバイダーの規則機能が、ページのルーティング、検出、および処理の制御にどのように役立つかについて確認します。"
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a><span data-ttu-id="a5b8c-103">ASP.NET Core での Razor ページのルートとアプリの規則機能</span><span class="sxs-lookup"><span data-stu-id="a5b8c-103">Razor Pages route and app convention features in ASP.NET Core</span></span>

<span data-ttu-id="a5b8c-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a5b8c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a5b8c-105">ページ ルートとアプリ モデル プロバイダーの規則機能を使用して、Razor ページ アプリでページのルーティング、検出、および処理を制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-105">Learn how to use page route and app model provider convention features to control page routing, discovery, and processing in Razor Pages apps.</span></span> <span data-ttu-id="a5b8c-106">個別のページにカスタムのページ ルートを構成する必要がある場合、このトピックで後述される「[AddPageRoute convention](#configure-a-page-route)」で、ページへのルーティングを構成します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="a5b8c-107">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) を使用して、このトピックで説明される機能について調べます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-107">Use the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

| <span data-ttu-id="a5b8c-108">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="a5b8c-108">Features</span></span> | <span data-ttu-id="a5b8c-109">このサンプルでは、次のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-109">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="a5b8c-110">ルート モデル規則とアプリ モデル規則</span><span class="sxs-lookup"><span data-stu-id="a5b8c-110">Route and app model conventions</span></span>](#add-route-and-app-model-conventions)<br><br><span data-ttu-id="a5b8c-111">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="a5b8c-111">Conventions.Add</span></span><ul><li><span data-ttu-id="a5b8c-112">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="a5b8c-112">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="a5b8c-113">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="a5b8c-113">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="a5b8c-114">ルート テンプレートとヘッダーをアプリのページに追加する。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-114">Adding a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="a5b8c-115">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="a5b8c-115">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="a5b8c-116">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="a5b8c-116">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="a5b8c-117">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="a5b8c-117">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="a5b8c-118">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="a5b8c-118">AddPageRoute</span></span></li></ul> | <span data-ttu-id="a5b8c-119">ルート テンプレートをフォルダー内のページおよび単一ページに追加する。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-119">Adding a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="a5b8c-120">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="a5b8c-120">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="a5b8c-121">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="a5b8c-121">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="a5b8c-122">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="a5b8c-122">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="a5b8c-123">ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</span><span class="sxs-lookup"><span data-stu-id="a5b8c-123">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="a5b8c-124">ヘッダーをフォルダー内のページに追加する、ヘッダーを単一ページに追加する、ヘッダーをアプリのページに追加するように[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を構成する。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-124">Adding a header to pages in a folder, adding a header to a single page, and configuring a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="a5b8c-125">既定のページ アプリ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="a5b8c-125">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="a5b8c-126">既定のページ モデル プロバイダーを置き換えて、名前付けハンドラーの規則を変更する。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-126">Replacing the default page model provider to change the conventions for handler naming.</span></span> |

## <a name="add-route-and-app-model-conventions"></a><span data-ttu-id="a5b8c-127">ルート モデル規則とアプリ モデル規則の追加</span><span class="sxs-lookup"><span data-stu-id="a5b8c-127">Add route and app model conventions</span></span>

<span data-ttu-id="a5b8c-128">[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) の委任を追加して、Razor ページに適用するルート モデル規則とアプリ モデル規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-128">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add route and app model conventions that apply to Razor Pages.</span></span>

<span data-ttu-id="a5b8c-129">**すべてのページにルート モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-129">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="a5b8c-130">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成し、ルートとページ モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-130">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="a5b8c-131">サンプル アプリでは、`{globalTemplate?}` ルート テンプレートをアプリ内のすべてのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-131">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="a5b8c-132">`AttributeRouteModel` の `Order` プロパティに `0` (ゼロ) が設定されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-132">The `Order` property for the `AttributeRouteModel` is set to `0` (zero).</span></span> <span data-ttu-id="a5b8c-133">この設定によって、単一のルート値が指定されたときに、このテンプレートが優先的に最初のルート データ値の位置に指定されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-133">This ensures that this template is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="a5b8c-134">たとえば、このサンプルでは、このトピックの後で `{aboutTemplate?}` ルート テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-134">For example, the sample adds an `{aboutTemplate?}` route template later in the topic.</span></span> <span data-ttu-id="a5b8c-135">`{aboutTemplate?}` テンプレートの `Order` には `1` が指定されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-135">The `{aboutTemplate?}` template is given an `Order` of `1`.</span></span> <span data-ttu-id="a5b8c-136">[About] ページが `/About/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = 0`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["aboutTemplate"]` (`Order = 1`) には読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-136">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="a5b8c-137">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a5b8c-137">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="a5b8c-138">`localhost:5000/About/GlobalRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-138">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![[About] ページは、GlobalRouteValue のルート セグメントで要求されます。](razor-pages-convention-features/_static/about-page-global-template.png)

<span data-ttu-id="a5b8c-141">**すべてのページにアプリ モデル規則を追加する**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-141">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="a5b8c-142">[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を使用して、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成し、ルートとページ モデルの構築中に適用される [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) インスタンスのコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-142">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="a5b8c-143">この規則やこのトピックで後述されるその他の規則のデモを実行するには、サンプル アプリに `AddHeaderAttribute` クラスを含めます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-143">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="a5b8c-144">クラス コンストラクターは、`name` 文字列と `values` 文字列配列を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-144">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="a5b8c-145">これらの値は、応答ヘッダーを設定するために、その `OnResultExecuting` メソッド内で使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-145">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="a5b8c-146">完全クラスは、このトピックで後述される「[ページ モデル アクション規則](#page-model-action-conventions)」セクションで示されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-146">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="a5b8c-147">サンプル アプリでは、`AddHeaderAttribute` クラスを使用して、ヘッダー (`GlobalHeader`) をアプリ内のすべてのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-147">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="a5b8c-148">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a5b8c-148">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="a5b8c-149">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-149">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、GlobalHeader が追加されたことを示しています。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a><span data-ttu-id="a5b8c-151">ページ ルート アクション規則</span><span class="sxs-lookup"><span data-stu-id="a5b8c-151">Page route action conventions</span></span>

<span data-ttu-id="a5b8c-152">[IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) から派生する既定のルート モデル プロバイダーは、ページ ルートを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-152">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="a5b8c-153">**フォルダー ルート モデル規則**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-153">**Folder route model convention**</span></span>

<span data-ttu-id="a5b8c-154">[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) を使用して、指定したフォルダーのページにある [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-154">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="a5b8c-155">サンプル アプリでは `AddFolderRouteModelConvention` を使用して、`{otherPagesTemplate?}` ルート テンプレートを *OtherPages* フォルダーのページに追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-155">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="a5b8c-156">`AttributeRouteModel` の `Order` プロパティに `1` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-156">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="a5b8c-157">この設定により、単一のルート値が指定されたときに、(このトピックの前半で設定した) `{globalTemplate?}` のテンプレートが優先的に最初のルート データ値の位置に指定されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-157">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="a5b8c-158">Page1 ページが `/OtherPages/Page1/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = 0`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) には読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-158">If the Page1 page is requested at `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="a5b8c-159">`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` でサンプルの Page1 ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-159">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![OtherPages フォルダーの Page1 は、GlobalRouteValue と OtherPagesRouteValue のルート セグメントで要求されます。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="a5b8c-162">**ページ ルート モデル規則**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-162">**Page route model convention**</span></span>

<span data-ttu-id="a5b8c-163">[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) を使用して、指定した名前でページの [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) のアクションを呼び出す、[IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-163">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="a5b8c-164">サンプル アプリでは `AddPageRouteModelConvention` を使用して、`{aboutTemplate?}` ルート テンプレートを [About] ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-164">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> <span data-ttu-id="a5b8c-165">`AttributeRouteModel` の `Order` プロパティに `1` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-165">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="a5b8c-166">この設定により、単一のルート値が指定されたときに、(このトピックの前半で設定した) `{globalTemplate?}` のテンプレートが優先的に最初のルート データ値の位置に指定されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-166">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="a5b8c-167">[About] ページが `/About/RouteDataValue` で要求されると、"RouteDataValue" は `RouteData.Values["globalTemplate"]` (`Order = 0`) に読み込まれ、`Order` プロパティが設定されるため `RouteData.Values["aboutTemplate"]` (`Order = 1`) には読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-167">If the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="a5b8c-168">`localhost:5000/About/GlobalRouteValue/AboutRouteValue` でサンプルの [About] ページを要求し、その結果を調べます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-168">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![[About] ページは、GlobalRouteValue と AboutRouteValue のルート セグメントで要求されます。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="a5b8c-171">ページ ルートの構成</span><span class="sxs-lookup"><span data-stu-id="a5b8c-171">Configure a page route</span></span>

<span data-ttu-id="a5b8c-172">[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) を使用して、特定のページ パスでページへのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-172">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="a5b8c-173">そのページに対して生成されたリンクでは、指定したルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-173">Generated links to the page use your specified route.</span></span> <span data-ttu-id="a5b8c-174">`AddPageRoute` では、`AddPageRouteModelConvention` を使用してルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-174">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="a5b8c-175">サンプル アプリでは、*Contact.cshtml* の `/TheContactPage` へのルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-175">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="a5b8c-176">[Contact] ページには、既定のルート経由の `/Contact` でアクセスすることもできます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-176">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="a5b8c-177">サンプル アプリの [Contact] ページに対するカスタム ルートでは、省略可能な `text` ルート セグメント (`{text?}`) を許可します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-177">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="a5b8c-178">また、訪問者が `/Contact` ルートでページにアクセスする場合、ページの `@page` ディレクティブにはこの省略可能なセグメントも含まれます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-178">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="a5b8c-179">レンダリングされたページの **Contact** リンク用に生成された URL には、更新されたルートが反映されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-179">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![ナビゲーション バーのサンプル アプリの [Contact] リンク](razor-pages-convention-features/_static/contact-link.png)

![レンダリングされた HTML の [Contact] リンクを調べると、href に '/TheContactPage' が設定されています](razor-pages-convention-features/_static/contact-link-source.png)

<span data-ttu-id="a5b8c-182">通常のルート (`/Contact`) またはカスタム ルート (`/TheContactPage`) のいずれかで、[Contact] ページにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-182">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="a5b8c-183">追加の `text` ルート セグメントを指定した場合、ページには指定した HTML エンコードのセグメントが示されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-183">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![URL の 'TextValue' の省略可能な 'text' ルート セグメントを適用する Edge ブラウザーの例。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="a5b8c-186">ページ モデル アクション規則</span><span class="sxs-lookup"><span data-stu-id="a5b8c-186">Page model action conventions</span></span>

<span data-ttu-id="a5b8c-187">[IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) を実装する既定のページ モデル プロバイダーは、ページ モデルを構成するための拡張ポイントを提供するようにデザインされた規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-187">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="a5b8c-188">これらの規則は、ページ検出をビルドおよび変更したり、機能を処理したりするときに便利です。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-188">These conventions are useful when building and modifying page discovery and processing features.</span></span>

<span data-ttu-id="a5b8c-189">このセクションの例の場合、サンプル アプリでは、応答ヘッダーを適用する [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute) である、`AddHeaderAttribute` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-189">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="a5b8c-190">規則を使用して、このサンプルでは、フォルダー内のすべてのページおよび単一ページに属性を適用する方法のデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-190">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="a5b8c-191">**フォルダー アプリ モデル規則**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-191">**Folder app model convention**</span></span>

<span data-ttu-id="a5b8c-192">[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) を使用して、指定したフォルダーにあるすべてのページの [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) インスタンスのアクションを呼び出す、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-192">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="a5b8c-193">このサンプルでは、ヘッダー (`OtherPagesHeader`) をアプリの *OtherPages* フォルダー内にあるページに追加して、`AddFolderApplicationModelConvention` を使用するデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-193">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="a5b8c-194">`localhost:5000/OtherPages/Page1` でサンプルの Page1 ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-194">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 ページの応答ヘッダーは、OtherPagesHeader が追加されていることを示しています。](razor-pages-convention-features/_static/page1-otherpages-header.png)

<span data-ttu-id="a5b8c-196">**ページ アプリ モデル規則**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-196">**Page app model convention**</span></span>

<span data-ttu-id="a5b8c-197">[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) を使用して、指定した名前でページの [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) のアクションを呼び出す、[IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) を作成して追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-197">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the speciifed name.</span></span>

<span data-ttu-id="a5b8c-198">サンプルでは、ヘッダー (`AboutHeader`) を [About] ページに追加して、`AddPageApplicationModelConvention` を使用するデモを実行します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-198">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="a5b8c-199">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-199">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、AboutHeader が追加されたことを示しています。](razor-pages-convention-features/_static/about-page-about-header.png)

<span data-ttu-id="a5b8c-201">**フィルターの構成**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-201">**Configure a filter**</span></span>

<span data-ttu-id="a5b8c-202">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) では、指定したフィルターを適用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-202">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="a5b8c-203">フィルター クラスを実装できますが、サンプル アプリでは、ラムダ式でフィルターを実装する方法を示しています。これは、フィルターを返すファクトリとしてバックグラウンドで実装されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-203">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="a5b8c-204">ページ アプリ モデルは、*OtherPages* フォルダーの Page2 ページにつながるセグメントの相対パスを確認するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-204">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="a5b8c-205">条件を満たすと、ヘッダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-205">If the condition passes, a header is added.</span></span> <span data-ttu-id="a5b8c-206">満たさない場合は、`EmptyFilter` が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-206">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="a5b8c-207">`EmptyFilter` は[アクション フィルター](xref:mvc/controllers/filters#action-filters)です。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-207">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="a5b8c-208">アクション フィルターは Razor ページによって無視されるため、パスに `OtherPages/Page2` が含まれない場合は、意図されたように `EmptyFilter` は操作されません。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-208">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="a5b8c-209">`localhost:5000/OtherPages/Page2` でサンプルの Page2 ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-209">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header は、Page2 への応答に追加されます。](razor-pages-convention-features/_static/page2-filter-header.png)

<span data-ttu-id="a5b8c-211">**フィルター ファクトリの構成**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-211">**Configure a filter factory**</span></span>

<span data-ttu-id="a5b8c-212">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) では、[フィルター](xref:mvc/controllers/filters)をすべての Razor ページに適用するように、指定したファクトリを構成します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-212">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="a5b8c-213">サンプル アプリでは、アプリのページに対する 2 つの値と共にヘッダー (`FilterFactoryHeader`) を追加して、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)を使用する例を提供します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-213">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="a5b8c-214">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="a5b8c-214">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="a5b8c-215">`localhost:5000/About` でサンプルの [About] ページを要求し、そのヘッダーを調べて結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-215">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![[About] ページの応答ヘッダーは、FilterFactoryHeader ヘッダーが 2 つ追加されたことを示しています。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="a5b8c-217">既定のページ アプリ モデル プロバイダーを置き換える</span><span class="sxs-lookup"><span data-stu-id="a5b8c-217">Replace the default page app model provider</span></span>

<span data-ttu-id="a5b8c-218">Razor ページでは、`IPageApplicationModelProvider` インターフェイスを使用して、[DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider) を作成します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-218">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="a5b8c-219">既定のモデル プロバイダーから継承し、ハンドラーの検出と処理に独自の実装ロジックを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-219">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="a5b8c-220">既定の実装 ([参照ソース](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) では、以下に示すように、*名前なし*と*名前付き*の名前付けハンドラーの規則を確立します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-220">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="a5b8c-221">**既定の名前なしハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-221">**Default unnamed handler methods**</span></span>

<span data-ttu-id="a5b8c-222">HTTP 動詞のハンドラー メソッド ("名前なし" ハンドラー メソッド) は、`On<HTTP verb>[Async]` の規則に従います (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます)。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-222">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="a5b8c-223">名前なしハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="a5b8c-223">Unnamed handler method</span></span>     | <span data-ttu-id="a5b8c-224">操作</span><span class="sxs-lookup"><span data-stu-id="a5b8c-224">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="a5b8c-225">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-225">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="a5b8c-226">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-226">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="a5b8c-227">DELETE 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-227">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="a5b8c-228">PUT 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-228">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="a5b8c-229">PATCH 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-229">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="a5b8c-230">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-230">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="a5b8c-231">**既定の名前付きハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-231">**Default named handler methods**</span></span>

<span data-ttu-id="a5b8c-232">開発者 ("名前付き" ハンドラー メソッド) によって指定されたハンドラー メソッドは、同様の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-232">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="a5b8c-233">ハンドラー名は HTTP 動詞の後、または HTTP 動詞と `Async`: `On<HTTP verb><handler name>[Async]` (`Async` の追加は省略可能ですが、非同期メソッドの場合は推奨されます) の間に表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-233">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="a5b8c-234">たとえば、メッセージを処理するメソッドは、以下の表に示されている名前指定を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-234">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="a5b8c-235">名前付きハンドラー メソッドの例</span><span class="sxs-lookup"><span data-stu-id="a5b8c-235">Example named handler method</span></span>             | <span data-ttu-id="a5b8c-236">操作の例</span><span class="sxs-lookup"><span data-stu-id="a5b8c-236">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="a5b8c-237">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-237">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="a5b8c-238">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-238">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="a5b8c-239">メッセージを削除します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-239">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="a5b8c-240">メッセージを配置します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-240">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="a5b8c-241">メッセージを修正します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-241">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="a5b8c-242">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-242">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="a5b8c-243">**ハンドラー メソッド名をカスタマイズする**</span><span class="sxs-lookup"><span data-stu-id="a5b8c-243">**Customize handler method names**</span></span>

<span data-ttu-id="a5b8c-244">名前なしハンドラー メソッドと名前付きハンドラー メソッドに名前を付ける方法を変更するとします。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-244">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="a5b8c-245">別の名前付けスキームは、メソッド名が "On" で始まることを避けて、最初の単語セグメントを使用して HTTP 動詞を決定するためのものです。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-245">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="a5b8c-246">DELETE、PUT、PATCH の動詞を POST に変換するなど、その他の変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-246">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="a5b8c-247">このようなスキームは、次の表に示すようなメソッド名を指定します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-247">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="a5b8c-248">ハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="a5b8c-248">Handler method</span></span>                       | <span data-ttu-id="a5b8c-249">操作</span><span class="sxs-lookup"><span data-stu-id="a5b8c-249">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="a5b8c-250">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-250">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="a5b8c-251">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-251">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="a5b8c-252">DELETE 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-252">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="a5b8c-253">PUT 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-253">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="a5b8c-254">PATCH 要求を処理します&#8224;。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-254">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="a5b8c-255">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-255">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="a5b8c-256">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-256">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="a5b8c-257">削除するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-257">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="a5b8c-258">配置するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-258">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="a5b8c-259">修正するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-259">POST a message to patch.</span></span>       |

<span data-ttu-id="a5b8c-260">&#8224;ページへの API 呼び出しを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-260">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="a5b8c-261">このスキームを確立するには、`DefaultPageApplicationModelProvider` クラスから継承して [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) メソッドを上書きし、カスタム ロジックを適用して [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) ハンドラー名を解決します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-261">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="a5b8c-262">サンプル アプリは、その `CustomPageApplicationModelProvider` クラスでこれがどのように行われるかを示します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-262">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="a5b8c-263">クラスの主な内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-263">Highlights of the class include:</span></span>

* <span data-ttu-id="a5b8c-264">クラスは、`DefaultPageApplicationModelProvider` を継承します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-264">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="a5b8c-265">`PageHandlerModel` を作成するときに、`TryParseHandlerMethod` は HTTP 動詞 (`httpMethod`) と名前付きハンドラー名 (`handlerName`) を決定するハンドラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-265">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="a5b8c-266">存在する場合、`Async` の後置形式は無視されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-266">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="a5b8c-267">メソッド名から HTTP 動詞を解析するために、文字種が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-267">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="a5b8c-268">メソッド名 (`Async` なし) が HTTP 動詞名と同じ場合、名前付きハンドラーはありません。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-268">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="a5b8c-269">`handlerName` に `null` が設定され、メソッド名は `Get`、`Post`、`Delete`、`Put`、または `Patch` になります。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-269">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="a5b8c-270">メソッド名 (`Async` なし) が HTTP 動詞名より長い場合、名前付きハンドラーは存在します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-270">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="a5b8c-271">`handlerName` に `<method name (less 'Async', if present)>` が設定されています。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-271">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="a5b8c-272">たとえば、"GetMessage" と "GetMessageAsync" は両方、"GetMessage" のハンドラー名を使用します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-272">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="a5b8c-273">DELETE、PUT、および PATCH HTTP 動詞は、POST に変換されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-273">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="a5b8c-274">`Startup` クラスに `CustomPageApplicationModelProvider` を登録します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-274">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="a5b8c-275">*Index.cshtml.cs* のページ モデルは、通常のハンドラー メソッドの名前付け規則が、アプリのページに対してどのように変更されるかを示します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-275">The page model in *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="a5b8c-276">Razor ページで使用される通常の "On" プレフィックスの名前付けは削除されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-276">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="a5b8c-277">ページの状態を初期化するメソッドは、`Get` という名前になりました。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-277">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="a5b8c-278">いずれかのページでいずれかのモデルを開くと、アプリ全体で使用されるこの規則を表示できます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-278">You can see this convention used throughout the app if you open any page model for any of the pages.</span></span>

<span data-ttu-id="a5b8c-279">その他の各メソッドは、その処理について説明する HTTP 動詞で始まります。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-279">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="a5b8c-280">通常、`Delete` で開始する 2 つのメソッドは、DELETE HTTP 動詞として処理されますが、`TryParseHandlerMethod` のロジックは、明示的に両方のハンドラーの動詞を POST に設定します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-280">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="a5b8c-281">`Async` は、`DeleteAllMessages` と `DeleteMessageAsync` の間で省略可能であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-281">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="a5b8c-282">これらは両方、非同期メソッドですが、`Async` の後置形式を使用するかどうかを選択できます。使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-282">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="a5b8c-283">ここでは、`DeleteAllMessages` はデモンストレーション目的で使用されますが、メソッド `DeleteAllMessagesAsync` のように名前を付けることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-283">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="a5b8c-284">これはサンプルの実装の処理に影響しませんが、`Async` の後置形式を使用して、非同期メソッドというファクトを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-284">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="a5b8c-285">*Index.cshtml* で指定されたハンドラー名は、`DeleteAllMessages` と `DeleteMessageAsync` ハンドラー メソッドに一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-285">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="a5b8c-286">ハンドラー メソッド名 `DeleteMessageAsync` の `Async` は、メソッドへの POST 要求に一致するハンドラーの `TryParseHandlerMethod` によって除外されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-286">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="a5b8c-287">`DeleteMessage` の `asp-page-handler` の名前は、ハンドラー メソッド `DeleteMessageAsync` に一致します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-287">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="a5b8c-288">MVC フィルターとページ フィルター (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="a5b8c-288">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="a5b8c-289">Razor ページはハンドラー メソッドを使用するため、MVC [アクション フィルター](xref:mvc/controllers/filters#action-filters)は Razor ページによって無視されます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-289">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="a5b8c-290">その他の型の MVC フィルターは、[承認](xref:mvc/controllers/filters#authorization-filters)、[例外](xref:mvc/controllers/filters#exception-filters)、[リソース](xref:mvc/controllers/filters#resource-filters)、および[結果](xref:mvc/controllers/filters#result-filters)を使用するために利用できます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-290">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="a5b8c-291">詳細については、「[フィルター](xref:mvc/controllers/filters)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-291">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="a5b8c-292">ページ フィルター ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) は、Razor ページに適用されるフィルターです。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-292">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="a5b8c-293">このフィルターで、ページ ハンドラー メソッドの実行を囲みます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-293">It surrounds the execution of a page handler method.</span></span> <span data-ttu-id="a5b8c-294">これにより、ページ ハンドラー メソッドの実行のステージでカスタムのコードを処理できます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-294">It allows you to process custom code at stages of page handler method execution.</span></span> <span data-ttu-id="a5b8c-295">サンプル アプリの例は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-295">Here's an example from the sample app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

<span data-ttu-id="a5b8c-296">このフィルターは、"TriggerValue" の `globalTemplate` ルート値をチェックし、"ReplacementValue" で入れ替えます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-296">This filter checks for a `globalTemplate` route value of "TriggerValue" and swaps in "ReplacementValue".</span></span>

<span data-ttu-id="a5b8c-297">`ReplaceRouteValueFilter` 属性は `PageModel` に直接適用できます。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-297">The `ReplaceRouteValueFilter` attribute can be applied directly to a `PageModel`:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

<span data-ttu-id="a5b8c-298">`localhost:5000/OtherPages/Page3/TriggerValue` でサンプル アプリから Page3 ページを要求します。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-298">Request the Page3 page from the sample app with at `localhost:5000/OtherPages/Page3/TriggerValue`.</span></span> <span data-ttu-id="a5b8c-299">フィルターでどのようにルート値を置き換えるのかがわかります。</span><span class="sxs-lookup"><span data-stu-id="a5b8c-299">Notice how the filter replaces the route value:</span></span>

![TriggerValue ルート セグメントを使った OtherPages/Page3 への要求は、ルートの値を ReplacementValue に置き換えるフィルターになります。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a><span data-ttu-id="a5b8c-301">関連項目</span><span class="sxs-lookup"><span data-stu-id="a5b8c-301">See also</span></span>

* [<span data-ttu-id="a5b8c-302">Razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="a5b8c-302">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
