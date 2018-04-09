---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: キャッシュされたページ (VB) への動的なコンテンツの追加 |Microsoft ドキュメント
author: microsoft
description: 同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。 キャッシュ後置換では、バナー広告 o などの動的なコンテンツを表示することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 89421b4bec2170e408ded87ccc918a7a16844a98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="21168-104">キャッシュされたページ (VB) に動的なコンテンツを追加します。</span><span class="sxs-lookup"><span data-stu-id="21168-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="21168-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="21168-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="21168-106">同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="21168-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="21168-107">キャッシュ後置換では、バナー広告またはキャッシュされた出力されているページ内のニュース項目などの動的なコンテンツを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="21168-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="21168-108">出力キャッシュの利用して、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上できます。</span><span class="sxs-lookup"><span data-stu-id="21168-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="21168-109">ページ、ページが要求されるたびに、再生成するのではなくページを 1 回生成し、複数のユーザー用のメモリにキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="21168-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="21168-110">問題があります。</span><span class="sxs-lookup"><span data-stu-id="21168-110">But there is a problem.</span></span> <span data-ttu-id="21168-111">場合、ページに動的なコンテンツを表示する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="21168-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="21168-112">たとえば、バナー広告をページに表示することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="21168-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="21168-113">すべてのユーザーが非常に同じ提供情報を表示するようにキャッシュされるバナー広告をしたくありません。</span><span class="sxs-lookup"><span data-stu-id="21168-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="21168-114">このように、money を行うはありません!</span><span class="sxs-lookup"><span data-stu-id="21168-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="21168-115">幸いにも、簡単な解決策があります。</span><span class="sxs-lookup"><span data-stu-id="21168-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="21168-116">呼ばれる、ASP.NET フレームワークの機能の活用*キャッシュ後置換*です。</span><span class="sxs-lookup"><span data-stu-id="21168-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="21168-117">キャッシュ後置換では、メモリにキャッシュされているページの動的コンテンツを代用することができます。</span><span class="sxs-lookup"><span data-stu-id="21168-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="21168-118">通常、出力する場合、このページを使用して、キャッシュ、 &lt;OutputCache&gt;サーバーとクライアント (web ブラウザー) の両方で、属性、ページがキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="21168-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="21168-119">キャッシュ後置換を使用すると、ページは、サーバー上でのみキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="21168-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="21168-120">キャッシュ後置換を使用します。</span><span class="sxs-lookup"><span data-stu-id="21168-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="21168-121">キャッシュ後置換を使用するには、2 つの手順が必要です。</span><span class="sxs-lookup"><span data-stu-id="21168-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="21168-122">最初に、キャッシュされたページに表示する動的なコンテンツを表す文字列を返すメソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="21168-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="21168-123">次に、ページに動的なコンテンツを注入 HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="21168-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="21168-124">たとえば、キャッシュされたページの異なるニュース項目をランダムに表示することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="21168-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="21168-125">リスト 1 のクラスは、ランダムに 3 つのニュース項目の一覧から 1 つのニュース項目を返す RenderNews() をという名前の 1 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="21168-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="21168-126">**1 – Models\News.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="21168-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="21168-127">キャッシュ後置換を利用するには、HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="21168-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="21168-128">WriteSubstitution() メソッドでコードを設定すると、動的コンテンツを含む、キャッシュされたページの領域を置換します。</span><span class="sxs-lookup"><span data-stu-id="21168-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="21168-129">WriteSubstitution() メソッドがビューを一覧表示する 2 で、ランダムなニュース項目を表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="21168-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="21168-130">**Listing 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="21168-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="21168-131">RenderNews メソッドは、WriteSubstitution() メソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="21168-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="21168-132">RenderNews メソッドが呼び出されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="21168-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="21168-133">代わりに、メソッドへの参照は、AddressOf 演算子を利用して WriteSubstitution() に渡されます。</span><span class="sxs-lookup"><span data-stu-id="21168-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="21168-134">インデックス ビューがキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="21168-134">The Index view is cached.</span></span> <span data-ttu-id="21168-135">ビューがリスト 3 のコント ローラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="21168-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="21168-136">Index() アクションで装飾されてことに注意してください、 &lt;OutputCache&gt;を 60 秒間キャッシュに保存するインデックス ビューの原因となる属性。</span><span class="sxs-lookup"><span data-stu-id="21168-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="21168-137">**3 – Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="21168-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="21168-138">場合でも、インデックス ビューをキャッシュすると、インデックス ページを要求するときに別のランダムなニュース項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="21168-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="21168-139">インデックス ページを要求するときにページが表示される時刻は 60 秒間の (図 1 を参照してください) は変更されません。</span><span class="sxs-lookup"><span data-stu-id="21168-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="21168-140">時刻が変更されないという事実は、ページがキャッシュされていることを証明します。</span><span class="sxs-lookup"><span data-stu-id="21168-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="21168-141">各要求と一緒に WriteSubstitution() メソッド – ランダムなニュース項目 – 変更によって、コンテンツが挿入ただし、します。</span><span class="sxs-lookup"><span data-stu-id="21168-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="21168-142">**図 1 – キャッシュされたページ内の動的なニュース項目を挿入**</span><span class="sxs-lookup"><span data-stu-id="21168-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="21168-144">ヘルパー メソッドでのキャッシュ後置換の使用</span><span class="sxs-lookup"><span data-stu-id="21168-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="21168-145">キャッシュ後置換を活用する簡単な方法では、カスタム ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="21168-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="21168-146">このアプローチは、ヘルパー メソッドを一覧表示する 4 で説明されています。</span><span class="sxs-lookup"><span data-stu-id="21168-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="21168-147">**4 – Helpers\AdHelper.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="21168-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="21168-148">2 つのメソッドを公開する Visual Basic モジュールを含むリスト 4: RenderBanner() と RenderBannerInternal() です。</span><span class="sxs-lookup"><span data-stu-id="21168-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="21168-149">RenderBanner() メソッドは、実際のヘルパー メソッドを表します。</span><span class="sxs-lookup"><span data-stu-id="21168-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="21168-150">このメソッドは、他のヘルパー メソッドと同じようにビューで Html.RenderBanner() を呼び出すことができるように、標準の ASP.NET MVC の HtmlHelper クラスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="21168-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="21168-151">RenderBanner() メソッドは、RenderBannerInternal() メソッドを WriteSubsitution() メソッドに渡す HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="21168-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="21168-152">RenderBannerInternal() メソッドは、プライベート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="21168-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="21168-153">このメソッドは、ヘルパー メソッドとして公開されません。</span><span class="sxs-lookup"><span data-stu-id="21168-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="21168-154">RenderBannerInternal() メソッドは、バナー広告の 3 つのイメージの一覧から、バナー広告の 1 つのイメージをランダムに返します。</span><span class="sxs-lookup"><span data-stu-id="21168-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="21168-155">インデックス ビューを一覧表示する 5 では、RenderBanner() ヘルパー メソッドを使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="21168-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="21168-156">注意して、追加&lt;% @ インポート %&gt;ディレクティブは MvcApplication1.Helpers 名前空間をインポートするビューの上部に含まれます。</span><span class="sxs-lookup"><span data-stu-id="21168-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="21168-157">この名前空間をインポートした場合、RenderBanner() メソッドは Html プロパティのメソッドとして表示されません。</span><span class="sxs-lookup"><span data-stu-id="21168-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="21168-158">**5 – Views\Home\Index.aspx で (RenderBanner() メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="21168-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="21168-159">5 の一覧表示するビューで表示されたページを要求すると、要求ごとに異なるバナー広告が表示されます (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="21168-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="21168-160">ページがキャッシュされるが、RenderBanner() ヘルパー メソッドによって、バナー広告が動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="21168-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="21168-161">**図 2 – ランダム バナー広告を表示するインデックス ビュー**</span><span class="sxs-lookup"><span data-stu-id="21168-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="21168-163">まとめ</span><span class="sxs-lookup"><span data-stu-id="21168-163">Summary</span></span>

<span data-ttu-id="21168-164">このチュートリアルでは、キャッシュされたページのコンテンツを動的に更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="21168-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="21168-165">HttpResponse.WriteSubstitution() メソッドを使用して、キャッシュされたページに挿入する動的なコンテンツを有効にする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="21168-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="21168-166">HTML ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="21168-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="21168-167">利用可能な限り – 持つ、web アプリケーションのパフォーマンスに大幅な影響を受けることができますをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="21168-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="21168-168">このチュートリアルで説明、キャッシュ、ページに動的なコンテンツを表示する必要がある場合にも利用できます。</span><span class="sxs-lookup"><span data-stu-id="21168-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21168-169">[前へ](improving-performance-with-output-caching-vb.md)
> [次へ](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="21168-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
