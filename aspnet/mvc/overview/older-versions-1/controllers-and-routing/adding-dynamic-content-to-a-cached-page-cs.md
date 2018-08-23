---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: キャッシュされたページ (c#) への動的なコンテンツの追加 |Microsoft Docs
author: microsoft
description: 同じページで、動的およびキャッシュ済みコンテンツを混在させる方法について説明します。 キャッシュ後置換では、バナー広告 o などの動的なコンテンツを表示することができます.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: a03f943b936c68215d65dca92e62431642226993
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831522"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="07ccc-104">キャッシュされたページ (c#) への動的なコンテンツの追加</span><span class="sxs-lookup"><span data-stu-id="07ccc-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="07ccc-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="07ccc-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="07ccc-106">同じページで、動的およびキャッシュ済みコンテンツを混在させる方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="07ccc-107">キャッシュ後置換では、バナー広告またはキャッシュされた出力されたページ内のニュース項目などの動的なコンテンツを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="07ccc-108">出力キャッシュの利用して、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上できます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="07ccc-109">ページ、ページが要求されるたびに、再生成するのではなくページを 1 回生成し、複数のユーザー用のメモリにキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="07ccc-110">問題があります。</span><span class="sxs-lookup"><span data-stu-id="07ccc-110">But there is a problem.</span></span> <span data-ttu-id="07ccc-111">場合、ページに動的なコンテンツを表示する必要がありますか。</span><span class="sxs-lookup"><span data-stu-id="07ccc-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="07ccc-112">たとえば、バナー広告をページに表示することを想像してください。</span><span class="sxs-lookup"><span data-stu-id="07ccc-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="07ccc-113">すべてのユーザーがまったく同じ提供情報を認識するようにキャッシュするバナー広告は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="07ccc-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="07ccc-114">その方法も収益を上げるはありません。</span><span class="sxs-lookup"><span data-stu-id="07ccc-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="07ccc-115">さいわい、簡単なソリューションがあります。</span><span class="sxs-lookup"><span data-stu-id="07ccc-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="07ccc-116">呼ばれる ASP.NET フレームワークの機能の利用*キャッシュ後の置換*します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="07ccc-117">キャッシュ後の置換を使用すると、メモリにキャッシュされているページで動的なコンテンツを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="07ccc-118">通常、[OutputCache] 属性を使用してキャッシュ ページを出力すると、ページは、サーバーとクライアント (web ブラウザー) の両方でキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="07ccc-119">キャッシュ後の置換を使用すると、ページがサーバー上でのみキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="07ccc-120">キャッシュ後の置換を使用します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="07ccc-121">キャッシュ後の置換を使用するには、2 つの手順が必要です。</span><span class="sxs-lookup"><span data-stu-id="07ccc-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="07ccc-122">最初に、キャッシュされたページに表示する動的なコンテンツを表す文字列を返すメソッドを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="07ccc-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="07ccc-123">次に、ページに動的なコンテンツを挿入する HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="07ccc-124">たとえば、キャッシュされたページでさまざまなニュース項目をランダムに表示することに想像してください。</span><span class="sxs-lookup"><span data-stu-id="07ccc-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="07ccc-125">リスト 1 でクラスでは、3 つのニュース項目の一覧からランダムに 1 つのニュース項目を返す、RenderNews() という名前の 1 つのメソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="07ccc-126">**1 – Models\News.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="07ccc-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="07ccc-127">キャッシュ後の置換を利用するには、HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="07ccc-128">WriteSubstitution() メソッドでコードを設定すると、動的コンテンツを含む、キャッシュされたページの領域を置換します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="07ccc-129">WriteSubstitution() メソッドは、リスト 2 でのビューでランダムなニュース項目の表示に使用されます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="07ccc-130">**2 – Views\Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="07ccc-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="07ccc-131">RenderNews メソッドは、WriteSubstitution() メソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="07ccc-132">RenderNews メソッドが呼び出されない点に注意してください (かっこはありません)。</span><span class="sxs-lookup"><span data-stu-id="07ccc-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="07ccc-133">代わりに、メソッドへの参照は、WriteSubstitution() に渡されます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="07ccc-134">インデックス ビューにキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-134">The Index view is cached.</span></span> <span data-ttu-id="07ccc-135">ビューは、リスト 3 のコント ローラーによって返されます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="07ccc-136">Index() アクションが 60 秒間キャッシュされるように、インデックス ビューを原因となる [OutputCache] 属性で修飾されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="07ccc-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="07ccc-137">**3 – controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="07ccc-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="07ccc-138">インデックス ビューがキャッシュされている場合でも、インデックス ページを要求するときに異なるランダムなニュースの項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="07ccc-139">インデックス ページを要求するときに、ページが表示される時刻は 60 秒間の (図 1 参照) は変わりません。</span><span class="sxs-lookup"><span data-stu-id="07ccc-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="07ccc-140">時刻が変更されないという事実は、ページがキャッシュされていることを証明します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="07ccc-141">WriteSubstitution() メソッド – ランダム ニュース項目 – が変更された各要求によって、コンテンツを挿入するただし、します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="07ccc-142">**図 1 – キャッシュされたページで動的ニュース項目を挿入します。**</span><span class="sxs-lookup"><span data-stu-id="07ccc-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="07ccc-144">キャッシュ後の置換を使用して、ヘルパー メソッド</span><span class="sxs-lookup"><span data-stu-id="07ccc-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="07ccc-145">キャッシュ後の置換を活用する簡単な方法では、カスタム ヘルパー メソッド内で WriteSubstitution() メソッドの呼び出しをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="07ccc-146">このアプローチは、リスト 4 ヘルパー メソッドに示します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="07ccc-147">**4 – AdHelper.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="07ccc-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="07ccc-148">リスト 4 は、2 つのメソッドを公開する静的クラスが含まれています: RenderBanner() と RenderBannerInternal() します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="07ccc-149">RenderBanner() メソッドでは、実際のヘルパー メソッドを表します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="07ccc-150">このメソッドは、標準の ASP.NET MVC の HtmlHelper クラスを拡張するので、他のヘルパー メソッドと同様のビューで Html.RenderBanner() を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="07ccc-151">RenderBanner() メソッドを WriteSubsitution() メソッド RenderBannerInternal() メソッドに渡す HttpResponse.WriteSubstitution() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="07ccc-152">RenderBannerInternal() メソッドは、プライベート メソッドです。</span><span class="sxs-lookup"><span data-stu-id="07ccc-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="07ccc-153">このメソッドは、ヘルパー メソッドとして公開されません。</span><span class="sxs-lookup"><span data-stu-id="07ccc-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="07ccc-154">RenderBannerInternal() メソッドは、バナー広告の 3 つのイメージの一覧からランダムに 1 つのバナー広告イメージを返します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="07ccc-155">変更したのインデックス ビュー リスト 5 では、RenderBanner() ヘルパー メソッドを使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="07ccc-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="07ccc-156">注意して、追加&lt;インポート % @ %&gt;ディレクティブは MvcApplication1.Helpers 名前空間をインポートするには、ビューの上部に含まれます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="07ccc-157">この名前空間のインポートを怠ると、RenderBanner() メソッドは、Html プロパティのメソッドとして表示されません。</span><span class="sxs-lookup"><span data-stu-id="07ccc-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="07ccc-158">**5 – (RenderBanner() メソッド) を使用して Views\Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="07ccc-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="07ccc-159">リスト 5 で、ビューによって表示されるページを要求すると、要求ごとに異なるバナー広告が表示されます (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="07ccc-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="07ccc-160">ページ キャッシュされますが、RenderBanner() ヘルパー メソッドによって動的にバナー広告を挿入します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="07ccc-161">**図 2 – ランダムなバナー広告を表示するインデックス ビュー**</span><span class="sxs-lookup"><span data-stu-id="07ccc-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="07ccc-163">まとめ</span><span class="sxs-lookup"><span data-stu-id="07ccc-163">Summary</span></span>

<span data-ttu-id="07ccc-164">このチュートリアルでは、キャッシュされたページのコンテンツを動的に更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="07ccc-165">HttpResponse.WriteSubstitution() メソッドを使用して、キャッシュされたページに挿入されるようにする動的なコンテンツを有効にする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="07ccc-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="07ccc-166">HTML ヘルパー メソッド内で WriteSubstitution() メソッドの呼び出しをカプセル化する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="07ccc-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="07ccc-167">Web アプリケーションのパフォーマンスに大きな影響を及ぼすことができます: 可能な限りキャッシュを活用します。</span><span class="sxs-lookup"><span data-stu-id="07ccc-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="07ccc-168">前述のこのチュートリアルでは、ページに動的なコンテンツを表示する必要があるときにも、キャッシュの利用できます。</span><span class="sxs-lookup"><span data-stu-id="07ccc-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="07ccc-169">[前へ](improving-performance-with-output-caching-cs.md)
> [次へ](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="07ccc-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
