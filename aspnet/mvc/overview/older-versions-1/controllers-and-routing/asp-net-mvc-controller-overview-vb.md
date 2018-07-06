---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: ASP.NET MVC コント ローラーの概要 (VB) |Microsoft Docs
author: StephenWalther
description: このチュートリアルで Stephen Walther がわかる ASP.NET MVC コント ローラー。 新しいコント ローラーを作成し、さまざまな種類のアクション res を返す方法を学習します.
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ac5e9242f494b8472e582bc76a6f4805db2f770f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809224"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="ab552-104">ASP.NET MVC コント ローラーの概要 (VB)</span><span class="sxs-lookup"><span data-stu-id="ab552-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="ab552-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ab552-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ab552-106">このチュートリアルで Stephen Walther がわかる ASP.NET MVC コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="ab552-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="ab552-107">新しいコント ローラーを作成し、さまざまな種類のアクションの結果を返す方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="ab552-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="ab552-108">このチュートリアルでは、ASP.NET MVC コント ローラー、コント ローラーのアクションおよびアクションの結果のトピックについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ab552-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="ab552-109">このチュートリアルを完了すると、コント ローラーを使用して ASP.NET MVC web サイトを訪問者と対話する方法を制御する方法を理解できます。</span><span class="sxs-lookup"><span data-stu-id="ab552-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="ab552-110">コント ローラーの説明</span><span class="sxs-lookup"><span data-stu-id="ab552-110">Understanding Controllers</span></span>

<span data-ttu-id="ab552-111">MVC コント ローラーは、ASP.NET MVC の web サイトに対して行われた要求に応答します。</span><span class="sxs-lookup"><span data-stu-id="ab552-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="ab552-112">各ブラウザーの要求は、特定のコント ローラーにマップされます。</span><span class="sxs-lookup"><span data-stu-id="ab552-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="ab552-113">たとえば、お使いのブラウザーのアドレス バーに次の URL を入力してください。</span><span class="sxs-lookup"><span data-stu-id="ab552-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="ab552-114">この場合は、「productcontroller」という名前のコント ローラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ab552-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="ab552-115">「Productcontroller」はブラウザー要求に対する応答を生成しますします。</span><span class="sxs-lookup"><span data-stu-id="ab552-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="ab552-116">たとえば、コント ローラーは、ブラウザーに特定のビューを返す可能性があります。 またはコント ローラーは別のコント ローラーにユーザーをリダイレクトする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ab552-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="ab552-117">1 を一覧表示するには、「productcontroller」という名前のシンプルなコント ローラーにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ab552-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="ab552-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="ab552-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="ab552-119">リスト 1 からわかるように、コント ローラー、クラス (Visual Basic .NET または c# のクラス) だけです。</span><span class="sxs-lookup"><span data-stu-id="ab552-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="ab552-120">コント ローラーは、基本の System.Web.Mvc.Controller クラスから派生したクラスです。</span><span class="sxs-lookup"><span data-stu-id="ab552-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="ab552-121">コント ローラーが無料でいくつかの便利なメソッドを継承するコント ローラーは、この基本クラスから継承、するため (これらのメソッドで説明する時点)。</span><span class="sxs-lookup"><span data-stu-id="ab552-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="ab552-122">コント ローラー アクションを理解します。</span><span class="sxs-lookup"><span data-stu-id="ab552-122">Understanding Controller Actions</span></span>

<span data-ttu-id="ab552-123">コント ローラーは、コント ローラー アクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="ab552-123">A controller exposes controller actions.</span></span> <span data-ttu-id="ab552-124">アクションは、ブラウザーのアドレス バーで、特定の URL を入力するときに呼び出されるコント ローラーのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="ab552-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="ab552-125">たとえば、次の URL の要求を行うことを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="ab552-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="ab552-126">この場合、Index() メソッドは、「productcontroller」クラスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ab552-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="ab552-127">Index() メソッドは、コント ローラー アクションの例を示します。</span><span class="sxs-lookup"><span data-stu-id="ab552-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="ab552-128">コント ローラーのアクションは、コント ローラー クラスのパブリック メソッドである必要があります。</span><span class="sxs-lookup"><span data-stu-id="ab552-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="ab552-129">Visual Basic.NET メソッドは、既定では、パブリック メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ab552-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="ab552-130">コント ローラー クラスに追加したすべてのパブリック メソッドがコント ローラーのアクションとして自動的に公開されることを実現 (する必要がありますこれについて注意が必要ですので、コント ローラーのアクションは、ブラウザーのアドレス バーに適切な URL を入力するだけで、世界中のだれでも呼び出すことができます)。</span><span class="sxs-lookup"><span data-stu-id="ab552-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="ab552-131">コント ローラーのアクションで満たす必要があるいくつかの追加要件があります。</span><span class="sxs-lookup"><span data-stu-id="ab552-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="ab552-132">コント ローラーのアクションとして使用されるメソッドをオーバー ロードできません。</span><span class="sxs-lookup"><span data-stu-id="ab552-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="ab552-133">さらに、コント ローラーのアクションは、静的メソッドをすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ab552-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="ab552-134">それ以外には、コント ローラーのアクションとしてほとんどすべてのメソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ab552-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="ab552-135">アクションの結果を理解します。</span><span class="sxs-lookup"><span data-stu-id="ab552-135">Understanding Action Results</span></span>

<span data-ttu-id="ab552-136">コント ローラーのアクションを返しますと呼ばれるものを*アクション結果*します。</span><span class="sxs-lookup"><span data-stu-id="ab552-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="ab552-137">アクションの結果は、コント ローラーのアクションはブラウザーの要求に応答を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="ab552-138">ASP.NET MVC フレームワークでは、アクションの結果などのいくつかの種類をサポートします。</span><span class="sxs-lookup"><span data-stu-id="ab552-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="ab552-139">ViewResult - 表します HTML とマークアップ。</span><span class="sxs-lookup"><span data-stu-id="ab552-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="ab552-140">EmptyResult のない結果を表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="ab552-141">RedirectResult - は、新しい URL へのリダイレクトを表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="ab552-142">JsonResult - は、AJAX アプリケーションで使用できる JavaScript Object Notation 結果を表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="ab552-143">JavaScriptResult - JavaScript のスクリプトを表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="ab552-144">ContentResult - は、テキストの結果を表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="ab552-145">FileContentResult - は、ダウンロード可能な (バイナリ コンテンツ) を含むファイルを表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="ab552-146">FilePathResult - は、ダウンロード可能な (パス) を使用してファイルを表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="ab552-147">FileStreamResult - は、(ファイル ストリーム) をダウンロード可能なファイルを表します。</span><span class="sxs-lookup"><span data-stu-id="ab552-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="ab552-148">これらのアクション結果のすべては、基本の ActionResult クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="ab552-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="ab552-149">ほとんどの場合は、コント ローラーのアクションは、ViewResult を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="ab552-150">たとえば、リスト 2 でインデックスのコント ローラー アクションは、ViewResult を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="ab552-151">**2 - Controllers\BookController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="ab552-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="ab552-152">アクション、ViewResult が戻るとき、ブラウザーに HTML が返されます。</span><span class="sxs-lookup"><span data-stu-id="ab552-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="ab552-153">リスト 2 で Index() メソッドは、ブラウザーにインデックスをという名前のビューを返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="ab552-154">リスト 2 で Index() アクションが、ViewResult() を返さないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ab552-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="ab552-155">代わりに、コント ローラーの基本クラスの View() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ab552-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="ab552-156">通常、アクションの結果を直接返すはされません。</span><span class="sxs-lookup"><span data-stu-id="ab552-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="ab552-157">代わりに、コント ローラーの基本クラスの次のメソッドのいずれかを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ab552-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="ab552-158">表示 - ViewResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="ab552-159">リダイレクト - RedirectResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="ab552-160">RedirectToAction - RedirectToRouteResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="ab552-161">RedirectToRoute - RedirectToRouteResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="ab552-162">Json - JsonResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="ab552-163">JavaScriptResult - は、JavaScriptResult を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="ab552-164">コンテンツ - ContentResult アクションの結果を返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="ab552-165">ファイル - FileContentResult、FilePathResult、またはパラメーターに応じて FileStreamResult がメソッドに渡されるを返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="ab552-166">そのため、ブラウザーにビューを返す場合には、View() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ab552-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="ab552-167">1 つのコント ローラー アクションからユーザーをリダイレクトする場合は、RedirectToAction() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ab552-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="ab552-168">たとえば、リスト 3 の Details() アクション ビューを表示します。 またはユーザー Id パラメーターが値を持つかどうかに応じて Index() アクションにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ab552-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="ab552-169">**3 - CustomerController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="ab552-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="ab552-170">ContentResult アクションの結果は特殊です。</span><span class="sxs-lookup"><span data-stu-id="ab552-170">The ContentResult action result is special.</span></span> <span data-ttu-id="ab552-171">ContentResult アクションの結果を使用して、プレーン テキストとしてアクションの結果を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="ab552-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="ab552-172">たとえば、リスト 4 Index() メソッドは、HTML ではなく、プレーン テキストとして、メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="ab552-173">**4 - Controllers\StatusController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="ab552-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="ab552-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="ab552-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="ab552-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="ab552-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="ab552-176">StatusController.Index() アクションが呼び出されたときに、ビューは返されません。</span><span class="sxs-lookup"><span data-stu-id="ab552-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="ab552-177">代わりに、未加工のテキスト"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="ab552-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="ab552-178">ブラウザーに返されます。</span><span class="sxs-lookup"><span data-stu-id="ab552-178">is returned to the browser.</span></span>

<span data-ttu-id="ab552-179">コント ローラーのアクションは、日付や整数 - の結果などのアクションの結果のないであるを返す場合は、結果が自動的にラップされた ContentResult でし。</span><span class="sxs-lookup"><span data-stu-id="ab552-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="ab552-180">たとえば、リスト 5 WorkController の Index() アクションが呼び出されたときに、日付が返されます ContentResult として自動的に。</span><span class="sxs-lookup"><span data-stu-id="ab552-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="ab552-181">**5 - WorkController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="ab552-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="ab552-182">リスト 5 Index() アクションは、DateTime オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="ab552-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="ab552-183">ASP.NET MVC フレームワークでは、DateTime オブジェクトを文字列に変換し、ContentResult で DateTime 値を自動的にラップします。</span><span class="sxs-lookup"><span data-stu-id="ab552-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="ab552-184">ブラウザーは、日付と時刻をプレーン テキストとして受信します。</span><span class="sxs-lookup"><span data-stu-id="ab552-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="ab552-185">まとめ</span><span class="sxs-lookup"><span data-stu-id="ab552-185">Summary</span></span>

<span data-ttu-id="ab552-186">このチュートリアルの目的は、ASP.NET MVC コント ローラー、コント ローラー アクション、コント ローラー アクションの結果の概念を紹介しました。</span><span class="sxs-lookup"><span data-stu-id="ab552-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="ab552-187">最初のセクションでは、ASP.NET MVC プロジェクトに新しいコント ローラーを追加する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="ab552-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="ab552-188">次に、コント ローラーのメソッドを公開する方法を学習しました。 コント ローラー アクションとして、世界に公開されます。</span><span class="sxs-lookup"><span data-stu-id="ab552-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="ab552-189">最後に、コント ローラー アクションから返されるアクションの結果の種類を説明します。</span><span class="sxs-lookup"><span data-stu-id="ab552-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="ab552-190">具体的には、コント ローラー アクションから ViewResult、RedirectToActionResult、および ContentResult を返す方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="ab552-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ab552-191">[前へ](creating-a-custom-route-constraint-cs.md)
> [次へ](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ab552-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
