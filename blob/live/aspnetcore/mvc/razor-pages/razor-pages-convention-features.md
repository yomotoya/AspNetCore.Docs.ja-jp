---
title: "ASP.NET Core で razor ページのルートとアプリの規則機能"
author: guardrex
description: "モデル プロバイダーの規則の機能をルートとアプリの利用方法コントロール ページのルーティング、探索、および処理を検出します。"
keywords: "ASP.NET Core、Razor ページ、規則、AddFolderRouteModelConvention、AddPageRouteModelConvention、AddPageRoute、AddFolderApplicationModelConvention、AddPageApplicationModelConvention、ConfigureFilter、フィルター処理"
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.assetid: 6b60514c-81ad-485b-bb22-9b71416dff08
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 81fe5198e25c4275f5cf0a123536a9130be8c1d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a><span data-ttu-id="746e6-104">ASP.NET Core で razor ページのルートとアプリの規則機能</span><span class="sxs-lookup"><span data-stu-id="746e6-104">Razor Pages route and app convention features in ASP.NET Core</span></span>

<span data-ttu-id="746e6-105">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="746e6-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="746e6-106">ページのルーティング、探索、および Razor ページのアプリでの処理を制御するページのルートとアプリ モデル プロバイダー規約機能を使用する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="746e6-106">Learn how to use page route and app model provider convention features to control page routing, discovery, and processing in Razor Pages apps.</span></span> <span data-ttu-id="746e6-107">使用してページにルーティングを構成する個々 のページのページのカスタム ルートを構成する必要がある場合、 [AddPageRoute 規約](#configure-a-page-route)このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="746e6-107">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="746e6-108">使用して、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) をこのトピックで説明する機能を探索します。</span><span class="sxs-lookup"><span data-stu-id="746e6-108">Use the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

| <span data-ttu-id="746e6-109">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="746e6-109">Features</span></span> | <span data-ttu-id="746e6-110">このサンプルでは.</span><span class="sxs-lookup"><span data-stu-id="746e6-110">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="746e6-111">モデルのルートとアプリの規則</span><span class="sxs-lookup"><span data-stu-id="746e6-111">Route and app model conventions</span></span>](#add-route-and-app-model-conventions)<br><br><span data-ttu-id="746e6-112">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="746e6-112">Conventions.Add</span></span><ul><li><span data-ttu-id="746e6-113">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="746e6-113">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="746e6-114">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="746e6-114">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="746e6-115">アプリのページへのルート テンプレートとヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="746e6-115">Adding a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="746e6-116">ページのルート アクションの表記規則</span><span class="sxs-lookup"><span data-stu-id="746e6-116">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="746e6-117">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="746e6-117">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="746e6-118">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="746e6-118">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="746e6-119">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="746e6-119">AddPageRoute</span></span></li></ul> | <span data-ttu-id="746e6-120">1 つのページと、フォルダー内のページには、ルート テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="746e6-120">Adding a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="746e6-121">ページのモデルの操作規則</span><span class="sxs-lookup"><span data-stu-id="746e6-121">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="746e6-122">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="746e6-122">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="746e6-123">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="746e6-123">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="746e6-124">ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</span><span class="sxs-lookup"><span data-stu-id="746e6-124">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="746e6-125">フォルダー内のページにヘッダーを追加する、単一のページにヘッダーを追加して、構成、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)アプリのページにヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="746e6-125">Adding a header to pages in a folder, adding a header to a single page, and configuring a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="746e6-126">既定ページ アプリ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="746e6-126">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="746e6-127">ハンドラーの名前付け規則を変更するには、既定ページ モデル プロバイダーを置換します。</span><span class="sxs-lookup"><span data-stu-id="746e6-127">Replacing the default page model provider to change the conventions for handler naming.</span></span> |

## <a name="add-route-and-app-model-conventions"></a><span data-ttu-id="746e6-128">モデルのルートとアプリの規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="746e6-128">Add route and app model conventions</span></span>

<span data-ttu-id="746e6-129">デリゲートを追加[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) Razor ページに適用されるルートとアプリのモデルの規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="746e6-129">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add route and app model conventions that apply to Razor Pages.</span></span>

<span data-ttu-id="746e6-130">**すべてのページにルート モデルの規則を追加します。**</span><span class="sxs-lookup"><span data-stu-id="746e6-130">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="746e6-131">使用して[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を作成し、追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)のコレクションに[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)ルートとページのモデルの中に適用されるインスタンス構築します。</span><span class="sxs-lookup"><span data-stu-id="746e6-131">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="746e6-132">サンプル アプリを追加、`{globalTemplate?}`すべてのアプリのページにルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="746e6-132">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="746e6-133">`Order`プロパティを`AttributeRouteModel`に設定されている`0`(ゼロ)。</span><span class="sxs-lookup"><span data-stu-id="746e6-133">The `Order` property for the `AttributeRouteModel` is set to `0` (zero).</span></span> <span data-ttu-id="746e6-134">これにより、このテンプレートが与えられているルート データ値の位置は、最初の優先順位 1 つのルート値を指定するときにします。</span><span class="sxs-lookup"><span data-stu-id="746e6-134">This ensures that this template is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="746e6-135">たとえば、サンプルが追加されて、`{aboutTemplate?}`トピックの後半でルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="746e6-135">For example, the sample adds an `{aboutTemplate?}` route template later in the topic.</span></span> <span data-ttu-id="746e6-136">`{aboutTemplate?}`テンプレートが指定された、`Order`の`1`します。</span><span class="sxs-lookup"><span data-stu-id="746e6-136">The `{aboutTemplate?}` template is given an `Order` of `1`.</span></span> <span data-ttu-id="746e6-137">[バージョン情報] ページが要求された場合`/About/RouteDataValue`、"RouteDataValue"が読み込まれます`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["aboutTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="746e6-137">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="746e6-138">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="746e6-138">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="746e6-139">[要求について] ページで、サンプルの`localhost:5000/About/GlobalRouteValue`され、結果を調べる。</span><span class="sxs-lookup"><span data-stu-id="746e6-139">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![GlobalRouteValue のルート セグメントでは、バージョン情報 ページが要求されます。](razor-pages-convention-features/_static/about-page-global-template.png)

<span data-ttu-id="746e6-142">**すべてのページにアプリ モデルの規則を追加します。**</span><span class="sxs-lookup"><span data-stu-id="746e6-142">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="746e6-143">使用して[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)のコレクションに[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)ルートとページの中に適用されるインスタンスモデルの構築します。</span><span class="sxs-lookup"><span data-stu-id="746e6-143">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="746e6-144">これとトピックの以降の他の規則を示すため、サンプル アプリケーションが含まれています、`AddHeaderAttribute`クラスです。</span><span class="sxs-lookup"><span data-stu-id="746e6-144">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="746e6-145">クラスのコンス トラクターを受け入れる、`name`文字列と`values`文字列配列。</span><span class="sxs-lookup"><span data-stu-id="746e6-145">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="746e6-146">これらの値が使用されるその`OnResultExecuting`応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="746e6-146">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="746e6-147">完全クラスを示す、[モデル操作の規則 ページ](#page-model-action-conventions)トピックの以降のセクションでします。</span><span class="sxs-lookup"><span data-stu-id="746e6-147">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="746e6-148">サンプル アプリは、 `AddHeaderAttribute` 、ヘッダーを追加するクラス`GlobalHeader`、すべてのアプリのページに。</span><span class="sxs-lookup"><span data-stu-id="746e6-148">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="746e6-149">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="746e6-149">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="746e6-150">要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。</span><span class="sxs-lookup"><span data-stu-id="746e6-150">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![バージョン情報 ページの応答ヘッダー、GlobalHeader が追加されていることを示します。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a><span data-ttu-id="746e6-152">ページのルート アクションの表記規則</span><span class="sxs-lookup"><span data-stu-id="746e6-152">Page route action conventions</span></span>

<span data-ttu-id="746e6-153">派生する既定のルート モデル プロバイダー [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider)をページのルートを構成するための機能拡張ポイントを提供できるように設計された規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="746e6-153">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="746e6-154">**フォルダーのルート モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="746e6-154">**Folder route model convention**</span></span>

<span data-ttu-id="746e6-155">使用して[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention)を作成して追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)でアクションを呼び出す、 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)すべての下にあるページ、指定したフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="746e6-155">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="746e6-156">サンプル アプリは`AddFolderRouteModelConvention`を追加する、`{otherPagesTemplate?}`ルート テンプレート内のページに、 *OtherPages*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="746e6-156">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="746e6-157">`Order`プロパティを`AttributeRouteModel`に設定されている`1`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-157">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="746e6-158">これにより、テンプレートを`{globalTemplate?}`(このトピックではセット) が優先単一のルート値を指定するときに、ルートの最初のデータ値の位置。</span><span class="sxs-lookup"><span data-stu-id="746e6-158">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="746e6-159">Page1 ページが要求されている場合`/OtherPages/Page1/RouteDataValue`、"RouteDataValue"に読み込まれる`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="746e6-159">If the Page1 page is requested at `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="746e6-160">サンプルの Page1 ページ要求`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue`が表示され、結果を調べる。</span><span class="sxs-lookup"><span data-stu-id="746e6-160">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Page1 OtherPages フォルダーには、GlobalRouteValue と OtherPagesRouteValue のルート セグメントを持つ要求されます。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="746e6-163">**ページのルート モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="746e6-163">**Page route model convention**</span></span>

<span data-ttu-id="746e6-164">使用して[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention)を作成し、追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)でアクションを呼び出す、 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)指定したページの名前です。</span><span class="sxs-lookup"><span data-stu-id="746e6-164">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="746e6-165">サンプル アプリは`AddPageRouteModelConvention`を追加する、`{aboutTemplate?}`バージョン情報 ページにルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="746e6-165">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> <span data-ttu-id="746e6-166">`Order`プロパティを`AttributeRouteModel`に設定されている`1`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-166">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="746e6-167">これにより、テンプレートを`{globalTemplate?}`(このトピックではセット) が優先単一のルート値を指定するときに、ルートの最初のデータ値の位置。</span><span class="sxs-lookup"><span data-stu-id="746e6-167">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="746e6-168">[バージョン情報] ページが要求されている場合`/About/RouteDataValue`、"RouteDataValue"が読み込まれます`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["aboutTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="746e6-168">If the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="746e6-169">[要求について] ページで、サンプルの`localhost:5000/About/GlobalRouteValue/AboutRouteValue`され、結果を調べる。</span><span class="sxs-lookup"><span data-stu-id="746e6-169">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![ページに関する要求に含まれるルート セグメント GlobalRouteValue および AboutRouteValue されます。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="746e6-172">ページのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="746e6-172">Configure a page route</span></span>

<span data-ttu-id="746e6-173">使用して[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute)を指定したページのパスにあるページへのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="746e6-173">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="746e6-174">ページに生成されたリンクは、指定されたルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-174">Generated links to the page use your specified route.</span></span> <span data-ttu-id="746e6-175">`AddPageRoute`使用して`AddPageRouteModelConvention`ルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="746e6-175">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="746e6-176">サンプル アプリへのルートを作成する`/TheContactPage`の*Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="746e6-176">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="746e6-177">連絡先 ページは、現在は到達も`/Contact`既定のルートを使用しています。</span><span class="sxs-lookup"><span data-stu-id="746e6-177">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="746e6-178">メンバーのページに、サンプル アプリのカスタム ルートでは、省略可能な`text`ルート セグメント (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="746e6-178">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="746e6-179">ページは、この省略可能なセグメントでも含まれています。 その`@page`ディレクティブの訪問者のページにアクセスする場合にその`/Contact`ルート。</span><span class="sxs-lookup"><span data-stu-id="746e6-179">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="746e6-180">URL が生成されることに注意してください、**連絡先**レンダリングされるページのリンクには、更新されたルートが反映されます。</span><span class="sxs-lookup"><span data-stu-id="746e6-180">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![ナビゲーション バーで、サンプル アプリにお問い合わせくださいリンク](razor-pages-convention-features/_static/contact-link.png)

![レンダリングされる HTML の連絡先のリンクの検査 href に設定されていることを示します '/TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

<span data-ttu-id="746e6-183">ページにアクセスして、連絡先どちらでも、通常そのルート上`/Contact`、またはカスタムのルート`/TheContactPage`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-183">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="746e6-184">追加の指定した場合`text`ルート セグメント ページを示しています、HTML でエンコードされたセグメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="746e6-184">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![オプション 'text' ルート セグメント、URL で 'TextValue' を指定する edge ブラウザー例です。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="746e6-187">ページのモデルの操作規則</span><span class="sxs-lookup"><span data-stu-id="746e6-187">Page model action conventions</span></span>

<span data-ttu-id="746e6-188">実装してページ モデルの既定のプロバイダー [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider)ページ モデルを構成するための機能拡張ポイントを提供するように設計されている規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="746e6-188">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="746e6-189">これらの規則は、ビルドとページの検出と処理機能を変更すると便利です。</span><span class="sxs-lookup"><span data-stu-id="746e6-189">These conventions are useful when building and modifying page discovery and processing features.</span></span>

<span data-ttu-id="746e6-190">このセクションの例については、サンプル アプリを使用して、`AddHeaderAttribute`はクラス、 [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)、応答ヘッダーを適用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-190">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="746e6-191">規則を使用して、サンプルは、1 つのページと、フォルダー内のページのすべてには、属性を適用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="746e6-191">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="746e6-192">**フォルダー アプリ モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="746e6-192">**Folder app model convention**</span></span>

<span data-ttu-id="746e6-193">使用して[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)でアクションを呼び出す[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)のインスタンス指定したフォルダーの下のすべてのページです。</span><span class="sxs-lookup"><span data-stu-id="746e6-193">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="746e6-194">サンプルの使用`AddFolderApplicationModelConvention`、ヘッダーを追加することによって`OtherPagesHeader`、内のページに、 *OtherPages*アプリのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="746e6-194">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="746e6-195">サンプルの Page1 ページ要求`localhost:5000/OtherPages/Page1`結果を表示するヘッダーを検査および。</span><span class="sxs-lookup"><span data-stu-id="746e6-195">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 ページの応答ヘッダー、OtherPagesHeader が追加されていることを示します。](razor-pages-convention-features/_static/page1-otherpages-header.png)

<span data-ttu-id="746e6-197">**ページ アプリ モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="746e6-197">**Page app model convention**</span></span>

<span data-ttu-id="746e6-198">使用して[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)でアクションを呼び出す、 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)ページの名前に置き換えます speciifed。</span><span class="sxs-lookup"><span data-stu-id="746e6-198">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the speciifed name.</span></span>

<span data-ttu-id="746e6-199">サンプルの使用`AddPageApplicationModelConvention`、ヘッダーを追加することによって`AboutHeader`、バージョン情報 ページに。</span><span class="sxs-lookup"><span data-stu-id="746e6-199">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="746e6-200">要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。</span><span class="sxs-lookup"><span data-stu-id="746e6-200">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![バージョン情報 ページの応答ヘッダー、AboutHeader が追加されていることを示します。](razor-pages-convention-features/_static/about-page-about-header.png)

<span data-ttu-id="746e6-202">**フィルターを構成します。**</span><span class="sxs-lookup"><span data-stu-id="746e6-202">**Configure a filter**</span></span>

<span data-ttu-id="746e6-203">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter)指定されたフィルターを適用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="746e6-203">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="746e6-204">フィルター クラスを実装することができますが、サンプル アプリは舞台裏フィルターを返すファクトリとして実装されているラムダ式でフィルターを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="746e6-204">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="746e6-205">セグメントで Page2 ページへの相対パスを確認するページ アプリ モデルが使用される、 *OtherPages*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="746e6-205">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="746e6-206">条件が成功した場合、ヘッダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="746e6-206">If the condition passes, a header is added.</span></span> <span data-ttu-id="746e6-207">ない場合、`EmptyFilter`を適用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-207">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="746e6-208">`EmptyFilter`[アクション フィルター](xref:mvc/controllers/filters#action-filters)です。</span><span class="sxs-lookup"><span data-stu-id="746e6-208">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="746e6-209">Razor ページによってアクション フィルターが無視されるので、`EmptyFilter`キャッシュなしで、パスが含まれていないかどうかは意図されたように`OtherPages/Page2`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-209">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="746e6-210">サンプルのページ 2 ページを要求`localhost:5000/OtherPages/Page2`され、結果を表示するヘッダーを調べる。</span><span class="sxs-lookup"><span data-stu-id="746e6-210">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header は Page2 の応答に追加されます。](razor-pages-convention-features/_static/page2-filter-header.png)

<span data-ttu-id="746e6-212">**フィルターのファクトリを構成します。**</span><span class="sxs-lookup"><span data-stu-id="746e6-212">**Configure a filter factory**</span></span>

<span data-ttu-id="746e6-213">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__)構成を適用する指定されたファクトリ[フィルター](xref:mvc/controllers/filters)すべての Razor ページにします。</span><span class="sxs-lookup"><span data-stu-id="746e6-213">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="746e6-214">サンプル アプリを使用する例を提供する、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)、ヘッダーを追加することによって`FilterFactoryHeader`アプリのページへの 2 つの値。</span><span class="sxs-lookup"><span data-stu-id="746e6-214">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="746e6-215">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="746e6-215">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="746e6-216">要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。</span><span class="sxs-lookup"><span data-stu-id="746e6-216">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![バージョン情報 ページの応答ヘッダーは、2 つの FilterFactoryHeader ヘッダーが追加されたことを示します。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="746e6-218">既定ページ アプリ モデル プロバイダーを置き換える</span><span class="sxs-lookup"><span data-stu-id="746e6-218">Replace the default page app model provider</span></span>

<span data-ttu-id="746e6-219">Razor ページを使用して、`IPageApplicationModelProvider`を作成するインターフェイス、 [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)です。</span><span class="sxs-lookup"><span data-stu-id="746e6-219">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="746e6-220">ハンドラーの検出と処理のため、独自の実装ロジックを提供する既定のモデル プロバイダーから継承することができます。</span><span class="sxs-lookup"><span data-stu-id="746e6-220">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="746e6-221">既定の実装 ([参照ソース](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) の規則を確立*名前のない*と*という*は以下の説明、名前を付けるハンドラー。</span><span class="sxs-lookup"><span data-stu-id="746e6-221">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="746e6-222">**既定の名前のないハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="746e6-222">**Default unnamed handler methods**</span></span>

<span data-ttu-id="746e6-223">ハンドラー メソッドの HTTP 動詞 (「名前のない」ハンドラー メソッド)、規則に従って: `On<HTTP verb>[Async]` (追加`Async`は任意ですが、非同期メソッドのために推奨)。</span><span class="sxs-lookup"><span data-stu-id="746e6-223">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="746e6-224">名前のないハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="746e6-224">Unnamed handler method</span></span>     | <span data-ttu-id="746e6-225">操作</span><span class="sxs-lookup"><span data-stu-id="746e6-225">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="746e6-226">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="746e6-226">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="746e6-227">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-227">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="746e6-228">#8224; (&)、DELETE 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-228">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="746e6-229">#8224; (&)、PUT 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-229">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="746e6-230">#8224; (&)、PATCH 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-230">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="746e6-231">&#8224;です。ページに API 呼び出しを行うために使用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-231">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="746e6-232">**既定の名前付きハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="746e6-232">**Default named handler methods**</span></span>

<span data-ttu-id="746e6-233">(「名前」ハンドラー メソッド)、開発者によって提供されるハンドラー メソッドでは、同様の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="746e6-233">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="746e6-234">HTTP 動詞の後、または HTTP 動詞の間に、ハンドラーの名前が表示されます、 `Async`: `On<HTTP verb><handler name>[Async]` (追加`Async`は任意ですが、非同期メソッドのために推奨)。</span><span class="sxs-lookup"><span data-stu-id="746e6-234">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="746e6-235">たとえば、メッセージを処理するメソッドは、次の表に示すように名前が付けかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="746e6-235">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="746e6-236">ハンドラー メソッドをという名前の例</span><span class="sxs-lookup"><span data-stu-id="746e6-236">Example named handler method</span></span>             | <span data-ttu-id="746e6-237">例の操作</span><span class="sxs-lookup"><span data-stu-id="746e6-237">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="746e6-238">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="746e6-238">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="746e6-239">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="746e6-239">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="746e6-240">#8224; (&)、メッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="746e6-240">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="746e6-241">#8224; (&)、メッセージを配置します。</span><span class="sxs-lookup"><span data-stu-id="746e6-241">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="746e6-242">メッセージ &#8224; 修正プログラムを適用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-242">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="746e6-243">&#8224;です。ページに API 呼び出しを行うために使用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-243">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="746e6-244">**ハンドラー メソッドの名前を変更します。**</span><span class="sxs-lookup"><span data-stu-id="746e6-244">**Customize handler method names**</span></span>

<span data-ttu-id="746e6-245">名前と名前付きのハンドラー メソッドの名前を変更したいとします。</span><span class="sxs-lookup"><span data-stu-id="746e6-245">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="746e6-246">別の名前付けスキームでは、"On"に、メソッド名に開始しないようにし、HTTP 動詞を決定する単語の最初のセグメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-246">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="746e6-247">その他の変更を行うことができます for DELETE 動詞を変換するなど PUT、および投稿に修正プログラムを適用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-247">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="746e6-248">このようなスキームでは、次の表に示すように、メソッド名を提供します。</span><span class="sxs-lookup"><span data-stu-id="746e6-248">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="746e6-249">ハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="746e6-249">Handler method</span></span>                       | <span data-ttu-id="746e6-250">操作</span><span class="sxs-lookup"><span data-stu-id="746e6-250">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="746e6-251">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="746e6-251">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="746e6-252">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-252">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="746e6-253">#8224; (&)、DELETE 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-253">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="746e6-254">#8224; (&)、PUT 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-254">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="746e6-255">#8224; (&)、PATCH 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="746e6-255">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="746e6-256">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="746e6-256">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="746e6-257">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="746e6-257">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="746e6-258">削除するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="746e6-258">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="746e6-259">メッセージを投稿して配置します。</span><span class="sxs-lookup"><span data-stu-id="746e6-259">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="746e6-260">メッセージを投稿する修正プログラムを適用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-260">POST a message to patch.</span></span>       |

<span data-ttu-id="746e6-261">&#8224;です。ページに API 呼び出しを行うために使用します。</span><span class="sxs-lookup"><span data-stu-id="746e6-261">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="746e6-262">継承するこのパターンを確立するために、`DefaultPageApplicationModelProvider`クラスし、オーバーライド、 [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel)を解決するためのカスタム ロジックを提供するメソッド[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel)ハンドラー名。</span><span class="sxs-lookup"><span data-stu-id="746e6-262">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="746e6-263">サンプル アプリこれを行う方法を示します、`CustomPageApplicationModelProvider`クラス。</span><span class="sxs-lookup"><span data-stu-id="746e6-263">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="746e6-264">クラスの主な特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="746e6-264">Highlights of the class include:</span></span>

* <span data-ttu-id="746e6-265">クラスを継承`DefaultPageApplicationModelProvider`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-265">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="746e6-266">`TryParseHandlerMethod` HTTP 動詞を確認するハンドラーの処理 (`httpMethod`) と名前付きのハンドラーの名前 (`handlerName`) を作成するとき、`PageHandlerModel`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-266">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="746e6-267">`Async`存在する場合、後置形式は無視されます。</span><span class="sxs-lookup"><span data-stu-id="746e6-267">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="746e6-268">大文字と小文字を使用すると、メソッドの名前の HTTP 動詞を解析します。</span><span class="sxs-lookup"><span data-stu-id="746e6-268">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="746e6-269">ときに、メソッド名 (せず`Async`) は、HTTP 動詞名と同じ、ハンドラーがない名前付きです。</span><span class="sxs-lookup"><span data-stu-id="746e6-269">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="746e6-270">`handlerName`に設定されている`null`、メソッド名と`Get`、 `Post`、 `Delete`、 `Put`、または`Patch`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-270">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="746e6-271">ときに、メソッド名 (せず`Async`) HTTP 動詞名よりも長い名前付きハンドラーがあります。</span><span class="sxs-lookup"><span data-stu-id="746e6-271">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="746e6-272">`handlerName` に `<method name (less 'Async', if present)>` が設定されています。</span><span class="sxs-lookup"><span data-stu-id="746e6-272">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="746e6-273">たとえば、"GetMessage"と"GetMessageAsync"の両方は、"GetMessage"のハンドラー名を生成します。</span><span class="sxs-lookup"><span data-stu-id="746e6-273">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="746e6-274">削除、PUT、PATCH HTTP 動詞が POST に変換されます。</span><span class="sxs-lookup"><span data-stu-id="746e6-274">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="746e6-275">登録、`CustomPageApplicationModelProvider`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="746e6-275">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="746e6-276">分離コード ファイル*Index.cshtml.cs*アプリ内のページの通常のハンドラー メソッドの名前付け規則を変更する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="746e6-276">The code-behind file *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="746e6-277">"On"Razor ページで使用される名前が付けられ、通常は削除されます。</span><span class="sxs-lookup"><span data-stu-id="746e6-277">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="746e6-278">ページの状態を初期化するメソッドの名前は今すぐ`Get`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-278">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="746e6-279">ページのいずれかの任意の分離コード ファイルを開く場合に、アプリ全体で使用されるこの規則を表示できます。</span><span class="sxs-lookup"><span data-stu-id="746e6-279">You can see this convention used throughout the app if you open any code-behind file for any of the pages.</span></span>

<span data-ttu-id="746e6-280">それぞれの他の方法は、その処理を説明する HTTP 動詞を使用して開始します。</span><span class="sxs-lookup"><span data-stu-id="746e6-280">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="746e6-281">2 つの方法で始まる`Delete`は通常として扱う、DELETE HTTP 動詞がロジックでは、`TryParseHandlerMethod`両方ハンドラーに対して post 動詞を明示的に設定します。</span><span class="sxs-lookup"><span data-stu-id="746e6-281">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="746e6-282">なお`Async`は間で省略可能`DeleteAllMessages`と`DeleteMessageAsync`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-282">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="746e6-283">非同期のどちらを使用することもできますが、`Async`か後置; を実行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="746e6-283">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="746e6-284">`DeleteAllMessages`ここでは、デモンストレーションのための使用しますが、これらのメソッドの名前を付けることをお勧め`DeleteAllMessagesAsync`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-284">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="746e6-285">処理に影響しませんのサンプルの実装を使用して、`Async`後置非同期メソッドであるファクトを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="746e6-285">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="746e6-286">提供されるハンドラーの名前を控えておきます*Index.cshtml*一致、`DeleteAllMessages`と`DeleteMessageAsync`ハンドラー メソッド。</span><span class="sxs-lookup"><span data-stu-id="746e6-286">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="746e6-287">`Async`ハンドラー メソッドの名前で`DeleteMessageAsync`でサインアウトが考慮され、`TryParseHandlerMethod`メソッドへの POST 要求の一致するハンドラー。</span><span class="sxs-lookup"><span data-stu-id="746e6-287">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="746e6-288">`asp-page-handler`名前`DeleteMessage`ハンドラー メソッドに一致する`DeleteMessageAsync`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-288">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="746e6-289">MVC のフィルターと、ページ フィルター (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="746e6-289">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="746e6-290">MVC[アクション フィルター](xref:mvc/controllers/filters#action-filters) Razor ページ ハンドラーのメソッドを使用するために Razor ページによって無視されます。</span><span class="sxs-lookup"><span data-stu-id="746e6-290">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="746e6-291">使用するための他の種類の MVC フィルターの利用できます:[承認](xref:mvc/controllers/filters#authorization-filters)、[例外](xref:mvc/controllers/filters#exception-filters)、[リソース](xref:mvc/controllers/filters#resource-filters)、および[結果](xref:mvc/controllers/filters#result-filters)です。</span><span class="sxs-lookup"><span data-stu-id="746e6-291">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="746e6-292">詳細については、次を参照してください。、[フィルター](xref:mvc/controllers/filters)トピックです。</span><span class="sxs-lookup"><span data-stu-id="746e6-292">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="746e6-293">ページ フィルター ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) は、Razor ページに適用されるフィルターです。</span><span class="sxs-lookup"><span data-stu-id="746e6-293">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="746e6-294">ページのハンドラー メソッドの実行が囲まれます。</span><span class="sxs-lookup"><span data-stu-id="746e6-294">It surrounds the execution of a page handler method.</span></span> <span data-ttu-id="746e6-295">ページのハンドラー メソッドの実行の段階でカスタム コードを処理することができます。</span><span class="sxs-lookup"><span data-stu-id="746e6-295">It allows you to process custom code at stages of page handler method execution.</span></span> <span data-ttu-id="746e6-296">サンプル アプリからの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="746e6-296">Here's an example from the sample app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

<span data-ttu-id="746e6-297">このフィルターでのチェック、 `globalTemplate` "ReplacementValue"で"TriggerValue"とスワップの値をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="746e6-297">This filter checks for a `globalTemplate` route value of "TriggerValue" and swaps in "ReplacementValue".</span></span>

<span data-ttu-id="746e6-298">`ReplaceRouteValueFilter`に直接適用できる属性、`PageModel`分離コード。</span><span class="sxs-lookup"><span data-stu-id="746e6-298">The `ReplaceRouteValueFilter` attribute can be applied directly to a `PageModel` in code-behind:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

<span data-ttu-id="746e6-299">要求、Page3 ページでのサンプル アプリから`localhost:5000/OtherPages/Page3/TriggerValue`です。</span><span class="sxs-lookup"><span data-stu-id="746e6-299">Request the Page3 page from the sample app with at `localhost:5000/OtherPages/Page3/TriggerValue`.</span></span> <span data-ttu-id="746e6-300">フィルターが、ルート値を置換する方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="746e6-300">Notice how the filter replaces the route value:</span></span>

![ルートの値を置き換える値に置き換えてフィルターで TriggerValue ルート セグメント結果を含む OtherPages/Page3 を要求します。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a><span data-ttu-id="746e6-302">関連項目</span><span class="sxs-lookup"><span data-stu-id="746e6-302">See also</span></span>

* [<span data-ttu-id="746e6-303">Razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="746e6-303">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
