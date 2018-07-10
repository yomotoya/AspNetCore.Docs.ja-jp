---
title: ASP.NET Web API から ASP.NET Core に移行します。
author: ardalis
description: ASP.NET Web API から ASP.NET Core mvc Web API の実装を移行する方法について説明します。
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894193"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="1c031-103">ASP.NET Web API から ASP.NET Core に移行します。</span><span class="sxs-lookup"><span data-stu-id="1c031-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="1c031-104">作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="1c031-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="1c031-105">Web Api は、さまざまなブラウザーやモバイル デバイスなどのクライアントに提供される HTTP サービスです。</span><span class="sxs-lookup"><span data-stu-id="1c031-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="1c031-106">ASP.NET Core MVC には、1 つで一貫性のある web アプリケーションの構築方法を提供する Web Api を構築するためのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1c031-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="1c031-107">この記事では、ASP.NET Web API から ASP.NET Core mvc Web API の実装を移行するために必要な手順を示します。</span><span class="sxs-lookup"><span data-stu-id="1c031-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="1c031-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1c031-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="1c031-109">ASP.NET Web API プロジェクトのレビュー</span><span class="sxs-lookup"><span data-stu-id="1c031-109">Review ASP.NET Web API project</span></span>

<span data-ttu-id="1c031-110">この記事では、サンプル プロジェクトを使用して*ProductsApp*、情報の記事で作成された[ASP.NET Web API 2 の概要](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)を基盤とします。</span><span class="sxs-lookup"><span data-stu-id="1c031-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="1c031-111">そのプロジェクトでは、単純な ASP.NET Web API プロジェクトを次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="1c031-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="1c031-112">*Global.asax.cs*、呼び出しに`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="1c031-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="1c031-113">`WebApiConfig` 定義されている*App_Start*、1 つだけの静的であり`Register`メソッド。</span><span class="sxs-lookup"><span data-stu-id="1c031-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

<span data-ttu-id="1c031-114">このクラスを構成します[属性ルーティング](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)、実際には、プロジェクトで使用されています。</span><span class="sxs-lookup"><span data-stu-id="1c031-114">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="1c031-115">ASP.NET Web API で使用される、ルーティング テーブルも構成します。</span><span class="sxs-lookup"><span data-stu-id="1c031-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="1c031-116">この場合、ASP.NET Web API の形式と一致する Url が期待 */api/{controller}/{id}* で *{id}* で省略可能します。</span><span class="sxs-lookup"><span data-stu-id="1c031-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="1c031-117">*ProductsApp*プロジェクトから継承される 1 つだけの単純なコントロールに含まれる`ApiController`と 2 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="1c031-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="1c031-118">最後に、次のようなモデルがあると、*製品*によって使用される、 *ProductsApp*、単純なクラスには。</span><span class="sxs-lookup"><span data-stu-id="1c031-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="1c031-119">あるので、単純なプロジェクトを開始するには、ASP.NET Core MVC にこの Web API プロジェクトを移行する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="1c031-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="1c031-120">対象のプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c031-120">Create the Destination Project</span></span>

<span data-ttu-id="1c031-121">Visual Studio を使用して、新しい空のソリューションを作成し、名前*WebAPIMigration*します。</span><span class="sxs-lookup"><span data-stu-id="1c031-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="1c031-122">既存の追加*ProductsApp*プロジェクトし、新しい ASP.NET Core Web アプリケーション プロジェクトをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="1c031-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="1c031-123">新しいプロジェクトの名前*ProductsCore*します。</span><span class="sxs-lookup"><span data-stu-id="1c031-123">Name the new project *ProductsCore*.</span></span>

![Web テンプレートに新しいプロジェクト ダイアログ ボックスを開く](webapi/_static/add-web-project.png)

<span data-ttu-id="1c031-125">次に、Web API プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="1c031-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="1c031-126">移行は、 *ProductsApp*内容をこの新しいプロジェクトをします。</span><span class="sxs-lookup"><span data-stu-id="1c031-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Web API プロジェクト テンプレートの ASP.NET Core テンプレートの一覧で選択されている新しい Web アプリケーション ダイアログ](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="1c031-128">削除、`Project_Readme.html`新しいプロジェクトからファイル。</span><span class="sxs-lookup"><span data-stu-id="1c031-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="1c031-129">ソリューションは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1c031-129">Your solution should now look like this:</span></span>

![ProductsApp と ProductsCore プロジェクトのファイルとフォルダーを表示したソリューション エクスプ ローラーでアプリケーション ソリューションを開く](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="1c031-131">構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="1c031-131">Migrate Configuration</span></span>

<span data-ttu-id="1c031-132">ASP.NET Core を使用しなく*Global.asax*、 *web.config*、または*App_Start*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="1c031-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="1c031-133">すべてのスタートアップ タスクを実行する代わりに、 *Startup.cs*プロジェクトのルート (を参照してください[アプリケーションの起動](../fundamentals/startup.md))。</span><span class="sxs-lookup"><span data-stu-id="1c031-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="1c031-134">ASP.NET Core mvc で属性ベースのルーティングに組み込まれた既定と`UseMvc()`が呼び出されます。 および、この Web API ルートを構成するための推奨アプローチです (とは、Web API のスターター プロジェクトがルーティングを処理する方法)。</span><span class="sxs-lookup"><span data-stu-id="1c031-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="1c031-135">今後、プロジェクト内の属性ルーティングを使用すると仮定すると、追加の構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="1c031-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="1c031-136">サンプルで行われるように、コント ローラーとアクション、必要な属性を適用`ValuesController`Web API のスタート プロジェクトに含まれているクラス。</span><span class="sxs-lookup"><span data-stu-id="1c031-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="1c031-137">存在に注意してください *[controller]* 8 行目にします。</span><span class="sxs-lookup"><span data-stu-id="1c031-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="1c031-138">など特定のトークンをサポートするようになりました属性ベースのルーティング *[controller]* と *[アクション]* します。</span><span class="sxs-lookup"><span data-stu-id="1c031-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="1c031-139">これらのトークンに置き換えられます実行時に、コント ローラーまたはアクションの名前、属性が適用されています。</span><span class="sxs-lookup"><span data-stu-id="1c031-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="1c031-140">これには、プロジェクト魔法の文字列の数を減らすし、ルートと同期されている、対応するコント ローラーとアクション自動名前変更リファクタリングを適用した場合になります。</span><span class="sxs-lookup"><span data-stu-id="1c031-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="1c031-141">製品の API コント ローラーを移行する必要がありますまずコピー *ProductsController*新しいプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="1c031-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="1c031-142">コント ローラーのルート属性を含めるだけです。</span><span class="sxs-lookup"><span data-stu-id="1c031-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="1c031-143">追加する必要があります、`[HttpGet]`両方は、HTTP Get を使用して呼び出す必要があるために、2 つのメソッドに属性します。</span><span class="sxs-lookup"><span data-stu-id="1c031-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="1c031-144">属性に"id"パラメーターの想定を含める`GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="1c031-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="1c031-145">この時点では、ルーティングを正しく構成します。ただし、私たちできませんまだテストします。</span><span class="sxs-lookup"><span data-stu-id="1c031-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="1c031-146">前に、追加の変更を行う必要があります*ProductsController*コンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="1c031-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="1c031-147">モデルとコント ローラーを移行します。</span><span class="sxs-lookup"><span data-stu-id="1c031-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="1c031-148">この単純な Web API プロジェクトの移行プロセスの最後の手順では、コント ローラーとすべてのモデルを使用して、それらをコピーします。</span><span class="sxs-lookup"><span data-stu-id="1c031-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="1c031-149">この場合は、単にコピー *Controllers/ProductsController.cs*から元のプロジェクトに新しいものです。</span><span class="sxs-lookup"><span data-stu-id="1c031-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="1c031-150">新しいものを元のプロジェクトからモデル フォルダー全体をコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="1c031-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="1c031-151">新しいプロジェクト名と一致する名前空間を調整 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="1c031-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="1c031-152">この時点で、アプリケーションをビルドして、コンパイル エラーの数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1c031-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="1c031-153">これらは一般に、次のカテゴリに分類されます。</span><span class="sxs-lookup"><span data-stu-id="1c031-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="1c031-154">*ApiController*が存在しません。</span><span class="sxs-lookup"><span data-stu-id="1c031-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="1c031-155">*System.Web.Http*名前空間が存在しません</span><span class="sxs-lookup"><span data-stu-id="1c031-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="1c031-156">*IHttpActionResult*が存在しません。</span><span class="sxs-lookup"><span data-stu-id="1c031-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="1c031-157">さいわい、これらはすべて非常に簡単に修正です。</span><span class="sxs-lookup"><span data-stu-id="1c031-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="1c031-158">変更*ApiController*に*コント ローラー* (追加する必要があります*Microsoft.AspNetCore.Mvc を使用して*)</span><span class="sxs-lookup"><span data-stu-id="1c031-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="1c031-159">削除のいずれかを参照するステートメントを使用して*System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="1c031-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="1c031-160">変更するメソッドを返す*IHttpActionResult*を返す、 *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="1c031-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="1c031-161">これらの変更が加えられた未使用とステートメントを使用して削除されると、移行された*ProductsController*クラスは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1c031-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="1c031-162">移行されたプロジェクトを実行しを参照できるはず*api/製品*; と、3 つの製品の一覧を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c031-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="1c031-163">参照する */api/products/1*と最初の製品を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c031-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a><span data-ttu-id="1c031-164">ASP.NET 4.x Web API 2 の互換性 shim</span><span class="sxs-lookup"><span data-stu-id="1c031-164">ASP.NET 4.x Web API 2 compatibility shim</span></span>

<span data-ttu-id="1c031-165">ASP.NET core プロジェクトを ASP.NET Web API を移行するときに便利なツールは、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="1c031-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="1c031-166">互換性 shim は、ASP.NET Core を使用する Web API 2 とは異なる規則の数を指定できるを拡張します。</span><span class="sxs-lookup"><span data-stu-id="1c031-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="1c031-167">このドキュメントで既に移植サンプルは、基本互換性 shim は必要なかったことです。</span><span class="sxs-lookup"><span data-stu-id="1c031-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="1c031-168">大規模なプロジェクトは、互換性 shim を使用してできる一時的に ASP.NET Core と ASP.NET Web API 2 API のギャップを埋めるに便利です。</span><span class="sxs-lookup"><span data-stu-id="1c031-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="1c031-169">Web API の互換性 shim は、ASP.NET Core への大規模な Web API プロジェクトの移行を容易に一時的な措置として使用します。</span><span class="sxs-lookup"><span data-stu-id="1c031-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="1c031-170">時間の経過と共に ASP.NET Core のパターンを使用して、互換性 shim を使用する代わりにプロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c031-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="1c031-171">含まれる互換性機能`Microsoft.AspNetCore.Mvc.WebApiCompatShim`が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1c031-171">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="1c031-172">追加、`ApiController`コント ローラーの基本型を更新する必要があるように入力します。</span><span class="sxs-lookup"><span data-stu-id="1c031-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="1c031-173">Web API スタイルのモデル バインドを有効にします。</span><span class="sxs-lookup"><span data-stu-id="1c031-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="1c031-174">既定では、MVC 5 と同様に ASP.NET Core MVC モデル バインディング機能します。</span><span class="sxs-lookup"><span data-stu-id="1c031-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="1c031-175">互換性 shim の変更はモデル バインドを Web API 2 のモデル バインドの規則に似ています。</span><span class="sxs-lookup"><span data-stu-id="1c031-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="1c031-176">たとえば、複合型は、要求本文から自動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="1c031-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="1c031-177">コント ローラー アクションは、型のパラメーターを実行できるように、モデル バインドを拡張`HttpRequestMessage`します。</span><span class="sxs-lookup"><span data-stu-id="1c031-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="1c031-178">型の結果を返すアクションを許可するメッセージ フォーマッタを追加します。`HttpResponseMessage`します。</span><span class="sxs-lookup"><span data-stu-id="1c031-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="1c031-179">Web API 2 の操作が応答を処理するために使用する追加の応答メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="1c031-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="1c031-180">HttpResponseMessage ジェネレーター。</span><span class="sxs-lookup"><span data-stu-id="1c031-180">HttpResponseMessage generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="1c031-181">結果のアクション メソッド:</span><span class="sxs-lookup"><span data-stu-id="1c031-181">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="1c031-182">インスタンスを追加します`IContentNegotiator`アプリの DI コンテナーとコンテンツのネゴシエーションに関連する型から[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c031-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="1c031-183">ような種類が含まれます`DefaultContentNegotiator`、`MediaTypeFormatter`など。</span><span class="sxs-lookup"><span data-stu-id="1c031-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="1c031-184">互換性 shim を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c031-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="1c031-185">インストール、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="1c031-185">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="1c031-186">互換性 shim のサービスを呼び出すことによって、アプリの DI コンテナーを登録`services.AddMvc().AddWebApiConventions()`アプリの`Startup.ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="1c031-186">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="1c031-187">使用して Web API に固有のルートを定義`MapWebApiRoute`上、`IRouteBuilder`アプリの`IApplicationBuilder.UseMvc`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1c031-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="1c031-188">まとめ</span><span class="sxs-lookup"><span data-stu-id="1c031-188">Summary</span></span>

<span data-ttu-id="1c031-189">ASP.NET Core MVC への単純な ASP.NET Web API プロジェクトの移行はとても簡単です、ありがとう Web Api ASP.NET Core MVC での組み込みサポートをします。</span><span class="sxs-lookup"><span data-stu-id="1c031-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="1c031-190">移行する必要がありますすべての ASP.NET Web API プロジェクトの主要部分は、ルート、コント ローラー、およびモデルでは、コント ローラーとアクションで使用される型の更新とは。</span><span class="sxs-lookup"><span data-stu-id="1c031-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
