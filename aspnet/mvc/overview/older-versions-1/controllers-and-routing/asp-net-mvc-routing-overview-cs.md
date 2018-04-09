---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: ASP.NET MVC ルーティングの概要 (c#) |Microsoft ドキュメント
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラーのアクションをブラウザーの要求をマップする方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: fa565d2ef253539844f5224df00bdcdc047bb3f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="4a14f-103">ASP.NET MVC ルーティングの概要 (c#)</span><span class="sxs-lookup"><span data-stu-id="4a14f-103">ASP.NET MVC Routing Overview (C#)</span></span>
====================
<span data-ttu-id="4a14f-104">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4a14f-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4a14f-105">このチュートリアルでは、Stephen Walther は、ASP.NET MVC フレームワークがコント ローラーのアクションをブラウザーの要求をマップする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="4a14f-106">このチュートリアルと呼ばれるすべての ASP.NET MVC アプリケーションの重要な特徴に導入する*ASP.NET ルーティング*です。</span><span class="sxs-lookup"><span data-stu-id="4a14f-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="4a14f-107">ASP.NET ルーティング モジュールは、特定の MVC コント ローラー アクションへのブラウザーの入力方向の要求のマッピングを行います。</span><span class="sxs-lookup"><span data-stu-id="4a14f-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="4a14f-108">このチュートリアルの目的は、標準的なルート テーブルがコント ローラーのアクションに要求をマップする方法を理解します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="4a14f-109">既定のルート テーブルを使用します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-109">Using the Default Route Table</span></span>

<span data-ttu-id="4a14f-110">新しい ASP.NET MVC アプリケーションを作成するときに ASP.NET のルーティングを使用するアプリケーションが既に構成されています。</span><span class="sxs-lookup"><span data-stu-id="4a14f-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="4a14f-111">ASP.NET ルーティングでは、セットアップが 2 つの場所がいます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="4a14f-112">最初に、ASP.NET ルーティングが有効になっているアプリケーションの Web 構成ファイル (Web.config ファイル) にします。</span><span class="sxs-lookup"><span data-stu-id="4a14f-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="4a14f-113">ルーティングに関連する構成ファイル内の 4 つのセクションがある: system.web.httpModules セクション、system.web.httpHandlers セクション、system.webserver.modules セクションおよび system.webserver.handlers セクションです。</span><span class="sxs-lookup"><span data-stu-id="4a14f-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="4a14f-114">これらのセクションではなくルーティングが機能しないために、これらのセクションを削除しないように注意します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="4a14f-115">次に、し、さらに、アプリケーションの Global.asax ファイルで、ルート テーブルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="4a14f-116">Global.asax ファイルは、ASP.NET アプリケーションのライフ サイクル イベントのイベント ハンドラーを含む特殊なファイルです。</span><span class="sxs-lookup"><span data-stu-id="4a14f-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="4a14f-117">アプリケーションの開始イベントの中では、ルート テーブルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="4a14f-118">1 のリスト内のファイルには、ASP.NET MVC アプリケーションの既定の Global.asax ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4a14f-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="4a14f-119">**1 - Global.asax.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4a14f-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="4a14f-120">MVC アプリケーション最初の開始時、アプリケーション\_Start() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="4a14f-121">このメソッドは、さらに、RegisterRoutes() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="4a14f-122">RegisterRoutes() メソッドでは、ルート テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="4a14f-123">既定のルート テーブルには、1 つのルート (Default という名前) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4a14f-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="4a14f-124">既定のルートがコント ローラー名、コント ローラー アクションへの URL は、2 番目のセグメントおよびという名前のパラメーターを 3 番目のセグメントに URL の最初のセグメントをマップ**id**です。</span><span class="sxs-lookup"><span data-stu-id="4a14f-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="4a14f-125">Web ブラウザーのアドレス バーに次の URL を入力することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="4a14f-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="4a14f-126">/Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="4a14f-126">/Home/Index/3</span></span>

<span data-ttu-id="4a14f-127">既定のルートは、この URL を次のパラメーターにマッピングされます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="4a14f-128">コント ローラー ホーム =</span><span class="sxs-lookup"><span data-stu-id="4a14f-128">controller = Home</span></span>

- <span data-ttu-id="4a14f-129">アクション インデックスを =</span><span class="sxs-lookup"><span data-stu-id="4a14f-129">action = Index</span></span>

- <span data-ttu-id="4a14f-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="4a14f-130">id = 3</span></span>

<span data-ttu-id="4a14f-131">URL/Home、インデックス、3 を要求するときに、次のコードは実行されます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="4a14f-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="4a14f-132">HomeController.Index(3)</span></span>

<span data-ttu-id="4a14f-133">既定のルートには、次の 3 つすべてのパラメーターの既定値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4a14f-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="4a14f-134">コント ローラーを指定しない場合、コント ローラー パラメーターの既定値、値**ホーム**です。</span><span class="sxs-lookup"><span data-stu-id="4a14f-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="4a14f-135">Action パラメーターの値に既定値アクションを指定しない場合**インデックス**です。</span><span class="sxs-lookup"><span data-stu-id="4a14f-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="4a14f-136">最後に、id を指定しない場合、id パラメーターの既定値、空の文字列。</span><span class="sxs-lookup"><span data-stu-id="4a14f-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="4a14f-137">既定のルートが Url をコント ローラーのアクションにマップする方法の例をいくつか見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="4a14f-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="4a14f-138">ブラウザーのアドレス バーに次の URL を入力することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="4a14f-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="4a14f-139">/ホーム</span><span class="sxs-lookup"><span data-stu-id="4a14f-139">/Home</span></span>

<span data-ttu-id="4a14f-140">既定ルート パラメーターの既定値のためこの URL を入力すると、Index() メソッドは HomeController クラスのリストの 2 を呼び出せるでします。</span><span class="sxs-lookup"><span data-stu-id="4a14f-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="4a14f-141">**2 - HomeController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4a14f-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="4a14f-142">HomeController クラスには 2 の一覧表示するには id。 をという名前の単一パラメーターを受け入れる Index() をという名前のメソッドが含まれていますURL/Home が、Id パラメーターの値として空の文字列で呼び出される Index() メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4a14f-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="4a14f-143">MVC フレームワークがコント ローラーのアクションを呼び出すことにより、URL/Home とも一致リスト 3 のテンプレートを使用するクラスの Index() メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4a14f-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="4a14f-144">**3 - HomeController.cs (パラメーターなしでインデックス操作) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4a14f-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="4a14f-145">Index() メソッドを一覧表示する 3 では、パラメーターを受け入れません。</span><span class="sxs-lookup"><span data-stu-id="4a14f-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="4a14f-146">URL/Home をこの Index() メソッドが呼び出されるとなります。</span><span class="sxs-lookup"><span data-stu-id="4a14f-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="4a14f-147">また、URL/Home、インデックス、3 は、(Id は無視されます) このメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="4a14f-148">URL/Home では、リスト 4 HomeController クラスの Index() メソッドとも一致します。</span><span class="sxs-lookup"><span data-stu-id="4a14f-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="4a14f-149">**4 - HomeController.cs (null 許容パラメーターを持つインデックスの操作) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4a14f-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="4a14f-150">4 の一覧表示するのには、Index() メソッドは、1 つの整数パラメーターを持ちます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="4a14f-151">パラメーターは (Null 値を保持できます)、null 許容パラメーターでは、エラーを発生させず、Index() を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="4a14f-152">最後に、Id パラメーターから例外が発生した URL/Home を一覧表示する 5 で Index() メソッドを呼び出す*は*null 許容パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4a14f-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="4a14f-153">Index() メソッドを呼び出すしようとすると、エラーが発生した図 1 に表示されます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="4a14f-154">**5 - HomeController.cs (インデックスの Id パラメーターを持つ操作) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="4a14f-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


<span data-ttu-id="4a14f-155">[![パラメーターの値を期待しているコント ローラー アクションの呼び出し](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4a14f-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="4a14f-156">**図 01**: パラメーターの値を期待しているコント ローラー アクションの呼び出し ([フルサイズのイメージを表示するをクリックして](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4a14f-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="4a14f-157">URL/Home/インデックス/3 一方で、正常に動作だけを一覧表示する 5 でインデックス コント ローラーのアクション。</span><span class="sxs-lookup"><span data-stu-id="4a14f-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="4a14f-158">要求/Home/Index/3 が 3 の値を持つ Id パラメーターを指定して呼び出される Index() メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4a14f-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="4a14f-159">まとめ</span><span class="sxs-lookup"><span data-stu-id="4a14f-159">Summary</span></span>

<span data-ttu-id="4a14f-160">このチュートリアルの目的は、ASP.NET ルーティングの概要情報を提供しました。</span><span class="sxs-lookup"><span data-stu-id="4a14f-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="4a14f-161">ここには、新しい ASP.NET MVC アプリケーションで得られる既定のルート テーブルが調べられます。</span><span class="sxs-lookup"><span data-stu-id="4a14f-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="4a14f-162">既定のルートが Url をコント ローラーのアクションにマップする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="4a14f-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4a14f-163">次へ</span><span class="sxs-lookup"><span data-stu-id="4a14f-163">Next</span></span>](understanding-action-filters-cs.md)
