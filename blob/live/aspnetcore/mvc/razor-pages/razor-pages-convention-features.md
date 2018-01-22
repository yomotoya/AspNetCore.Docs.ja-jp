---
title: "ASP.NET Core で razor ページのルートとアプリの規則機能"
author: guardrex
description: "モデル プロバイダーの規則の機能をルートとアプリの利用方法コントロール ページのルーティング、探索、および処理を検出します。"
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 69475ca9abd4e732dc704ad6a8a2fffe219984f7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a><span data-ttu-id="61a0e-103">ASP.NET Core で razor ページのルートとアプリの規則機能</span><span class="sxs-lookup"><span data-stu-id="61a0e-103">Razor Pages route and app convention features in ASP.NET Core</span></span>

<span data-ttu-id="61a0e-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="61a0e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="61a0e-105">ページのルーティング、探索、および Razor ページのアプリでの処理を制御するページのルートとアプリ モデル プロバイダー規約機能を使用する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-105">Learn how to use page route and app model provider convention features to control page routing, discovery, and processing in Razor Pages apps.</span></span> <span data-ttu-id="61a0e-106">使用してページにルーティングを構成する個々 のページのページのカスタム ルートを構成する必要がある場合、 [AddPageRoute 規約](#configure-a-page-route)このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="61a0e-107">使用して、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) をこのトピックで説明する機能を探索します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-107">Use the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

| <span data-ttu-id="61a0e-108">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="61a0e-108">Features</span></span> | <span data-ttu-id="61a0e-109">このサンプルでは.</span><span class="sxs-lookup"><span data-stu-id="61a0e-109">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="61a0e-110">モデルのルートとアプリの規則</span><span class="sxs-lookup"><span data-stu-id="61a0e-110">Route and app model conventions</span></span>](#add-route-and-app-model-conventions)<br><br><span data-ttu-id="61a0e-111">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="61a0e-111">Conventions.Add</span></span><ul><li><span data-ttu-id="61a0e-112">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="61a0e-112">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="61a0e-113">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="61a0e-113">IPageApplicationModelConvention</span></span></li></ul> | <span data-ttu-id="61a0e-114">アプリのページへのルート テンプレートとヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-114">Adding a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="61a0e-115">ページのルート アクションの表記規則</span><span class="sxs-lookup"><span data-stu-id="61a0e-115">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="61a0e-116">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="61a0e-116">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="61a0e-117">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="61a0e-117">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="61a0e-118">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="61a0e-118">AddPageRoute</span></span></li></ul> | <span data-ttu-id="61a0e-119">1 つのページと、フォルダー内のページには、ルート テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-119">Adding a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="61a0e-120">ページのモデルの操作規則</span><span class="sxs-lookup"><span data-stu-id="61a0e-120">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="61a0e-121">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="61a0e-121">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="61a0e-122">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="61a0e-122">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="61a0e-123">ConfigureFilter (フィルター クラス、ラムダ式、またはフィルター ファクトリ)</span><span class="sxs-lookup"><span data-stu-id="61a0e-123">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="61a0e-124">フォルダー内のページにヘッダーを追加する、単一のページにヘッダーを追加して、構成、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)アプリのページにヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-124">Adding a header to pages in a folder, adding a header to a single page, and configuring a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |
| [<span data-ttu-id="61a0e-125">既定ページ アプリ モデル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="61a0e-125">Default page app model provider</span></span>](#replace-the-default-page-app-model-provider) | <span data-ttu-id="61a0e-126">ハンドラーの名前付け規則を変更するには、既定ページ モデル プロバイダーを置換します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-126">Replacing the default page model provider to change the conventions for handler naming.</span></span> |

## <a name="add-route-and-app-model-conventions"></a><span data-ttu-id="61a0e-127">モデルのルートとアプリの規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-127">Add route and app model conventions</span></span>

<span data-ttu-id="61a0e-128">デリゲートを追加[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) Razor ページに適用されるルートとアプリのモデルの規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-128">Add a delegate for [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) to add route and app model conventions that apply to Razor Pages.</span></span>

<span data-ttu-id="61a0e-129">**すべてのページにルート モデルの規則を追加します。**</span><span class="sxs-lookup"><span data-stu-id="61a0e-129">**Add a route model convention to all pages**</span></span>

<span data-ttu-id="61a0e-130">使用して[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を作成し、追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)のコレクションに[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)ルートとページのモデルの中に適用されるインスタンス構築します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-130">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="61a0e-131">サンプル アプリを追加、`{globalTemplate?}`すべてのアプリのページにルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="61a0e-131">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="61a0e-132">`Order`プロパティを`AttributeRouteModel`に設定されている`0`(ゼロ)。</span><span class="sxs-lookup"><span data-stu-id="61a0e-132">The `Order` property for the `AttributeRouteModel` is set to `0` (zero).</span></span> <span data-ttu-id="61a0e-133">これにより、このテンプレートが与えられているルート データ値の位置は、最初の優先順位 1 つのルート値を指定するときにします。</span><span class="sxs-lookup"><span data-stu-id="61a0e-133">This ensures that this template is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="61a0e-134">たとえば、サンプルが追加されて、`{aboutTemplate?}`トピックの後半でルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="61a0e-134">For example, the sample adds an `{aboutTemplate?}` route template later in the topic.</span></span> <span data-ttu-id="61a0e-135">`{aboutTemplate?}`テンプレートが指定された、`Order`の`1`します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-135">The `{aboutTemplate?}` template is given an `Order` of `1`.</span></span> <span data-ttu-id="61a0e-136">[バージョン情報] ページが要求された場合`/About/RouteDataValue`、"RouteDataValue"が読み込まれます`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["aboutTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-136">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="61a0e-137">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="61a0e-137">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="61a0e-138">[要求について] ページで、サンプルの`localhost:5000/About/GlobalRouteValue`され、結果を調べる。</span><span class="sxs-lookup"><span data-stu-id="61a0e-138">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![GlobalRouteValue のルート セグメントでは、バージョン情報 ページが要求されます。](razor-pages-convention-features/_static/about-page-global-template.png)

<span data-ttu-id="61a0e-141">**すべてのページにアプリ モデルの規則を追加します。**</span><span class="sxs-lookup"><span data-stu-id="61a0e-141">**Add an app model convention to all pages**</span></span>

<span data-ttu-id="61a0e-142">使用して[規則](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)のコレクションに[IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention)ルートとページの中に適用されるインスタンスモデルの構築します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-142">Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) to the collection of [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) instances that are applied during route and page model construction.</span></span>

<span data-ttu-id="61a0e-143">これとトピックの以降の他の規則を示すため、サンプル アプリケーションが含まれています、`AddHeaderAttribute`クラスです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-143">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="61a0e-144">クラスのコンス トラクターを受け入れる、`name`文字列と`values`文字列配列。</span><span class="sxs-lookup"><span data-stu-id="61a0e-144">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="61a0e-145">これらの値が使用されるその`OnResultExecuting`応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-145">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="61a0e-146">完全クラスを示す、[モデル操作の規則 ページ](#page-model-action-conventions)トピックの以降のセクションでします。</span><span class="sxs-lookup"><span data-stu-id="61a0e-146">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="61a0e-147">サンプル アプリは、 `AddHeaderAttribute` 、ヘッダーを追加するクラス`GlobalHeader`、すべてのアプリのページに。</span><span class="sxs-lookup"><span data-stu-id="61a0e-147">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="61a0e-148">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="61a0e-148">*Startup.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="61a0e-149">要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-149">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![バージョン情報 ページの応答ヘッダー、GlobalHeader が追加されていることを示します。](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a><span data-ttu-id="61a0e-151">ページのルート アクションの表記規則</span><span class="sxs-lookup"><span data-stu-id="61a0e-151">Page route action conventions</span></span>

<span data-ttu-id="61a0e-152">派生する既定のルート モデル プロバイダー [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider)をページのルートを構成するための機能拡張ポイントを提供できるように設計された規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-152">The default route model provider that derives from [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

<span data-ttu-id="61a0e-153">**フォルダーのルート モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="61a0e-153">**Folder route model convention**</span></span>

<span data-ttu-id="61a0e-154">使用して[AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention)を作成して追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)でアクションを呼び出す、 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)すべての下にあるページ、指定したフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-154">Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for all of the pages under the specified folder.</span></span>

<span data-ttu-id="61a0e-155">サンプル アプリは`AddFolderRouteModelConvention`を追加する、`{otherPagesTemplate?}`ルート テンプレート内のページに、 *OtherPages*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="61a0e-155">The sample app uses `AddFolderRouteModelConvention` to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="61a0e-156">`Order`プロパティを`AttributeRouteModel`に設定されている`1`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-156">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="61a0e-157">これにより、テンプレートを`{globalTemplate?}`(このトピックではセット) が優先単一のルート値を指定するときに、ルートの最初のデータ値の位置。</span><span class="sxs-lookup"><span data-stu-id="61a0e-157">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="61a0e-158">Page1 ページが要求されている場合`/OtherPages/Page1/RouteDataValue`、"RouteDataValue"に読み込まれる`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-158">If the Page1 page is requested at `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="61a0e-159">サンプルの Page1 ページ要求`localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue`が表示され、結果を調べる。</span><span class="sxs-lookup"><span data-stu-id="61a0e-159">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Page1 OtherPages フォルダーには、GlobalRouteValue と OtherPagesRouteValue のルート セグメントを持つ要求されます。](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

<span data-ttu-id="61a0e-162">**ページのルート モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="61a0e-162">**Page route model convention**</span></span>

<span data-ttu-id="61a0e-163">使用して[AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention)を作成し、追加、 [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention)でアクションを呼び出す、 [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel)指定したページの名前です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-163">Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) to create and add an [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) that invokes an action on the [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) for the page with the specified name.</span></span>

<span data-ttu-id="61a0e-164">サンプル アプリは`AddPageRouteModelConvention`を追加する、`{aboutTemplate?}`バージョン情報 ページにルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="61a0e-164">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> <span data-ttu-id="61a0e-165">`Order`プロパティを`AttributeRouteModel`に設定されている`1`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-165">The `Order` property for the `AttributeRouteModel` is set to `1`.</span></span> <span data-ttu-id="61a0e-166">これにより、テンプレートを`{globalTemplate?}`(このトピックではセット) が優先単一のルート値を指定するときに、ルートの最初のデータ値の位置。</span><span class="sxs-lookup"><span data-stu-id="61a0e-166">This ensures that the template for `{globalTemplate?}` (set earlier in the topic) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="61a0e-167">[バージョン情報] ページが要求されている場合`/About/RouteDataValue`、"RouteDataValue"が読み込まれます`RouteData.Values["globalTemplate"]`(`Order = 0`) および not `RouteData.Values["aboutTemplate"]` (`Order = 1`) の設定のため、`Order`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-167">If the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 0`) and not `RouteData.Values["aboutTemplate"]` (`Order = 1`) due to setting the `Order` property.</span></span>

<span data-ttu-id="61a0e-168">[要求について] ページで、サンプルの`localhost:5000/About/GlobalRouteValue/AboutRouteValue`され、結果を調べる。</span><span class="sxs-lookup"><span data-stu-id="61a0e-168">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![ページに関する要求に含まれるルート セグメント GlobalRouteValue および AboutRouteValue されます。](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="61a0e-171">ページのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-171">Configure a page route</span></span>

<span data-ttu-id="61a0e-172">使用して[AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute)を指定したページのパスにあるページへのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-172">Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="61a0e-173">ページに生成されたリンクは、指定されたルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-173">Generated links to the page use your specified route.</span></span> <span data-ttu-id="61a0e-174">`AddPageRoute`使用して`AddPageRouteModelConvention`ルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-174">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="61a0e-175">サンプル アプリへのルートを作成する`/TheContactPage`の*Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="61a0e-175">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

<span data-ttu-id="61a0e-176">連絡先 ページは、現在は到達も`/Contact`既定のルートを使用しています。</span><span class="sxs-lookup"><span data-stu-id="61a0e-176">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="61a0e-177">メンバーのページに、サンプル アプリのカスタム ルートでは、省略可能な`text`ルート セグメント (`{text?}`)。</span><span class="sxs-lookup"><span data-stu-id="61a0e-177">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="61a0e-178">ページは、この省略可能なセグメントでも含まれています。 その`@page`ディレクティブの訪問者のページにアクセスする場合にその`/Contact`ルート。</span><span class="sxs-lookup"><span data-stu-id="61a0e-178">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="61a0e-179">URL が生成されることに注意してください、**連絡先**レンダリングされるページのリンクには、更新されたルートが反映されます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-179">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![ナビゲーション バーで、サンプル アプリにお問い合わせくださいリンク](razor-pages-convention-features/_static/contact-link.png)

![レンダリングされる HTML の連絡先のリンクの検査 href に設定されていることを示します '/TheContactPage'](razor-pages-convention-features/_static/contact-link-source.png)

<span data-ttu-id="61a0e-182">ページにアクセスして、連絡先どちらでも、通常そのルート上`/Contact`、またはカスタムのルート`/TheContactPage`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-182">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="61a0e-183">追加の指定した場合`text`ルート セグメント ページを示しています、HTML でエンコードされたセグメントを指定します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-183">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![オプション 'text' ルート セグメント、URL で 'TextValue' を指定する edge ブラウザー例です。](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="61a0e-186">ページのモデルの操作規則</span><span class="sxs-lookup"><span data-stu-id="61a0e-186">Page model action conventions</span></span>

<span data-ttu-id="61a0e-187">実装してページ モデルの既定のプロバイダー [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider)ページ モデルを構成するための機能拡張ポイントを提供するように設計されている規則を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-187">The default page model provider that implements [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="61a0e-188">これらの規則は、ビルドとページの検出と処理機能を変更すると便利です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-188">These conventions are useful when building and modifying page discovery and processing features.</span></span>

<span data-ttu-id="61a0e-189">このセクションの例については、サンプル アプリを使用して、`AddHeaderAttribute`はクラス、 [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)、応答ヘッダーを適用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-189">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), that applies a response header:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="61a0e-190">規則を使用して、サンプルは、1 つのページと、フォルダー内のページのすべてには、属性を適用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-190">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="61a0e-191">**フォルダー アプリ モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="61a0e-191">**Folder app model convention**</span></span>

<span data-ttu-id="61a0e-192">使用して[AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)でアクションを呼び出す[PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)のインスタンス指定したフォルダーの下のすべてのページです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-192">Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) instances for all pages under the specified folder.</span></span>

<span data-ttu-id="61a0e-193">サンプルの使用`AddFolderApplicationModelConvention`、ヘッダーを追加することによって`OtherPagesHeader`、内のページに、 *OtherPages*アプリのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="61a0e-193">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

<span data-ttu-id="61a0e-194">サンプルの Page1 ページ要求`localhost:5000/OtherPages/Page1`結果を表示するヘッダーを検査および。</span><span class="sxs-lookup"><span data-stu-id="61a0e-194">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![OtherPages/Page1 ページの応答ヘッダー、OtherPagesHeader が追加されていることを示します。](razor-pages-convention-features/_static/page1-otherpages-header.png)

<span data-ttu-id="61a0e-196">**ページ アプリ モデルの規則**</span><span class="sxs-lookup"><span data-stu-id="61a0e-196">**Page app model convention**</span></span>

<span data-ttu-id="61a0e-197">使用して[AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention)を作成し、追加、 [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention)でアクションを呼び出す、 [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel)ページの名前に置き換えます speciifed。</span><span class="sxs-lookup"><span data-stu-id="61a0e-197">Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) to create and add an [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) that invokes an action on the [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) for the page with the speciifed name.</span></span>

<span data-ttu-id="61a0e-198">サンプルの使用`AddPageApplicationModelConvention`、ヘッダーを追加することによって`AboutHeader`、バージョン情報 ページに。</span><span class="sxs-lookup"><span data-stu-id="61a0e-198">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

<span data-ttu-id="61a0e-199">要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-199">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![バージョン情報 ページの応答ヘッダー、AboutHeader が追加されていることを示します。](razor-pages-convention-features/_static/about-page-about-header.png)

<span data-ttu-id="61a0e-201">**フィルターを構成します。**</span><span class="sxs-lookup"><span data-stu-id="61a0e-201">**Configure a filter**</span></span>

<span data-ttu-id="61a0e-202">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter)指定されたフィルターを適用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-202">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configures the specified filter to apply.</span></span> <span data-ttu-id="61a0e-203">フィルター クラスを実装することができますが、サンプル アプリは舞台裏フィルターを返すファクトリとして実装されているラムダ式でフィルターを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-203">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

<span data-ttu-id="61a0e-204">セグメントで Page2 ページへの相対パスを確認するページ アプリ モデルが使用される、 *OtherPages*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-204">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="61a0e-205">条件が成功した場合、ヘッダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-205">If the condition passes, a header is added.</span></span> <span data-ttu-id="61a0e-206">ない場合、`EmptyFilter`を適用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-206">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="61a0e-207">`EmptyFilter`[アクション フィルター](xref:mvc/controllers/filters#action-filters)です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-207">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="61a0e-208">Razor ページによってアクション フィルターが無視されるので、`EmptyFilter`キャッシュなしで、パスが含まれていないかどうかは意図されたように`OtherPages/Page2`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-208">Since Action filters are ignored by Razor Pages, the `EmptyFilter` no-ops as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="61a0e-209">サンプルのページ 2 ページを要求`localhost:5000/OtherPages/Page2`され、結果を表示するヘッダーを調べる。</span><span class="sxs-lookup"><span data-stu-id="61a0e-209">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header は Page2 の応答に追加されます。](razor-pages-convention-features/_static/page2-filter-header.png)

<span data-ttu-id="61a0e-211">**フィルターのファクトリを構成します。**</span><span class="sxs-lookup"><span data-stu-id="61a0e-211">**Configure a filter factory**</span></span>

<span data-ttu-id="61a0e-212">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__)構成を適用する指定されたファクトリ[フィルター](xref:mvc/controllers/filters)すべての Razor ページにします。</span><span class="sxs-lookup"><span data-stu-id="61a0e-212">[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="61a0e-213">サンプル アプリを使用する例を提供する、[フィルター ファクトリ](xref:mvc/controllers/filters#ifilterfactory)、ヘッダーを追加することによって`FilterFactoryHeader`アプリのページへの 2 つの値。</span><span class="sxs-lookup"><span data-stu-id="61a0e-213">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

<span data-ttu-id="61a0e-214">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="61a0e-214">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="61a0e-215">要求について ページで、サンプルの`localhost:5000/About`し、結果を表示するヘッダーを検査します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-215">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![バージョン情報 ページの応答ヘッダーは、2 つの FilterFactoryHeader ヘッダーが追加されたことを示します。](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a><span data-ttu-id="61a0e-217">既定ページ アプリ モデル プロバイダーを置き換える</span><span class="sxs-lookup"><span data-stu-id="61a0e-217">Replace the default page app model provider</span></span>

<span data-ttu-id="61a0e-218">Razor ページを使用して、`IPageApplicationModelProvider`を作成するインターフェイス、 [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider)です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-218">Razor Pages uses the `IPageApplicationModelProvider` interface to create a [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider).</span></span> <span data-ttu-id="61a0e-219">ハンドラーの検出と処理のため、独自の実装ロジックを提供する既定のモデル プロバイダーから継承することができます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-219">You can inherit from the default model provider to provide your own implementation logic for handler discovery and processing.</span></span> <span data-ttu-id="61a0e-220">既定の実装 ([参照ソース](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) の規則を確立*名前のない*と*という*は以下の説明、名前を付けるハンドラー。</span><span class="sxs-lookup"><span data-stu-id="61a0e-220">The default implementation ([reference source](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establishes conventions for *unnamed* and *named* handler naming, which is described below.</span></span>

<span data-ttu-id="61a0e-221">**既定の名前のないハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="61a0e-221">**Default unnamed handler methods**</span></span>

<span data-ttu-id="61a0e-222">ハンドラー メソッドの HTTP 動詞 (「名前のない」ハンドラー メソッド)、規則に従って: `On<HTTP verb>[Async]` (追加`Async`は任意ですが、非同期メソッドのために推奨)。</span><span class="sxs-lookup"><span data-stu-id="61a0e-222">Handler methods for HTTP verbs ("unnamed" handler methods) follow a convention: `On<HTTP verb>[Async]` (appending `Async` is optional but recommended for async methods).</span></span>

| <span data-ttu-id="61a0e-223">名前のないハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="61a0e-223">Unnamed handler method</span></span>     | <span data-ttu-id="61a0e-224">操作</span><span class="sxs-lookup"><span data-stu-id="61a0e-224">Operation</span></span>                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | <span data-ttu-id="61a0e-225">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-225">Initialize the page state.</span></span>     |
| `OnPost`/`OnPostAsync`     | <span data-ttu-id="61a0e-226">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-226">Handle POST requests.</span></span>          |
| `OnDelete`/`OnDeleteAsync` | <span data-ttu-id="61a0e-227">#8224; (&)、DELETE 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-227">Handle DELETE requests&#8224;.</span></span> |
| `OnPut`/`OnPutAsync`       | <span data-ttu-id="61a0e-228">#8224; (&)、PUT 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-228">Handle PUT requests&#8224;.</span></span>    |
| `OnPatch`/`OnPatchAsync`   | <span data-ttu-id="61a0e-229">#8224; (&)、PATCH 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-229">Handle PATCH requests&#8224;.</span></span>  |

<span data-ttu-id="61a0e-230">&#8224;です。ページに API 呼び出しを行うために使用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-230">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="61a0e-231">**既定の名前付きハンドラー メソッド**</span><span class="sxs-lookup"><span data-stu-id="61a0e-231">**Default named handler methods**</span></span>

<span data-ttu-id="61a0e-232">(「名前」ハンドラー メソッド)、開発者によって提供されるハンドラー メソッドでは、同様の規則に従います。</span><span class="sxs-lookup"><span data-stu-id="61a0e-232">Handler methods provided by the developer ("named" handler methods) follow a similar convention.</span></span> <span data-ttu-id="61a0e-233">HTTP 動詞の後、または HTTP 動詞の間に、ハンドラーの名前が表示されます、 `Async`: `On<HTTP verb><handler name>[Async]` (追加`Async`は任意ですが、非同期メソッドのために推奨)。</span><span class="sxs-lookup"><span data-stu-id="61a0e-233">The handler name appears after the HTTP verb or between the HTTP verb and `Async`: `On<HTTP verb><handler name>[Async]` (appending `Async` is optional but recommended for async methods).</span></span> <span data-ttu-id="61a0e-234">たとえば、メッセージを処理するメソッドは、次の表に示すように名前が付けかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="61a0e-234">For example, methods that process messages might take the naming shown in the table below.</span></span>

| <span data-ttu-id="61a0e-235">ハンドラー メソッドをという名前の例</span><span class="sxs-lookup"><span data-stu-id="61a0e-235">Example named handler method</span></span>             | <span data-ttu-id="61a0e-236">例の操作</span><span class="sxs-lookup"><span data-stu-id="61a0e-236">Example operation</span></span>        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | <span data-ttu-id="61a0e-237">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-237">Obtain a message.</span></span>        |
| `OnPostMessage`/`OnPostMessageAsync`     | <span data-ttu-id="61a0e-238">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-238">POST a message.</span></span>          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | <span data-ttu-id="61a0e-239">#8224; (&)、メッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-239">DELETE a message&#8224;.</span></span> |
| `OnPutMessage`/`OnPutMessageAsync`       | <span data-ttu-id="61a0e-240">#8224; (&)、メッセージを配置します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-240">PUT a message&#8224;.</span></span>    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | <span data-ttu-id="61a0e-241">メッセージ &#8224; 修正プログラムを適用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-241">PATCH a message&#8224;.</span></span>  |

<span data-ttu-id="61a0e-242">&#8224;です。ページに API 呼び出しを行うために使用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-242">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="61a0e-243">**ハンドラー メソッドの名前を変更します。**</span><span class="sxs-lookup"><span data-stu-id="61a0e-243">**Customize handler method names**</span></span>

<span data-ttu-id="61a0e-244">名前と名前付きのハンドラー メソッドの名前を変更したいとします。</span><span class="sxs-lookup"><span data-stu-id="61a0e-244">Assume that you prefer to change the way unnamed and named handler methods are named.</span></span> <span data-ttu-id="61a0e-245">別の名前付けスキームでは、"On"に、メソッド名に開始しないようにし、HTTP 動詞を決定する単語の最初のセグメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-245">An alternative naming scheme is to avoid starting the method names with "On" and use the first word segment to determine the HTTP verb.</span></span> <span data-ttu-id="61a0e-246">その他の変更を行うことができます for DELETE 動詞を変換するなど PUT、および投稿に修正プログラムを適用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-246">You can make other changes, such as converting the verbs for DELETE, PUT, and PATCH to POST.</span></span> <span data-ttu-id="61a0e-247">このようなスキームでは、次の表に示すように、メソッド名を提供します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-247">Such a scheme provides the method names shown in the following table.</span></span>

| <span data-ttu-id="61a0e-248">ハンドラー メソッド</span><span class="sxs-lookup"><span data-stu-id="61a0e-248">Handler method</span></span>                       | <span data-ttu-id="61a0e-249">操作</span><span class="sxs-lookup"><span data-stu-id="61a0e-249">Operation</span></span>                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | <span data-ttu-id="61a0e-250">ページの状態を初期化します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-250">Initialize the page state.</span></span>     |
| `Post`/`PostAsync`                   | <span data-ttu-id="61a0e-251">POST 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-251">Handle POST requests.</span></span>          |
| `Delete`/`DeleteAsync`               | <span data-ttu-id="61a0e-252">#8224; (&)、DELETE 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-252">Handle DELETE requests&#8224;.</span></span> |
| `Put`/`PutAsync`                     | <span data-ttu-id="61a0e-253">#8224; (&)、PUT 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-253">Handle PUT requests&#8224;.</span></span>    |
| `Patch`/`PatchAsync`                 | <span data-ttu-id="61a0e-254">#8224; (&)、PATCH 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-254">Handle PATCH requests&#8224;.</span></span>  |
| `GetMessage`                         | <span data-ttu-id="61a0e-255">メッセージを取得します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-255">Obtain a message.</span></span>              |
| `PostMessage`/`PostMessageAsync`     | <span data-ttu-id="61a0e-256">メッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-256">POST a message.</span></span>                |
| `DeleteMessage`/`DeleteMessageAsync` | <span data-ttu-id="61a0e-257">削除するメッセージを投稿します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-257">POST a message to delete.</span></span>      |
| `PutMessage`/`PutMessageAsync`       | <span data-ttu-id="61a0e-258">メッセージを投稿して配置します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-258">POST a message to put.</span></span>         |
| `PatchMessage`/`PatchMessageAsync`   | <span data-ttu-id="61a0e-259">メッセージを投稿する修正プログラムを適用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-259">POST a message to patch.</span></span>       |

<span data-ttu-id="61a0e-260">&#8224;です。ページに API 呼び出しを行うために使用します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-260">&#8224;Used for making API calls to the page.</span></span>

<span data-ttu-id="61a0e-261">継承するこのパターンを確立するために、`DefaultPageApplicationModelProvider`クラスし、オーバーライド、 [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel)を解決するためのカスタム ロジックを提供するメソッド[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel)ハンドラー名。</span><span class="sxs-lookup"><span data-stu-id="61a0e-261">To establish this scheme, inherit from the `DefaultPageApplicationModelProvider` class and override the [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) method to supply custom logic for resolving [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) handler names.</span></span> <span data-ttu-id="61a0e-262">サンプル アプリこれを行う方法を示します、`CustomPageApplicationModelProvider`クラス。</span><span class="sxs-lookup"><span data-stu-id="61a0e-262">The sample app shows you how this is done in its `CustomPageApplicationModelProvider` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

<span data-ttu-id="61a0e-263">クラスの主な特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-263">Highlights of the class include:</span></span>

* <span data-ttu-id="61a0e-264">クラスを継承`DefaultPageApplicationModelProvider`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-264">The class inherits from `DefaultPageApplicationModelProvider`.</span></span>
* <span data-ttu-id="61a0e-265">`TryParseHandlerMethod` HTTP 動詞を確認するハンドラーの処理 (`httpMethod`) と名前付きのハンドラーの名前 (`handlerName`) を作成するとき、`PageHandlerModel`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-265">The `TryParseHandlerMethod` processes a handler to determine the HTTP verb (`httpMethod`) and named handler name (`handlerName`) when creating the `PageHandlerModel`.</span></span>
  * <span data-ttu-id="61a0e-266">`Async`存在する場合、後置形式は無視されます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-266">An `Async` postfix is ignored, if present.</span></span>
  * <span data-ttu-id="61a0e-267">大文字と小文字を使用すると、メソッドの名前の HTTP 動詞を解析します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-267">Casing is used to parse the HTTP verb from the method name.</span></span>
  * <span data-ttu-id="61a0e-268">ときに、メソッド名 (せず`Async`) は、HTTP 動詞名と同じ、ハンドラーがない名前付きです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-268">When the method name (without `Async`) is equal to the HTTP verb name, there's no named handler.</span></span> <span data-ttu-id="61a0e-269">`handlerName`に設定されている`null`、メソッド名と`Get`、 `Post`、 `Delete`、 `Put`、または`Patch`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-269">The `handlerName` is set to `null`, and the method name is `Get`, `Post`, `Delete`, `Put`, or `Patch`.</span></span>
  * <span data-ttu-id="61a0e-270">ときに、メソッド名 (せず`Async`) HTTP 動詞名よりも長い名前付きハンドラーがあります。</span><span class="sxs-lookup"><span data-stu-id="61a0e-270">When the method name (without `Async`) is longer than the HTTP verb name, there's a named handler.</span></span> <span data-ttu-id="61a0e-271">`handlerName` に `<method name (less 'Async', if present)>` が設定されています。</span><span class="sxs-lookup"><span data-stu-id="61a0e-271">The `handlerName` is set to `<method name (less 'Async', if present)>`.</span></span> <span data-ttu-id="61a0e-272">たとえば、"GetMessage"と"GetMessageAsync"の両方は、"GetMessage"のハンドラー名を生成します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-272">For example, both "GetMessage" and "GetMessageAsync" yield a handler name of "GetMessage".</span></span>
  * <span data-ttu-id="61a0e-273">削除、PUT、PATCH HTTP 動詞が POST に変換されます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-273">DELETE, PUT, and PATCH HTTP verbs are converted to POST.</span></span>

<span data-ttu-id="61a0e-274">登録、`CustomPageApplicationModelProvider`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="61a0e-274">Register the `CustomPageApplicationModelProvider` in the `Startup` class:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

<span data-ttu-id="61a0e-275">分離コード ファイル*Index.cshtml.cs*アプリ内のページの通常のハンドラー メソッドの名前付け規則を変更する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="61a0e-275">The code-behind file *Index.cshtml.cs* shows how the ordinary handler method naming conventions are changed for pages in the app.</span></span> <span data-ttu-id="61a0e-276">"On"Razor ページで使用される名前が付けられ、通常は削除されます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-276">The ordinary "On" prefix naming used with Razor Pages is removed.</span></span> <span data-ttu-id="61a0e-277">ページの状態を初期化するメソッドの名前は今すぐ`Get`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-277">The method that initializes the page state is now named `Get`.</span></span> <span data-ttu-id="61a0e-278">ページのいずれかの任意の分離コード ファイルを開く場合に、アプリ全体で使用されるこの規則を表示できます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-278">You can see this convention used throughout the app if you open any code-behind file for any of the pages.</span></span>

<span data-ttu-id="61a0e-279">それぞれの他の方法は、その処理を説明する HTTP 動詞を使用して開始します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-279">Each of the other methods start with the HTTP verb that describes its processing.</span></span> <span data-ttu-id="61a0e-280">2 つの方法で始まる`Delete`は通常として扱う、DELETE HTTP 動詞がロジックでは、`TryParseHandlerMethod`両方ハンドラーに対して post 動詞を明示的に設定します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-280">The two methods that start with `Delete` would normally be treated as DELETE HTTP verbs, but the logic in `TryParseHandlerMethod` explicitly sets the verb to POST for both handlers.</span></span>

<span data-ttu-id="61a0e-281">なお`Async`は間で省略可能`DeleteAllMessages`と`DeleteMessageAsync`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-281">Note that `Async` is optional between `DeleteAllMessages` and `DeleteMessageAsync`.</span></span> <span data-ttu-id="61a0e-282">非同期のどちらを使用することもできますが、`Async`か後置; を実行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="61a0e-282">They're both asynchronous methods, but you can choose to use the `Async` postfix or not; we recommend that you do.</span></span> <span data-ttu-id="61a0e-283">`DeleteAllMessages`ここでは、デモンストレーションのための使用しますが、これらのメソッドの名前を付けることをお勧め`DeleteAllMessagesAsync`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-283">`DeleteAllMessages` is used here for demonstration purposes, but we recommend that you name such a method `DeleteAllMessagesAsync`.</span></span> <span data-ttu-id="61a0e-284">処理に影響しませんのサンプルの実装を使用して、`Async`後置非同期メソッドであるファクトを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-284">It doesn't affect the processing of the sample's implementation, but using the `Async` postfix calls out the fact that it's an asynchronous method.</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

<span data-ttu-id="61a0e-285">提供されるハンドラーの名前を控えておきます*Index.cshtml*一致、`DeleteAllMessages`と`DeleteMessageAsync`ハンドラー メソッド。</span><span class="sxs-lookup"><span data-stu-id="61a0e-285">Note the handler names provided in *Index.cshtml* match the `DeleteAllMessages` and `DeleteMessageAsync` handler methods:</span></span>

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

<span data-ttu-id="61a0e-286">`Async`ハンドラー メソッドの名前で`DeleteMessageAsync`でサインアウトが考慮され、`TryParseHandlerMethod`メソッドへの POST 要求の一致するハンドラー。</span><span class="sxs-lookup"><span data-stu-id="61a0e-286">`Async` in the handler method name `DeleteMessageAsync` is factored out by the `TryParseHandlerMethod` for handler matching of POST request to method.</span></span> <span data-ttu-id="61a0e-287">`asp-page-handler`名前`DeleteMessage`ハンドラー メソッドに一致する`DeleteMessageAsync`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-287">The `asp-page-handler` name of `DeleteMessage` is matched to the handler method `DeleteMessageAsync`.</span></span>

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="61a0e-288">MVC のフィルターと、ページ フィルター (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="61a0e-288">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="61a0e-289">MVC[アクション フィルター](xref:mvc/controllers/filters#action-filters) Razor ページ ハンドラーのメソッドを使用するために Razor ページによって無視されます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-289">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="61a0e-290">使用するための他の種類の MVC フィルターの利用できます:[承認](xref:mvc/controllers/filters#authorization-filters)、[例外](xref:mvc/controllers/filters#exception-filters)、[リソース](xref:mvc/controllers/filters#resource-filters)、および[結果](xref:mvc/controllers/filters#result-filters)です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-290">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="61a0e-291">詳細については、次を参照してください。、[フィルター](xref:mvc/controllers/filters)トピックです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-291">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="61a0e-292">ページ フィルター ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) は、Razor ページに適用されるフィルターです。</span><span class="sxs-lookup"><span data-stu-id="61a0e-292">The Page filter ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="61a0e-293">ページのハンドラー メソッドの実行が囲まれます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-293">It surrounds the execution of a page handler method.</span></span> <span data-ttu-id="61a0e-294">ページのハンドラー メソッドの実行の段階でカスタム コードを処理することができます。</span><span class="sxs-lookup"><span data-stu-id="61a0e-294">It allows you to process custom code at stages of page handler method execution.</span></span> <span data-ttu-id="61a0e-295">サンプル アプリからの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="61a0e-295">Here's an example from the sample app:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

<span data-ttu-id="61a0e-296">このフィルターでのチェック、 `globalTemplate` "ReplacementValue"で"TriggerValue"とスワップの値をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="61a0e-296">This filter checks for a `globalTemplate` route value of "TriggerValue" and swaps in "ReplacementValue".</span></span>

<span data-ttu-id="61a0e-297">`ReplaceRouteValueFilter`に直接適用できる属性、`PageModel`分離コード。</span><span class="sxs-lookup"><span data-stu-id="61a0e-297">The `ReplaceRouteValueFilter` attribute can be applied directly to a `PageModel` in code-behind:</span></span>

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

<span data-ttu-id="61a0e-298">要求、Page3 ページでのサンプル アプリから`localhost:5000/OtherPages/Page3/TriggerValue`です。</span><span class="sxs-lookup"><span data-stu-id="61a0e-298">Request the Page3 page from the sample app with at `localhost:5000/OtherPages/Page3/TriggerValue`.</span></span> <span data-ttu-id="61a0e-299">フィルターが、ルート値を置換する方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="61a0e-299">Notice how the filter replaces the route value:</span></span>

![ルートの値を置き換える値に置き換えてフィルターで TriggerValue ルート セグメント結果を含む OtherPages/Page3 を要求します。](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a><span data-ttu-id="61a0e-301">関連項目</span><span class="sxs-lookup"><span data-stu-id="61a0e-301">See also</span></span>

* [<span data-ttu-id="61a0e-302">Razor ページの承認規則</span><span class="sxs-lookup"><span data-stu-id="61a0e-302">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
