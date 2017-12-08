---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: "ASP.NET MVC コント ローラーの概要 (VB) |Microsoft ドキュメント"
author: StephenWalther
description: "このチュートリアルでは Stephen Walther 紹介 ASP.NET MVC のコント ローラーにします。 新しいコント ローラーを作成してさまざまな種類のアクション res を返す方法を説明したとしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 452e2cf771e8b1f298ce9693ec2a707e7c1d4dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="0270d-104">ASP.NET MVC コント ローラーの概要 (VB)</span><span class="sxs-lookup"><span data-stu-id="0270d-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="0270d-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="0270d-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="0270d-106">このチュートリアルでは Stephen Walther 紹介 ASP.NET MVC のコント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="0270d-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="0270d-107">新しいコント ローラーを作成してさまざまな種類のアクションの結果を返す方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="0270d-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="0270d-108">このチュートリアルでは、ASP.NET MVC コント ローラー、コント ローラーのアクションおよびアクションの結果のトピックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0270d-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="0270d-109">このチュートリアルを完了すると、コント ローラーを使用して、訪問者が ASP.NET MVC の web サイトと対話する方法を制御する方法を理解できます。</span><span class="sxs-lookup"><span data-stu-id="0270d-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="0270d-110">コント ローラーの説明</span><span class="sxs-lookup"><span data-stu-id="0270d-110">Understanding Controllers</span></span>

<span data-ttu-id="0270d-111">MVC コント ローラーは、ASP.NET MVC の web サイトに対して行われた要求に応答して行います。</span><span class="sxs-lookup"><span data-stu-id="0270d-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="0270d-112">各ブラウザーの要求は、特定のコント ローラーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="0270d-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="0270d-113">たとえば、お使いのブラウザーのアドレス バーに次の URL を入力してください。</span><span class="sxs-lookup"><span data-stu-id="0270d-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="0270d-114">この場合、ProductController をという名前のコント ローラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0270d-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="0270d-115">ProductController をブラウザーの要求に対する応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="0270d-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="0270d-116">たとえば、コント ローラーは、ブラウザーに特定のビューを返す可能性があります。 またはコント ローラーは別のコント ローラーにユーザーをリダイレクトする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0270d-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="0270d-117">1 を一覧表示するには、ProductController をという名前の単純なコント ローラーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0270d-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="0270d-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="0270d-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="0270d-119">一覧表示する 1 からわかるように、コント ローラー、クラス (Visual Basic .NET や c# のクラス) だけです。</span><span class="sxs-lookup"><span data-stu-id="0270d-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="0270d-120">コント ローラーは、基本 System.Web.Mvc.Controller クラスから派生したクラスです。</span><span class="sxs-lookup"><span data-stu-id="0270d-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="0270d-121">コント ローラーは、この基本クラスから継承、ためにと、コント ローラーが無料にいくつかの便利なメソッドを継承 (触れこれらのメソッドはすぐに)。</span><span class="sxs-lookup"><span data-stu-id="0270d-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="0270d-122">コント ローラーのアクションを理解します。</span><span class="sxs-lookup"><span data-stu-id="0270d-122">Understanding Controller Actions</span></span>

<span data-ttu-id="0270d-123">コント ローラーは、コント ローラーのアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="0270d-123">A controller exposes controller actions.</span></span> <span data-ttu-id="0270d-124">アクションは、ブラウザーのアドレス バーで、特定の URL を入力するときに呼び出されるコント ローラーのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="0270d-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="0270d-125">たとえば、次の URL に対する要求を行うとします。</span><span class="sxs-lookup"><span data-stu-id="0270d-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="0270d-126">この場合、Index() メソッドは、ProductController クラスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0270d-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="0270d-127">Index() メソッドは、コント ローラーのアクションの例を示します。</span><span class="sxs-lookup"><span data-stu-id="0270d-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="0270d-128">コント ローラーのアクションは、コント ローラー クラスのパブリック メソッドである必要があります。</span><span class="sxs-lookup"><span data-stu-id="0270d-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="0270d-129">Visual Basic.NET メソッドは、既定では、パブリック メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0270d-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="0270d-130">コント ローラー クラスに追加するすべてのパブリック メソッドがコント ローラーのアクションとして自動的に公開されることに注意して (必要がありますに関する注意してこのコント ローラーのアクションは、右側の URL をブラウザーのアドレス バーに入力するだけで、universe 内の誰でも呼び出すことができますので)。</span><span class="sxs-lookup"><span data-stu-id="0270d-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="0270d-131">コント ローラーのアクションを満たす必要のあるいくつかの追加要件があります。</span><span class="sxs-lookup"><span data-stu-id="0270d-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="0270d-132">コント ローラーのアクションとして使用されるメソッドをオーバー ロードすることはできません。</span><span class="sxs-lookup"><span data-stu-id="0270d-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="0270d-133">さらに、コント ローラーのアクションは、静的メソッドをすることはできません。</span><span class="sxs-lookup"><span data-stu-id="0270d-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="0270d-134">それを除けば、コント ローラーのアクションとしてほとんどすべてのメソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0270d-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="0270d-135">アクションの結果を理解します。</span><span class="sxs-lookup"><span data-stu-id="0270d-135">Understanding Action Results</span></span>

<span data-ttu-id="0270d-136">コント ローラーのアクションを返しますと呼ばれるもの、*アクション結果*です。</span><span class="sxs-lookup"><span data-stu-id="0270d-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="0270d-137">アクションの結果は、コント ローラーのアクションはブラウザーの要求に応答を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="0270d-138">ASP.NET MVC フレームワークには、いくつか含むアクションの結果の種類がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="0270d-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="0270d-139">ViewResult - を表します HTML およびマークアップ。</span><span class="sxs-lookup"><span data-stu-id="0270d-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="0270d-140">EmptyResult のない結果を表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="0270d-141">RedirectResult - は、新しい URL へのリダイレクトを表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="0270d-142">JsonResult - は、AJAX アプリケーションで使用できる JavaScript Object Notation 結果を表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="0270d-143">JavaScriptResult - は、JavaScript のスクリプトを表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="0270d-144">ContentResult - は、テキストの結果を表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="0270d-145">FileContentResult - は、(バイナリ コンテンツ) のダウンロード可能なファイルを表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="0270d-146">FilePathResult - は、(パス) のダウンロード可能なファイルを表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="0270d-147">FileStreamResult - は、(ファイル ストリーム) のダウンロード可能なファイルを表します。</span><span class="sxs-lookup"><span data-stu-id="0270d-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="0270d-148">これらのアクション結果のすべては、基本の ActionResult クラスから継承されます。</span><span class="sxs-lookup"><span data-stu-id="0270d-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="0270d-149">ほとんどの場合は、コント ローラーのアクションは、ViewResult を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="0270d-150">たとえば、インデックス コント ローラーのアクションを一覧表示する 2 では、ViewResult を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="0270d-151">**2 - Controllers\BookController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="0270d-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="0270d-152">操作が戻るとき、ViewResult、ブラウザーに HTML が返されます。</span><span class="sxs-lookup"><span data-stu-id="0270d-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="0270d-153">Index() メソッドを一覧表示する 2 では、ブラウザーにインデックスをという名前のビューを返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="0270d-154">リスト 2 で Index() アクションが、ViewResult() を返さないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0270d-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="0270d-155">代わりに、コント ローラーの基本クラスの View() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0270d-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="0270d-156">通常を返さないアクション結果直接です。</span><span class="sxs-lookup"><span data-stu-id="0270d-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="0270d-157">代わりに、コント ローラーの基本クラスの次のメソッドのいずれかを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0270d-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="0270d-158">ビュー - ViewResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="0270d-159">リダイレクト - RedirectResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="0270d-160">RedirectToAction - RedirectToRouteResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="0270d-161">RedirectToRoute - RedirectToRouteResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="0270d-162">Json では、JsonResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="0270d-163">JavaScriptResult -、JavaScriptResult を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="0270d-164">コンテンツ - ContentResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="0270d-165">ファイルのメソッドに渡される FileContentResult、FilePathResult、またはパラメーターに応じて FileStreamResult を返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="0270d-166">そのため、ブラウザーにビューを返す場合には、View() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0270d-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="0270d-167">1 つのコント ローラーのアクションからユーザーをリダイレクトする場合は、RedirectToAction() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0270d-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="0270d-168">たとえば、Details() 操作リスト 3 のビューが表示されます。 または Id パラメーターの値がかどうかに応じて、Index() アクションにはユーザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0270d-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="0270d-169">**3 - CustomerController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="0270d-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="0270d-170">ContentResult アクションの結果は特殊です。</span><span class="sxs-lookup"><span data-stu-id="0270d-170">The ContentResult action result is special.</span></span> <span data-ttu-id="0270d-171">ContentResult アクションの結果を使用して、プレーン テキストとしてアクションの結果を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="0270d-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="0270d-172">たとえば、リスト 4 Index() メソッドは、プレーン テキストとして、および HTML ではなく、メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="0270d-173">**4 - Controllers\StatusController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="0270d-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="0270d-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="0270d-174">StatusController</span></span>


> <span data-ttu-id="0270d-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="0270d-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="0270d-176">StatusController.Index() アクションが呼び出されたときに、表示は返されません。</span><span class="sxs-lookup"><span data-stu-id="0270d-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="0270d-177">代わりに、生のテキスト"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="0270d-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="0270d-178">ブラウザーに返されます。</span><span class="sxs-lookup"><span data-stu-id="0270d-178">is returned to the browser.</span></span>

<span data-ttu-id="0270d-179">コント ローラーのアクション結果を返します、たとえば、アクションの結果のないである、日付や整数の場合、結果にラップされて、ContentResult 自動的にします。</span><span class="sxs-lookup"><span data-stu-id="0270d-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="0270d-180">たとえば、5 の一覧で WorkController の Index() アクションが呼び出されたときに日付が返されます、ContentResult として自動的に。</span><span class="sxs-lookup"><span data-stu-id="0270d-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="0270d-181">**5 - WorkController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="0270d-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="0270d-182">Index() アクション 5 の一覧表示するのには、DateTime オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="0270d-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="0270d-183">ASP.NET MVC フレームワークでは、DateTime オブジェクトを文字列に変換し、ContentResult で自動的に DateTime 値をラップします。</span><span class="sxs-lookup"><span data-stu-id="0270d-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="0270d-184">ブラウザーは、日付と時刻をプレーン テキストとして受信します。</span><span class="sxs-lookup"><span data-stu-id="0270d-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="0270d-185">概要</span><span class="sxs-lookup"><span data-stu-id="0270d-185">Summary</span></span>

<span data-ttu-id="0270d-186">このチュートリアルの目的は、ASP.NET MVC コント ローラー、コント ローラーのアクション、コント ローラー アクションの結果の概念を紹介することでした。</span><span class="sxs-lookup"><span data-stu-id="0270d-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="0270d-187">最初のセクションでは、ASP.NET MVC プロジェクトを新しいコント ローラーを追加する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="0270d-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="0270d-188">コント ローラーのパブリック メソッドを次に、学習したコント ローラーのアクションとして、universe に公開されます。</span><span class="sxs-lookup"><span data-stu-id="0270d-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="0270d-189">最後に、コント ローラーのアクションから返されるアクションの結果の種類を説明します。</span><span class="sxs-lookup"><span data-stu-id="0270d-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="0270d-190">具体的には、コント ローラーのアクションから ViewResult、RedirectToActionResult、および ContentResult を返す方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="0270d-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0270d-191">[前へ](creating-a-custom-route-constraint-cs.md)
[次へ](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0270d-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
