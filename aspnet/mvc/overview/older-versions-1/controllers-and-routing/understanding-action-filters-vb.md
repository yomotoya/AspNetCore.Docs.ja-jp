---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: アクション フィルター (VB) を理解する |Microsoft ドキュメント
author: microsoft
description: このチュートリアルの目的では、アクション フィルターをについて説明します。 コント ローラーのアクションまたはコント ローラー全体に適用可能な属性をアクション フィルターには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 2796b67cba6a2ddaee7a006a170dfb7e5ff89888
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869817"
---
<a name="understanding-action-filters-vb"></a><span data-ttu-id="1bc94-104">アクション フィルター (VB) を理解します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-104">Understanding Action Filters (VB)</span></span>
====================
<span data-ttu-id="1bc94-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1bc94-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="1bc94-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1bc94-106">Download PDF</span></span>](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> <span data-ttu-id="1bc94-107">このチュートリアルの目的では、アクション フィルターをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-107">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="1bc94-108">アクション フィルターは、コント ローラーのアクション--または全体コント ローラー--アクションを実行する方法を変更するに適用可能な属性です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-108">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span>


## <a name="understanding-action-filters"></a><span data-ttu-id="1bc94-109">アクション フィルターを理解します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-109">Understanding Action Filters</span></span>

<span data-ttu-id="1bc94-110">このチュートリアルの目的では、アクション フィルターをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-110">The goal of this tutorial is to explain action filters.</span></span> <span data-ttu-id="1bc94-111">アクション フィルターは、コント ローラーのアクション--または全体コント ローラー--アクションを実行する方法を変更するに適用可能な属性です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-111">An action filter is an attribute that you can apply to a controller action -- or an entire controller -- that modifies the way in which the action is executed.</span></span> <span data-ttu-id="1bc94-112">ASP.NET MVC フレームワークには、いくつかのアクション フィルターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1bc94-112">The ASP.NET MVC framework includes several action filters:</span></span>

- <span data-ttu-id="1bc94-113">OutputCache – このアクション フィルターは、一定の時間のコント ローラー アクションの出力をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="1bc94-113">OutputCache – This action filter caches the output of a controller action for a specified amount of time.</span></span>
- <span data-ttu-id="1bc94-114">HandleError – このアクション フィルターは、コント ローラーのアクションを実行するときに発生したエラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-114">HandleError – This action filter handles errors raised when a controller action executes.</span></span>
- <span data-ttu-id="1bc94-115">承認: このアクション フィルターでは、特定のユーザーまたはロールにアクセスを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-115">Authorize – This action filter enables you to restrict access to a particular user or role.</span></span>

<span data-ttu-id="1bc94-116">また、独自のカスタム アクション フィルターを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-116">You also can create your own custom action filters.</span></span> <span data-ttu-id="1bc94-117">たとえば、カスタム認証システムを実装するためにカスタム アクション フィルターを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-117">For example, you might want to create a custom action filter in order to implement a custom authentication system.</span></span> <span data-ttu-id="1bc94-118">または、コント ローラーのアクションによって返されるデータの表示を変更するアクション フィルターを作成する場合があります。</span><span class="sxs-lookup"><span data-stu-id="1bc94-118">Or, you might want to create an action filter that modifies the view data returned by a controller action.</span></span>

<span data-ttu-id="1bc94-119">このチュートリアルを最初からアクション フィルターを構築する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-119">In this tutorial, you learn how to build an action filter from the ground up.</span></span> <span data-ttu-id="1bc94-120">Visual Studio の出力ウィンドウに、アクションの処理のさまざまな段階をログに記録するログ アクション フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-120">We create a Log action filter that logs different stages of the processing of an action to the Visual Studio Output window.</span></span>

### <a name="using-an-action-filter"></a><span data-ttu-id="1bc94-121">アクション フィルターを使用します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-121">Using an Action Filter</span></span>

<span data-ttu-id="1bc94-122">アクション フィルターは、属性です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-122">An action filter is an attribute.</span></span> <span data-ttu-id="1bc94-123">個々 のコント ローラー アクションまたは全体のコント ローラーには、ほとんどのアクション フィルターを適用できます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-123">You can apply most action filters to either an individual controller action or an entire controller.</span></span>

<span data-ttu-id="1bc94-124">リスト 1 のデータのコント ローラーがという名前のアクションを公開するなど、`Index()`現在の時刻を返します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-124">For example, the Data controller in Listing 1 exposes an action named `Index()` that returns the current time.</span></span> <span data-ttu-id="1bc94-125">この操作で装飾されて、`OutputCache`アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="1bc94-125">This action is decorated with the `OutputCache` action filter.</span></span> <span data-ttu-id="1bc94-126">このフィルターによって、10 秒間キャッシュに保存する操作によって返される値。</span><span class="sxs-lookup"><span data-stu-id="1bc94-126">This filter causes the value returned by the action to be cached for 10 seconds.</span></span>

<span data-ttu-id="1bc94-127">**1 – を一覧表示します。 `Controllers\DataController.vb`**</span><span class="sxs-lookup"><span data-stu-id="1bc94-127">**Listing 1 – `Controllers\DataController.vb`**</span></span>

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

<span data-ttu-id="1bc94-128">繰り返し呼び出す場合、`Index()`お使いのブラウザーのアドレス バーに、URL データ/インデックスを入力して、更新のヒットのアクション ボタンは、同時に 10 秒間表示、複数回をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1bc94-128">If you repeatedly invoke the `Index()` action by entering the URL /Data/Index into the address bar of your browser and hitting the Refresh button multiple times, then you will see the same time for 10 seconds.</span></span> <span data-ttu-id="1bc94-129">出力、`Index()`アクションが (図 1 を参照してください) 10 秒間キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="1bc94-129">The output of the `Index()` action is cached for 10 seconds (see Figure 1).</span></span>


<span data-ttu-id="1bc94-130">[![キャッシュ時間](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1bc94-130">[![Cached time](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)</span></span>

<span data-ttu-id="1bc94-131">**図 01**: キャッシュ時間 ([フルサイズのイメージを表示するをクリックして](understanding-action-filters-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1bc94-131">**Figure 01**: Cached time ([Click to view full-size image](understanding-action-filters-vb/_static/image3.png))</span></span>


<span data-ttu-id="1bc94-132">1 では一覧表示する、1 つのアクション フィルター –、 `OutputCache` – アクション フィルターを適用、`Index()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1bc94-132">In Listing 1, a single action filter – the `OutputCache` action filter – is applied to the `Index()` method.</span></span> <span data-ttu-id="1bc94-133">必要がある場合は、同じアクションに複数のアクション フィルターを適用できます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-133">If you need, you can apply multiple action filters to the same action.</span></span> <span data-ttu-id="1bc94-134">両方を適用するなど、`OutputCache`と`HandleError`同じアクションにアクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="1bc94-134">For example, you might want to apply both the `OutputCache` and `HandleError` action filters to the same action.</span></span>

<span data-ttu-id="1bc94-135">リストの 1 の`OutputCache`にアクション フィルターが適用される、`Index()`アクション。</span><span class="sxs-lookup"><span data-stu-id="1bc94-135">In Listing 1, the `OutputCache` action filter is applied to the `Index()` action.</span></span> <span data-ttu-id="1bc94-136">この属性を適用でしたも、`DataController`クラス自体です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-136">You also could apply this attribute to the `DataController` class itself.</span></span> <span data-ttu-id="1bc94-137">その場合は、コント ローラーによって公開される何らかの操作によって返される結果は 10 秒間キャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-137">In that case, the result returned by any action exposed by the controller would be cached for 10 seconds.</span></span>

### <a name="the-different-types-of-filters"></a><span data-ttu-id="1bc94-138">さまざまな種類のフィルター</span><span class="sxs-lookup"><span data-stu-id="1bc94-138">The Different Types of Filters</span></span>

<span data-ttu-id="1bc94-139">ASP.NET MVC フレームワークには、次の 4 つの異なる種類のフィルターがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="1bc94-139">The ASP.NET MVC framework supports four different types of filters:</span></span>

1. <span data-ttu-id="1bc94-140">承認フィルター – を実装して、`IAuthorizationFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="1bc94-140">Authorization filters – Implements the `IAuthorizationFilter` attribute.</span></span>
2. <span data-ttu-id="1bc94-141">アクション フィルター – を実装して、`IActionFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="1bc94-141">Action filters – Implements the `IActionFilter` attribute.</span></span>
3. <span data-ttu-id="1bc94-142">結果フィルター-を実装して、`IResultFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="1bc94-142">Result filters – Implements the `IResultFilter` attribute.</span></span>
4. <span data-ttu-id="1bc94-143">例外フィルター – を実装して、`IExceptionFilter`属性。</span><span class="sxs-lookup"><span data-stu-id="1bc94-143">Exception filters – Implements the `IExceptionFilter` attribute.</span></span>

<span data-ttu-id="1bc94-144">フィルターは、上に示した順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-144">Filters are executed in the order listed above.</span></span> <span data-ttu-id="1bc94-145">たとえば、承認フィルターは、常にアクション フィルターの前に実行し、例外フィルターはフィルターの他のすべての型の後に常に実行します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-145">For example, authorization filters are always executed before action filters and exception filters are always executed after every other type of filter.</span></span>

<span data-ttu-id="1bc94-146">承認フィルターは、コント ローラー アクションの認証と承認の実装に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-146">Authorization filters are used to implement authentication and authorization for controller actions.</span></span> <span data-ttu-id="1bc94-147">たとえば、承認フィルターは、承認フィルターの例です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-147">For example, the Authorize filter is an example of an Authorization filter.</span></span>

<span data-ttu-id="1bc94-148">アクション フィルターには、コント ローラーのアクションが実行された前後に実行されるロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-148">Action filters contain logic that is executed before and after a controller action executes.</span></span> <span data-ttu-id="1bc94-149">たとえば、アクション フィルターを使用するコント ローラーのアクションを返すビュー データを変更します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-149">You can use an action filter, for instance, to modify the view data that a controller action returns.</span></span>

<span data-ttu-id="1bc94-150">結果フィルターには、ビューの結果が実行された前後に実行されるロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-150">Result filters contain logic that is executed before and after a view result is executed.</span></span> <span data-ttu-id="1bc94-151">ビューの結果を変更するなど、右、ビューがブラウザーに表示される前にします。</span><span class="sxs-lookup"><span data-stu-id="1bc94-151">For example, you might want to modify a view result right before the view is rendered to the browser.</span></span>

<span data-ttu-id="1bc94-152">例外フィルターは、最後に実行するフィルターの種類です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-152">Exception filters are the last type of filter to run.</span></span> <span data-ttu-id="1bc94-153">例外フィルターを使用して、コント ローラーのアクションまたはコント ローラー アクションの結果のいずれかによって発生したエラーを処理することができます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-153">You can use an exception filter to handle errors raised by either your controller actions or controller action results.</span></span> <span data-ttu-id="1bc94-154">また、例外フィルターをエラーをログに使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-154">You also can use exception filters to log errors.</span></span>

<span data-ttu-id="1bc94-155">各フィルターの種類は、特定の順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-155">Each different type of filter is executed in a particular order.</span></span> <span data-ttu-id="1bc94-156">同じ種類のフィルターが実行される順序を制御する場合は、フィルターの順序のプロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-156">If you want to control the order in which filters of the same type are executed then you can set a filter's Order property.</span></span>

<span data-ttu-id="1bc94-157">すべてのアクション フィルターの基本クラスは、`System.Web.Mvc.FilterAttribute`クラスです。</span><span class="sxs-lookup"><span data-stu-id="1bc94-157">The base class for all action filters is the `System.Web.Mvc.FilterAttribute` class.</span></span> <span data-ttu-id="1bc94-158">特定の種類のフィルターを実装する場合は、フィルターの基本クラスから継承し、1 つまたは複数の IAuthorizationFilter、IActionFilter、IResultFilter を実装するクラスを作成する必要があります。 または ExceptionFilter インターフェイスします。</span><span class="sxs-lookup"><span data-stu-id="1bc94-158">If you want to implement a particular type of filter, then you need to create a class that inherits from the base Filter class and implements one or more of the IAuthorizationFilter, IActionFilter, IResultFilter, or ExceptionFilter interfaces.</span></span>

### <a name="the-base-actionfilterattribute-class"></a><span data-ttu-id="1bc94-159">基本 ActionFilterAttribute クラス</span><span class="sxs-lookup"><span data-stu-id="1bc94-159">The Base ActionFilterAttribute Class</span></span>

<span data-ttu-id="1bc94-160">ASP.NET MVC フレームワークにはやすくためにカスタム アクション フィルターを実装するためには、ベースが含まれています`ActionFilterAttribute`クラスです。</span><span class="sxs-lookup"><span data-stu-id="1bc94-160">In order to make it easier for you to implement a custom action filter, the ASP.NET MVC framework includes a base `ActionFilterAttribute` class.</span></span> <span data-ttu-id="1bc94-161">このクラスでは両方とも、`IActionFilter`と`IResultFilter`インターフェイスから継承して、`Filter`クラスです。</span><span class="sxs-lookup"><span data-stu-id="1bc94-161">This class implements both the `IActionFilter` and `IResultFilter` interfaces and inherits from the `Filter` class.</span></span>

<span data-ttu-id="1bc94-162">用語をここでは、完全一致しません。</span><span class="sxs-lookup"><span data-stu-id="1bc94-162">The terminology here is not entirely consistent.</span></span> <span data-ttu-id="1bc94-163">技術的には、ActionFilterAttribute クラスから継承するクラスは、アクション フィルターと結果フィルターの両方です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-163">Technically, a class that inherits from the ActionFilterAttribute class is both an action filter and a result filter.</span></span> <span data-ttu-id="1bc94-164">ただし、緩やかな意味では、word のアクション フィルターは、ASP.NET MVC フレームワーク内のフィルターの任意の型を参照するで使用されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-164">However, in the loose sense, the word action filter is used to refer to any type of filter in the ASP.NET MVC framework.</span></span>

<span data-ttu-id="1bc94-165">基本 ActionFilterAttribute クラスでは、オーバーライド可能な次の方法があります。</span><span class="sxs-lookup"><span data-stu-id="1bc94-165">The base ActionFilterAttribute class has the following methods that you can override:</span></span>

- <span data-ttu-id="1bc94-166">コント ローラーのアクションが実行される前に、OnActionExecuting – ときにこのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-166">OnActionExecuting – This method is called before a controller action is executed.</span></span>
- <span data-ttu-id="1bc94-167">コント ローラーのアクションを実行した後、OnActionExecuted – ときにこのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-167">OnActionExecuted – This method is called after a controller action is executed.</span></span>
- <span data-ttu-id="1bc94-168">コント ローラー アクションの結果が実行される前に、OnResultExecuting – ときにこのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-168">OnResultExecuting – This method is called before a controller action result is executed.</span></span>
- <span data-ttu-id="1bc94-169">コント ローラー アクションの結果が実行された後に、OnResultExecuted – ときにこのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1bc94-169">OnResultExecuted – This method is called after a controller action result is executed.</span></span>

<span data-ttu-id="1bc94-170">次のセクションでは、これらのさまざまなメソッドを実装する方法おを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1bc94-170">In the next section, we'll see how you can implement each of these different methods.</span></span>

### <a name="creating-a-log-action-filter"></a><span data-ttu-id="1bc94-171">ログのアクション フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-171">Creating a Log Action Filter</span></span>

<span data-ttu-id="1bc94-172">カスタム アクション フィルターを構築する方法がわかるようにするために、Visual Studio 出力ウィンドウをコント ローラーのアクションを処理の段階をログに記録するカスタム アクション フィルターを作成します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-172">In order to illustrate how you can build a custom action filter, we'll create a custom action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span> <span data-ttu-id="1bc94-173">当社`LogActionFilter`2 のリストに含まれています。</span><span class="sxs-lookup"><span data-stu-id="1bc94-173">Our `LogActionFilter` is contained in Listing 2.</span></span>

<span data-ttu-id="1bc94-174">**2 – を一覧表示します。 `ActionFilters\LogActionFilter.vb`**</span><span class="sxs-lookup"><span data-stu-id="1bc94-174">**Listing 2 – `ActionFilters\LogActionFilter.vb`**</span></span>

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

<span data-ttu-id="1bc94-175">リストの 2 で、 `OnActionExecuting()`、 `OnActionExecuted()`、 `OnResultExecuting()`、および`OnResultExecuted()`すべてのメソッドを呼び出す、`Log()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1bc94-175">In Listing 2, the `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, and `OnResultExecuted()` methods all call the `Log()` method.</span></span> <span data-ttu-id="1bc94-176">渡される、メソッドの名前、および現在のルート データ、`Log()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="1bc94-176">The name of the method and the current route data is passed to the `Log()` method.</span></span> <span data-ttu-id="1bc94-177">`Log()`メソッドは、Visual Studio の出力ウィンドウにメッセージを書き込みます (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1bc94-177">The `Log()` method writes a message to the Visual Studio Output window (see Figure 2).</span></span>


<span data-ttu-id="1bc94-178">[![Visual Studio の出力 ウィンドウへの書き込み](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1bc94-178">[![Writing to the Visual Studio Output window](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)</span></span>

<span data-ttu-id="1bc94-179">**図 02**: Visual Studio の出力 ウィンドウへの書き込み ([フルサイズのイメージを表示するをクリックして](understanding-action-filters-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1bc94-179">**Figure 02**: Writing to the Visual Studio Output window ([Click to view full-size image](understanding-action-filters-vb/_static/image6.png))</span></span>


<span data-ttu-id="1bc94-180">3 の一覧で、Home コント ローラーは、全体のコント ローラー クラスに、ログのアクション フィルターを適用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="1bc94-180">The Home controller in Listing 3 illustrates how you can apply the Log action filter to an entire controller class.</span></span> <span data-ttu-id="1bc94-181">ときに、Home コント ローラーによって公開される操作のいずれかが呼び出される – か、`Index()`メソッドまたは`About()`メソッド – アクションは、Visual Studio の出力ウィンドウにログ記録処理の段階です。</span><span class="sxs-lookup"><span data-stu-id="1bc94-181">Whenever any of the actions exposed by the Home controller are invoked – either the `Index()` method or the `About()` method – the stages of processing the action are logged to the Visual Studio Output window.</span></span>

<span data-ttu-id="1bc94-182">**3 – を一覧表示します。 `Controllers\HomeController.vb`**</span><span class="sxs-lookup"><span data-stu-id="1bc94-182">**Listing 3 – `Controllers\HomeController.vb`**</span></span>

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a><span data-ttu-id="1bc94-183">まとめ</span><span class="sxs-lookup"><span data-stu-id="1bc94-183">Summary</span></span>

<span data-ttu-id="1bc94-184">このチュートリアルでは、ASP.NET MVC アクション フィルターに導入されました。</span><span class="sxs-lookup"><span data-stu-id="1bc94-184">In this tutorial, you were introduced to ASP.NET MVC action filters.</span></span> <span data-ttu-id="1bc94-185">次の 4 つの異なる種類のフィルターについて説明しました。 承認フィルター、アクション フィルター、結果フィルター、および例外フィルター。</span><span class="sxs-lookup"><span data-stu-id="1bc94-185">You learned about the four different types of filters: authorization filters, action filters, result filters, and exception filters.</span></span> <span data-ttu-id="1bc94-186">基本についても学びました`ActionFilterAttribute`クラスです。</span><span class="sxs-lookup"><span data-stu-id="1bc94-186">You also learned about the base `ActionFilterAttribute` class.</span></span>

<span data-ttu-id="1bc94-187">最後に、簡単なアクション フィルターを実装する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="1bc94-187">Finally, you learned how to implement a simple action filter.</span></span> <span data-ttu-id="1bc94-188">Visual Studio の出力ウィンドウにコント ローラーのアクションを処理の段階をログに記録するログ アクション フィルターを作成しました。</span><span class="sxs-lookup"><span data-stu-id="1bc94-188">We created a Log action filter that logs the stages of processing a controller action to the Visual Studio Output window.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1bc94-189">[前へ](asp-net-mvc-routing-overview-vb.md)
> [次へ](improving-performance-with-output-caching-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1bc94-189">[Previous](asp-net-mvc-routing-overview-vb.md)
[Next](improving-performance-with-output-caching-vb.md)</span></span>
