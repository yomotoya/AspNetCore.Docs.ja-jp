---
title: ASP.NET Web API から ASP.NET Core への移行します。
author: ardalis
description: ASP.NET Web API から ASP.NET Core mvc Web API の実装を移行する方法を説明します。
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 4f4dc140bd60463037be0757176dcf7a619918bd
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327510"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="d4e55-103">ASP.NET Web API から ASP.NET Core への移行します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="d4e55-104">作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="d4e55-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="d4e55-105">Web Api は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達する HTTP サービスです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="d4e55-106">ASP.NET Core MVC には、1 つ、一貫性のある web アプリケーションの構築方法を提供する Web Api を構築するためのサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4e55-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="d4e55-107">この記事では、ASP.NET Web API から ASP.NET Core MVC に Web API の実装を移行するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="d4e55-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d4e55-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="d4e55-109">レビュー ASP.NET Web API プロジェクト</span><span class="sxs-lookup"><span data-stu-id="d4e55-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="d4e55-110">この記事は、サンプル プロジェクトを使用して*ProductsApp*アーティクルで作成された[ASP.NET Web API 2 の概要](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)を基盤とします。</span><span class="sxs-lookup"><span data-stu-id="d4e55-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="d4e55-111">そのプロジェクトで単純な ASP.NET Web API プロジェクトが次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="d4e55-112">*Global.asax.cs*への呼び出しが行われた`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="d4e55-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="d4e55-113">`WebApiConfig` 定義された*App_Start*であり、1 つの静的`Register`メソッド。</span><span class="sxs-lookup"><span data-stu-id="d4e55-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="d4e55-114">このクラスを構成[属性がルーティング](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)は実際には、プロジェクトで使用されていますが、します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="d4e55-115">また、ASP.NET Web API で使用される、ルーティング テーブルを構成します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="d4e55-116">ASP.NET Web API が形式と一致する Url を期待するこの例では、 */api/{controller}/{id}* で *{id}* される省略可能です。</span><span class="sxs-lookup"><span data-stu-id="d4e55-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="d4e55-117">*ProductsApp*プロジェクトに含まれる 1 つだけ単純なコント ローラーから継承される`ApiController`と 2 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="d4e55-118">最後に、次のようなモデルがあると、*製品*で使用される、 *ProductsApp*、単純なクラスには。</span><span class="sxs-lookup"><span data-stu-id="d4e55-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="d4e55-119">これでが単純なプロジェクトから起動するときに、ASP.NET Core MVC にこの Web API プロジェクトを移行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="d4e55-120">コピー先のプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-120">Create the Destination Project</span></span>

<span data-ttu-id="d4e55-121">Visual Studio を使用して、新しい、空のソリューションを作成し、名前を付けます*WebAPIMigration*です。</span><span class="sxs-lookup"><span data-stu-id="d4e55-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="d4e55-122">既存の追加*ProductsApp*プロジェクトを次に、新しい ASP.NET Core Web アプリケーション プロジェクトをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="d4e55-123">新しいプロジェクトの名前*ProductsCore*です。</span><span class="sxs-lookup"><span data-stu-id="d4e55-123">Name the new project *ProductsCore*.</span></span>

![Web テンプレートを新しいプロジェクト ダイアログ ボックスを開く](webapi/_static/add-web-project.png)

<span data-ttu-id="d4e55-125">次に、Web API プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="d4e55-126">移行される、 *ProductsApp*内容をこの新しいプロジェクトをします。</span><span class="sxs-lookup"><span data-stu-id="d4e55-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core テンプレートの一覧で選択されている Web API プロジェクト テンプレートを使用して新しい Web アプリケーションのダイアログ](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="d4e55-128">削除、`Project_Readme.html`新しいプロジェクトからファイルです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="d4e55-129">ソリューションは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d4e55-129">Your solution should now look like this:</span></span>

![ソリューション エクスプ ローラーで、ProductsApp および ProductsCore プロジェクトのファイルとフォルダーを示すアプリケーション ソリューションを開く](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="d4e55-131">構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-131">Migrate Configuration</span></span>

<span data-ttu-id="d4e55-132">ASP.NET Core を使用しない*Global.asax*、 *web.config*、または*App_Start*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="d4e55-133">すべてのスタートアップ タスクを実行する代わりに、 *Startup.cs* 、プロジェクトのルートで (を参照してください[アプリケーションの起動](../fundamentals/startup.md))。</span><span class="sxs-lookup"><span data-stu-id="d4e55-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="d4e55-134">ASP.NET Core mvc で属性ベースのルーティングはここで既定で含まれてとき`UseMvc()`が呼び出されます。 および、この Web API ルートを構成するための推奨アプローチです (とは、Web API のスタート プロジェクトがルーティングを処理する方法)。</span><span class="sxs-lookup"><span data-stu-id="d4e55-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="d4e55-135">属性は、プロジェクトの今後のルーティングを使用すると仮定した場合、追加の構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d4e55-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="d4e55-136">サンプルで行われるように、コント ローラーとアクションに必要な属性を適用`ValuesController`Web API のスタート プロジェクトに含まれているクラス。</span><span class="sxs-lookup"><span data-stu-id="d4e55-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="d4e55-137">存在することに注意してください *[コント ローラー]* 8 行です。</span><span class="sxs-lookup"><span data-stu-id="d4e55-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="d4e55-138">これで属性ベースのルーティングをサポートしている特定のトークンなど *[コント ローラー]* と *[アクション]* です。</span><span class="sxs-lookup"><span data-stu-id="d4e55-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="d4e55-139">これらのトークンは置き換えられます実行時に、コント ローラーまたはアクションの名前でそれぞれ、属性が適用されています。</span><span class="sxs-lookup"><span data-stu-id="d4e55-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="d4e55-140">これには、プロジェクトのマジック文字列の数を減らすし、ルート保持される自動名前の変更リファクタリングを適用するときに、対応するコント ローラーとアクションで同期されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="d4e55-141">製品の API コント ローラーを移行する必要があります最初にコピー *ProductsController*は新しいプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="d4e55-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="d4e55-142">コント ローラーのルート属性を単に含めます。</span><span class="sxs-lookup"><span data-stu-id="d4e55-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="d4e55-143">追加する必要があります、`[HttpGet]`両者は、HTTP Get 経由で呼び出す必要がありますので、2 つのメソッドでは、属性します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="d4e55-144">属性に"id"パラメーターの期待値を含める`GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="d4e55-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="d4e55-145">この時点では、ルーティングが正しく構成されています。ただし、できませんまだテストすることです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="d4e55-146">追加する必要があります変更する前に*ProductsController*コンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="d4e55-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="d4e55-147">モデルとコント ローラーを移行します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="d4e55-148">この単純な Web API プロジェクトの移行プロセスの最後の手順では、コント ローラーおよびすべてのモデルを使用して経由でコピーします。</span><span class="sxs-lookup"><span data-stu-id="d4e55-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="d4e55-149">この場合、単にコピー *Controllers/ProductsController.cs*に元のプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="d4e55-150">新しいゲートウェイに、元のプロジェクトからモデル フォルダー全体をコピーしてください。</span><span class="sxs-lookup"><span data-stu-id="d4e55-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="d4e55-151">新しいプロジェクト名と一致する名前空間を調整 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="d4e55-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="d4e55-152">この時点で、アプリケーションをビルドすることができ、コンパイル エラーの数が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4e55-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="d4e55-153">一般に、次のカテゴリに分類これら必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4e55-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="d4e55-154">*ApiController*存在しません</span><span class="sxs-lookup"><span data-stu-id="d4e55-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="d4e55-155">*System.Web.Http*名前空間が存在しません</span><span class="sxs-lookup"><span data-stu-id="d4e55-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="d4e55-156">*IHttpActionResult*存在しません</span><span class="sxs-lookup"><span data-stu-id="d4e55-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="d4e55-157">幸いにも、これらはすべて非常に簡単に解決します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="d4e55-158">変更*ApiController*に*コント ローラー* (追加する必要があります*Microsoft.AspNetCore.Mvc を使用して*)</span><span class="sxs-lookup"><span data-stu-id="d4e55-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="d4e55-159">削除を参照するステートメントを使用して*System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="d4e55-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="d4e55-160">変更するメソッドを返す*IHttpActionResult*を返す、 *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="d4e55-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="d4e55-161">これらの変更が加えられた未使用ステートメントを使用して削除を移行された*ProductsController*クラスは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d4e55-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="d4e55-162">移行されたプロジェクトを実行しを参照することができます */api 製品*; し、3 つの製品の完全な一覧を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4e55-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="d4e55-163">参照 */api/products/1*と最初の製品を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4e55-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a><span data-ttu-id="d4e55-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="d4e55-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span></span>

<span data-ttu-id="d4e55-165">移行の ASP.NET Web API プロジェクト ASP.NET Core をときに便利なツールは、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="d4e55-166">互換性 shim は、使用する別の Web API 2 規則の数を許可する ASP.NET Core を拡張します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="d4e55-167">このドキュメントで以前に移植したサンプルは、基本互換性 shim が必要なことです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="d4e55-168">大規模なプロジェクト互換性 shim を使用して役に立ちます一時的にあるギャップを埋める API ASP.NET Core と ASP.NET Web API 2 の間の。</span><span class="sxs-lookup"><span data-stu-id="d4e55-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="d4e55-169">Web API の互換性 shim は、ASP.NET Core に大規模な Web API プロジェクトの移行を容易にするために一時的な措置として使用するものです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="d4e55-170">時間の経過と共にパターンを使用する ASP.NET Core 互換性 shim に依存せずにプロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4e55-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span> 

<span data-ttu-id="d4e55-171">Microsoft.AspNetCore.Mvc.WebApiCompatShim に含まれる互換性機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-171">Compatibility features included in Microsoft.AspNetCore.Mvc.WebApiCompatShim include:</span></span>

* <span data-ttu-id="d4e55-172">追加、`ApiController`コント ローラーの基本データ型を更新する必要があるように入力します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="d4e55-173">Web API スタイル モデル バインディングを有効にします。</span><span class="sxs-lookup"><span data-stu-id="d4e55-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="d4e55-174">既定では、MVC 5 と同様に ASP.NET Core MVC モデル バインディング機能します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="d4e55-175">互換性 shim の変更は、ある Web API 2 モデル バインディング規則に類似したバインドをモデルします。</span><span class="sxs-lookup"><span data-stu-id="d4e55-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="d4e55-176">たとえば、複合型は、要求本文から自動的にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="d4e55-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="d4e55-177">モデル バインディングを拡張できるように、型のパラメーターを受け取り、コント ローラーのアクション`HttpRequestMessage`です。</span><span class="sxs-lookup"><span data-stu-id="d4e55-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="d4e55-178">型の結果を返すための操作の結果メッセージ フォーマッタを追加`HttpResponseMessage`です。</span><span class="sxs-lookup"><span data-stu-id="d4e55-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="d4e55-179">応答を提供する Web API 2 の操作を使用した可能性がある追加の応答方法を追加します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
    * <span data-ttu-id="d4e55-180">HttpResponseMessage ジェネレーター。</span><span class="sxs-lookup"><span data-stu-id="d4e55-180">HttpResponseMessage generators:</span></span>
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * <span data-ttu-id="d4e55-181">結果のアクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="d4e55-181">Action result methods:</span></span>
        * `BadRequestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* <span data-ttu-id="d4e55-182">インスタンスを追加`IContentNegotiator`アプリの DI コンテナーからの型のネゴシエーションに関連するコンテンツと[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)使用可能な。</span><span class="sxs-lookup"><span data-stu-id="d4e55-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="d4e55-183">ような種類が含まれます`DefaultContentNegotiator`、 `MediaTypeFormatter`, などです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="d4e55-184">互換性 shim を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4e55-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="d4e55-185">参照、 [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-185">Reference the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="d4e55-186">呼び出すことによって、アプリの DI コンテナーとの互換性 shim のサービスを登録`services.AddWebApiConventions()`アプリケーションの`Startup.ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-186">Register the compatibility shim's services with the app's DI container by calling `services.AddWebApiConventions()` in the application's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="d4e55-187">使用して Web API に固有のルートを定義`MapWebApiRoute`上、`IRouteBuilder`アプリケーションの`IApplicationBuilder.UseMvc`呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d4e55-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the application's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="d4e55-188">まとめ</span><span class="sxs-lookup"><span data-stu-id="d4e55-188">Summary</span></span>

<span data-ttu-id="d4e55-189">ASP.NET Core MVC への単純な ASP.NET Web API プロジェクトの移行は、かなり簡単ありがとう ASP.NET Core MVC の Web Api の組み込みサポートです。</span><span class="sxs-lookup"><span data-stu-id="d4e55-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="d4e55-190">すべての ASP.NET Web API プロジェクトは移行する必要があります、メインの部分では、ルート、コント ローラー、およびモデルでは、コント ローラーとアクションで使用される型への更新プログラムと共にがします。</span><span class="sxs-lookup"><span data-stu-id="d4e55-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
