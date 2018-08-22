---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: カスタム ルート制約を作成 (c#) |Microsoft Docs
author: StephenWalther
description: Stephen Walther では、カスタム ルート制約を作成する方法を示します。 単純な実装のルートがされたりすることを防止するカスタムの制約に一致する w.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 0a0b6b706fdb212a745346ffaefc118e85c2a245
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825836"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="953d2-104">カスタム ルート制約 (c#) を作成します。</span><span class="sxs-lookup"><span data-stu-id="953d2-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="953d2-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="953d2-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="953d2-106">Stephen Walther では、カスタム ルート制約を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="953d2-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="953d2-107">ルートがリモート コンピューターからブラウザーの要求が行われたときに一致することを防止する単純なカスタム制約を実装します。</span><span class="sxs-lookup"><span data-stu-id="953d2-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="953d2-108">このチュートリアルの目的は、カスタム ルート制約を作成する方法を示すです。</span><span class="sxs-lookup"><span data-stu-id="953d2-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="953d2-109">カスタム ルート制約では、ルートがいくつかのカスタム条件が一致しない限り、一致することを防止することができます。</span><span class="sxs-lookup"><span data-stu-id="953d2-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="953d2-110">このチュートリアルでは、Localhost のルート制約を作成します。</span><span class="sxs-lookup"><span data-stu-id="953d2-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="953d2-111">Localhost のルート制約では、ローカル コンピューターからの要求のみと一致します。</span><span class="sxs-lookup"><span data-stu-id="953d2-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="953d2-112">インターネット経由でのリモート要求は一致していません。</span><span class="sxs-lookup"><span data-stu-id="953d2-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="953d2-113">カスタム ルート制約を実装するには、また、IRouteConstraint インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="953d2-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="953d2-114">これは、1 つのメソッドを記述する非常にシンプルなインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="953d2-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="953d2-115">メソッドは、ブール値を返します。</span><span class="sxs-lookup"><span data-stu-id="953d2-115">The method returns a Boolean value.</span></span> <span data-ttu-id="953d2-116">False を返さない場合、制約に関連付けられているルートは、ブラウザーの要求に一致しません。</span><span class="sxs-lookup"><span data-stu-id="953d2-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="953d2-117">リスト 1 で、Localhost の制約が含まれています。</span><span class="sxs-lookup"><span data-stu-id="953d2-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="953d2-118">**1 - LocalhostConstraint.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="953d2-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="953d2-119">リスト 1 で制約では、HttpRequest クラスによって公開される IsLocal プロパティを利用します。</span><span class="sxs-lookup"><span data-stu-id="953d2-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="953d2-120">このプロパティは、要求の IP アドレスが、127.0.0.1、または要求の ip アドレスがサーバーの IP アドレスと同じ場合に true を返します。</span><span class="sxs-lookup"><span data-stu-id="953d2-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="953d2-121">Global.asax ファイルで定義されているルート内のカスタムの制約を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="953d2-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="953d2-122">リスト 2 で Global.asax ファイルでは、Localhost の制約を使用して、ローカル サーバーから要求を行った場合を除き、[管理] ページを要求できないようにします。</span><span class="sxs-lookup"><span data-stu-id="953d2-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="953d2-123">たとえば、リモート サーバーから行われたときに、/Admin/DeleteAll の要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="953d2-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="953d2-124">**2 - Global.asax を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="953d2-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="953d2-125">Localhost の制約は、管理者のルートの定義に使用されます。</span><span class="sxs-lookup"><span data-stu-id="953d2-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="953d2-126">このルートは、リモートのブラウザー要求によってとも一致しません。</span><span class="sxs-lookup"><span data-stu-id="953d2-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="953d2-127">ただし、Global.asax で定義されている他のルートが同じ要求を一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="953d2-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="953d2-128">制約は、要求を照合から特定のルートを防止し、いないすべてのルートが Global.asax ファイルで定義されていることを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="953d2-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="953d2-129">既定のルート コメント アウトされているリスト 2 で、Global.asax ファイルからに注意してください。</span><span class="sxs-lookup"><span data-stu-id="953d2-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="953d2-130">既定のルートを含める場合、既定のルートは、管理コントローラの要求と一致は。</span><span class="sxs-lookup"><span data-stu-id="953d2-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="953d2-131">その場合は、管理者ルートは一致しません、要求でも、リモート ユーザーは管理者コント ローラーのアクションを呼び出しても場合があります。</span><span class="sxs-lookup"><span data-stu-id="953d2-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="953d2-132">[前へ](creating-a-route-constraint-cs.md)
> [次へ](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="953d2-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
