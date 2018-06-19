---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: カスタム ルート (VB) の作成 |Microsoft ドキュメント
author: microsoft
description: ASP.NET MVC アプリケーションにカスタム ルートを追加する方法を説明します。 このチュートリアルでは、Global.asax ファイルで既定のルート テーブルを変更する方法を学習します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872326"
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="be89d-104">(VB) のカスタム ルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="be89d-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="be89d-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="be89d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="be89d-106">ASP.NET MVC アプリケーションにカスタム ルートを追加する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="be89d-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="be89d-107">このチュートリアルでは、Global.asax ファイルで既定のルート テーブルを変更する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="be89d-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="be89d-108">このチュートリアルでは、ASP.NET MVC アプリケーションにカスタム ルートを追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="be89d-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="be89d-109">カスタム ルートを Global.asax ファイルで既定のルート テーブルを変更する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="be89d-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="be89d-110">ASP.NET MVC アプリケーションでは、既定のルート テーブルが正しく機能します。</span><span class="sxs-lookup"><span data-stu-id="be89d-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="be89d-111">ただし、ルーティングのニーズが専門的なことを検出できます。</span><span class="sxs-lookup"><span data-stu-id="be89d-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="be89d-112">その場合は、カスタム ルートを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="be89d-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="be89d-113">たとえば、ブログ アプリケーションを構築することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="be89d-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="be89d-114">次のように入力方向の要求を処理する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="be89d-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="be89d-115">/アーカイブ/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="be89d-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="be89d-116">日付に対応するブログ エントリを返すユーザーは、この要求が入力、2009 年 12 月 25 日。</span><span class="sxs-lookup"><span data-stu-id="be89d-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="be89d-117">この種類の要求を処理するためには、カスタム ルートを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="be89d-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="be89d-118">リスト 1 の Global.asax ファイルには、ブログ、/Archive/のようなどのハンドル要求をという名前の新しいカスタム ルートが含まれている*エントリ日付*です。</span><span class="sxs-lookup"><span data-stu-id="be89d-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="be89d-119">**1 - (カスタム ルーティング) を持つ Global.asax を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="be89d-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="be89d-120">ルート テーブルに追加するルートの順序が重要です。</span><span class="sxs-lookup"><span data-stu-id="be89d-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="be89d-121">既存の既定のルートの前に、新しいカスタム ブログ ルートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="be89d-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="be89d-122">順序を反転する場合、既定のルート常に呼び出されるカスタム ルートの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="be89d-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="be89d-123">カスタムのブログ ルートでは、/アーカイブ/で始まるすべての要求と一致します。</span><span class="sxs-lookup"><span data-stu-id="be89d-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="be89d-124">そのため、すべて次の Url の一致します。</span><span class="sxs-lookup"><span data-stu-id="be89d-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="be89d-125">/アーカイブ/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="be89d-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="be89d-126">/アーカイブ 2004/10-6-</span><span class="sxs-lookup"><span data-stu-id="be89d-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="be89d-127">/Archive/apple</span><span class="sxs-lookup"><span data-stu-id="be89d-127">/Archive/apple</span></span>

<span data-ttu-id="be89d-128">カスタム ルートは、Archive というコント ローラーに、受信要求をマップし、Entry() アクションを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="be89d-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="be89d-129">Entry() メソッドが呼び出されると、エントリの日付は entryDate をという名前のパラメーターとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="be89d-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="be89d-130">一覧表示する 2 でのコント ローラーでは、ブログのカスタム ルートを使用できます。</span><span class="sxs-lookup"><span data-stu-id="be89d-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="be89d-131">**2 - ArchiveController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="be89d-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="be89d-132">リスト 2 で Entry() メソッドが DateTime 型のパラメーターを受け入れることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="be89d-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="be89d-133">MVC フレームワークには、URL からのエントリの日付を自動的に DateTime 値に変換します。</span><span class="sxs-lookup"><span data-stu-id="be89d-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="be89d-134">エラーが発生する場合は、URL からのエントリの日付のパラメーターは、DateTime に変換できない、(図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="be89d-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="be89d-135">**図 1 - パラメーターの変換からのエラー**</span><span class="sxs-lookup"><span data-stu-id="be89d-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="be89d-136">[![[新しいプロジェクト] ダイアログ ボックス](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="be89d-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="be89d-137">**図 01**: パラメーターの変換からのエラー ([フルサイズのイメージを表示するをクリックして](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="be89d-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="be89d-138">まとめ</span><span class="sxs-lookup"><span data-stu-id="be89d-138">Summary</span></span>

<span data-ttu-id="be89d-139">このチュートリアルの目的は、カスタム ルートを作成する方法を示すためにでした。</span><span class="sxs-lookup"><span data-stu-id="be89d-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="be89d-140">ブログ エントリを表す Global.asax ファイルでルート テーブルに対するカスタム ルートを追加する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="be89d-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="be89d-141">ブログ エントリの要求を ArchiveController をという名前、コント ローラーと Entry() をという名前のコント ローラー アクションにマップする方法を説明したとします。</span><span class="sxs-lookup"><span data-stu-id="be89d-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be89d-142">[前へ](asp-net-mvc-controller-overview-vb.md)
> [次へ](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="be89d-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
