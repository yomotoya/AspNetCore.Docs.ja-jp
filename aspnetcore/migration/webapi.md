---
title: ASP.NET Web API から ASP.NET Core に移行します。
author: ardalis
description: ASP.NET 4.x Web API から ASP.NET Core mvc web API の実装を移行する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: f5d886a7c3182b5cd372762ade67c2e748051049
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207278"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="31efd-103">ASP.NET Web API から ASP.NET Core に移行します。</span><span class="sxs-lookup"><span data-stu-id="31efd-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="31efd-104">によって[Scott Addie](https://twitter.com/scott_addie)と[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="31efd-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="31efd-105">ASP.NET 4.x Web API は、クライアント、ブラウザーやモバイル デバイスなどの広範な範囲に到達する HTTP サービスです。</span><span class="sxs-lookup"><span data-stu-id="31efd-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="31efd-106">ASP.NET Core は ASP.NET 4.x の MVC を統一し、Web API アプリを ASP.NET Core MVC と呼ばれる単純なプログラミング モデルにモデル化します。</span><span class="sxs-lookup"><span data-stu-id="31efd-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="31efd-107">この記事では、ASP.NET 4.x Web API から ASP.NET Core MVC への移行に必要な手順を示します。</span><span class="sxs-lookup"><span data-stu-id="31efd-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="31efd-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="31efd-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31efd-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="31efd-109">Prerequisites</span></span>

* <span data-ttu-id="31efd-110">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/all) 以降</span><span class="sxs-lookup"><span data-stu-id="31efd-110">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/download/all)</span></span>
* <span data-ttu-id="31efd-111">**ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7.3 以降</span><span class="sxs-lookup"><span data-stu-id="31efd-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="31efd-112">ASP.NET 4.x Web API プロジェクトを確認してください。</span><span class="sxs-lookup"><span data-stu-id="31efd-112">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="31efd-113">この記事では、開始点として、 *ProductsApp*で作成したプロジェクト[ASP.NET Web API 2 の概要](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)します。</span><span class="sxs-lookup"><span data-stu-id="31efd-113">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="31efd-114">そのプロジェクトでは、単純な ASP.NET 4.x Web API プロジェクトを次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="31efd-114">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="31efd-115">*Global.asax.cs*、呼び出しに`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="31efd-115">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="31efd-116">`WebApiConfig` 定義されている、 *App_Start*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="31efd-116">`WebApiConfig` is defined in the *App_Start* folder.</span></span> <span data-ttu-id="31efd-117">1 つだけの静的`Register`メソッド。</span><span class="sxs-lookup"><span data-stu-id="31efd-117">It has just one static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

<span data-ttu-id="31efd-118">このクラスを構成します[属性ルーティング](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)、実際には、プロジェクトで使用されています。</span><span class="sxs-lookup"><span data-stu-id="31efd-118">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="31efd-119">ASP.NET Web API で使用される、ルーティング テーブルも構成します。</span><span class="sxs-lookup"><span data-stu-id="31efd-119">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="31efd-120">ASP.NET 4.x Web API Url の形式に一致が必要ですがこの場合、`/api/{controller}/{id}`で`{id}`で省略可能します。</span><span class="sxs-lookup"><span data-stu-id="31efd-120">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="31efd-121">*ProductsApp*プロジェクトには、1 つのコント ローラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="31efd-121">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="31efd-122">コント ローラーが継承`ApiController`と 2 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="31efd-122">The controller inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="31efd-123">`Product`によって使用されるモデル`ProductsController`は単純なクラスです。</span><span class="sxs-lookup"><span data-stu-id="31efd-123">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="31efd-124">次のセクションでは、ASP.NET Core mvc Web API プロジェクトの移行について説明します。</span><span class="sxs-lookup"><span data-stu-id="31efd-124">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="31efd-125">コピー先のプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="31efd-125">Create destination project</span></span>

<span data-ttu-id="31efd-126">Visual Studio で、次の手順を完了するには。</span><span class="sxs-lookup"><span data-stu-id="31efd-126">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="31efd-127">移動して**ファイル** > **新しい** > **プロジェクト** > **他のプロジェクト タイプ** > **Visual Studio ソリューション**します。</span><span class="sxs-lookup"><span data-stu-id="31efd-127">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="31efd-128">選択**空のソリューション**、ソリューションの名前と*WebAPIMigration*します。</span><span class="sxs-lookup"><span data-stu-id="31efd-128">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="31efd-129">をクリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="31efd-129">Click the **OK** button.</span></span>
* <span data-ttu-id="31efd-130">既存の追加*ProductsApp*プロジェクトがソリューションにします。</span><span class="sxs-lookup"><span data-stu-id="31efd-130">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="31efd-131">新しい追加**ASP.NET Core Web アプリケーション**プロジェクトがソリューションにします。</span><span class="sxs-lookup"><span data-stu-id="31efd-131">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="31efd-132">選択、 **.NET Core**ドロップダウン リストからフレームワークをターゲットにして、選択、 **API**プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="31efd-132">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="31efd-133">プロジェクトに名前を*ProductsCore*、 をクリックし、 **OK**ボタン。</span><span class="sxs-lookup"><span data-stu-id="31efd-133">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="31efd-134">ソリューションには、2 つのプロジェクトが含まれるようになりました。</span><span class="sxs-lookup"><span data-stu-id="31efd-134">The solution now contains two projects.</span></span> <span data-ttu-id="31efd-135">次のセクションでは、移行について説明します、 *ProductsApp*プロジェクトのコンテンツを*ProductsCore*プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="31efd-135">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="31efd-136">構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="31efd-136">Migrate configuration</span></span>

<span data-ttu-id="31efd-137">ASP.NET Core を使用していない、 *App_Start*フォルダーまたは*Global.asax*ファイル、および*web.config*発行時にファイルが追加します。</span><span class="sxs-lookup"><span data-stu-id="31efd-137">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="31efd-138">*Startup.cs*の置換は、 *Global.asax*プロジェクトのルートにあるとします。</span><span class="sxs-lookup"><span data-stu-id="31efd-138">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="31efd-139">`Startup`クラスはすべてのアプリのスタートアップ タスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="31efd-139">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="31efd-140">詳細については、「 <xref:fundamentals/startup> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="31efd-140">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="31efd-141">ASP.NET Core mvc で属性ルーティングは、既定で含まれてとき<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>で呼び出される`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-141">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="31efd-142">次`UseMvc`置換を呼び出し、 *ProductsApp*プロジェクトの*App_Start/WebApiConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="31efd-142">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="31efd-143">モデルとコント ローラーを移行します。</span><span class="sxs-lookup"><span data-stu-id="31efd-143">Migrate models and controllers</span></span>

<span data-ttu-id="31efd-144">経由でコピー、 *ProductApp*プロジェクトのコント ローラーと、モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="31efd-144">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="31efd-145">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="31efd-145">Follow these steps:</span></span>

1. <span data-ttu-id="31efd-146">コピー *Controllers/ProductsController.cs*から元のプロジェクトに新しいものです。</span><span class="sxs-lookup"><span data-stu-id="31efd-146">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="31efd-147">全体をコピーします*モデル*フォルダーを元のプロジェクトから新しいものにします。</span><span class="sxs-lookup"><span data-stu-id="31efd-147">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="31efd-148">新しいプロジェクトの名前に一致するように、コピーしたファイルの名前空間を変更 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="31efd-148">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="31efd-149">調整、`using ProductsApp.Models;`ステートメント*ProductsController.cs*すぎます。</span><span class="sxs-lookup"><span data-stu-id="31efd-149">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="31efd-150">この時点では、コンパイル エラーの数のアプリの結果を構築します。</span><span class="sxs-lookup"><span data-stu-id="31efd-150">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="31efd-151">ASP.NET Core で、次のコンポーネントが存在しないため、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="31efd-151">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="31efd-152">`ApiController` クラス</span><span class="sxs-lookup"><span data-stu-id="31efd-152">`ApiController` class</span></span>
* <span data-ttu-id="31efd-153">`System.Web.Http` 名前空間</span><span class="sxs-lookup"><span data-stu-id="31efd-153">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="31efd-154">`IHttpActionResult` インターフェイス</span><span class="sxs-lookup"><span data-stu-id="31efd-154">`IHttpActionResult` interface</span></span>

<span data-ttu-id="31efd-155">次のように、エラーを修正します。</span><span class="sxs-lookup"><span data-stu-id="31efd-155">Fix the errors as follows:</span></span>

1. <span data-ttu-id="31efd-156">変更`ApiController`に<xref:Microsoft.AspNetCore.Mvc.ControllerBase>します。</span><span class="sxs-lookup"><span data-stu-id="31efd-156">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="31efd-157">追加`using Microsoft.AspNetCore.Mvc;`解決するのには、`ControllerBase`参照。</span><span class="sxs-lookup"><span data-stu-id="31efd-157">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="31efd-158">`using System.Web.Http;`を削除します。</span><span class="sxs-lookup"><span data-stu-id="31efd-158">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="31efd-159">変更、`GetProduct`アクションの戻り値の型から`IHttpActionResult`に`ActionResult<Product>`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-159">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

## <a name="configure-routing"></a><span data-ttu-id="31efd-160">ルーティングを構成します。</span><span class="sxs-lookup"><span data-stu-id="31efd-160">Configure routing</span></span>

<span data-ttu-id="31efd-161">次のようにルーティングを構成します。</span><span class="sxs-lookup"><span data-stu-id="31efd-161">Configure routing as follows:</span></span>

1. <span data-ttu-id="31efd-162">装飾、`ProductsController`クラスで、次の属性。</span><span class="sxs-lookup"><span data-stu-id="31efd-162">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="31efd-163">上記の[[ルート]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)属性は、コント ローラーの属性のルーティング パターンを構成します。</span><span class="sxs-lookup"><span data-stu-id="31efd-163">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="31efd-164">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)属性は、このコント ローラーで、すべてのアクションの要件をルーティングする属性。</span><span class="sxs-lookup"><span data-stu-id="31efd-164">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="31efd-165">などのトークンをサポートする属性ルーティング`[controller]`と`[action]`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-165">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="31efd-166">時に、各トークンに置き換えられますコント ローラーまたはアクションの名前、属性が適用されています。</span><span class="sxs-lookup"><span data-stu-id="31efd-166">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="31efd-167">トークンは、プロジェクト魔法の文字列の数を減らします。</span><span class="sxs-lookup"><span data-stu-id="31efd-167">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="31efd-168">トークンには、ルートに対応するコント ローラーとの同期を維持し、自動リファクタリングの名前を変更する際のアクションが適用されることも確認してください。</span><span class="sxs-lookup"><span data-stu-id="31efd-168">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="31efd-169">HTTP Get 要求を有効にする、`ProductController`アクション。</span><span class="sxs-lookup"><span data-stu-id="31efd-169">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="31efd-170">適用、 [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)属性を`GetAllProducts`アクション。</span><span class="sxs-lookup"><span data-stu-id="31efd-170">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="31efd-171">適用、`[HttpGet("{id}")]`属性を`GetProduct`アクション。</span><span class="sxs-lookup"><span data-stu-id="31efd-171">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="31efd-172">これらの変更と未使用の削除後`using`ステートメント、 *ProductsController.cs*ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="31efd-172">After these changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="31efd-173">移行されたプロジェクトを実行しを参照`/api/products`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-173">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="31efd-174">3 つの製品の完全な一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="31efd-174">A full list of three products appears.</span></span> <span data-ttu-id="31efd-175">`/api/products/1` を参照します。</span><span class="sxs-lookup"><span data-stu-id="31efd-175">Browse to `/api/products/1`.</span></span> <span data-ttu-id="31efd-176">最初の製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="31efd-176">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="31efd-177">互換性 shim</span><span class="sxs-lookup"><span data-stu-id="31efd-177">Compatibility shim</span></span>

<span data-ttu-id="31efd-178">[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)ライブラリは ASP.NET 4.x Web API プロジェクトを ASP.NET Core に移動する互換性 shim を提供します。</span><span class="sxs-lookup"><span data-stu-id="31efd-178">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="31efd-179">互換性 shim は、多くの ASP.NET 4.x Web API 2 の規則をサポートするために ASP.NET Core を拡張します。</span><span class="sxs-lookup"><span data-stu-id="31efd-179">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="31efd-180">このドキュメントで既に移植サンプルは、基本互換性 shim は必要なかったことです。</span><span class="sxs-lookup"><span data-stu-id="31efd-180">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="31efd-181">大規模なプロジェクトは、互換性 shim を使用してできる一時的に ASP.NET Core と ASP.NET 4.x Web API 2 API のギャップを埋めるに便利です。</span><span class="sxs-lookup"><span data-stu-id="31efd-181">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="31efd-182">Web API の互換性 shim は、移行する大規模な ASP.NET 4.x Web API プロジェクトを ASP.NET Core をサポートするために、一時的な措置として使用します。</span><span class="sxs-lookup"><span data-stu-id="31efd-182">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="31efd-183">時間の経過と共に ASP.NET Core のパターンを使用して、互換性 shim を使用する代わりにプロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="31efd-183">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="31efd-184">含まれる互換性機能`Microsoft.AspNetCore.Mvc.WebApiCompatShim`が含まれます。</span><span class="sxs-lookup"><span data-stu-id="31efd-184">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="31efd-185">追加、`ApiController`コント ローラーの基本型を更新する必要があるように入力します。</span><span class="sxs-lookup"><span data-stu-id="31efd-185">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="31efd-186">Web API スタイルのモデル バインドを有効にします。</span><span class="sxs-lookup"><span data-stu-id="31efd-186">Enables Web API-style model binding.</span></span> <span data-ttu-id="31efd-187">ASP.NET Core MVC のモデル バインドの関数と同様に ASP.NET の 4.x MVC 5 の場合は、既定では。</span><span class="sxs-lookup"><span data-stu-id="31efd-187">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="31efd-188">互換性 shim の変更はモデル バインドを ASP.NET 4.x Web API 2 のモデル バインドの規則に似ています。</span><span class="sxs-lookup"><span data-stu-id="31efd-188">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="31efd-189">たとえば、複合型は、要求本文から自動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="31efd-189">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="31efd-190">コント ローラー アクションは、型のパラメーターを実行できるように、モデル バインドを拡張`HttpRequestMessage`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-190">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="31efd-191">型の結果を返すアクションを許可するメッセージ フォーマッタを追加します。`HttpResponseMessage`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-191">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="31efd-192">Web API 2 の操作が応答を処理するために使用する追加の応答メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="31efd-192">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="31efd-193">`HttpResponseMessage` ジェネレーター。</span><span class="sxs-lookup"><span data-stu-id="31efd-193">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="31efd-194">結果のアクション メソッド:</span><span class="sxs-lookup"><span data-stu-id="31efd-194">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="31efd-195">インスタンスを追加`IContentNegotiator`アプリへの依存関係の挿入 (DI) コンテナーと、コンテンツ ネゴシエーションに関連する型から使用できるように[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)します。</span><span class="sxs-lookup"><span data-stu-id="31efd-195">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="31efd-196">このような種類の例として、`DefaultContentNegotiator`と`MediaTypeFormatter`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-196">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="31efd-197">互換性 shim を使用するには。</span><span class="sxs-lookup"><span data-stu-id="31efd-197">To use the compatibility shim:</span></span>

1. <span data-ttu-id="31efd-198">インストール、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="31efd-198">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="31efd-199">互換性 shim のサービスを呼び出すことによって、アプリの DI コンテナーを登録`services.AddMvc().AddWebApiConventions()`で`Startup.ConfigureServices`します。</span><span class="sxs-lookup"><span data-stu-id="31efd-199">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="31efd-200">Web API に固有のルーティングを使用して定義`MapWebApiRoute`上、`IRouteBuilder`アプリの`IApplicationBuilder.UseMvc`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="31efd-200">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31efd-201">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="31efd-201">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
