---
title: "ASP.NET Web API からの移行"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4f0564b4-ed4e-4e1e-9755-c1144d21a0ef
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 2dd2d40aef3803ad2f75504920a1174fee5c2444
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="38d58-103">ASP.NET Web API からの移行</span><span class="sxs-lookup"><span data-stu-id="38d58-103">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="38d58-104">によって[Steve Smith](https://ardalis.com/)と[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="38d58-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="38d58-105">Web Api は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達する HTTP サービスです。</span><span class="sxs-lookup"><span data-stu-id="38d58-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="38d58-106">ASP.NET Core MVC には、1 つ、一貫性のある web アプリケーションの構築方法を提供する Web Api を構築するためのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="38d58-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="38d58-107">この記事では、ASP.NET Web API から ASP.NET Core MVC に Web API の実装を移行するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="38d58-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

[<span data-ttu-id="38d58-108">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="38d58-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="38d58-109">レビュー ASP.NET Web API プロジェクト</span><span class="sxs-lookup"><span data-stu-id="38d58-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="38d58-110">この記事は、サンプル プロジェクトを使用して*ProductsApp*アーティクルで作成された[ASP.NET Web API の使用を開始する](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)を基盤とします。</span><span class="sxs-lookup"><span data-stu-id="38d58-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="38d58-111">そのプロジェクトで単純な ASP.NET Web API プロジェクトが次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="38d58-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="38d58-112">*Global.asax.cs*への呼び出しが行われた`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="38d58-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

<span data-ttu-id="38d58-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="38d58-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span></span>

<span data-ttu-id="38d58-114">`WebApiConfig`定義された*App_Start*であり、1 つの静的`Register`メソッド。</span><span class="sxs-lookup"><span data-stu-id="38d58-114">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

<span data-ttu-id="38d58-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span><span class="sxs-lookup"><span data-stu-id="38d58-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span></span>


<span data-ttu-id="38d58-116">このクラスを構成[属性がルーティング](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)は実際には、プロジェクトで使用されていますが、します。</span><span class="sxs-lookup"><span data-stu-id="38d58-116">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="38d58-117">また、ASP.NET Web API によって使用される、ルーティング テーブルを構成します。</span><span class="sxs-lookup"><span data-stu-id="38d58-117">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="38d58-118">ASP.NET Web API が形式と一致する Url を期待するこの例では、 */api/{controller}/{id}*で*{id}*される省略可能です。</span><span class="sxs-lookup"><span data-stu-id="38d58-118">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="38d58-119">*ProductsApp*プロジェクトに含まれる 1 つだけ単純なコント ローラーから継承される`ApiController`と 2 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="38d58-119">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

<span data-ttu-id="38d58-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span><span class="sxs-lookup"><span data-stu-id="38d58-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span></span>

<span data-ttu-id="38d58-121">最後に、次のようなモデルがあると、*製品*で使用される、 *ProductsApp*、単純なクラスには。</span><span class="sxs-lookup"><span data-stu-id="38d58-121">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

<span data-ttu-id="38d58-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span><span class="sxs-lookup"><span data-stu-id="38d58-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span></span>

<span data-ttu-id="38d58-123">これでが単純なプロジェクトから起動するときに、ASP.NET Core MVC にこの Web API プロジェクトを移行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="38d58-123">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="38d58-124">コピー先のプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="38d58-124">Create the Destination Project</span></span>

<span data-ttu-id="38d58-125">Visual Studio を使用して、新しい、空のソリューションを作成し、名前を付けます*WebAPIMigration*です。</span><span class="sxs-lookup"><span data-stu-id="38d58-125">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="38d58-126">既存の追加*ProductsApp*プロジェクトを次に、新しい ASP.NET Core Web アプリケーション プロジェクトをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="38d58-126">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="38d58-127">新しいプロジェクトの名前*ProductsCore*です。</span><span class="sxs-lookup"><span data-stu-id="38d58-127">Name the new project *ProductsCore*.</span></span>

![Web テンプレートを新しいプロジェクト ダイアログ ボックスを開く](webapi/_static/add-web-project.png)

<span data-ttu-id="38d58-129">次に、Web API プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="38d58-129">Next, choose the Web API project template.</span></span> <span data-ttu-id="38d58-130">移行される、 *ProductsApp*内容をこの新しいプロジェクトをします。</span><span class="sxs-lookup"><span data-stu-id="38d58-130">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core テンプレートの一覧で選択されている Web API プロジェクト テンプレートを使用して新しい Web アプリケーションのダイアログ](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="38d58-132">削除、`Project_Readme.html`新しいプロジェクトからファイルです。</span><span class="sxs-lookup"><span data-stu-id="38d58-132">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="38d58-133">ソリューションは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="38d58-133">Your solution should now look like this:</span></span>

![ソリューション エクスプ ローラーで、ProductsApp および ProductsCore プロジェクトのファイルとフォルダーを示すアプリケーション ソリューションを開く](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="38d58-135">構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="38d58-135">Migrate Configuration</span></span>

<span data-ttu-id="38d58-136">ASP.NET Core を使用しない*Global.asax*、 *web.config*、または*App_Start*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="38d58-136">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="38d58-137">すべてのスタートアップ タスクを実行する代わりに、 *Startup.cs* 、プロジェクトのルートで (を参照してください[アプリケーションの起動](../fundamentals/startup.md))。</span><span class="sxs-lookup"><span data-stu-id="38d58-137">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="38d58-138">ASP.NET Core mvc で属性ベースのルーティングはここで既定で含まれてとき`UseMvc()`が呼び出されます。 および、この Web API ルートを構成するための推奨アプローチです (とは、Web API のスタート プロジェクトがルーティングを処理する方法)。</span><span class="sxs-lookup"><span data-stu-id="38d58-138">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

<span data-ttu-id="38d58-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span><span class="sxs-lookup"><span data-stu-id="38d58-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span></span>

<span data-ttu-id="38d58-140">属性は、プロジェクトの今後のルーティングを使用すると仮定した場合、追加の構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="38d58-140">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="38d58-141">サンプルで行われるように、コント ローラーとアクションに必要な属性を適用`ValuesController`Web API のスタート プロジェクトに含まれているクラス。</span><span class="sxs-lookup"><span data-stu-id="38d58-141">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

<span data-ttu-id="38d58-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span><span class="sxs-lookup"><span data-stu-id="38d58-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span></span>

<span data-ttu-id="38d58-143">存在することに注意してください*[コント ローラー]* 8 行です。</span><span class="sxs-lookup"><span data-stu-id="38d58-143">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="38d58-144">これで属性ベースのルーティングをサポートしている特定のトークンなど*[コント ローラー]*と*[アクション]*です。</span><span class="sxs-lookup"><span data-stu-id="38d58-144">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="38d58-145">これらのトークンは置き換えられます実行時に、コント ローラーまたはアクションの名前でそれぞれ、属性が適用されています。</span><span class="sxs-lookup"><span data-stu-id="38d58-145">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="38d58-146">これには、プロジェクトのマジック文字列の数を減らすし、ルート保持される自動名前の変更リファクタリングを適用するときに、対応するコント ローラーとアクションで同期されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="38d58-146">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="38d58-147">製品の API コント ローラーを移行する必要があります最初にコピー *ProductsController*は新しいプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="38d58-147">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="38d58-148">コント ローラーのルート属性を単に含めます。</span><span class="sxs-lookup"><span data-stu-id="38d58-148">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="38d58-149">追加する必要があります、`[HttpGet]`両者は、HTTP Get 経由で呼び出す必要がありますので、2 つのメソッドでは、属性します。</span><span class="sxs-lookup"><span data-stu-id="38d58-149">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="38d58-150">属性に"id"パラメーターの期待値を含める`GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="38d58-150">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="38d58-151">この時点では、ルーティングが正しく構成されています。ただし、できませんまだテストすることです。</span><span class="sxs-lookup"><span data-stu-id="38d58-151">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="38d58-152">追加する必要があります変更する前に*ProductsController*コンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="38d58-152">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="38d58-153">モデルとコント ローラーを移行します。</span><span class="sxs-lookup"><span data-stu-id="38d58-153">Migrate Models and Controllers</span></span>

<span data-ttu-id="38d58-154">この単純な Web API プロジェクトの移行プロセスの最後の手順では、コント ローラーおよびすべてのモデルを使用して経由でコピーします。</span><span class="sxs-lookup"><span data-stu-id="38d58-154">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="38d58-155">この場合、単にコピー *Controllers/ProductsController.cs*に元のプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="38d58-155">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="38d58-156">新しいゲートウェイに、元のプロジェクトからモデル フォルダー全体をコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="38d58-156">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="38d58-157">新しいプロジェクト名と一致する名前空間を調整 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="38d58-157">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="38d58-158">この時点で、アプリケーションをビルドすることができ、コンパイル エラーの数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="38d58-158">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="38d58-159">一般に、次のカテゴリに分類これら必要があります。</span><span class="sxs-lookup"><span data-stu-id="38d58-159">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="38d58-160">*ApiController*存在しません</span><span class="sxs-lookup"><span data-stu-id="38d58-160">*ApiController* does not exist</span></span>

* <span data-ttu-id="38d58-161">*System.Web.Http*名前空間が存在しません</span><span class="sxs-lookup"><span data-stu-id="38d58-161">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="38d58-162">*IHttpActionResult*存在しません</span><span class="sxs-lookup"><span data-stu-id="38d58-162">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="38d58-163">幸いにも、これらはすべて非常に簡単に解決します。</span><span class="sxs-lookup"><span data-stu-id="38d58-163">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="38d58-164">変更*ApiController*に*コント ローラー* (追加する必要があります*Microsoft.AspNetCore.Mvc を使用して*)</span><span class="sxs-lookup"><span data-stu-id="38d58-164">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="38d58-165">削除を参照するステートメントを使用して*System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="38d58-165">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="38d58-166">変更するメソッドを返す*IHttpActionResult*を返す、 *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="38d58-166">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="38d58-167">これらの変更が加えられた未使用ステートメントを使用して削除を移行された*ProductsController*クラスは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="38d58-167">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

<span data-ttu-id="38d58-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span><span class="sxs-lookup"><span data-stu-id="38d58-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span></span>

<span data-ttu-id="38d58-169">移行されたプロジェクトを実行しを参照することができます*/api 製品*; し、3 つの製品の完全な一覧を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38d58-169">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="38d58-170">参照*/api/products/1*と最初の製品を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="38d58-170">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="38d58-171">概要</span><span class="sxs-lookup"><span data-stu-id="38d58-171">Summary</span></span>

<span data-ttu-id="38d58-172">ASP.NET Core MVC への単純な ASP.NET Web API プロジェクトの移行は、かなり簡単ありがとう ASP.NET Core MVC の Web Api の組み込みサポートです。</span><span class="sxs-lookup"><span data-stu-id="38d58-172">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="38d58-173">すべての ASP.NET Web API プロジェクトは移行する必要があります、メインの部分では、ルート、コント ローラー、およびモデルでは、コント ローラーとアクションで使用される型への更新プログラムと共にがします。</span><span class="sxs-lookup"><span data-stu-id="38d58-173">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
