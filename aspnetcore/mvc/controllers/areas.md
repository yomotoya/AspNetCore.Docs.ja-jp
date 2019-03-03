---
title: ASP.NET Core の区分
author: rick-anderson
description: 区分は ASP.NET MVC の機能であり、関連する機能を別の名前空間 (ルーティングの場合) およびフォルダー構造 (ビューの場合) としてグループにまとめるために使用する方法を説明します。
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833528"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="c9549-103">ASP.NET Core の区分</span><span class="sxs-lookup"><span data-stu-id="c9549-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="c9549-104">作成者: [Dhananjay Kumar](https://twitter.com/debug_mode) および [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c9549-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c9549-105">区分は ASP.NET の機能であり、関連する機能を別の名前空間 (ルーティングの場合) およびフォルダー構造 (ビューの場合) としてグループにまとめるために使用されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="c9549-106">ルーティングを行うために、区分を使用して、別のルート パラメーター `area` を `controller` と `action` または Razor Page `page` に追加して階層を作成します。</span><span class="sxs-lookup"><span data-stu-id="c9549-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="c9549-107">区分は、ASP.NET Core Web アプリをより小さな機能グループにパーティション分割する方法であり、分割した各グループにそれぞれの Razor Pages、コントローラー、ビュー、モデルが与えられます。</span><span class="sxs-lookup"><span data-stu-id="c9549-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="c9549-108">区分は、実質的にはアプリ内の構造体となります。</span><span class="sxs-lookup"><span data-stu-id="c9549-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="c9549-109">ASP.NET Core Web プロジェクトでは、ページ、モデル、コントローラー、ビューなどの論理コンポーネントが別々のフォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="c9549-110">ASP.NET Core ランタイムでは、名前付け規則を使用し、これらのコンポーネント間のリレーションシップを作成します。</span><span class="sxs-lookup"><span data-stu-id="c9549-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="c9549-111">大きなアプリでは、アプリを機能の個別の高レベル区分に分割すると便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="c9549-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="c9549-112">チェックアウト、請求、検索などの複数のビジネス ユニットがある eコマース アプリの場合です。</span><span class="sxs-lookup"><span data-stu-id="c9549-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="c9549-113">これらのユニットにはそれぞれ、ビュー、コントローラー、Razor Pages、モデルを含める独自の論理区分があります。</span><span class="sxs-lookup"><span data-stu-id="c9549-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="c9549-114">次のような場合は、プロジェクトで区分を使用することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="c9549-114">Consider using Areas in an project when:</span></span>

* <span data-ttu-id="c9549-115">論理的に大まかに区切れる複数の機能コンポーネントでアプリが構成されている。</span><span class="sxs-lookup"><span data-stu-id="c9549-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="c9549-116">各機能区分を個別に使用できるようにアプリを分割したい。</span><span class="sxs-lookup"><span data-stu-id="c9549-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="c9549-117">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c9549-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c9549-118">ダウンロード サンプルからは、区分をテストするための基本的なアプリが与えられます。</span><span class="sxs-lookup"><span data-stu-id="c9549-118">The download sample provides a basic app for testing areas.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="c9549-119">ビューを伴うコントローラーの区分</span><span class="sxs-lookup"><span data-stu-id="c9549-119">Areas for controllers with views</span></span>

<span data-ttu-id="c9549-120">区分、コントローラー、ビューを使用する一般的な ASP.NET Core Web アプリに含まれる内容:</span><span class="sxs-lookup"><span data-stu-id="c9549-120">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="c9549-121">[区分フォルダーの構造](#area-folder-structure)。</span><span class="sxs-lookup"><span data-stu-id="c9549-121">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="c9549-122">コントローラーと区分を関連付ける目的で [&lbrack;Area&rbrack;](#attribute) 属性で装飾されたコントローラー: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c9549-122">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="c9549-123">[スタートアップに追加された区分ルート](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="c9549-123">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

## <a name="area-folder-structure"></a><span data-ttu-id="c9549-124">区分フォルダーの構造</span><span class="sxs-lookup"><span data-stu-id="c9549-124">Area folder structure</span></span>
<span data-ttu-id="c9549-125">あるアプリに *Products* と *Services* という 2 つの論理グループが与えられているとします。</span><span class="sxs-lookup"><span data-stu-id="c9549-125">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="c9549-126">区分を利用すると、フォルダーの構造は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c9549-126">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="c9549-127">Project name</span><span class="sxs-lookup"><span data-stu-id="c9549-127">Project name</span></span>
  * <span data-ttu-id="c9549-128">Areas</span><span class="sxs-lookup"><span data-stu-id="c9549-128">Areas</span></span>
    * <span data-ttu-id="c9549-129">Products</span><span class="sxs-lookup"><span data-stu-id="c9549-129">Products</span></span>
      * <span data-ttu-id="c9549-130">Controllers</span><span class="sxs-lookup"><span data-stu-id="c9549-130">Controllers</span></span>
        * <span data-ttu-id="c9549-131">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="c9549-131">HomeController.cs</span></span>
        * <span data-ttu-id="c9549-132">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="c9549-132">ManageController.cs</span></span>
      * <span data-ttu-id="c9549-133">Views</span><span class="sxs-lookup"><span data-stu-id="c9549-133">Views</span></span>
        * <span data-ttu-id="c9549-134">Home</span><span class="sxs-lookup"><span data-stu-id="c9549-134">Home</span></span>
          * <span data-ttu-id="c9549-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c9549-135">Index.cshtml</span></span>
        * <span data-ttu-id="c9549-136">Manage</span><span class="sxs-lookup"><span data-stu-id="c9549-136">Manage</span></span>
          * <span data-ttu-id="c9549-137">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c9549-137">Index.cshtml</span></span>
          * <span data-ttu-id="c9549-138">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="c9549-138">About.cshtml</span></span>
    * <span data-ttu-id="c9549-139">Services</span><span class="sxs-lookup"><span data-stu-id="c9549-139">Services</span></span>
      * <span data-ttu-id="c9549-140">Controllers</span><span class="sxs-lookup"><span data-stu-id="c9549-140">Controllers</span></span>
        * <span data-ttu-id="c9549-141">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="c9549-141">HomeController.cs</span></span>
      * <span data-ttu-id="c9549-142">Views</span><span class="sxs-lookup"><span data-stu-id="c9549-142">Views</span></span>
        * <span data-ttu-id="c9549-143">Home</span><span class="sxs-lookup"><span data-stu-id="c9549-143">Home</span></span>
          * <span data-ttu-id="c9549-144">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c9549-144">Index.cshtml</span></span>

<span data-ttu-id="c9549-145">区分を使用するとき、前述のレイアウトが一般的ですが、このフォルダー構造を使用するにはビュー ファイルのみが求められます。</span><span class="sxs-lookup"><span data-stu-id="c9549-145">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="c9549-146">ビューの検出では、一致する区分ビュー ファイルを次の順序で検索します。</span><span class="sxs-lookup"><span data-stu-id="c9549-146">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="c9549-147">*Controllers* や *Models* など、ビュー以外のフォルダーの場所は問題では**ありません**。</span><span class="sxs-lookup"><span data-stu-id="c9549-147">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="c9549-148">たとえば、*Controllers* や *Models* のフォルダーは不要です。</span><span class="sxs-lookup"><span data-stu-id="c9549-148">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="c9549-149">*Controllers* と *Models* の内容はコードであり、.dll にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="c9549-149">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="c9549-150">*Views* の内容は、ビューに対する要求が行われるまでコンパイルされません。</span><span class="sxs-lookup"><span data-stu-id="c9549-150">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="c9549-151">コントローラーを区分に関連付ける</span><span class="sxs-lookup"><span data-stu-id="c9549-151">Associate the controller with an Area</span></span>

<span data-ttu-id="c9549-152">区分コントローラーは [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) 属性で指名されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-152">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="c9549-153">区分ルートを追加する</span><span class="sxs-lookup"><span data-stu-id="c9549-153">Add Area route</span></span>

<span data-ttu-id="c9549-154">区分ルートでは通常、属性ルーティングではなく、従来のルーティングが使用されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-154">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="c9549-155">規則ルーティングは順序に依存します。</span><span class="sxs-lookup"><span data-stu-id="c9549-155">Conventional routing is order-dependent.</span></span> <span data-ttu-id="c9549-156">一般に、区分のあるルートは区分を持たないルートより具体的なので、区分のあるルートはルート テーブルの前の方に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c9549-156">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="c9549-157">URL スペースがすべての区分で統一されている場合、ルート テンプレートでトークンとして `{area:...}` を使用できます。</span><span class="sxs-lookup"><span data-stu-id="c9549-157">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="c9549-158">上記のコードでは、ルートは 1 つの区分に一致しなければならないという制約が `exists` によって適用されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-158">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="c9549-159">`{area:...}` の使用は、ルーティングを区分に追加するメカニズムとして最も単純です。</span><span class="sxs-lookup"><span data-stu-id="c9549-159">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="c9549-160">次のコードでは、<xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> を使用し、名前の付いた区分ルートが 2 つ作成されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-160">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="c9549-161">ASP.NET Core 2.2 で `MapAreaRoute` を使用するときは、[この GitHub 問題](https://github.com/aspnet/AspNetCore/issues/7772)を確認してください。</span><span class="sxs-lookup"><span data-stu-id="c9549-161">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="c9549-162">詳細については、[区分のルーティング](xref:mvc/controllers/routing#areas)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c9549-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-areas"></a><span data-ttu-id="c9549-163">区分を含むリンクの生成</span><span class="sxs-lookup"><span data-stu-id="c9549-163">Link Generation with Areas</span></span>

<span data-ttu-id="c9549-164">[サンプル ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)に含まれる次のコードでは、区分が指定された上でリンクが生成されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-164">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="c9549-165">上記のコードで生成されたリンクは、アプリ内のあらゆる場所で有効となります。</span><span class="sxs-lookup"><span data-stu-id="c9549-165">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="c9549-166">サンプル ダウンロードには、[部分ビュー](xref:mvc/views/partial)が含まれます。部分ビューには、上記のリンクと区分が指定されていない同じリンクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c9549-166">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="c9549-167">部分ビューは[レイアウト ファイル]()で参照されます。そのため、生成されたリンクがアプリのすべてのページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-167">The partial view is referenced in the [layout file](), so every page in the app displays the generated links.</span></span> <span data-ttu-id="c9549-168">区分が指定されずに生成されたリンクは、同じ区分やコントローラーのページから参照されるときにのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="c9549-168">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="c9549-169">区分またはコントローラーが指定されていないとき、ルーティングは*アンビエント*値に依存します。</span><span class="sxs-lookup"><span data-stu-id="c9549-169">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="c9549-170">現在の要求の現在のルート値は、リンク生成の場合、アンビエント値として見なされます。</span><span class="sxs-lookup"><span data-stu-id="c9549-170">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="c9549-171">サンプル アプリでは多くの場合、アンビエント値を使用すると、間違ったリンクが生成されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-171">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="c9549-172">詳細については、「[コントローラー アクションへのルーティング](xref:mvc/controllers/routing)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c9549-172">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="c9549-173">_ViewStart.cshtml ファイルを使用した区分の共有レイアウト</span><span class="sxs-lookup"><span data-stu-id="c9549-173">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="c9549-174">アプリ全体で共通レイアウトを共有するには、アプリケーションのルート フォルダーに *_ViewStart.cshtml* を移動します。</span><span class="sxs-lookup"><span data-stu-id="c9549-174">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="c9549-175">ビューが保存されている既定の区分フォルダーを変更する</span><span class="sxs-lookup"><span data-stu-id="c9549-175">Change default area folder where views are stored</span></span>

<span data-ttu-id="c9549-176">次のコードでは、既定の区分フォルダーが `"Areas"` から `"MyAreas"` に変更されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-176">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a><span data-ttu-id="c9549-177">区分の発行</span><span class="sxs-lookup"><span data-stu-id="c9549-177">Publishing Areas</span></span>

<span data-ttu-id="c9549-178">`<Project Sdk="Microsoft.NET.Sdk.Web">` が *.csproj* ファイルに含まれている場合、すべての `*.cshtml` および `wwwroot/**` ファイルが発行され、出力されます。</span><span class="sxs-lookup"><span data-stu-id="c9549-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
