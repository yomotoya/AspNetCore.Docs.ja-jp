---
title: ASP.NET Core の区分
author: rick-anderson
description: 区分は ASP.NET MVC の機能であり、関連する機能を別の名前空間 (ルーティングの場合) およびフォルダー構造 (ビューの場合) としてグループにまとめるために使用する方法を説明します。
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: b78bb5146f1ab9039fa9ff015471654510718ed6
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312219"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="6b0db-103">ASP.NET Core の区分</span><span class="sxs-lookup"><span data-stu-id="6b0db-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="6b0db-104">作成者: [Dhananjay Kumar](https://twitter.com/debug_mode) および [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6b0db-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6b0db-105">区分は ASP.NET MVC の機能であり、関連する機能を別の名前空間 (ルーティングの場合) およびフォルダー構造 (ビューの場合) としてグループにまとめるために使用されます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="6b0db-106">ルーティングを行うために、区分を使用して、別のルート パラメーター `area` を `controller` と `action` に追加して階層を作成します。</span><span class="sxs-lookup"><span data-stu-id="6b0db-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="6b0db-107">区分は、大きな ASP.NET Core MVC Web アプリを小さい機能グループに分割するための方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="6b0db-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="6b0db-108">区分は、実質的にはアプリケーション内の MVC 構造体となります。</span><span class="sxs-lookup"><span data-stu-id="6b0db-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="6b0db-109">MVC プロジェクトでは、モデル、コント ローラー、ビューなどの論理コンポーネントが異なるフォルダーに保持され、MVC では名前付け規則を使用して、これらのコンポーネント間のリレーションシップを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b0db-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="6b0db-110">大きなアプリでは、アプリを機能の個別の高レベル区分に分割すると便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="6b0db-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="6b0db-111">たとえば、チェックアウト、請求、検索などの複数のビジネス ユニットがある e コマース アプリの場合です。これらのユニットにはそれぞれ独自の論理コンポーネント ビュー、コントローラー、およびモデルがあります。</span><span class="sxs-lookup"><span data-stu-id="6b0db-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="6b0db-112">このシナリオでは、区分を使用して、同じプロジェクト内のビジネス コンポーネントを物理的に分割できます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="6b0db-113">区分は、独自のコントローラー、ビュー、およびモデルのセットがある、ASP.NET Core MVC プロジェクトのより小さい機能ユニットとして定義できます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="6b0db-114">次のような場合は、MVC プロジェクトで区分を使用することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="6b0db-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="6b0db-115">アプリケーションが、論理的に区切る必要がある複数の高レベル機能コンポーネントで構成されている。</span><span class="sxs-lookup"><span data-stu-id="6b0db-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="6b0db-116">各機能区分を個別に使用できるように、MVC プロジェクトを分割したい。</span><span class="sxs-lookup"><span data-stu-id="6b0db-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="6b0db-117">区分の機能:</span><span class="sxs-lookup"><span data-stu-id="6b0db-117">Area features:</span></span>

* <span data-ttu-id="6b0db-118">ASP.NET Core MVC アプリは任意の数の区分を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-118">An ASP.NET Core MVC app can have any number of areas.</span></span>

* <span data-ttu-id="6b0db-119">各区分には独自のコントローラー、モデル、およびビューがあります。</span><span class="sxs-lookup"><span data-stu-id="6b0db-119">Each area has its own controllers, models, and views.</span></span>

* <span data-ttu-id="6b0db-120">区分を使用すると、大きな MVC プロジェクトを、個別に使用できる複数の高レベル コンポーネントにまとめることができます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-120">Areas allow you to organize large MVC projects into multiple high-level components that can be worked on independently.</span></span>

* <span data-ttu-id="6b0db-121">区分では同じ名前の複数のコントローラーがサポートされます (*区分* が異なる場合のみ)。</span><span class="sxs-lookup"><span data-stu-id="6b0db-121">Areas support multiple controllers with the same name, as long as they have different *areas*.</span></span>

<span data-ttu-id="6b0db-122">区分の作成および使用方法を示す例を見てましょう。</span><span class="sxs-lookup"><span data-stu-id="6b0db-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="6b0db-123">たとえば、ストア アプリに製品とサービスという、コントローラーとビューの 2 つの異なるグループがあるとします。</span><span class="sxs-lookup"><span data-stu-id="6b0db-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="6b0db-124">MVC 区分を使用する場合の一般的なフォルダー構造は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6b0db-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="6b0db-125">Project name</span><span class="sxs-lookup"><span data-stu-id="6b0db-125">Project name</span></span>

  * <span data-ttu-id="6b0db-126">Areas</span><span class="sxs-lookup"><span data-stu-id="6b0db-126">Areas</span></span>

    * <span data-ttu-id="6b0db-127">Products</span><span class="sxs-lookup"><span data-stu-id="6b0db-127">Products</span></span>

      * <span data-ttu-id="6b0db-128">Controllers</span><span class="sxs-lookup"><span data-stu-id="6b0db-128">Controllers</span></span>

        * <span data-ttu-id="6b0db-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6b0db-129">HomeController.cs</span></span>

        * <span data-ttu-id="6b0db-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="6b0db-130">ManageController.cs</span></span>

      * <span data-ttu-id="6b0db-131">Views</span><span class="sxs-lookup"><span data-stu-id="6b0db-131">Views</span></span>

        * <span data-ttu-id="6b0db-132">Home</span><span class="sxs-lookup"><span data-stu-id="6b0db-132">Home</span></span>

          * <span data-ttu-id="6b0db-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6b0db-133">Index.cshtml</span></span>

        * <span data-ttu-id="6b0db-134">Manage</span><span class="sxs-lookup"><span data-stu-id="6b0db-134">Manage</span></span>

          * <span data-ttu-id="6b0db-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6b0db-135">Index.cshtml</span></span>

    * <span data-ttu-id="6b0db-136">Services</span><span class="sxs-lookup"><span data-stu-id="6b0db-136">Services</span></span>

      * <span data-ttu-id="6b0db-137">Controllers</span><span class="sxs-lookup"><span data-stu-id="6b0db-137">Controllers</span></span>

        * <span data-ttu-id="6b0db-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6b0db-138">HomeController.cs</span></span>

      * <span data-ttu-id="6b0db-139">Views</span><span class="sxs-lookup"><span data-stu-id="6b0db-139">Views</span></span>

        * <span data-ttu-id="6b0db-140">Home</span><span class="sxs-lookup"><span data-stu-id="6b0db-140">Home</span></span>

          * <span data-ttu-id="6b0db-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6b0db-141">Index.cshtml</span></span>

<span data-ttu-id="6b0db-142">MVC は区分でのビューのレンダリングを試みる場合、既定では、次の場所で検索を試みます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="6b0db-143">これらは、`Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions` の `AreaViewLocationFormats` を使用して変更できる、既定の場所です。</span><span class="sxs-lookup"><span data-stu-id="6b0db-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="6b0db-144">たとえば、以下のコードでは、'Areas' というフォルダー名が、'Categories' に変更されています。</span><span class="sxs-lookup"><span data-stu-id="6b0db-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="6b0db-145">注目すべき 1 つの点は、ここでは *Views* フォルダーの構造のみが重要と見なされ、*Controllers* や *Models* などの他のフォルダーは重要では**ない**ということです。</span><span class="sxs-lookup"><span data-stu-id="6b0db-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="6b0db-146">たとえば、*Controllers* や *Models* フォルダーはまったく必要ありません。</span><span class="sxs-lookup"><span data-stu-id="6b0db-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="6b0db-147">*Controllers* や *Models* のコンテンツはコードのみであり、.dll にコンパイルされますが、*Views* のコンテンツはそのビューに対する要求が実行されるまでコンパイルされないためです。</span><span class="sxs-lookup"><span data-stu-id="6b0db-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="6b0db-148">フォルダー階層を定義したら、各コントローラーが区分に関連付けられていることを MVC に知らせる必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b0db-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="6b0db-149">コントローラー名を `[Area]` 属性で修飾することでこれを行います。</span><span class="sxs-lookup"><span data-stu-id="6b0db-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="6b0db-150">新しく作成した区分を操作するルート定義を設定します。</span><span class="sxs-lookup"><span data-stu-id="6b0db-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="6b0db-151">従来のルートと属性ルートの使用を含む、ルート定義の作成方法の詳細については、「[コントローラー アクションへのルーティング](routing.md)」記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6b0db-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="6b0db-152">この例では、従来のルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="6b0db-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="6b0db-153">そのためには、*Startup.cs* ファイルを開き、以下の `areaRoute` というルート定義を追加して変更します。</span><span class="sxs-lookup"><span data-stu-id="6b0db-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="6b0db-154">`http://<yourApp>/products` を参照することで、`Products` 区分内の `HomeController` の `Index` アクション メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="6b0db-155">リンクの生成</span><span class="sxs-lookup"><span data-stu-id="6b0db-155">Link Generation</span></span>

* <span data-ttu-id="6b0db-156">区分ベースのコントローラー内のアクションから、同じコントローラー内の別のアクションへのリンクの生成。</span><span class="sxs-lookup"><span data-stu-id="6b0db-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="6b0db-157">たとえば、現在の要求のパスが `/Products/Home/Create` であるとします。</span><span class="sxs-lookup"><span data-stu-id="6b0db-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="6b0db-158">HtmlHelper 構文: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="6b0db-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="6b0db-159">TagHelper 構文: `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6b0db-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="6b0db-160">'area' と 'controller' の値は現在の要求のコンテキストで既に使用可能であるため、ここでは指定する必要がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6b0db-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="6b0db-161">このような値は `ambient` 値といいます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="6b0db-162">区分ベースのコントローラー内のアクションから、異なるコントローラー内の別のアクションへのリンクの生成</span><span class="sxs-lookup"><span data-stu-id="6b0db-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="6b0db-163">たとえば、現在の要求のパスが `/Products/Home/Create` であるとします。</span><span class="sxs-lookup"><span data-stu-id="6b0db-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="6b0db-164">HtmlHelper 構文: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="6b0db-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="6b0db-165">TagHelper 構文: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6b0db-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="6b0db-166">ここでは 'area' のアンビエント値が使用されていますが、'controller' 値は上記では明示的に指定されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6b0db-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="6b0db-167">区分ベースのコントローラー内のアクションから、異なるコントローラーと異なる区分の別のアクションへのリンクの生成。</span><span class="sxs-lookup"><span data-stu-id="6b0db-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="6b0db-168">たとえば、現在の要求のパスが `/Products/Home/Create` であるとします。</span><span class="sxs-lookup"><span data-stu-id="6b0db-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="6b0db-169">HtmlHelper 構文: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="6b0db-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="6b0db-170">TagHelper 構文: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6b0db-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="6b0db-171">ここではアンビエント値が使用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6b0db-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="6b0db-172">区分ベースのコントローラー内のアクションから、異なるコントローラーと、区分には**ない**別のアクションへのリンクの生成。</span><span class="sxs-lookup"><span data-stu-id="6b0db-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="6b0db-173">HtmlHelper 構文: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="6b0db-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="6b0db-174">TagHelper 構文: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="6b0db-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="6b0db-175">非区分ベースのコントローラー アクションへのリンクを生成する必要があるため、ここでは 'area' のアンビエント値は空にします。</span><span class="sxs-lookup"><span data-stu-id="6b0db-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="6b0db-176">区分の発行</span><span class="sxs-lookup"><span data-stu-id="6b0db-176">Publishing Areas</span></span>

<span data-ttu-id="6b0db-177">`<Project Sdk="Microsoft.NET.Sdk.Web">` が *.csproj* ファイルに含まれている場合、すべての `*.cshtml` および `wwwroot/**` ファイルが発行され、出力されます。</span><span class="sxs-lookup"><span data-stu-id="6b0db-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
