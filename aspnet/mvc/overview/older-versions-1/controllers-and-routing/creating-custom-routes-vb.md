---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: カスタム ルート (VB) を作成する |Microsoft Docs
author: microsoft
description: ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。 このチュートリアルでは、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: cd7ad46161b3f44d915bc4b2fa201606b00d194a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835245"
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="04698-104">カスタム ルート (VB) を作成します。</span><span class="sxs-lookup"><span data-stu-id="04698-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="04698-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="04698-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="04698-106">ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="04698-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="04698-107">このチュートリアルでは、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="04698-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="04698-108">このチュートリアルでは、ASP.NET MVC アプリケーションにカスタム ルートを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="04698-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="04698-109">カスタム ルートを使用して、Global.asax ファイルの既定のルート テーブルを変更する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="04698-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="04698-110">ASP.NET MVC アプリケーションでは既定のルーティング テーブルが正しく機能します。</span><span class="sxs-lookup"><span data-stu-id="04698-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="04698-111">ただし、ルーティングのニーズが特殊化されたことを見つけることがあります。</span><span class="sxs-lookup"><span data-stu-id="04698-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="04698-112">その場合は、カスタム ルートを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="04698-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="04698-113">たとえば、ブログ アプリケーションを作成することは想像してください。</span><span class="sxs-lookup"><span data-stu-id="04698-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="04698-114">次のように受信要求を処理する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="04698-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="04698-115">/アーカイブ/12-25-2009 年</span><span class="sxs-lookup"><span data-stu-id="04698-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="04698-116">日付に対応するブログ エントリを取得するユーザーには、この要求が入ると、2009 年 12 月 25 日。</span><span class="sxs-lookup"><span data-stu-id="04698-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="04698-117">この種の要求を処理するためには、カスタム ルートを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="04698-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="04698-118">リスト 1 で、Global.asax ファイルには、ブログ、/Archive/のような要求を処理するという名前の新しいカスタム ルートが含まれています。*エントリの日付*します。</span><span class="sxs-lookup"><span data-stu-id="04698-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="04698-119">**1 - Global.asax (カスタム ルーティングあり) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="04698-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="04698-120">ルート テーブルに追加するルートの順序が重要です。</span><span class="sxs-lookup"><span data-stu-id="04698-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="04698-121">既存の既定のルートの前に、新しいカスタムのブログのルートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="04698-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="04698-122">順序を反転する場合、既定のルート常に呼び出されるカスタム ルートではなく。</span><span class="sxs-lookup"><span data-stu-id="04698-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="04698-123">カスタムのブログのルートでは、/アーカイブ/で始まるすべての要求と一致します。</span><span class="sxs-lookup"><span data-stu-id="04698-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="04698-124">そのため、次の Url はすべて一致します。</span><span class="sxs-lookup"><span data-stu-id="04698-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="04698-125">/アーカイブ/12-25-2009 年</span><span class="sxs-lookup"><span data-stu-id="04698-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="04698-126">/アーカイブ/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="04698-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="04698-127">/アーカイブ/apple</span><span class="sxs-lookup"><span data-stu-id="04698-127">/Archive/apple</span></span>

<span data-ttu-id="04698-128">カスタム ルートでは、アーカイブをという名前のコント ローラーに、受信要求をマップし、Entry() アクションを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="04698-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="04698-129">Entry() メソッドが呼び出されたときに、エントリの日付が entryDate という名前のパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="04698-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="04698-130">リスト 2 でコント ローラーでは、ブログのカスタム ルートを使用できます。</span><span class="sxs-lookup"><span data-stu-id="04698-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="04698-131">**2 - ArchiveController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="04698-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="04698-132">リスト 2 で Entry() メソッドが DateTime 型のパラメーターを受け入れることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="04698-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="04698-133">MVC フレームワークには、URL からエントリの日付を自動的に DateTime 値に変換します。</span><span class="sxs-lookup"><span data-stu-id="04698-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="04698-134">URL からエントリの日付のパラメーターは、DateTime に変換できない、エラーが発生します (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="04698-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="04698-135">**図 1 - パラメーターの変換からのエラー**</span><span class="sxs-lookup"><span data-stu-id="04698-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="04698-136">[![[新しいプロジェクト] ダイアログ ボックス](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="04698-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="04698-137">**図 01**: パラメーターの変換からのエラー ([フルサイズの画像を表示する をクリックします](creating-custom-routes-vb/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="04698-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="04698-138">まとめ</span><span class="sxs-lookup"><span data-stu-id="04698-138">Summary</span></span>

<span data-ttu-id="04698-139">このチュートリアルの目的は、カスタム ルートを作成する方法を説明することでした。</span><span class="sxs-lookup"><span data-stu-id="04698-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="04698-140">ブログ エントリを表す、Global.asax ファイルのルート テーブルにカスタム ルートを追加する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="04698-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="04698-141">ブログのエントリに対する要求を ArchiveController という名前のコント ローラーと Entry() という名前のコント ローラー アクションにマップする方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="04698-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="04698-142">[前へ](asp-net-mvc-controller-overview-vb.md)
> [次へ](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="04698-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
