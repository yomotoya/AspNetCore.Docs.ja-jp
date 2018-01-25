---
title: "ASP.NET Web API からの移行"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 39224d1b2b0a54b043aa279659ff38134c5af7b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="786ff-102">ASP.NET Web API からの移行</span><span class="sxs-lookup"><span data-stu-id="786ff-102">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="786ff-103">によって[Steve Smith](https://ardalis.com/)と[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="786ff-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="786ff-104">Web Api は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達する HTTP サービスです。</span><span class="sxs-lookup"><span data-stu-id="786ff-104">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="786ff-105">ASP.NET Core MVC には、1 つ、一貫性のある web アプリケーションの構築方法を提供する Web Api を構築するためのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="786ff-105">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="786ff-106">この記事では、ASP.NET Web API から ASP.NET Core MVC に Web API の実装を移行するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="786ff-106">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="786ff-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="786ff-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="786ff-108">レビュー ASP.NET Web API プロジェクト</span><span class="sxs-lookup"><span data-stu-id="786ff-108">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="786ff-109">この記事は、サンプル プロジェクトを使用して*ProductsApp*アーティクルで作成された[ASP.NET Web API の使用を開始する](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)を基盤とします。</span><span class="sxs-lookup"><span data-stu-id="786ff-109">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="786ff-110">そのプロジェクトで単純な ASP.NET Web API プロジェクトが次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="786ff-110">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="786ff-111">*Global.asax.cs*への呼び出しが行われた`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="786ff-111">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="786ff-112">`WebApiConfig`定義された*App_Start*であり、1 つの静的`Register`メソッド。</span><span class="sxs-lookup"><span data-stu-id="786ff-112">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="786ff-113">このクラスを構成[属性がルーティング](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)は実際には、プロジェクトで使用されていますが、します。</span><span class="sxs-lookup"><span data-stu-id="786ff-113">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="786ff-114">また、ASP.NET Web API によって使用される、ルーティング テーブルを構成します。</span><span class="sxs-lookup"><span data-stu-id="786ff-114">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="786ff-115">ASP.NET Web API が形式と一致する Url を期待するこの例では、 */api/{controller}/{id}*で*{id}*される省略可能です。</span><span class="sxs-lookup"><span data-stu-id="786ff-115">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="786ff-116">*ProductsApp*プロジェクトに含まれる 1 つだけ単純なコント ローラーから継承される`ApiController`と 2 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="786ff-116">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="786ff-117">最後に、次のようなモデルがあると、*製品*で使用される、 *ProductsApp*、単純なクラスには。</span><span class="sxs-lookup"><span data-stu-id="786ff-117">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="786ff-118">これでが単純なプロジェクトから起動するときに、ASP.NET Core MVC にこの Web API プロジェクトを移行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="786ff-118">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="786ff-119">コピー先のプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="786ff-119">Create the Destination Project</span></span>

<span data-ttu-id="786ff-120">Visual Studio を使用して、新しい、空のソリューションを作成し、名前を付けます*WebAPIMigration*です。</span><span class="sxs-lookup"><span data-stu-id="786ff-120">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="786ff-121">既存の追加*ProductsApp*プロジェクトを次に、新しい ASP.NET Core Web アプリケーション プロジェクトをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="786ff-121">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="786ff-122">新しいプロジェクトの名前*ProductsCore*です。</span><span class="sxs-lookup"><span data-stu-id="786ff-122">Name the new project *ProductsCore*.</span></span>

![Web テンプレートを新しいプロジェクト ダイアログ ボックスを開く](webapi/_static/add-web-project.png)

<span data-ttu-id="786ff-124">次に、Web API プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="786ff-124">Next, choose the Web API project template.</span></span> <span data-ttu-id="786ff-125">移行される、 *ProductsApp*内容をこの新しいプロジェクトをします。</span><span class="sxs-lookup"><span data-stu-id="786ff-125">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core テンプレートの一覧で選択されている Web API プロジェクト テンプレートを使用して新しい Web アプリケーションのダイアログ](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="786ff-127">削除、`Project_Readme.html`新しいプロジェクトからファイルです。</span><span class="sxs-lookup"><span data-stu-id="786ff-127">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="786ff-128">ソリューションは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="786ff-128">Your solution should now look like this:</span></span>

![ソリューション エクスプ ローラーで、ProductsApp および ProductsCore プロジェクトのファイルとフォルダーを示すアプリケーション ソリューションを開く](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="786ff-130">構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="786ff-130">Migrate Configuration</span></span>

<span data-ttu-id="786ff-131">ASP.NET Core を使用しない*Global.asax*、 *web.config*、または*App_Start*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="786ff-131">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="786ff-132">すべてのスタートアップ タスクを実行する代わりに、 *Startup.cs* 、プロジェクトのルートで (を参照してください[アプリケーションの起動](../fundamentals/startup.md))。</span><span class="sxs-lookup"><span data-stu-id="786ff-132">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="786ff-133">ASP.NET Core mvc で属性ベースのルーティングはここで既定で含まれてとき`UseMvc()`が呼び出されます。 および、この Web API ルートを構成するための推奨アプローチです (とは、Web API のスタート プロジェクトがルーティングを処理する方法)。</span><span class="sxs-lookup"><span data-stu-id="786ff-133">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="786ff-134">属性は、プロジェクトの今後のルーティングを使用すると仮定した場合、追加の構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="786ff-134">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="786ff-135">サンプルで行われるように、コント ローラーとアクションに必要な属性を適用`ValuesController`Web API のスタート プロジェクトに含まれているクラス。</span><span class="sxs-lookup"><span data-stu-id="786ff-135">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="786ff-136">存在することに注意してください*[コント ローラー]* 8 行です。</span><span class="sxs-lookup"><span data-stu-id="786ff-136">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="786ff-137">これで属性ベースのルーティングをサポートしている特定のトークンなど*[コント ローラー]*と*[アクション]*です。</span><span class="sxs-lookup"><span data-stu-id="786ff-137">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="786ff-138">これらのトークンは置き換えられます実行時に、コント ローラーまたはアクションの名前でそれぞれ、属性が適用されています。</span><span class="sxs-lookup"><span data-stu-id="786ff-138">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="786ff-139">これには、プロジェクトのマジック文字列の数を減らすし、ルート保持される自動名前の変更リファクタリングを適用するときに、対応するコント ローラーとアクションで同期されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="786ff-139">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="786ff-140">製品の API コント ローラーを移行する必要があります最初にコピー *ProductsController*は新しいプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="786ff-140">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="786ff-141">コント ローラーのルート属性を単に含めます。</span><span class="sxs-lookup"><span data-stu-id="786ff-141">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="786ff-142">追加する必要があります、`[HttpGet]`両者は、HTTP Get 経由で呼び出す必要がありますので、2 つのメソッドでは、属性します。</span><span class="sxs-lookup"><span data-stu-id="786ff-142">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="786ff-143">属性に"id"パラメーターの期待値を含める`GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="786ff-143">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="786ff-144">この時点では、ルーティングが正しく構成されています。ただし、できませんまだテストすることです。</span><span class="sxs-lookup"><span data-stu-id="786ff-144">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="786ff-145">追加する必要があります変更する前に*ProductsController*コンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="786ff-145">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="786ff-146">モデルとコント ローラーを移行します。</span><span class="sxs-lookup"><span data-stu-id="786ff-146">Migrate Models and Controllers</span></span>

<span data-ttu-id="786ff-147">この単純な Web API プロジェクトの移行プロセスの最後の手順では、コント ローラーおよびすべてのモデルを使用して経由でコピーします。</span><span class="sxs-lookup"><span data-stu-id="786ff-147">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="786ff-148">この場合、単にコピー *Controllers/ProductsController.cs*に元のプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="786ff-148">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="786ff-149">新しいゲートウェイに、元のプロジェクトからモデル フォルダー全体をコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="786ff-149">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="786ff-150">新しいプロジェクト名と一致する名前空間を調整 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="786ff-150">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="786ff-151">この時点で、アプリケーションをビルドすることができ、コンパイル エラーの数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="786ff-151">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="786ff-152">一般に、次のカテゴリに分類これら必要があります。</span><span class="sxs-lookup"><span data-stu-id="786ff-152">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="786ff-153">*ApiController*存在しません</span><span class="sxs-lookup"><span data-stu-id="786ff-153">*ApiController* does not exist</span></span>

* <span data-ttu-id="786ff-154">*System.Web.Http*名前空間が存在しません</span><span class="sxs-lookup"><span data-stu-id="786ff-154">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="786ff-155">*IHttpActionResult*存在しません</span><span class="sxs-lookup"><span data-stu-id="786ff-155">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="786ff-156">幸いにも、これらはすべて非常に簡単に解決します。</span><span class="sxs-lookup"><span data-stu-id="786ff-156">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="786ff-157">変更*ApiController*に*コント ローラー* (追加する必要があります*Microsoft.AspNetCore.Mvc を使用して*)</span><span class="sxs-lookup"><span data-stu-id="786ff-157">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="786ff-158">削除を参照するステートメントを使用して*System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="786ff-158">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="786ff-159">変更するメソッドを返す*IHttpActionResult*を返す、 *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="786ff-159">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="786ff-160">これらの変更が加えられた未使用ステートメントを使用して削除を移行された*ProductsController*クラスは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="786ff-160">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="786ff-161">移行されたプロジェクトを実行しを参照することができます*/api 製品*; し、3 つの製品の完全な一覧を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="786ff-161">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="786ff-162">参照*/api/products/1*と最初の製品を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="786ff-162">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="786ff-163">まとめ</span><span class="sxs-lookup"><span data-stu-id="786ff-163">Summary</span></span>

<span data-ttu-id="786ff-164">ASP.NET Core MVC への単純な ASP.NET Web API プロジェクトの移行は、かなり簡単ありがとう ASP.NET Core MVC の Web Api の組み込みサポートです。</span><span class="sxs-lookup"><span data-stu-id="786ff-164">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="786ff-165">すべての ASP.NET Web API プロジェクトは移行する必要があります、メインの部分では、ルート、コント ローラー、およびモデルでは、コント ローラーとアクションで使用される型への更新プログラムと共にがします。</span><span class="sxs-lookup"><span data-stu-id="786ff-165">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
