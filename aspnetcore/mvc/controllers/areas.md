---
title: "区分"
author: rick-anderson
description: "領域を使用する方法を示します。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 87bf2eaad1c13d21412051be769992411f685e2e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="areas"></a><span data-ttu-id="f8578-103">区分</span><span class="sxs-lookup"><span data-stu-id="f8578-103">Areas</span></span>

<span data-ttu-id="f8578-104">によって[Dhananjay Kumar](https://twitter.com/debug_mode)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8578-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f8578-105">領域は、別の名前空間 (ルーティング) と (views) 用のフォルダー構造をグループに関連する機能を整理するため、ASP.NET MVC 機能です。</span><span class="sxs-lookup"><span data-stu-id="f8578-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="f8578-106">別のルート パラメーターを追加することでルーティングするために階層を作成する領域を使用して`area`を`controller`と`action`です。</span><span class="sxs-lookup"><span data-stu-id="f8578-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="f8578-107">領域は、大規模な ASP.NET Core MVC Web アプリケーションを小さい機能グループに分割する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f8578-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="f8578-108">領域は、事実上、アプリケーション内部 MVC 構造です。</span><span class="sxs-lookup"><span data-stu-id="f8578-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="f8578-109">MVC プロジェクトでは、モデル、コント ローラー、およびビューなどの論理コンポーネントが、他のフォルダーに保持され、MVC では、名前付け規則を使用して、これらのコンポーネント間のリレーションシップを作成します。</span><span class="sxs-lookup"><span data-stu-id="f8578-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="f8578-110">大規模なアプリの機能の個別の高レベル領域に、アプリを分割すると便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="f8578-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="f8578-111">たとえば、電子商取引を使用したアプリなど、チェック アウト、請求、および検索などの複数の事業単位です。各事業ユニットは、独自の論理コンポーネント ビュー、コント ローラー、およびモデルがあります。</span><span class="sxs-lookup"><span data-stu-id="f8578-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="f8578-112">このシナリオでは、同じプロジェクトでは、ビジネス コンポーネントの物理的にパーティション分割に領域を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f8578-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="f8578-113">領域は、コント ローラー、ビュー、およびモデルの独自セットと ASP.NET Core MVC プロジェクトでより小さな機能ユニットとして定義することができます。</span><span class="sxs-lookup"><span data-stu-id="f8578-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="f8578-114">MVC の領域の使用を検討時にプロジェクトします。</span><span class="sxs-lookup"><span data-stu-id="f8578-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="f8578-115">論理的に区切る必要があります複数の高度な機能コンポーネントのアプリケーションが行われます</span><span class="sxs-lookup"><span data-stu-id="f8578-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="f8578-116">各機能領域に作業できるいない個別にできるように、MVC プロジェクトのパーティション分割します。</span><span class="sxs-lookup"><span data-stu-id="f8578-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="f8578-117">領域の機能:</span><span class="sxs-lookup"><span data-stu-id="f8578-117">Area features:</span></span>

* <span data-ttu-id="f8578-118">ASP.NET Core MVC アプリの領域の任意の数を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="f8578-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="f8578-119">各領域には、独自のコント ローラー、モデル、およびビュー</span><span class="sxs-lookup"><span data-stu-id="f8578-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="f8578-120">作業できるいない個別に複数の高度なコンポーネントに大規模な MVC プロジェクトを整理することができます。</span><span class="sxs-lookup"><span data-stu-id="f8578-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="f8578-121">異なるを持っていれば、同じ名前の複数のコント ローラーをサポートしている*領域*</span><span class="sxs-lookup"><span data-stu-id="f8578-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="f8578-122">領域の作成方法および使用方法を説明する例を見てをみましょう。</span><span class="sxs-lookup"><span data-stu-id="f8578-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="f8578-123">コント ローラーとビューの 2 つの異なるグループのあるストア アプリがあるとしましょう。 製品およびサービス。</span><span class="sxs-lookup"><span data-stu-id="f8578-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="f8578-124">一般的なフォルダーは、ある MVC 領域を使用して次のように下の構造化します。</span><span class="sxs-lookup"><span data-stu-id="f8578-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="f8578-125">プロジェクト名</span><span class="sxs-lookup"><span data-stu-id="f8578-125">Project name</span></span>

  * <span data-ttu-id="f8578-126">区分</span><span class="sxs-lookup"><span data-stu-id="f8578-126">Areas</span></span>

    * <span data-ttu-id="f8578-127">製品</span><span class="sxs-lookup"><span data-stu-id="f8578-127">Products</span></span>

      * <span data-ttu-id="f8578-128">コント ローラー</span><span class="sxs-lookup"><span data-stu-id="f8578-128">Controllers</span></span>

        * <span data-ttu-id="f8578-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f8578-129">HomeController.cs</span></span>

        * <span data-ttu-id="f8578-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="f8578-130">ManageController.cs</span></span>

      * <span data-ttu-id="f8578-131">ビュー</span><span class="sxs-lookup"><span data-stu-id="f8578-131">Views</span></span>

        * <span data-ttu-id="f8578-132">Home</span><span class="sxs-lookup"><span data-stu-id="f8578-132">Home</span></span>

          * <span data-ttu-id="f8578-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f8578-133">Index.cshtml</span></span>

        * <span data-ttu-id="f8578-134">管理</span><span class="sxs-lookup"><span data-stu-id="f8578-134">Manage</span></span>

          * <span data-ttu-id="f8578-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f8578-135">Index.cshtml</span></span>

    * <span data-ttu-id="f8578-136">サービス</span><span class="sxs-lookup"><span data-stu-id="f8578-136">Services</span></span>

      * <span data-ttu-id="f8578-137">コント ローラー</span><span class="sxs-lookup"><span data-stu-id="f8578-137">Controllers</span></span>

        * <span data-ttu-id="f8578-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="f8578-138">HomeController.cs</span></span>

      * <span data-ttu-id="f8578-139">ビュー</span><span class="sxs-lookup"><span data-stu-id="f8578-139">Views</span></span>

        * <span data-ttu-id="f8578-140">Home</span><span class="sxs-lookup"><span data-stu-id="f8578-140">Home</span></span>

          * <span data-ttu-id="f8578-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="f8578-141">Index.cshtml</span></span>

<span data-ttu-id="f8578-142">MVC は、既定では、領域内のビューをレンダリングするときは、次の場所で検索を試みます。</span><span class="sxs-lookup"><span data-stu-id="f8578-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="f8578-143">これらは、既定の場所を使用して変更することができます、`AreaViewLocationFormats`上、`Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`です。</span><span class="sxs-lookup"><span data-stu-id="f8578-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="f8578-144">たとえばで、以下のコード '領域' として、フォルダー名ではなく、変更されている '分類' をします。</span><span class="sxs-lookup"><span data-stu-id="f8578-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="f8578-145">1 つに注目することですの構造、*ビュー*フォルダーと考えられる重要ここで 1 つのみでありなどの他のフォルダーのコンテンツ*コント ローラー*と*モデル*は**いない**は重要です。</span><span class="sxs-lookup"><span data-stu-id="f8578-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="f8578-146">たとえば、する必要はありません、*コント ローラー*と*モデル*まったくフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="f8578-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="f8578-147">これは機能するためのコンテンツ*コント ローラー*と*モデル*のコンテンツの場所としてコンパイルして .dll に取得される単なるコード、*ビュー*までに要求がビューが加えられました。</span><span class="sxs-lookup"><span data-stu-id="f8578-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="f8578-148">フォルダー階層を定義したら、各コント ローラーが領域に関連付けられている MVC に指示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f8578-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="f8578-149">コント ローラー名を修飾すれば、`[Area]`属性。</span><span class="sxs-lookup"><span data-stu-id="f8578-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="f8578-150">新しく作成された分野で動作するルート定義を設定します。</span><span class="sxs-lookup"><span data-stu-id="f8578-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="f8578-151">[コント ローラー アクションへのルーティング](routing.md)属性ルートと従来のルートを使用するなどの route 定義を作成する方法について詳細にアーティクルが入ります。</span><span class="sxs-lookup"><span data-stu-id="f8578-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="f8578-152">この例では、従来のルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="f8578-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="f8578-153">開く、 *Startup.cs*ファイルし、追加することによって変更、`areaRoute`下記の route 定義をという名前です。</span><span class="sxs-lookup"><span data-stu-id="f8578-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="f8578-154">参照して`http://<yourApp>/products`、`Index`のアクション メソッド、`HomeController`で、`Products`領域が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f8578-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="f8578-155">リンクの生成</span><span class="sxs-lookup"><span data-stu-id="f8578-155">Link Generation</span></span>

* <span data-ttu-id="f8578-156">領域内でのアクションからのリンクを生成すると、同じコント ローラー内の別のアクションのコント ローラーが基づいています。</span><span class="sxs-lookup"><span data-stu-id="f8578-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="f8578-157">たとえば、現在の要求のパスはのように、`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="f8578-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f8578-158">HtmlHelper 構文:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="f8578-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="f8578-159">TagHelper 構文:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f8578-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="f8578-160">'区分' と 'controller' の値を指定しない必要があるおことに注意してください既に現在の要求のコンテキストで可能になるとは、ここです。</span><span class="sxs-lookup"><span data-stu-id="f8578-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="f8578-161">このような値と呼ばれる`ambient`値。</span><span class="sxs-lookup"><span data-stu-id="f8578-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="f8578-162">別のコント ローラー上の別のアクションのコント ローラーに基づく領域内でのアクションからのリンクを生成します。</span><span class="sxs-lookup"><span data-stu-id="f8578-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="f8578-163">たとえば、現在の要求のパスはのように、`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="f8578-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f8578-164">HtmlHelper 構文:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="f8578-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="f8578-165">TagHelper 構文:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f8578-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="f8578-166">アンビエント '領域' の値を使用するここでは、上で 'controller' 値が明示的に指定したことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f8578-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="f8578-167">領域内でのアクションからのリンクを生成するコント ローラーは別のアクションを別のコント ローラーとに基づいて別の領域。</span><span class="sxs-lookup"><span data-stu-id="f8578-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="f8578-168">たとえば、現在の要求のパスはのように、`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="f8578-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="f8578-169">HtmlHelper 構文:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="f8578-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="f8578-170">TagHelper 構文:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f8578-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="f8578-171">なおここアンビエント値は使用されません。</span><span class="sxs-lookup"><span data-stu-id="f8578-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="f8578-172">領域のコント ローラー内のアクションから別のコント ローラー上の別のアクション リンクを生成して**いない**領域。</span><span class="sxs-lookup"><span data-stu-id="f8578-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="f8578-173">HtmlHelper 構文:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="f8578-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="f8578-174">TagHelper 構文:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="f8578-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="f8578-175">生成するため非領域へのリンク ベースのコント ローラー アクション、空のお '領域' ここでのアンビエント値。</span><span class="sxs-lookup"><span data-stu-id="f8578-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="f8578-176">発行の領域</span><span class="sxs-lookup"><span data-stu-id="f8578-176">Publishing Areas</span></span>

<span data-ttu-id="f8578-177">すべて`*.cshtml`と`wwwroot/**`ファイルに出力するときにパブリッシュされます`<Project Sdk="Microsoft.NET.Sdk.Web">`に含まれる、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f8578-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
