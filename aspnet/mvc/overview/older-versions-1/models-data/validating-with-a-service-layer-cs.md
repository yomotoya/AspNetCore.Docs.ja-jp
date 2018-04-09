---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: サービス層 (c#) |Microsoft ドキュメント
author: StephenWalther
description: コント ローラーのアクションから、別のサービス層には、検証ロジックを移動する方法を説明します。 Stephen Walther このチュートリアルで説明する方法をしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 06042ac197cc54da767a94a44c57eb09bb3db9fa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="925df-104">サービス層 (c#)</span><span class="sxs-lookup"><span data-stu-id="925df-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="925df-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="925df-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="925df-106">コント ローラーのアクションから、別のサービス層には、検証ロジックを移動する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="925df-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="925df-107">このチュートリアルでは、Stephen Walther は、コント ローラーの層から、サービス層を分離することによって、関心のシャープな分離を維持する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="925df-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="925df-108">このチュートリアルの目的は、ASP.NET MVC アプリケーションで検証を実行する 1 つの方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="925df-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="925df-109">このチュートリアルでは、コント ローラーから、別のサービス層には、検証ロジックを移動する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="925df-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="925df-110">上の問題を分離します。</span><span class="sxs-lookup"><span data-stu-id="925df-110">Separating Concerns</span></span>

<span data-ttu-id="925df-111">ASP.NET MVC アプリケーションをビルドするときに、コント ローラーのアクションの内部データベース ロジックを配置する必要がありますできません。</span><span class="sxs-lookup"><span data-stu-id="925df-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="925df-112">データベースとコント ローラーのロジックを混在させるとが、アプリケーションの時間の経過と共に維持するために困難になります。</span><span class="sxs-lookup"><span data-stu-id="925df-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="925df-113">すべてのデータベース ロジックを別のリポジトリのレイヤーに配置することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="925df-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="925df-114">たとえば、1 一覧表示するにはには、ProductRepository をという名前の単純なリポジトリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="925df-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="925df-115">製品のリポジトリには、すべてのアプリケーションのデータ アクセス コードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="925df-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="925df-116">一覧には、製品のリポジトリを実装する IProductRepository インターフェイスも含まれています。</span><span class="sxs-lookup"><span data-stu-id="925df-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="925df-117">**1--Models\ProductRepository.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="925df-118">リスト 2 でコント ローラーは、その Index() および Create() 操作の両方のリポジトリ レイヤーを使用します。</span><span class="sxs-lookup"><span data-stu-id="925df-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="925df-119">このコント ローラーに、データベース、任意のロジックが含まれていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="925df-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="925df-120">リポジトリのレイヤーを作成するには、上の問題を明確に分離を維持することができます。</span><span class="sxs-lookup"><span data-stu-id="925df-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="925df-121">コント ローラーは、アプリケーションのフロー制御ロジックとリポジトリはデータ アクセス ロジックを担当します。</span><span class="sxs-lookup"><span data-stu-id="925df-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="925df-122">**2 - Controllers\ProductController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="925df-123">サービス レイヤーを作成します。</span><span class="sxs-lookup"><span data-stu-id="925df-123">Creating a Service Layer</span></span>

<span data-ttu-id="925df-124">そのため、アプリケーションのフロー制御ロジック、コント ローラーに属しリポジトリにデータ アクセス ロジックが属しています。</span><span class="sxs-lookup"><span data-stu-id="925df-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="925df-125">その場合は、操作を配置する、検証ロジックしますか。</span><span class="sxs-lookup"><span data-stu-id="925df-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="925df-126">1 つのオプションは、検証ロジックを配置する、*サービス層*です。</span><span class="sxs-lookup"><span data-stu-id="925df-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="925df-127">サービス層は、ASP.NET MVC アプリケーション コント ローラーおよびリポジトリ レイヤー間の通信を仲介するレイヤーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="925df-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="925df-128">サービス層には、ビジネス ロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="925df-128">The service layer contains business logic.</span></span> <span data-ttu-id="925df-129">具体的には、検証ロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="925df-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="925df-130">たとえば、3 の一覧で製品のサービス層では、CreateProduct() メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="925df-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="925df-131">CreateProduct() メソッドは、製品を製品リポジトリに渡す前に、新しい製品を検証する ValidateProduct() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="925df-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="925df-132">**3 - Models\ProductService.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="925df-133">リポジトリ レイヤーではなく、サービス レイヤーを使用するコード サンプル 4 の製品のコント ローラーが更新されました。</span><span class="sxs-lookup"><span data-stu-id="925df-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="925df-134">コント ローラー レイヤーは、サービス層を説明します。</span><span class="sxs-lookup"><span data-stu-id="925df-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="925df-135">サービス レイヤーは、リポジトリのレイヤーに説明します。</span><span class="sxs-lookup"><span data-stu-id="925df-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="925df-136">各レイヤーは、個別の責任を持ちます。</span><span class="sxs-lookup"><span data-stu-id="925df-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="925df-137">**4 - Controllers\ProductController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="925df-138">製品のサービスが、product コント ローラーのコンス トラクターで作成されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="925df-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="925df-139">製品のサービスが作成されると、モデル状態ディクショナリは、サービスに渡されます。</span><span class="sxs-lookup"><span data-stu-id="925df-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="925df-140">製品のサービスでは、モデルの状態を使用して、コント ローラーに検証エラー メッセージを渡します。</span><span class="sxs-lookup"><span data-stu-id="925df-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="925df-141">サービス層を分離</span><span class="sxs-lookup"><span data-stu-id="925df-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="925df-142">私たちは、コント ローラーと 1 つの点では、サービス層に分離に失敗しました。</span><span class="sxs-lookup"><span data-stu-id="925df-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="925df-143">コント ローラーとサービス層は、モデルの状態を通信します。</span><span class="sxs-lookup"><span data-stu-id="925df-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="925df-144">つまり、サービス層、ASP.NET MVC フレームワークの特定の機能に依存しています。</span><span class="sxs-lookup"><span data-stu-id="925df-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="925df-145">可能な限り、コント ローラー層から、サービス レイヤーを分離することができます。</span><span class="sxs-lookup"><span data-stu-id="925df-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="925df-146">理論上は、どのタイプのアプリケーションと ASP.NET MVC アプリケーションだけでなく、サービス レイヤーを使用することもあります。</span><span class="sxs-lookup"><span data-stu-id="925df-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="925df-147">たとえば、今後、可能性があること、WPF アプリケーションのフロント エンドをビルドできます。</span><span class="sxs-lookup"><span data-stu-id="925df-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="925df-148">サービス層からお語る ASP.NET MVC 依存関係を削除する方法モデルの状態を検索する必要があります。</span><span class="sxs-lookup"><span data-stu-id="925df-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="925df-149">リストの 5 では、サービス層が更新されましたモデルの状態が使用されないようにします。</span><span class="sxs-lookup"><span data-stu-id="925df-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="925df-150">代わりに、IValidationDictionary インターフェイスを実装する任意のクラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="925df-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="925df-151">**5 - Models\ProductService.cs (分離) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="925df-152">IValidationDictionary インターフェイスは、一覧表示する 6 で定義されます。</span><span class="sxs-lookup"><span data-stu-id="925df-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="925df-153">この単純なインターフェイスは、1 つのメソッドとプロパティを 1 つがします。</span><span class="sxs-lookup"><span data-stu-id="925df-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="925df-154">**6 - Models\IValidationDictionary.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="925df-155">名前付き ModelStateWrapper クラスを一覧表示する 7 クラスは、IValidationDictionary インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="925df-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="925df-156">モデル状態ディクショナリをコンス トラクターに渡すことによって、ModelStateWrapper クラスをインスタンス化することができます。</span><span class="sxs-lookup"><span data-stu-id="925df-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="925df-157">**7 - Models\ModelStateWrapper.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="925df-158">最後に、リスト 8 に更新されたコント ローラーは、そのコンス トラクターで、サービス レイヤーを作成するときに、ModelStateWrapper を使用します。</span><span class="sxs-lookup"><span data-stu-id="925df-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="925df-159">**8 - Controllers\ProductController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="925df-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="925df-160">IValidationDictionary を使用してインターフェイスを ModelStateWrapper クラスことにより、コント ローラーの層から、サービス層を完全に分離します。</span><span class="sxs-lookup"><span data-stu-id="925df-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="925df-161">サービス層がモデル状態に依存するではなくなりました。</span><span class="sxs-lookup"><span data-stu-id="925df-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="925df-162">サービス層に IValidationDictionary インターフェイスを実装する任意のクラスを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="925df-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="925df-163">たとえば、WPF アプリケーションは、単純なコレクション クラスを使用 IValidationDictionary インターフェイスを実装する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="925df-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="925df-164">まとめ</span><span class="sxs-lookup"><span data-stu-id="925df-164">Summary</span></span>

<span data-ttu-id="925df-165">このチュートリアルの目的は、ASP.NET MVC アプリケーションで検証を実行するための 1 つの方法を説明することでした。</span><span class="sxs-lookup"><span data-stu-id="925df-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="925df-166">このチュートリアルでは、すべてのコント ローラーから、別のサービス層に、検証ロジックを移動する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="925df-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="925df-167">ModelStateWrapper クラスを作成することで、コント ローラーの層から、サービス層を分離する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="925df-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="925df-168">[前へ](validating-with-the-idataerrorinfo-interface-cs.md)
> [次へ](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="925df-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
