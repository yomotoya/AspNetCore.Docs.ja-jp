---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: アクション フィルター (c#) について理解する |Microsoft Docs
author: microsoft
description: このチュートリアルの目的では、アクション フィルターについて説明します。 コント ローラーのアクションまたはコント ローラー全体に適用できる属性をアクション フィルターには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: b1294eb61164d9f5c71152a542daa673de37f4ca
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398233"
---
<a name="understanding-action-filters-c"></a><span data-ttu-id="eb415-104">アクション フィルター (c#) について理解します。</span><span class="sxs-lookup"><span data-stu-id="eb415-104">Understanding Action Filters (C#)</span></span>
====================
<span data-ttu-id="eb415-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="eb415-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="eb415-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="eb415-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> <span data-ttu-id="eb415-107">このチュートリアルの目的では、アクション フィルターについて説明します。</span><span class="sxs-lookup"><span data-stu-id="eb415-107">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="eb415-108">アクション フィルターは、コント ローラーのアクションまたは全体コント ローラー--アクションを実行する方法を変更するに適用できる属性です。</span><span class="sxs-lookup"><span data-stu-id="eb415-108">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span>


## <a name="understanding-action-filters"></a><span data-ttu-id="eb415-109">アクション フィルターについて理解します。</span><span class="sxs-lookup"><span data-stu-id="eb415-109">Understanding Action Filters</span></span>

<span data-ttu-id="eb415-110">このチュートリアルの目的では、アクション フィルターについて説明します。</span><span class="sxs-lookup"><span data-stu-id="eb415-110">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="eb415-111">アクション フィルターは、コント ローラーのアクションまたは全体コント ローラー--アクションを実行する方法を変更するに適用できる属性です。</span><span class="sxs-lookup"><span data-stu-id="eb415-111">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span> <span data-ttu-id="eb415-112">ASP.NET MVC フレームワークには、いくつかのアクション フィルターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="eb415-112">The ASP.NET MVC framework includes several action filters:</span></span>

- <span data-ttu-id="eb415-113">OutputCache – このアクション フィルターは、一定の時間のコント ローラー アクションの出力をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="eb415-113">OutputCache – This action filter caches the output of a controller action for a specified amount of time.</span></span>
- <span data-ttu-id="eb415-114">HandleError – このアクション フィルターは、コント ローラーのアクションを実行するときに発生したエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="eb415-114">HandleError – This action filter handles errors raised when a controller action executes.</span></span>
- <span data-ttu-id="eb415-115">承認-このアクション フィルターでは、特定のユーザーまたはロールのアクセスを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="eb415-115">Authorize – This action filter enables you to restrict access to a particular user or role.</span></span>

<span data-ttu-id="eb415-116">また、独自のカスタム アクション フィルターを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="eb415-116">You also can create your own custom action filters.</span></span> <span data-ttu-id="eb415-117">たとえば、カスタム認証システムを実装するためにカスタム アクション フィルターを作成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="eb415-117">For example, you might want to create a custom action filter in order to implement a custom authentication system.</span></span> <span data-ttu-id="eb415-118">または、コント ローラーのアクションによって返されるデータの表示を変更するアクション フィルターを作成したい場合があります。</span><span class="sxs-lookup"><span data-stu-id="eb415-118">Or, you might want to create an action filter that modifies the view data returned by a controller action.</span></span>

<span data-ttu-id="eb415-119">このチュートリアル ゼロからアクション フィルターを構築する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="eb415-119">In this tutorial, you learn how to build an action filter from the ground up.</span></span> <span data-ttu-id="eb415-120">Visual Studio の出力ウィンドウをアクションの処理のさまざまな段階に記録するログのアクション フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="eb415-120">We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.</span></span>

### <a name="using-an-action-filter"></a><span data-ttu-id="eb415-121">アクション フィルターの使用</span><span class="sxs-lookup"><span data-stu-id="eb415-121">Using an Action Filter</span></span>

<span data-ttu-id="eb415-122">アクション フィルターは、属性です。</span><span class="sxs-lookup"><span data-stu-id="eb415-122">An action filter is an attribute.</span></span> <span data-ttu-id="eb415-123">ほとんどのアクション フィルターは、個々 のコント ローラー アクションまたはコント ローラー全体に適用できます。</span><span class="sxs-lookup"><span data-stu-id="eb415-123">You can apply most action filters to either an individual controller action or an entire controller.</span></span>

<span data-ttu-id="eb415-124">たとえば、リスト 1 でのデータのコント ローラーはという名前のアクションを公開`Index()`現在の時刻を返します。</span><span class="sxs-lookup"><span data-stu-id="eb415-124">For example, the Data controller in Listing 1 exposes an action named `Index()` that returns the current time.</span></span> <span data-ttu-id="eb415-125">このアクションがで修飾された、`OutputCache`アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="eb415-125">This action is decorated with the `OutputCache` action filter.</span></span> <span data-ttu-id="eb415-126">このフィルターによって、10 秒間キャッシュされるアクションによって返される値。</span><span class="sxs-lookup"><span data-stu-id="eb415-126">This filter causes the value returned by the action to be cached for 10 seconds.</span></span>

<span data-ttu-id="eb415-127">**1 – を一覧表示します。 `Controllers\DataController.cs`**</span><span class="sxs-lookup"><span data-stu-id="eb415-127">**Listing 1 – `Controllers\DataController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

<span data-ttu-id="eb415-128">繰り返しを起動する場合、`Index()`お使いのブラウザーのアドレス バーに URL データ/インデックスを入力して、更新操作ボタンを複数回同時には 10 秒間表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-128">If you repeatedly invoke the `Index()` action by entering the URL /Data/Index into the address bar of your browser and hitting the Refresh button multiple times, then you will see the same time for 10 seconds.</span></span> <span data-ttu-id="eb415-129">出力、`Index()`アクションが (図 1 参照)、10 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="eb415-129">The output of the `Index()` action is cached for 10 seconds (see Figure 1).</span></span>


<span data-ttu-id="eb415-130">[![キャッシュされる時間](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eb415-130">[![Cached time](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)</span></span>

<span data-ttu-id="eb415-131">**図 01**: キャッシュ時間 ([フルサイズの画像を表示する をクリックします](understanding-action-filters-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="eb415-131">**Figure 01**: Cached time ([Click to view full-size image](understanding-action-filters-cs/_static/image3.png))</span></span>


<span data-ttu-id="eb415-132">リスト 1、1 つのアクション フィルター – で、`OutputCache`アクション フィルター – に適用されます、`Index()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="eb415-132">In Listing 1, a single action filter – the `OutputCache` action filter – is applied to the `Index()` method.</span></span> <span data-ttu-id="eb415-133">が必要な場合は、同じアクションに複数のアクション フィルターを適用できます。</span><span class="sxs-lookup"><span data-stu-id="eb415-133">If you need, you can apply multiple action filters to the same action.</span></span> <span data-ttu-id="eb415-134">両方を適用するなど、`OutputCache`と`HandleError`同じアクションにアクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="eb415-134">For example, you might want to apply both the `OutputCache` and `HandleError` action filters to the same action.</span></span>

<span data-ttu-id="eb415-135">リストの 1 で、`OutputCache`にアクション フィルターを適用、`Index()`アクション。</span><span class="sxs-lookup"><span data-stu-id="eb415-135">In Listing 1, the `OutputCache` action filter is applied to the `Index()` action.</span></span> <span data-ttu-id="eb415-136">この属性を適用できますも、`DataController`クラス自体。</span><span class="sxs-lookup"><span data-stu-id="eb415-136">You also could apply this attribute to the `DataController` class itself.</span></span> <span data-ttu-id="eb415-137">その場合は、コント ローラーによって公開される何らかの操作によって返される結果は 10 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="eb415-137">In that case, the result returned by any action exposed by the controller would be cached for 10 seconds.</span></span>

### <a name="the-different-types-of-filters"></a><span data-ttu-id="eb415-138">さまざまな種類のフィルター</span><span class="sxs-lookup"><span data-stu-id="eb415-138">The Different Types of Filters</span></span>

<span data-ttu-id="eb415-139">ASP.NET MVC フレームワークには、次の 4 つの異なる種類のフィルターがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="eb415-139">The ASP.NET MVC framework supports four different types of filters:</span></span>

1. <span data-ttu-id="eb415-140">承認フィルター – 実装、`IAuthorizationFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="eb415-140">Authorization filters – Implements the `IAuthorizationFilter` attribute.</span></span>
2. <span data-ttu-id="eb415-141">アクション フィルター – 実装、`IActionFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="eb415-141">Action filters – Implements the `IActionFilter` attribute.</span></span>
3. <span data-ttu-id="eb415-142">結果フィルター – 実装、`IResultFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="eb415-142">Result filters – Implements the `IResultFilter` attribute.</span></span>
4. <span data-ttu-id="eb415-143">例外フィルター – 実装、`IExceptionFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="eb415-143">Exception filters – Implements the `IExceptionFilter` attribute.</span></span>

<span data-ttu-id="eb415-144">フィルターは、上に示した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-144">Filters are executed in the order listed above.</span></span> <span data-ttu-id="eb415-145">たとえば、承認フィルターは、常にアクション フィルターの前に実行し、例外フィルターは、常に他のすべての種類フィルターの後に実行します。</span><span class="sxs-lookup"><span data-stu-id="eb415-145">For example, authorization filters are always executed before action filters and exception filters are always executed after every other type of filter.</span></span>

<span data-ttu-id="eb415-146">承認フィルターは、コント ローラー アクションの認証と承認を実装するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-146">Authorization filters are used to implement authentication and authorization for controller actions.</span></span> <span data-ttu-id="eb415-147">たとえば、承認フィルターは、承認フィルターの例です。</span><span class="sxs-lookup"><span data-stu-id="eb415-147">For example, the Authorize filter is an example of an Authorization filter.</span></span>

<span data-ttu-id="eb415-148">アクション フィルターには、コント ローラーのアクションが実行された前後に実行されるロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="eb415-148">Action filters contain logic that is executed before and after a controller action executes.</span></span> <span data-ttu-id="eb415-149">たとえば、コント ローラーのアクションが返すデータの表示を変更するアクション フィルターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="eb415-149">You can use an action filter, for instance, to modify the view data that a controller action returns.</span></span>

<span data-ttu-id="eb415-150">結果フィルターには、ビューの結果が実行された前後に実行されるロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="eb415-150">Result filters contain logic that is executed before and after a view result is executed.</span></span> <span data-ttu-id="eb415-151">ビューの結果を変更するなど、ブラウザーにビューが表示される直前です。</span><span class="sxs-lookup"><span data-stu-id="eb415-151">For example, you might want to modify a view result right before the view is rendered to the browser.</span></span>

<span data-ttu-id="eb415-152">例外フィルターは、最後に実行するフィルターの種類です。</span><span class="sxs-lookup"><span data-stu-id="eb415-152">Exception filters are the last type of filter to run.</span></span> <span data-ttu-id="eb415-153">コント ローラー アクションまたはコント ローラー アクションの結果のいずれかによって発生したエラーを処理するために、例外フィルターを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="eb415-153">You can use an exception filter to handle errors raised by either your controller actions or controller action results.</span></span> <span data-ttu-id="eb415-154">また、例外フィルターをエラーをログに使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="eb415-154">You also can use exception filters to log errors.</span></span>

<span data-ttu-id="eb415-155">各フィルターの別の種類は、特定の順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-155">Each different type of filter is executed in a particular order.</span></span> <span data-ttu-id="eb415-156">同じ種類のフィルターが実行される順序を制御する場合は、フィルターの順序のプロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="eb415-156">If you want to control the order in which filters of the same type are executed then you can set a filter's Order property.</span></span>

<span data-ttu-id="eb415-157">すべてのアクション フィルターの基本クラスは、`System.Web.Mvc.FilterAttribute`クラス。</span><span class="sxs-lookup"><span data-stu-id="eb415-157">The base class for all action filters is the `System.Web.Mvc.FilterAttribute` class.</span></span> <span data-ttu-id="eb415-158">特定の種類のフィルターを実装するかどうかは、フィルターの基本クラスから継承し、1 つ以上を実装するクラスを作成する必要がある、 `IAuthorizationFilter`、 `IActionFilter`、 `IResultFilter`、または`IExceptionFilter`インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="eb415-158">If you want to implement a particular type of filter, then you need to create a class that inherits from the base Filter class and implements one or more of the `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, or `IExceptionFilter` interfaces.</span></span>

### <a name="the-base-actionfilterattribute-class"></a><span data-ttu-id="eb415-159">基本 ActionFilterAttribute クラス</span><span class="sxs-lookup"><span data-stu-id="eb415-159">The Base ActionFilterAttribute Class</span></span>

<span data-ttu-id="eb415-160">簡単なカスタム アクション フィルターを実装するために、ASP.NET MVC framework にはベースが含まれています`ActionFilterAttribute`クラス。</span><span class="sxs-lookup"><span data-stu-id="eb415-160">In order to make it easier for you to implement a custom action filter, the ASP.NET MVC framework includes a base `ActionFilterAttribute` class.</span></span> <span data-ttu-id="eb415-161">このクラスは、両方を実装、`IActionFilter`と`IResultFilter`インターフェイスから継承して、`Filter`クラス。</span><span class="sxs-lookup"><span data-stu-id="eb415-161">This class implements both the `IActionFilter` and `IResultFilter` interfaces and inherits from the `Filter` class.</span></span>

<span data-ttu-id="eb415-162">用語をここでは、完全一致しません。</span><span class="sxs-lookup"><span data-stu-id="eb415-162">The terminology here is not entirely consistent.</span></span> <span data-ttu-id="eb415-163">技術的には、ActionFilterAttribute クラスから継承するクラスは、アクション フィルターと結果フィルターの両方です。</span><span class="sxs-lookup"><span data-stu-id="eb415-163">Technically, a class that inherits from the ActionFilterAttribute class is both an action filter and a result filter.</span></span> <span data-ttu-id="eb415-164">ただし、loose の意味では、word のアクション フィルターは、ASP.NET MVC フレームワークでのフィルターの任意の型を参照する使用されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-164">However, in the loose sense, the word action filter is used to refer to any type of filter in the ASP.NET MVC framework.</span></span>

<span data-ttu-id="eb415-165">基本`ActionFilterAttribute`クラスには次のメソッドをオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="eb415-165">The base `ActionFilterAttribute` class has the following methods that you can override:</span></span>

- <span data-ttu-id="eb415-166">OnActionExecuting – コント ローラーのアクションが実行される前に、このメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-166">OnActionExecuting – This method is called before a controller action is executed.</span></span>
- <span data-ttu-id="eb415-167">OnActionExecuted – コント ローラーのアクションが実行された後、このメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-167">OnActionExecuted – This method is called after a controller action is executed.</span></span>
- <span data-ttu-id="eb415-168">OnResultExecuting – コント ローラー アクションの結果が実行される前に、このメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-168">OnResultExecuting – This method is called before a controller action result is executed.</span></span>
- <span data-ttu-id="eb415-169">OnResultExecuted – コント ローラー アクションの結果が実行された後、このメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-169">OnResultExecuted – This method is called after a controller action result is executed.</span></span>

<span data-ttu-id="eb415-170">次のセクションでそれぞれのさまざまな方法を実装する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb415-170">In the next section, we'll see how you can implement each of these different methods.</span></span>

### <a name="creating-a-log-action-filter"></a><span data-ttu-id="eb415-171">ログのアクション フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="eb415-171">Creating a Log Action Filter</span></span>

<span data-ttu-id="eb415-172">カスタム アクション フィルターを作成する方法を示すためには、するには、Visual Studio 出力ウィンドウにコント ローラー アクションの処理のステージを記録するカスタム アクション フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="eb415-172">In order to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span> <span data-ttu-id="eb415-173">この`LogActionFilter`リスト 2 に含まれています。</span><span class="sxs-lookup"><span data-stu-id="eb415-173">Our `LogActionFilter` is contained in Listing 2.</span></span>

<span data-ttu-id="eb415-174">**2 – を一覧表示します。 `ActionFilters\LogActionFilter.cs`**</span><span class="sxs-lookup"><span data-stu-id="eb415-174">**Listing 2 – `ActionFilters\LogActionFilter.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

<span data-ttu-id="eb415-175">リストの 2 で、 `OnActionExecuting()`、 `OnActionExecuted()`、 `OnResultExecuting()`、および`OnResultExecuted()`すべてのメソッドを呼び出す、`Log()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="eb415-175">In Listing 2, the `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, and `OnResultExecuted()` methods all call the `Log()` method.</span></span> <span data-ttu-id="eb415-176">メソッドと現在のルート データの名前に渡される、`Log()`メソッド。</span><span class="sxs-lookup"><span data-stu-id="eb415-176">The name of the method and the current route data is passed to the `Log()` method.</span></span> <span data-ttu-id="eb415-177">`Log()`メソッドは、Visual Studio の出力ウィンドウにメッセージを書き込みます (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="eb415-177">The `Log()` method writes a message to the Visual Studio Output window (see Figure 2).</span></span>


<span data-ttu-id="eb415-178">[![Visual Studio の出力ウィンドウへの書き込み](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="eb415-178">[![Writing to the Visual Studio Output window](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)</span></span>

<span data-ttu-id="eb415-179">**図 02**: Visual Studio の出力ウィンドウへの書き込み ([フルサイズの画像を表示する をクリックします](understanding-action-filters-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="eb415-179">**Figure 02**: Writing to the Visual Studio Output window ([Click to view full-size image](understanding-action-filters-cs/_static/image6.png))</span></span>


<span data-ttu-id="eb415-180">リスト 3 の Home コント ローラーは、全体のコント ローラー クラスに、ログのアクション フィルターを適用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="eb415-180">The Home controller in Listing 3 illustrates how you can apply the Log action filter to an entire controller class.</span></span> <span data-ttu-id="eb415-181">たびに、Home コント ローラーによって公開されるアクションのいずれかが呼び出される – か、`Index()`メソッドまたは`About()`メソッド – Visual Studio の出力ウィンドウに、アクションがログに記録する処理の段階です。</span><span class="sxs-lookup"><span data-stu-id="eb415-181">Whenever any of the actions exposed by the Home controller are invoked – either the `Index()` method or the `About()` method – the stages of processing the action are logged to the Visual Studio Output window.</span></span>

<span data-ttu-id="eb415-182">**3 – を一覧表示します。 `Controllers\HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="eb415-182">**Listing 3 – `Controllers\HomeController.cs`**</span></span>

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a><span data-ttu-id="eb415-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="eb415-183">Summary</span></span>

<span data-ttu-id="eb415-184">このチュートリアルでは、ASP.NET MVC のアクション フィルターに導入されました。</span><span class="sxs-lookup"><span data-stu-id="eb415-184">In this tutorial, you were introduced to ASP.NET MVC action filters.</span></span> <span data-ttu-id="eb415-185">4 つの異なる種類のフィルターについて説明しました。 承認フィルター、アクション フィルター、結果フィルター、および例外フィルター。</span><span class="sxs-lookup"><span data-stu-id="eb415-185">You learned about the four different types of filters: authorization filters, action filters, result filters, and exception filters.</span></span> <span data-ttu-id="eb415-186">基本についても学習しました`ActionFilterAttribute`クラス。</span><span class="sxs-lookup"><span data-stu-id="eb415-186">You also learned about the base `ActionFilterAttribute` class.</span></span>

<span data-ttu-id="eb415-187">最後に、簡単なアクション フィルターを実装する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="eb415-187">Finally, you learned how to implement a simple action filter.</span></span> <span data-ttu-id="eb415-188">Visual Studio の出力ウィンドウにコント ローラー アクションの処理のステージを記録するログのアクション フィルターを作成しました。</span><span class="sxs-lookup"><span data-stu-id="eb415-188">We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eb415-189">[前へ](asp-net-mvc-routing-overview-cs.md)
> [次へ](improving-performance-with-output-caching-cs.md)</span><span class="sxs-lookup"><span data-stu-id="eb415-189">[Previous](asp-net-mvc-routing-overview-cs.md)
[Next](improving-performance-with-output-caching-cs.md)</span></span>
