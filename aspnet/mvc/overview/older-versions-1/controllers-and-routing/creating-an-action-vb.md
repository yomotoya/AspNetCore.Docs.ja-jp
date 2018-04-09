---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: アクション (VB) の作成 |Microsoft ドキュメント
author: microsoft
description: ASP.NET MVC のコント ローラーに新しいアクションを追加する方法を説明します。 メソッドをアクションの要件について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-vb"></a><span data-ttu-id="969f6-104">アクション (VB) の作成</span><span class="sxs-lookup"><span data-stu-id="969f6-104">Creating an Action (VB)</span></span>
====================
<span data-ttu-id="969f6-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="969f6-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="969f6-106">ASP.NET MVC のコント ローラーに新しいアクションを追加する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="969f6-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="969f6-107">メソッドをアクションの要件について説明します。</span><span class="sxs-lookup"><span data-stu-id="969f6-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="969f6-108">このチュートリアルの目的では、新しいコント ローラーのアクションを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="969f6-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="969f6-109">アクション メソッドの要件について説明します。</span><span class="sxs-lookup"><span data-stu-id="969f6-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="969f6-110">メソッドがアクションとして公開されないようにする方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="969f6-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="969f6-111">コント ローラーにアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="969f6-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="969f6-112">コント ローラーに新しいアクションを追加するには、コント ローラーに新しいメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="969f6-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="969f6-113">たとえば、コント ローラー 1 の一覧表示するには、Index() をという名前のアクションと SayHello() をという名前のアクションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="969f6-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="969f6-114">どちらの方法は、操作として公開されます。</span><span class="sxs-lookup"><span data-stu-id="969f6-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="969f6-115">**1 - Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="969f6-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="969f6-116">メソッドは、アクションとして universe に公開するために、特定の要件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="969f6-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="969f6-117">メソッドはパブリックである必要があります。</span><span class="sxs-lookup"><span data-stu-id="969f6-117">The method must be public.</span></span>
- <span data-ttu-id="969f6-118">メソッドは、静的メソッドにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="969f6-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="969f6-119">メソッドは、拡張メソッドにすることはできません。</span><span class="sxs-lookup"><span data-stu-id="969f6-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="969f6-120">メソッドは、コンス トラクター、getter、または set アクセス操作子にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="969f6-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="969f6-121">メソッドは、オープン ジェネリック型を持つことはできません。</span><span class="sxs-lookup"><span data-stu-id="969f6-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="969f6-122">メソッドはコント ローラーの基本クラスのメソッドではありません。</span><span class="sxs-lookup"><span data-stu-id="969f6-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="969f6-123">メソッドを含めることはできません**ref**または**アウト**パラメーター。</span><span class="sxs-lookup"><span data-stu-id="969f6-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="969f6-124">コント ローラーのアクションの戻り値の型に関する制限がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="969f6-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="969f6-125">コント ローラーのアクションは、文字列、DateTime のインスタンス Random クラス、または void を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="969f6-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="969f6-126">ASP.NET MVC フレームワークはアクションの結果を文字列にではない戻り値の型に変換し、ブラウザーに文字列を表示します。</span><span class="sxs-lookup"><span data-stu-id="969f6-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="969f6-127">これらの要件をコント ローラーが違反していない任意のメソッドを追加すると、メソッドは、コント ローラーのアクションとして公開されます。</span><span class="sxs-lookup"><span data-stu-id="969f6-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="969f6-128">ここで注意してください。</span><span class="sxs-lookup"><span data-stu-id="969f6-128">Be careful here.</span></span> <span data-ttu-id="969f6-129">コント ローラーのアクションは、インターネットに接続されているすべてのユーザーが呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="969f6-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="969f6-130">、たとえば、作成しないで DeleteMyWebsite() コント ローラーのアクション。</span><span class="sxs-lookup"><span data-stu-id="969f6-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="969f6-131">パブリック メソッドが呼び出されていることを妨げてください。</span><span class="sxs-lookup"><span data-stu-id="969f6-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="969f6-132">かどうかは、コント ローラー クラスにパブリック メソッドを作成する必要があります、およびコント ローラーのアクションとしてメソッドを公開するメソッドを使用して呼び出されているように、 &lt;NonAction&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="969f6-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="969f6-133">たとえば、2 の一覧で、コント ローラーで装飾されて CompanySecrets() をという名前のパブリック メソッドが含まれます。、 &lt;NonAction&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="969f6-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="969f6-134">**2 - Controllers\WorkController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="969f6-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="969f6-135">/Work/CompanySecrets をお使いのブラウザーのアドレス バーに入力して CompanySecrets() コント ローラーのアクションを呼び出すしようとする場合は、図 1 に、エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="969f6-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="969f6-136">[![NonAction メソッドを呼び出す](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="969f6-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="969f6-137">**図 01**: NonAction メソッドを呼び出す ([フルサイズのイメージを表示するをクリックして](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="969f6-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="969f6-138">[前へ](creating-a-controller-vb.md)
> [次へ](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="969f6-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
