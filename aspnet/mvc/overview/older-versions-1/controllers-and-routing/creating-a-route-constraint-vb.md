---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: ルート制約 (VB) を作成する |Microsoft Docs
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、正規表現のルート制約を作成して、ブラウザーが一致するルートを要求する方法を制御する方法について説明します。
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a8e540a00d852d5b710bfdbf63a68f6e6d280ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832173"
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="da446-103">ルート制約 (VB) を作成します。</span><span class="sxs-lookup"><span data-stu-id="da446-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="da446-104">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="da446-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="da446-105">このチュートリアルでは、Stephen Walther は、正規表現のルート制約を作成して、ブラウザーが一致するルートを要求する方法を制御する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="da446-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="da446-106">ルート制約を使用すると、特定のルートに一致するブラウザーの要求を制限できます。</span><span class="sxs-lookup"><span data-stu-id="da446-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="da446-107">正規表現を使用して、ルート制約を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="da446-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="da446-108">たとえば、ルート、Global.asax ファイルでリスト 1 で定義したとします。</span><span class="sxs-lookup"><span data-stu-id="da446-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="da446-109">**1 - Global.asax.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="da446-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="da446-110">1 を一覧表示するには、製品をという名前のルートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="da446-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="da446-111">製品のルートを使用して、リスト 2 に含まれる「productcontroller」ブラウザー要求をマップすることができます。</span><span class="sxs-lookup"><span data-stu-id="da446-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="da446-112">**2 - Controllers\ProductController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="da446-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="da446-113">製品のコント ローラーによって公開される Details() アクションが productId という 1 つのパラメーターを受け入れることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="da446-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="da446-114">このパラメーターは、整数パラメーターです。</span><span class="sxs-lookup"><span data-stu-id="da446-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="da446-115">リスト 1 で定義されているルートは、次の Url のいずれかに一致します。</span><span class="sxs-lookup"><span data-stu-id="da446-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="da446-116">/製品/23</span><span class="sxs-lookup"><span data-stu-id="da446-116">/Product/23</span></span>
- <span data-ttu-id="da446-117">/製品/7</span><span class="sxs-lookup"><span data-stu-id="da446-117">/Product/7</span></span>

<span data-ttu-id="da446-118">残念ながら、ルートには、次の Url は一致も。</span><span class="sxs-lookup"><span data-stu-id="da446-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="da446-119">/製品/とおりです。</span><span class="sxs-lookup"><span data-stu-id="da446-119">/Product/blah</span></span>
- <span data-ttu-id="da446-120">/製品/apple</span><span class="sxs-lookup"><span data-stu-id="da446-120">/Product/apple</span></span>

<span data-ttu-id="da446-121">Details() アクションには、整数パラメーターが必要ですが、ため、整数値以外のものを含む要求を行うと、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="da446-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="da446-122">たとえば、URL/Product/apple をお使いのブラウザーに入力した場合は、図 1 エラー ページを表示がされます。</span><span class="sxs-lookup"><span data-stu-id="da446-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="da446-123">[![[新しいプロジェクト] ダイアログ ボックス](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="da446-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="da446-124">**図 01**: 展開のページが表示 ([フルサイズの画像を表示する をクリックします](creating-a-route-constraint-vb/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="da446-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="da446-125">本当にする内容は、のみ一致する適切な整数 productId を含む Url です。</span><span class="sxs-lookup"><span data-stu-id="da446-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="da446-126">ルートに一致する Url を制限するのにルートを定義するときに制約を使用できます。</span><span class="sxs-lookup"><span data-stu-id="da446-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="da446-127">リスト 3 で修正された製品のルートには、整数にのみ一致する正規表現の制約が含まれています。</span><span class="sxs-lookup"><span data-stu-id="da446-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="da446-128">**3 - Global.asax.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="da446-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="da446-129">正規表現 \d+ では、1 つまたは複数の整数と一致します。</span><span class="sxs-lookup"><span data-stu-id="da446-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="da446-130">この制約により、次の Url と一致する製品ルート。</span><span class="sxs-lookup"><span data-stu-id="da446-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="da446-131">/製品/3</span><span class="sxs-lookup"><span data-stu-id="da446-131">/Product/3</span></span>
- <span data-ttu-id="da446-132">/製品/8999</span><span class="sxs-lookup"><span data-stu-id="da446-132">/Product/8999</span></span>

<span data-ttu-id="da446-133">次の Url ではないです。</span><span class="sxs-lookup"><span data-stu-id="da446-133">But not the following URLs:</span></span>

- <span data-ttu-id="da446-134">/製品/apple</span><span class="sxs-lookup"><span data-stu-id="da446-134">/Product/apple</span></span>
- <span data-ttu-id="da446-135">/製品</span><span class="sxs-lookup"><span data-stu-id="da446-135">/Product</span></span>

<span data-ttu-id="da446-136">別のルートでこれらのブラウザー要求を処理します。 または、一致のルートがない場合、*リソースが見つかりませんでした*エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="da446-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="da446-137">[前へ](creating-custom-routes-vb.md)
> [次へ](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="da446-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
