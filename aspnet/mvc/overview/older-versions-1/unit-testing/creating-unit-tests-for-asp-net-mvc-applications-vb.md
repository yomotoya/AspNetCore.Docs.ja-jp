---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: ASP.NET MVC アプリケーション (VB) の単体テストの作成 |Microsoft ドキュメント
author: StephenWalther
description: コント ローラー アクションの単体テストを作成する方法を説明します。 このチュートリアルでは、Stephen Walther はコント ローラーのアクションが、parti を返すかどうかをテストする方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869674"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="06444-104">ASP.NET MVC アプリケーション (VB) の単体テストの作成</span><span class="sxs-lookup"><span data-stu-id="06444-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="06444-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="06444-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="06444-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="06444-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="06444-107">コント ローラー アクションの単体テストを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="06444-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="06444-108">このチュートリアルでは、Stephen Walther はコント ローラーのアクションの特定のビューを返します、特定のデータのセットを返しますまたは別の種類のアクションの結果を返すかどうかをテストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="06444-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="06444-109">このチュートリアルの目標を記述する方法、コント ローラーの単体テスト、ASP.NET MVC でのアプリケーションを示すことです。</span><span class="sxs-lookup"><span data-stu-id="06444-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="06444-110">次の 3 つの異なる種類の単体テストを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="06444-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="06444-111">コント ローラーのアクションによって返されるビューをテストする方法、コント ローラーのアクションによって返されるデータの表示をテストする方法、および 1 つのコント ローラー アクションが 2 番目のコント ローラー アクションにリダイレクトするかどうかをテストする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="06444-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="06444-112">テスト コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="06444-112">Creating the Controller under Test</span></span>

<span data-ttu-id="06444-113">テストしようとするコント ローラーの作成を始めます。</span><span class="sxs-lookup"><span data-stu-id="06444-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="06444-114">という名前のコント ローラー、 `ProductController`1 のリストに含まれています。</span><span class="sxs-lookup"><span data-stu-id="06444-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="06444-115">**1 – を一覧表示します。 `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="06444-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="06444-116">`ProductController`という 2 つのアクション メソッドを含む`Index()`と`Details()`です。</span><span class="sxs-lookup"><span data-stu-id="06444-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="06444-117">どちらのアクション メソッドは、ビューを返します。</span><span class="sxs-lookup"><span data-stu-id="06444-117">Both action methods return a view.</span></span> <span data-ttu-id="06444-118">注意して、`Details()`操作 id をという名前のパラメーターを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="06444-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="06444-119">ビューをテスト コント ローラーによって返される</span><span class="sxs-lookup"><span data-stu-id="06444-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="06444-120">テストすることを想像してくださいかどうか、`ProductController`右側のビューを返します。</span><span class="sxs-lookup"><span data-stu-id="06444-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="06444-121">確認するときに、`ProductController.Details()`アクションが呼び出されると、詳細ビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="06444-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="06444-122">リスト 2 でテスト クラスには、テストによって返されるビューの単体テストが含まれています、`ProductController.Details()`アクション。</span><span class="sxs-lookup"><span data-stu-id="06444-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="06444-123">**2 – を一覧表示します。 `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="06444-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="06444-124">2 のリスト内のクラスには、という名前のテスト メソッドが含まれています。`TestDetailsView()`です。</span><span class="sxs-lookup"><span data-stu-id="06444-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="06444-125">このメソッドには、次の 3 つの行コードにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="06444-125">This method contains three lines of code.</span></span> <span data-ttu-id="06444-126">コードの最初の行の新しいインスタンスを作成する、`ProductController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="06444-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="06444-127">コードの 2 行目は、コント ローラーを呼び出す`Details()`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="06444-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="06444-128">ビューがによって返されるかどうか、コードのチェックの最後の行、最後に、`Details()`操作は、詳細ビューです。</span><span class="sxs-lookup"><span data-stu-id="06444-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="06444-129">`ViewResult.ViewName`プロパティは、コント ローラーによって返されるビューの名前を表します。</span><span class="sxs-lookup"><span data-stu-id="06444-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="06444-130">このプロパティのテストの 1 つの大きな警告します。</span><span class="sxs-lookup"><span data-stu-id="06444-130">One big warning about testing this property.</span></span> <span data-ttu-id="06444-131">これには、コント ローラーが、ビューを返すことができます 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="06444-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="06444-132">コント ローラーは、次のようにビューを明示的に返すことができます。</span><span class="sxs-lookup"><span data-stu-id="06444-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="06444-133">また、ビューの名前は、次のようにコント ローラー アクションの名前から推論することができます。</span><span class="sxs-lookup"><span data-stu-id="06444-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="06444-134">という名前のビューを返すコント ローラー アクション`Details`です。</span><span class="sxs-lookup"><span data-stu-id="06444-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="06444-135">ただし、ビューの名前は、アクション名から推測されます。</span><span class="sxs-lookup"><span data-stu-id="06444-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="06444-136">ビューの名前をテストする場合は、コント ローラーのアクションからビューの名前に明示的に返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="06444-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="06444-137">リスト 2 で、単体テストを実行するには、キーボードの組み合わせを入力するか、 **R ctrl キーを押し、A**かをクリックして、**ソリューション内のすべてのテストを実行**(図 1 を参照してください) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="06444-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="06444-138">テストに合格した場合は、図 2 でテスト結果 ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="06444-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="06444-139">[![ソリューション内のすべてのテストを実行します。](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06444-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="06444-140">**図 01**: ソリューション内のすべてのテストの実行 ([フルサイズのイメージを表示するをクリックして](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06444-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="06444-141">[![正常にされました。](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="06444-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="06444-142">**図 02**: 成功!</span><span class="sxs-lookup"><span data-stu-id="06444-142">**Figure 02**: Success!</span></span> <span data-ttu-id="06444-143">([フルサイズのイメージを表示するをクリックして](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="06444-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="06444-144">コント ローラーによって返されるデータの表示のテスト</span><span class="sxs-lookup"><span data-stu-id="06444-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="06444-145">MVC のコント ローラーと呼ばれるものを使用してビューにデータを渡します *`View Data`* です。</span><span class="sxs-lookup"><span data-stu-id="06444-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="06444-146">たとえばを呼び出すときは、特定の製品の詳細を表示すること、`ProductController Details()`アクション。</span><span class="sxs-lookup"><span data-stu-id="06444-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="06444-147">インスタンスを作成する場合は、 `Product` (モデルで定義されている) クラスをインスタンスに渡すと、`Details`活用してビュー`View Data`です。</span><span class="sxs-lookup"><span data-stu-id="06444-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="06444-148">変更された`ProductController`リスト 3 の更新が含まれています`Details()`製品を返すアクションです。</span><span class="sxs-lookup"><span data-stu-id="06444-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="06444-149">**3 – を一覧表示します。 `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="06444-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="06444-150">最初に、`Details()`アクションの新しいインスタンスを作成する、`Product`ラップトップ コンピューターを表すクラスです。</span><span class="sxs-lookup"><span data-stu-id="06444-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="06444-151">次のインスタンス、`Product`クラスが 2 番目のパラメーターとして渡される、`View()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="06444-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="06444-152">単体テストを記述することができます、必要なデータがあるかどうかをテストするデータが含まれて表示されます。</span><span class="sxs-lookup"><span data-stu-id="06444-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="06444-153">単体テストに、4 の一覧表示するテストで呼び出すときに、ラップトップ コンピューターを表す製品が返されるかどうか、`ProductController Details()`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="06444-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="06444-154">**4 – を一覧表示します。 `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="06444-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="06444-155">4 の一覧で、`TestDetailsView()`メソッド呼び出しによって返されるデータの表示のテスト、`Details()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="06444-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="06444-156">`ViewData`にプロパティとして公開される、`ViewResult`呼び出しによって返される、`Details()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="06444-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="06444-157">`ViewData.Model`プロパティにはビューに渡される、製品が含まれています。</span><span class="sxs-lookup"><span data-stu-id="06444-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="06444-158">テストでは、単に名ラップトップがビュー データに含まれている製品であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="06444-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="06444-159">アクションの結果をテスト コント ローラーによって返される</span><span class="sxs-lookup"><span data-stu-id="06444-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="06444-160">複雑なコント ローラーのアクションは、さまざまな種類のコント ローラーのアクションに渡されるパラメーターの値によってアクション結果を返す場合があります。</span><span class="sxs-lookup"><span data-stu-id="06444-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="06444-161">コント ローラーのアクションはさまざまな種類のなどのアクション結果を返すことができます、 `ViewResult`、 `RedirectToRouteResult`、または`JsonResult`です。</span><span class="sxs-lookup"><span data-stu-id="06444-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="06444-162">たとえば、変更された`Details()`5 の一覧でアクションを返します、`Details`アクションに有効な製品 Id を渡す場合に表示します。</span><span class="sxs-lookup"><span data-stu-id="06444-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="06444-163">1--後にリダイレクトするよりも小さく、無効な製品 Id--値を持つ Id を渡す場合、`Index()`アクション。</span><span class="sxs-lookup"><span data-stu-id="06444-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="06444-164">**5 – を一覧表示します。 `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="06444-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="06444-165">動作をテストすることができます、`Details()`リスト 6 で、単体テストで操作します。</span><span class="sxs-lookup"><span data-stu-id="06444-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="06444-166">単体テストを一覧表示する 6 にリダイレクトされていることを確認、`Index`に値が-1 の Id が渡されるときの表示、`Details()`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="06444-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="06444-167">**6 – を一覧表示します。 `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="06444-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="06444-168">呼び出すと、`RedirectToAction()`コント ローラーのアクションで、コント ローラーのアクションを返します、`RedirectToRouteResult`です。</span><span class="sxs-lookup"><span data-stu-id="06444-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="06444-169">テスト チェックするかどうか、`RedirectToRouteResult`という名前のコント ローラー アクションをユーザーをリダイレクト`Index`です。</span><span class="sxs-lookup"><span data-stu-id="06444-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="06444-170">まとめ</span><span class="sxs-lookup"><span data-stu-id="06444-170">Summary</span></span>

<span data-ttu-id="06444-171">このチュートリアルでは、MVC コント ローラー アクションの単体テストを作成する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="06444-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="06444-172">最初に、右側のビューがコント ローラーのアクションによって返されるかどうかを確認する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="06444-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="06444-173">使用する方法を学習しました、`ViewResult.ViewName`プロパティをビューの名前を確認します。</span><span class="sxs-lookup"><span data-stu-id="06444-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="06444-174">内容をテストする方法を確認して次に、`View Data`です。</span><span class="sxs-lookup"><span data-stu-id="06444-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="06444-175">適切な製品が返されたかどうかを確認する方法を学習した`View Data`コント ローラーのアクションの呼び出し後にします。</span><span class="sxs-lookup"><span data-stu-id="06444-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="06444-176">最後に、さまざまな種類のアクションの結果はコント ローラーのアクションから返されるかどうかをテストする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="06444-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="06444-177">コント ローラーを返すかどうかをテストする方法を学習、`ViewResult`または`RedirectToRouteResult`です。</span><span class="sxs-lookup"><span data-stu-id="06444-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="06444-178">前へ</span><span class="sxs-lookup"><span data-stu-id="06444-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
