---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: カスタム ルート制約 (VB) を作成する |Microsoft ドキュメント
author: StephenWalther
description: Stephen Walther では、カスタム ルート制約を作成する方法について説明します。 単純な実装により、ルートを防止するカスタムの制約に一致する w.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 094077fa0cb546f4cc91dbf074f8014e62b3b19c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867500"
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="75de1-104">カスタム ルート制約 (VB) を作成します。</span><span class="sxs-lookup"><span data-stu-id="75de1-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="75de1-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="75de1-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="75de1-106">Stephen Walther では、カスタム ルート制約を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="75de1-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="75de1-107">ルートがリモート コンピューターからブラウザーの要求が行われたときに一致することを防止する単純なカスタム制約を実装します。</span><span class="sxs-lookup"><span data-stu-id="75de1-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="75de1-108">このチュートリアルの目的では、カスタム ルート制約を作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="75de1-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="75de1-109">カスタム ルート制約では、ルートがいくつかのカスタム条件が一致しない限りに一致することを防止することができます。</span><span class="sxs-lookup"><span data-stu-id="75de1-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="75de1-110">このチュートリアルでは、Localhost ルート制約を作成します。</span><span class="sxs-lookup"><span data-stu-id="75de1-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="75de1-111">ローカル ホスト ルートの制約には、ローカル コンピューターからの要求のみと一致します。</span><span class="sxs-lookup"><span data-stu-id="75de1-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="75de1-112">インターネットからリモート要求は一致していません。</span><span class="sxs-lookup"><span data-stu-id="75de1-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="75de1-113">カスタム ルート制約を実装するには、IRouteConstraint インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="75de1-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="75de1-114">これは、1 つのメソッドを記述する非常に単純なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="75de1-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="75de1-115">メソッドは、ブール値を返します。</span><span class="sxs-lookup"><span data-stu-id="75de1-115">The method returns a Boolean value.</span></span> <span data-ttu-id="75de1-116">False を返した場合、制約に関連付けられているルートがブラウザーの要求に一致しません。</span><span class="sxs-lookup"><span data-stu-id="75de1-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="75de1-117">Localhost の制約は、1 のリストに含まれます。</span><span class="sxs-lookup"><span data-stu-id="75de1-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="75de1-118">**Listing 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="75de1-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="75de1-119">HttpRequest クラスによって公開される IsLocal プロパティのリスト 1 の制約を活用します。</span><span class="sxs-lookup"><span data-stu-id="75de1-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="75de1-120">このプロパティは、要求の IP アドレスが、127.0.0.1、または要求の ip アドレスがサーバーの IP アドレスと同じ場合に true を返します。</span><span class="sxs-lookup"><span data-stu-id="75de1-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="75de1-121">Global.asax ファイルで定義されているルート内のユーザー設定の制約を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="75de1-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="75de1-122">Global.asax ファイルを一覧表示する 2 では、ローカル サーバーから要求を作成する場合を除き、[管理] ページを要求しないようにすべてのユーザー Localhost 制約を使用します。</span><span class="sxs-lookup"><span data-stu-id="75de1-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="75de1-123">たとえば、リモート サーバーから行われたときに、/Admin/DeleteAll の要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="75de1-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="75de1-124">**2 - Global.asax を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="75de1-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="75de1-125">Localhost の制約は、管理者ルートの定義で使用されます。</span><span class="sxs-lookup"><span data-stu-id="75de1-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="75de1-126">このルートは、リモート ブラウザーの要求で一致しません。</span><span class="sxs-lookup"><span data-stu-id="75de1-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="75de1-127">したがって、Global.asax で定義されている他のルートが同じ要求に一致があります。</span><span class="sxs-lookup"><span data-stu-id="75de1-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="75de1-128">制約により、特定のルートが要求に一致して、いないすべてのルートが Global.asax ファイルで定義されていることを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="75de1-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="75de1-129">既定のルート コメント アウトされています、Global.asax ファイルを一覧表示する 2 からに注意してください。</span><span class="sxs-lookup"><span data-stu-id="75de1-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="75de1-130">既定のルートを含める場合、既定のルートは、管理コント ローラーに対する要求を一致します。</span><span class="sxs-lookup"><span data-stu-id="75de1-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="75de1-131">その場合は、場合でも、その要求が管理者ルートを一致せず、リモート ユーザーは管理コント ローラーのアクションを呼び出すまだでした。</span><span class="sxs-lookup"><span data-stu-id="75de1-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="75de1-132">前へ</span><span class="sxs-lookup"><span data-stu-id="75de1-132">Previous</span></span>](creating-a-route-constraint-vb.md)
